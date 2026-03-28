# 분석·설계 산출물 검증 체크리스트(ALT) 도입

- date: 2026-03-28
- tags: playbook, opus, analysis, design, verification

## 맥락

POLT(common-rules.md 6-1)는 작업지시서의 코드 블록에만 적용된다. 분석·설계·오류탐지 등 비코드 작업은 playbook.md 수준에서 수행되며, common-rules.md를 읽지 않으므로 POLT가 트리거되지 않는다. D-01~D-06(설계 편향 방지)은 판단 편향을 잡지만, 산출물의 범위 누락·참조 불일치·전제 오류는 커버하지 못한다.

## 조사 근거

### LLM 추론 오류는 코드에 국한되지 않음
- Caltech/Stanford survey (arXiv 2602.06176, 2026-02): Working Memory 한계, Confirmation Bias, Omission Blind Spot이 문서 분석·원인 추정·기획 과정 전반에서 동일하게 발생.
- EMNLP 2025 BlindSpot study (ACL Anthology): 20개 LLM 대상 2,500건 실사용 데이터에서 누락 오류(omission error) 정량화.

### 단계별 자기비평이 전 영역에서 유효
- Stepwise Think-Critique (USTC/Microsoft, arXiv 2512.15662, 2026-03): 추론 단계마다 비평을 끼워넣는 방식이 수학·논리·계획 등 코드 이외 영역에서도 성능 향상.

### 규칙은 읽히는 문서에 있어야 함
- Anthropic Context Engineering (2025-09): "Context must be treated as a finite resource with diminishing marginal returns." 시스템 프롬프트는 "right altitude"를 유지해야 함.
- Kiro Spec-Driven Development: Requirements → Design → Implementation 각 단계별 검증 문서 분리. 설계 검증 규칙은 설계 문서에 배치.
- Reddit r/aiengineering (2026-02): 실행 시점에 읽히지 않는 규칙은 enforcement가 아닌 guidance에 불과.

### 규칙 과잉 경고
- Reddit "What happens when you stop adding rules to CLAUDE.md" (2026-03-20, 538 upvotes): 규칙 과다 시 에이전트 준수율 하락.
- Addy Osmani (O'Reilly, 2026): "over-specified agents lose focus."

### 실제 발견된 결함
playbook.md 트리거 경로 시뮬레이션(2026-03-28)에서 발견된 5건:
- F-1: backlog-archive.md URL 미기재
- F-2: troubleshooting-log.md URL 미기재
- F-3: decisions/INDEX.md URL 미기재
- F-4: §0 vs §2 백로그 크롤링 조건 불일치
- D-1: B-32 선행조건 충족 후 상태 미갱신

이 중 F-1~F-3(참조 정합성), F-4(전제 검증), D-1(범위 완전성)은 D-01~D-06으로 잡히지 않는 유형.

## D-01~D-06과의 관계

| 구분 | D-01~D-06 | ALT (4-2) |
|---|---|---|
| 목적 | 판단 편향 방지 | 산출물 누락·불일치 방지 |
| 대상 | 기술 선택, 아키텍처 결정, 사용자 제안 평가 | 분석 결과, 설계 문서, 오류 탐지 보고 |
| 잡는 것 | 동조, 앵커링, 확증 편향, 양비론, 프레이밍 | 범위 누락, 참조 불일치, 전제 오류, 역추적 누락 |
| 위치 | playbook.md 4-1 | playbook.md 4-2 |

## POLT(6-1)와의 관계

| 구분 | POLT (6-1) | ALT (4-2) |
|---|---|---|
| 목적 | 코드 결함 방지 | 분석·설계 산출물 결함 방지 |
| 트리거 | 작업지시서에 코드 블록 포함 시 | 분석·설계·오류탐지 산출물 출력 시 |
| 위치 | common-rules.md (작업지시서 생성 시 읽음) | playbook.md (세션 시작 시 읽음) |
| 단계 수 | 6단계 (⓪~⑤) | 4항목 (A-1~A-4) |

## 결론

- playbook.md 섹션 4-2에 ALT(Analysis Logic Trace) 4항목 체크리스트 신설
- L-20 교훈 추가
- 4항목으로 경량 유지 (L-09 규칙 비대화 방지 준수)

## 후속

- 2~3주 실사용 후 ALT 항목별 실제 결함 검출 빈도 회고
- 검출 0건 항목은 삭제 검토
