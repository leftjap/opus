## 2026-03-30

### Changed
- 교훈 15건 삭제, 4건(L-05/L-11/L-12/L-13) 유지 — common-rules.md·POLT·ALT·각 AGENTS.md에 흡수된 중복 항목 제거. 교훈은 "common-rules.md에 없는 크로스 프로젝트 실패 패턴"만 유지하는 원칙 확립. (opus.md)
- 교훈 테이블에 "만료 조건" 열 추가 — 임시 교훈 도입 시 만료 시점을 명시하기 위한 구조. 현재 4건은 영구(`—`). (opus.md)
- 교훈 추가 조건 3개로 재정의 + 추가 시 기존 항목 만료 체크 — 교훈이 Opus 판단 없이 자동 증가하던 문제 방지. 주체를 Opus로 한정. (opus.md)
- D-01 균형 조항, D-02 주어 명확화, D-06 AGENTS.md 참조로 변경 — 억지 반론 방지 + 검증 주체 명시 + 고정 목록 제거. (opus.md)
- 상태 점검 프로토콜에 CHANGELOG 원인 태그 크롤링 3단계 추가 — 교훈 추가 조건 ②의 실행 경로 확보. 원인 태그 미도입 레포는 자동 스킵. (opus.md)
- CHANGELOG.md 크롤링 한계 근접 시 연도별 분리 규칙 추가. (common-rules.md)
- CHANGELOG 갱신 트리거를 "opus.md 갱신 Step 연동"에서 "모든 작업지시서 커밋 직전"으로 변경 + 제외 규칙 삭제 — CHANGELOG를 모든 작업지시서의 실행 로그로 활용. (common-rules.md)
- 전 프로젝트 CLAUDE.md에서 playbook→opus 명칭 전면 교체 — opus 레포 리네임 반영. (CLAUDE.md ×7, common-rules.md)
- opus CLAUDE.md에서 Opus 관리 사항 4건 삭제 (모호한 표현 금지, opus.md 참조 포인터, 200줄 제한, 변경 이력 3일분) — HumanLayer 연구 기반 CLAUDE.md 최적화
- keep CLAUDE.md에서 switchTab() else 블록 항목 삭제 — 조건부 판단 규칙은 작업지시서 Step이 확실
- gym CLAUDE.md에서 syncFromServer 타임스탬프 항목 삭제 — 특정 함수 비교 연산자 규칙은 작업지시서 Step이 확실
- study CLAUDE.md에서 saveLangData()+saveToServer() 호출 및 언어 전환 패턴 항목 삭제 — 로직 순서·코딩 패턴은 작업지시서 Step이 확실
- clasp push 규칙을 opus CLAUDE.md(공통)로 통합하고 keep·gym·study·docs 개별 항목 삭제 — 4곳 중복 제거, HumanLayer 연구 기반 지시 수 최소화

### Removed
- opus CLAUDE.md: decisions/ 디렉토리 참조 (실사용 없음)
