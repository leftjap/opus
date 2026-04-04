## 2026-04-04

### Changed
- 세션 중 라우팅의 근거 출처 규칙을 서술형 블릿에서 독립 프로토콜(번호 시퀀스 3단계)로 구조 변경 — 서술형 금지 규칙이 LLM에서 반복 미트리거되어, 검색/크롤링 행동을 선행 조건으로 명시하는 구조로 전환. (opus.md)
- 상태 점검 3단계를 서술형에서 번호 시퀀스(3-a~3-e)로 분리 — 각 하위 단계에 출력을 명시하여 생략 방지. (opus.md)
- 상태 점검 3단계에 AGENTS.md 보호 체크리스트 기등록 확인 절차 추가 — 보호 코드 후보 제안 시 기등록 여부를 추측하지 않도록 방지. (opus.md)
- 세션 중 라우팅 근거 출처 표기를 3종 라벨(검색·참조·규칙)로 확장 — 기존 `(검색:)` 단일 라벨은 내부 규칙 대조 판단에서 트리거 누락을 유발. (opus.md)
- 세션 중 라우팅 규칙을 "추측 금지 + 검색 + 미검증 라벨" 3중 구조로 변경 — 금지형 규칙이 지시형보다 장기 준수율이 높고, 라벨 의무가 검증 행동을 유도함 (근거: arXiv 2602.11988, GPT-5 [TK—confirm] 패턴, Reddit r/LocalLLaMA 금지형/지시형 비교). (opus.md)
- 세션 중 라우팅: 추측 금지+미검증 라벨 구조를 검색 라벨 의무 구조로 교체 — 기술 선택·버그 원인·아키텍처 판단에 `(검색: [검색어] → [URL])` 라벨 필수, 라벨 없는 판단은 응답에서 제외. 이전 구조는 미검증 라벨이 면죄부로 작동하여 검색을 유도하지 못함 (opus.md)
- 세션 시작 3번: '크롤링 제외 목록' 용어 명확화 (opus.md)
- 세션 시작 3번: 직전 작업지시서로 수정한 파일의 크롤링 금지 규칙 추가 (opus.md)

### Removed
- common-rules.md 섹션 6-0 (증거 확보 프로토콜) 삭제 — 작업지시서 이전 단계의 트리거 규칙이 작업지시서 문서에 위치하면 적시에 읽히지 않음. 트리거는 opus.md로 이관, 잔류: 참조 우선순위·크롤링 규칙·혼합 참조. (common-rules.md)

## 2026-04-03

### Added
- 교훈 L-14 등록: Drive 폴더 변경 시 Code.js 경로 미갱신 → 데이터 분산 방지 (opus.md)

### Changed
- 운영 프로토콜에 "핸드오프(devlog)" 섹션 추가 — 사용자가 "핸드오프" 시 devlog-template.md 크롤링 후 양식 출력. (opus.md)

## 2026-04-02

