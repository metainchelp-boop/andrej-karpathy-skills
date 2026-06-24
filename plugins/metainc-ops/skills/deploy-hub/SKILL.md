---
name: deploy-hub
description: 배포 허브 — 여러 저장소의 ready PR을 취합해 충돌(mergeable)·빌드를 재검증하고, 의존성 순서대로 '오늘 저녁 배포 묶음' 계획을 만든다. 운영자가 "배포하자"(저녁) 하면 순서대로 머지·배포·검증까지 안내한다. 코드 섹션들이 올린 작업을 모아 저녁에 일괄 배포할 때 사용.
disable-model-invocation: true
---

# 배포 허브 (/metainc-ops:deploy-hub)

여러 '코드 섹션'이 올린 작업을 이 섹션이 취합·검증해 **저녁(업무시간 외)에 일괄 배포**하기 위한 스킬.
상세 절차·핸드오프 규약: `metainc-web-backend` 의 `docs/INTEGRATION_DEPLOY_RUNBOOK.md` · `docs/CODE_SECTION_HANDOFF.md`

## 대상 저장소 & 배포 특성 (실측)

| 저장소 (owner: metainchelp-boop) | base | 머지=배포? | 배포 특성 / 리스크 |
|---|---|---|---|
| `metainc-web-backend` (BE) | master | 예 | `push:master` + **paths-ignore**(`**/*.md`,`docs/**`,`.claude/**` 등) → 문서·.claude만 변경은 무배포. src/gradle 변경 시 `bootWar` → ROOT.war 원자적 교체 → **앱 교체 순간 사이트 다운 + Cafe24 패널 톰캣 수동 재시작** |
| `metainc-web-frontend` (FE) | master | 예 | `push:master` **경로 필터 없음** + `workflow_dispatch` → 문서 1줄도 `vite build` → `git push cafe24 HEAD:master --force`. 묶음당 FE 머지·배포 1회로 최소화 |
| `korean-law-mcp` | main | **아니오** | main 머지 ≠ 배포. 배포는 **Fly.io `/deploy` 스킬**로 저녁에 별도 수행. ready=코드 완료 신호일 뿐 |
| `logic-analysis` | main | 예 | `push:main` **경로 필터 없음** → VPS로 SCP + docker-compose 재기동 + nginx reload. 배포 대상 |
| `andrej-karpathy-skills` | (확인) | 미파악 | 스킬/문서 저장소 — 배포 특성 등재 전까지 묶음 제외 |
| `meta-chehumdan` | (없음) | — | **빈 저장소**(브랜치 0, main ref 미존재). 운영자 확인 전 초기화·배포 금지 |
| `-` | main | 아니오 | 배포 워크플로우 없음(거의 빈 저장소). 제품 초안/논의 — 통상 배포 대상 아님 |

> 미파악 저장소는 `.github/workflows`를 1회 조사해 위 표·CLAUDE.md에 등재한 뒤에만 묶음에 편입한다.

## 절대 규칙 (안전장치)

1. **머지/기준 브랜치 푸시/배포는 운영자가 "배포하자" 했을 때만, 저녁(업무시간 외)에.** 그 외엔 계획만 출력하고 멈춘다.
2. **ready PR만** 후보. draft는 절대 만지지 않는다. ready 3조건 미충족(자체검증·선행 머지·식별자 0) PR은 머지 차단.
3. 허브 ops 산출물은 지정 브랜치 `claude/vibrant-cerf-1dnqsa`에서. **트라이얼 통합은 별도 throwaway 스크래치 브랜치**(예: `hub/trial-YYYYMMDD`)에서 하고 어떤 PR의 head/base로도 쓰지 않으며 master로 머지하지 않는다. 코드 섹션 브랜치는 건드리지 않는다.
4. 배포 전 각 묶음의 **롤백 지점(직전 정상 커밋 SHA)** 기록. (FE 롤백은 GitHub revert만으론 부족 → `workflow_dispatch`로 직전 양호 산출물 재배포)
5. 머지 전 PR 본문·커밋의 **AI·모델 식별자 스캔**, 발견 시 머지 차단·스크럽. 커밋/PR에 식별자 금지. 한글 UTF-8.

