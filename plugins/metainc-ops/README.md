# metainc-ops (Claude Code 플러그인)

메타아이앤씨 운영 스킬 묶음. 현재 스킬: **deploy-hub**.

## 설치
```
/plugin marketplace add metainchelp-boop/andrej-karpathy-skills
/plugin install metainc-ops@metainc-plugins
/reload-plugins
```
> ⚠️ 마켓플레이스는 저장소 **기본 브랜치**에서 읽힙니다 → 이 PR이 기본 브랜치에 머지된 뒤 설치 가능.

## 사용
- `/metainc-ops:deploy-hub` — ready PR 취합·충돌/빌드 재검증·의존성 순서대로 '오늘 저녁 배포 묶음' 계획. "배포하자" 시 머지·배포·검증 안내.

## 계정 전역(웹·로컬 모든 세션)
```
/plugin install metainc-ops@metainc-plugins --scope user
```
