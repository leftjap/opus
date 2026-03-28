# POLT (Pre-Output Logic Trace) 도입 근거

**일자:** 2026-03-28
**결정:** common-rules.md에 섹션 6-1 신설 — Opus의 코드 사전 검증 6단계 프로토콜

---

## 도입 배경

AI 생성 코드는 인간 대비 1.7배 많은 버그를 생성하며 (CodeRabbit 2025.12, 470 GitHub 레포),
특히 로직·정확성 오류가 75% 더 높다 (PR 100건당 194건).
AI 코딩 에이전트의 75%가 장기 유지보수 시 회귀 버그를 발생시킨다 (SWE-CI 2026.03, 18개 모델·100개 레포·233일).
우리 프로젝트는 자동화 테스트 스위트가 없어 실행 후 버그를 잡을 수 없으므로,
Opus가 작업지시서 출력 전에 로직을 사전 검증하는 것이 유일한 품질 게이트이다.

## 오류 유형별 빈도·심각도 → POLT 단계 매핑

| 순위 | 오류 유형 | AI/인간 비율 | 대응 POLT 단계 |
|------|-----------|-------------|---------------|
| 1 | 로직·정확성 | 1.75× | ① Dry Run |
| 2 | 동시성·의존성 | 2× | ③ Wiring Check |
| 3 | 유지보수성 | 1.64× | ② Self-Critique #7 |
| 4 | 보안 | 1.57~2.74× | ② Self-Critique #4 |
| 5 | 에러 핸들링 과잉/부족 | 2× | ② Self-Critique #6 |
| 6 | 성능 (I/O) | 8× | ① Dry Run |
| 7 | 회귀 (fix→break) | 75% 에이전트 | ④ Preservation Check |

## 각 단계별 근거

### ⓪ Scope Lock
- Reddit 사용자 합의: CLAUDE.md에 "Do NOT modify files outside the task scope" 명시 필수
- SWE-CI (2026.03) Programmer 프롬프트: scope expansion 명시적 금지
- Incremental Fix Methodology (Vibe Coding 커뮤니티): One problem = One fix = One commit
- StackOverflow Blog (2026.01): AI 에이전트 Sledgehammer 문제 — 인간보다 2배 많은 guard clause 추가

### ① Dry Run
- SemCoder Monologue Reasoning (NeurIPS 2024): 변수 값 라인별 추적으로 실행 추론 정확도 21.7%p 향상
- IBM Execution Trace CoT (2025): 구체적 값 기반 CoT로 환각 30%p 감소

### ② Self-Critique
- Self-Refine (CMU, NeurIPS 2023): Generate→Feedback→Refine, 프롬프트만으로 품질 향상
- Self-Debugging (Google/UC Berkeley, ICLR 2024): unit test 없을 때 rubber-duck 설명이 가장 효과적
- Lessons 패턴 (커뮤니티 실증 2026): 세션 시작 시 LESSONS.md 로딩으로 반복 오류 ~70% 감소
- CodeScene (2026.02): AI self-harm mode — 자기가 유지보수할 수 없는 코드를 행복하게 작성
- arXiv 서베이 (2512.05239, 72개 연구): Hallucinated Objects, Omitted Edge Cases, Unconfirmed Assumptions

### ③ Wiring Check
- TDAD (2026.03, arXiv): AST 기반 코드-테스트 의존성 그래프로 회귀 70% 감소 (6.08%→1.82%)
- 핵심 발견: TDD 프롬프팅만 적용 시 오히려 회귀 증가 (6.08%→9.94%) — 절차 < 맥락
- Integration Gap (CodeConductor 2025, arXiv Fault Taxonomy 2026): 의존성·통합 실패 = 전체 장애의 19.5%

### ④ Preservation Check
- Kiro Property-Aware Code Evolution (AWS, 2026.02): Bug Condition(C) / Postcondition(P) 분리, ¬C 보존 검증
- SWE-CI (2026.03): Claude Opus 4.6만 0.76 무회귀율 — 나머지 대부분 0.25 이하
- Whack-a-Mole 패턴 (Reddit/LinkedIn/Facebook 2025~2026): 가장 빈번한 좌절 원인

### ⑤ Faithfulness Check
- Masood (2026.01): Scratchpad faithfulness gap — 내부 추론이 최종 출력에 반영 안 될 위험

### 반복 제한 (최대 2회)
- Yang et al. (EMNLP 2025): Verification Loop 수렴 — 라운드 1~2가 개선의 75%
- Tim Williams "LLM Verification Loops" (2026.03): 5~6회 이상은 oscillation 위험

## 우리 환경 특수성
- 자동화 테스트 스위트 없음 → 사전 검증이 유일한 품질 게이트
- Opus 무제한 / Haiku 과금 → Opus 내부 완결이 비용 최적
- Google/Berkeley Self-Debugging: "unit test 없을 때 rubber-duck 설명이 가장 효과적"

## Anthropic 공식 근거
- Think Tool (2025.03): τ-Bench에서 54% 성능 향상
- Extended Thinking (Opus 4.6): 정확도 핵심 작업에 효과 — 공식 권장
- Anthropic 2026 Agentic Coding Trends Report: 검증 비용을 먼저 줄이는 팀이 AI 활용 확장

## 참조 URL
- CodeRabbit: https://www.coderabbit.ai/blog/state-of-ai-vs-human-code-generation-report
- SWE-CI: https://arxiv.org/pdf/2603.03823
- TDAD: https://arxiv.org/html/2603.17973v1
- SemCoder: https://arxiv.org/abs/2406.01006
- Self-Refine: https://arxiv.org/abs/2303.17651
- Self-Debugging: https://arxiv.org/abs/2304.05128
- Kiro: https://kiro.dev/blog/bug-fix-paradox/
- Think Tool: https://www.anthropic.com/engineering/claude-think-tool
- CodeScene: https://codescene.com/blog/agentic-ai-coding-best-practice-patterns-for-speed-with-quality
- Augment: https://www.augmentcode.com/guides/ai-agent-pre-merge-verification
- Verification Loops: https://medium.com/@timjwilliams/llm-verification-loops-best-practices-and-patterns-07541c854fd8
- Bug Survey: https://arxiv.org/html/2512.05239v1
- Fault Taxonomy: https://arxiv.org/html/2603.06847v1
- Lessons Pattern: https://dev.to/diven_rastdus_c5af27d68f3/how-to-give-your-ai-agent-a-memory-that-actually-works-2k79
