# metainc-ops (Claude Code 플러그인)

메타아이앤씨 운영 + 범용 스킬 묶음. 한 번 설치하면 계정의 모든 섹션(웹·로컬)에서 쓴다.

## 포함 스킬
- **deploy-hub** (`/metainc-ops:deploy-hub`) — ready PR 취합·검증·저녁 일괄 배포 안내 (수동 전용)
- **web-design-guidelines** — UI 코드 접근성·UX 점검 (출처 vercel-labs/agent-skills)
- **dispatching-parallel-agents** — 독립 작업 병렬 에이전트 분배 (출처 obra/superpowers)
- **frontend-design** — 비주얼 디자인 방향·타이포 (출처 anthropics/skills)
- **github-actions-docs** — GitHub Actions 공식 문서 기반 답변 (출처 xixu-me/skills)

## 설치 (이 마켓플레이스가 기본 브랜치 main에 머지된 뒤)
```
/plugin marketplace add metainchelp-boop/andrej-karpathy-skills
/plugin install metainc-ops@metainc-plugins --scope user   # 계정 전역(웹·로컬 모든 섹션)
/reload-plugins
```

## 별도 — 출처에서 계정 enable 권장 (무견거나 동반 스킬/외부 설정 필요)
| 스킬 | 출처 | 비고 |
|---|---|---|
| skill-creator | anthropics/skills | 공식, 스크립트 다수 |
| react-best-practices | vercel-labs/agent-skills | 76파일 |
| microsoft-foundry / azure-ai / azure-upgrade | microsoft/azure-skills | Foundry 151파일 |
| improve-codebase-architecture | mattpocock/skills | codebase-design·grilling·domain-modeling 동반 필요 |
| brainstorming | obra/superpowers | 스크립트 포함 |
| sales-enablement | coreyhaines31/marketingskills | |
| agent-browser | vercel-labs/agent-browser | `npm i -g agent-browser` 필요 |
| sleek-design-mobile-apps | sleekdotdesign/agent-skills | SLEEK_API_KEY 필요 |

설치: 각 출처를 `npx skills add <owner/repo> --skill <name> -g`(로컬) 또는 `/plugin marketplace add`(계정 전역).