### Changed
- opus.md에 규칙 충돌 해소 우선순위 삽입 (사용자 지시+보호 코드 병렬, 이후 AGENTS > common-rules > opus). [opus-88]
- L-13 방지책 강화: 동일 영역 3회 실패 시 Track B 전환, 증거 없는 가설 변경 금지. [opus-88]
- common-rules.md에 GAS 배포 후 스모크 테스트 및 롤백 절차 추가 (clasp v2.x 기준). [opus-88]
- common-rules.md에 데이터 스키마 변경 규칙 추가 (fallback, 마이그레이션, 멱등성). [opus-88]
- POLT 강화 조건에 confident-but-wrong 방지 문구 추가 — 강화 조건 해당 시 최종 확인 항목에 구체적 입력값·기대 결과 명시, 리스크 기반 우선 배치. 근거: Meta Semi-Formal Reasoning(arXiv:2603.01896), Gawande Checklist Manifesto. (common-rules.md)
- ALT A-2 출력 규칙에 외부 참조 확인 상태 표기 추가 — 비개발자가 검증 신뢰도를 판단할 수 있도록 (확인됨)/(캐시 참조)/(미확인) 구분. (opus.md)
- POLT ① Dry Run에 테스트 생성 시 실제 출력 기반 expected 설정 규칙 추가 — AI가 expected를 추측하지 않도록 강제. (common-rules.md)
- CLAUDE.md "반복 실패 방지" 제거, "테스트 실행 규칙" 신설 — 반복 실패 방지는 Opus 측 규칙으로 이관 예정, 테스트 루프 방지는 Haiku 세션 내 감지 가능. (CLAUDE.md)
- D-check 출력 규칙: 적용 대상이면 항상 `▷ [변환된 질문]` 출력 필수로 변경. 편향 미감지 시에도 "— 전제 타당" 표기. 근거: 비가시 지시의 실행 보장 불가(Chen 2026, Baker 2025), Dubois(2026) 리프레이밍 효과는 변환 결과가 출력에 존재해야 발생. (opus.md)
- ALT 점검 항목 문구: "출력하지 않음" → "상세 과정은 생략"으로 교체. "출력하지 않음"이 실행 스킵 신호로 작용하는 문제 방지. (opus.md)
- POLT ② Self-Critique에 "AGENTS.md 체크리스트 대조" 추가 — 5개 프로젝트(gym, study, keep, Finder, backlog)의 작업 전 체크리스트를 POLT 출력 경로에 편입. (common-rules.md)
- CLAUDE.md 백로그 직접 등록 규칙 삭제 — 백로그 앱으로 이관 완료. (CLAUDE.md)

### Added
- 세션 인수인계 규칙 — 트리거·필수 5구성·분량 제약 (common-rules.md)
- 과압축 방지 규칙 — 검색결과·결정·실패 기록 시 조건/사유/출처 보존 의무 (common-rules.md)

## 2026-04-01

### Changed
- D-check 메커니즘 확장: 단정형뿐 아니라 암묵적 전제가 있는 확인·의문도 질문 변환 대상에 포함. 출력에 `▷` 표기를 의무화하여 실행 보장 — autoregressive 생성 원리에 따라 출력 토큰이 후속 답변 품질을 조건부로 결정함. 근거: Dubois(2026) 2-step > 1-step 효과, Kumaran(2025) 비판 과민 반응, Anthropic(2025) CoT 비충실성. (opus.md)

## 2026-04-02

### Added
- 작업지시서 규모 제한 규칙 추가 (common-rules.md 섹션 2) — Haiku 후반 Step 생략 방지 목적. (common-rules.md)
- CHANGELOG Fixed 항목 원인 태그 형식 `[영역.세부원인]` 정의 (common-rules.md) — 교훈 자동 감지의 실행 조건 충족 목적
- 교훈 후보 감지 시 커밋 diff 크롤링 절차 및 영역 3회+ 확인 규칙 추가 (opus.md)

### Changed
- 상태 점검 3단계: 조건부 CHANGELOG 크롤링 → 무조건 크롤링으로 변경. 원인 태그 스캔·diff 크롤링·영역 집계 절차를 통합 (opus.md) — 조건문에 의한 스캔 생략 방지 및 shift-left 원칙 적용
- D-check 테이블 → 산문 압축. 점검 항목 유지, 테이블 형식 제거 — Opus 4.6 내재 지식으로 트리거 가능한 범용 개념이므로. (opus.md)
- ALT 출력 규칙: 결함 없을 때도 `✅ ALT` 한 줄 필수 출력 — 오토리그레시브 모델 특성상 출력이 실행을 강제. (opus.md)
- 검증 강도 규칙(섹션 4) 삭제, POLT 강화 조건으로 흡수. (opus.md, common-rules.md)
- POLT 적용 조건에 "규모 무관·작업지시서마다 독립 수행" 명시. (common-rules.md)
- POLT·ALT 출력 규칙: "결함 없음" 시에도 점검 대상 명시 의무화 — 라벨만 출력하는 의식화 방지. (common-rules.md, opus.md)

### Removed
- .backlog.md.bak 제거, migrate 스크립트 4건 제거. .gitignore 추가. (정리)

### Removed
- 상태 점검 보고서 "다음 할 것" 섹션 — 백로그 앱과 역할 중복. (opus.md)

