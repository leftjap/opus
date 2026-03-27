# Claude Code 데스크톱 앱 검토

- date: 2026-03-24
- tags: tooling, claude-code, workflow

## 맥락
현재 워크플로우: 젠스파크(Opus) 설계 → VS Code 확장(Haiku) 실행.
Claude Code 데스크톱 앱이 VS Code 확장보다 나은지 검토.

## 결론
- 데스크톱 앱에만 있는 기능: 앱 프리뷰(dev 서버+자동 검증), PR 자동 모니터링/수정/머지, Connectors(GitHub·Slack·Linear 등 클릭 연결), 병렬 세션(Git worktree 격리), 스케줄 태스크, Dispatch(폰에서 작업 지시), Remote 세션(클라우드 실행).
- 토큰 효율: CLI가 가장 효율적, VS Code 확장이 그 다음, 데스크톱 앱이 가장 많이 소비(auto-verify 등 추가 기능 때문). Reddit 다수 보고.
- 데스크톱 앱 기능은 기술적으로 VS Code에서 불가능한 게 아님. 편의성을 토큰으로 사는 구조.
- GitHub 연결 테스트: private 레포(claude-config 등) 접근 확인됨.

## 후속
- 당장 전환 불필요. 토큰 비용이 최우선이면 현재 구조 유지.
- 레포 이름 정리 후 데스크톱 앱에서 간단한 작업 하나를 실제로 돌려보고 토큰 소비량 비교 테스트.