## 절차

### 1. 스캔 (언제든)
- 각 저장소 `list_pull_requests(state=open)` → **draft=false(ready)** 만 후보로, draft는 "대기"로 분류.
- `list_branches`로 PR 없는 작업 브랜치도 파악(ready 신호 누락 확인).

### 2. 충돌·빌드 재검증 (기계 검증 — 자기신고 신뢰 금지)
- 각 ready PR `pull_request_read(get)` → `mergeable_state`. `dirty`/`behind` → 해당 코드 섹션에 rebase 요청.
- `get_status`/`get_check_runs`로 실제 체크 확인. **PR 트리거 CI가 없을 수 있음**(BE/FE deploy는 push 전용) → 그 경우 스크래치 브랜치에서 직접 빌드(BE `bootWar` / FE `vite build` / MCP `npm run build`) 통과 확인.
- 같은 파일을 만지는 ready PR이 둘 이상이면 영역 분리/순서 조정.

### 3. 의존성 순서 결정
- PR 본문의 **선행/후행: `repo#번호`** + `⛔ BLOCKED-BY` 라벨을 파싱. **브랜치명 일치는 의존성 근거가 아님.**
- 선행 미머지면 후행은 스킵. 새 API 제공(BE)을 먼저, 호출(FE)을 나중. (동시 배포 시 BE 다운 윈도우 중 FE 선행 → API 404)

### 4. 배포 묶음 계획 출력 (낮 기본 동작은 여기까지)

```
## 오늘 저녁 배포 묶음 (안) — YYYY-MM-DD
| 순서 | 저장소 | PR | 묶음 | mergeable | 빌드 | 식별자 | 배포특성 | 롤백SHA |
|------|--------|----|------|-----------|------|--------|----------|---------|
| 1 | BE | #__ | __ | clean | ✓ | 0 | 톰캣 재시작 | ______ |
| 2 | FE | #__ | __ | clean | ✓ | 0 | force-push | ______ |

대기(draft, ready 전환 필요): ...
배포 대상 아님: ...
```

### 5. 저녁 실행 — 운영자 "배포하자" 시에만
순서대로 1건씩:
1. 배포 직전 재확인: `git fetch` + `mergeable_state` 재확인 + 스크래치 빌드 재검증 + 식별자 0 확인.
2. (마이그레이션 동반 시) DB 백업 → expand 스키마 적용.
3. ready PR → 기준 브랜치 머지/푸시. (스쿼시 시 식별자 스크럽된 요약을 커밋 메시지로)
4. 저장소별 배포 특성 처리:
   - **BE**: 푸시 후 Cafe24 패널에서 **톰캣 수동 재시작** → 헬스/주요 API 응답 확인.
   - **FE**: 같은 묶음 PR은 **한 번의 master 반영**으로 모아 배포 → 새 빌드 로드 확인.
   - **korean-law-mcp**: main 머지와 별개로 **`/deploy`(Fly.io)** 로 배포.
   - **logic-analysis**: main 푸시 시 VPS 자동 배포 → 컨테이너 기동·nginx 확인.
5. 변경 기능 스모크 테스트.
6. `docs/INTEGRATION_DEPLOY_RUNBOOK.md` §7 일일 로그에 **배포 결과 + 롤백 지점**을 append-only로 기록.

## 롤백
- BE: 직전 정상 커밋으로 master 재푸시 → 톰캣 재시작.
- FE: GitHub revert만으론 부족 — `workflow_dispatch` 재실행으로 직전 양호 dist를 Cafe24에 재배포.
- MCP: 직전 양호 버전으로 `/deploy` 재실행.
- 각 묶음의 직전 정상 커밋 SHA를 §4/§7 기록값으로 되돌린다.