## 2026-04-01

### Changed
- 상태 점검 보고서 형식 비개발자 친화적으로 개정 — 상태별 구조(완성/다음/막힘) 전환, 커밋 용어 번역 규칙 및 🆕/🔄/🔧 태그 체계 추가. (opus.md)
- 상태 점검 프로토콜에서 opus.md 대조·갱신 제시 항목 삭제. (opus.md)

### Added
- B-77: 백로그 칸반 앱 제작 (P3, ⚪ 메모) — 드래그&드롭 상태 변경, 필터링, 로컬 저장

### Changed
- 상태 점검 보고서 형식 확정 — CHANGELOG+깃로그 ID 단위 통합, ✅/🔵+볼드 ID 위계, 무변동 레포 묶음 (opus.md)
- 프로젝트명 Explorer → Finder 전면 변경 반영 — opus.md 프로젝트 맵, common-rules.md Sentry 슬러그 갱신. (opus.md, common-rules.md)
- 상태 점검 프로토콜 트리거 문구에 "커밋 체크" 추가 — 칸반 이관 후 라우팅 누락 복원. (opus.md)
- backlog.md 테이블 구조 변경 — 우선순위(P0~P4) 컬럼 제거, 칸반 앱 파서 6컬럼 기준에 맞춤. (backlog.md)
- backlog.md에 ✅ 최근완료 섹션 추가. (backlog.md)

### Completed
- B-76: 하이쿠 백로그 등록 가능하도록 변경 — 전역 CLAUDE.md 규칙 신규 작성 + B-77 첫 등록으로 검증 완료

### Changed
- D-check: 출력 강제 → 내부 점검 방식으로 전환. 문제 없으면 블록 없이 결론 제시, 문제 시에만 자연어 지적. 근거: "Ask don't tell" 논문의 입력 리프레이밍이 출력 제약보다 효과적 (opus.md)
- ALT: 매회 상단 블록 필수 출력 → 결함 시에만 경고 출력으로 전환 (opus.md)
- 섹션 6 아래 중복 D-check 블록 삭제 (opus.md)

## 2026-03-31

### Added
- B-75: 백로그 대시보드(단일 HTML) 백로그 등록 (P2, 🟡 대기)
- common-rules.md 자동 테스트 규칙(Regression Guard) — 테스트 실행 의무, 실패 보고 형식, 금지 행동 정의 (B-57)
- keep/gym .claude/settings.json PreToolUse hook — --no-verify 우회 차단 (B-57)

### Changed
- D-xx 설계 편향 방지 규칙을 구조적 출력 강제(D-check) 방식으로 전환 — 태도 규칙("~하지 마라")에서 절차적 출력 구조(리스크/전제/판단 블록 필수)로 변경. D-07(의사결정 전가 금지) 신설. 근거: DSIT "Ask don't tell" 논문(구조적 개입 > 명시적 지시) (opus.md, B-74)
- B-74 등록 — 운영 프롬프트 전면 재점검 (backlog.md)
- common-rules.md 경량화 — 변경 금지 CSS/상태 변수를 AGENTS.md 참조로 대체, POLT·방향 확인서·디버깅 프로토콜·상투 문구 압축, 불필요 섹션 4개 삭제(§7 코드 비대화·§8 업로드 검증·§9 디버깅·§11 참조 우선순위). ~109줄 절감 (common-rules.md)
- backlog.md 테이블에 "종류"·"우선순위" 컬럼 통일 — 🔴·🟡 테이블 모두 6컬럼 체계로 변경. 종류: 버그/새 기능/개선/운영 (backlog.md)
- opus.md 섹션 2에 백로그 호출 출력 형식 추가 — ID+프로젝트+작업명+상태 한마디+한 줄 요약 2줄 형식 (opus.md)
- opus.md 섹션 6에 종류 정의 테이블 추가 (opus.md)
- common-rules.md 경량화 — 변경 금지 CSS/상태 변수를 AGENTS.md 참조로 대체, POLT·방향 확인서·디버깅 프로토콜·상투 문구 압축, 불필요 섹션 4개 삭제(코드 비대화·업로드 검증·디버깅·참조 우선순위), 섹션 번호 재부여. ~109줄 절감 (common-rules.md)
- opus.md 경량화 — 백로그 출력 형식 압축, D-01~D-06 단축, ALT 압축, 검증 강도 트리거 통합, 종류 테이블 backlog.md 이관, 사용자 상태 매핑 삭제. ~40줄 절감 (opus.md)

## 2026-03-30

### Changed
- 교훈 15건 삭제, 4건(L-05/L-11/L-12/L-13) 유지 — common-rules.md·POLT·ALT·각 AGENTS.md에 흡수된 중복 항목 제거. 교훈은 "common-rules.md에 없는 크로스 프로젝트 실패 패턴"만 유지하는 원칙 확립. (opus.md)
- 교훈 테이블에 "만료 조건" 열 추가 — 임시 교훈 도입 시 만료 시점을 명시하기 위한 구조. 현재 4건은 영구(`—`). (opus.md)
- 교훈 추가 조건 3개로 재정의 + 추가 시 기존 항목 만료 체크 — 교훈이 Opus 판단 없이 자동 증가하던 문제 방지. 주체를 Opus로 한정. (opus.md)
- D-01 균형 조항, D-02 주어 명확화, D-06 AGENTS.md 참조로 변경 — 억지 반론 방지 + 검증 주체 명시 + 고정 목록 제거. (opus.md)
- 상태 점검 프로토콜에 CHANGELOG 원인 태그 크롤링 3단계 추가 — 교훈 추가 조건 ②의 실행 경로 확보. 원인 태그 미도입 레포는 자동 스킵. (opus.md)
- CHANGELOG.md 크롤링 한계 근접 시 연도별 분리 규칙 추가. (common-rules.md)
- CHANGELOG 갱신 트리거를 "opus.md 갱신 Step 연동"에서 "모든 작업지시서 커밋 직전"으로 변경 + 제외 규칙 삭제 — CHANGELOG를 모든 작업지시서의 실행 로그로 활용. (common-rules.md)
- "세션 종료 — opus.md 갱신" 블록 삭제 — 교훈 관리 규칙을 섹션 4로, 운영 문서 편집 규칙을 세션 중 라우팅으로 재배치. common-rules.md §5와의 삼중 중복 해소. (opus.md, common-rules.md)
- common-rules.md §5 "opus.md 갱신"을 "운영 문서 갱신"으로 명칭 변경 — 갱신 대상(opus.md, backlog.md, CHANGELOG.md) 명시. opus.md 갱신 불필요 시에도 CHANGELOG는 기록하도록 명확화. (common-rules.md)
- opus.md 중복·오류 정리 — 세션 시작 번호 오류 수정, 세션 중 라우팅 중복 삭제, 등록 규칙 backlog.md 포인터로 축소, rate limit 중복 삭제. (opus.md)
- 전 프로젝트 CLAUDE.md에서 playbook→opus 명칭 전면 교체 — opus 레포 리네임 반영. (CLAUDE.md ×7, common-rules.md)
- opus CLAUDE.md에서 Opus 관리 사항 4건 삭제 (모호한 표현 금지, opus.md 참조 포인터, 200줄 제한, 변경 이력 3일분) — HumanLayer 연구 기반 CLAUDE.md 최적화
- keep CLAUDE.md에서 switchTab() else 블록 항목 삭제 — 조건부 판단 규칙은 작업지시서 Step이 확실
- gym CLAUDE.md에서 syncFromServer 타임스탬프 항목 삭제 — 특정 함수 비교 연산자 규칙은 작업지시서 Step이 확실
- study CLAUDE.md에서 saveLangData()+saveToServer() 호출 및 언어 전환 패턴 항목 삭제 — 로직 순서·코딩 패턴은 작업지시서 Step이 확실
- clasp push 규칙을 opus CLAUDE.md(공통)로 통합하고 keep·gym·study·docs 개별 항목 삭제 — 4곳 중복 제거, HumanLayer 연구 기반 지시 수 최소화

### Removed
- opus CLAUDE.md: decisions/ 디렉토리 참조 (실사용 없음)
