## 🔴 진행 중
| ID | 등록일 | 프로젝트 | 작업 | 종류 | 메모 | ⭐ |
|---|---|---|---|---|---|---|
| todo-101 | 04/06 23:57 | 지오 'to do' | 커서 결제 | 신규 |  |  |
| study-99 | 04/06 23:47 | study | 긴급 수정 사항(상세내용참조) | 버그 | study 프로젝트 현황 진단
현재 상태 요약
AGENTS.md에 "Phase 2 진행 중"으로 되어 있습니다. 커밋 이력을 보면 Phase 1(웹앱 기본 골격 — 복습 판정, 카드 UI, GAS 동기화, 보호 코드/테스트)이 3월 말에 완료되었고, 오늘(4/6) Day 1 학습 데이터(영어 10문장, 일본어 8문장)가 처음 등록되었습니다. backlog.md에는 study 관련 미완료 항목이 0건입니다.

(참조: backlog.md, study CHANGELOG.md, GitHub Commits API)

발견 사항 — 수정/보완 의견
아래는 소스 코드와 EN-core.md 규칙을 대조하여 발견한 항목입니다. 중요도순으로 정리했습니다.

1. 테스트 데이터(injectTestData)가 프로덕션 코드에 잔존 (참조: app.js injectTestData 함수)

app.js에 150줄 규모의 injectTestData() 함수가 그대로 있습니다. 현재 어디서도 호출되지 않는 것으로 보이지만, 실수로 호출되면 _testInjected 플래그와 더미 reviewQueue가 실제 학습 데이터를 덮어쓸 위험이 있습니다. L-05(더미 데이터 복귀)에 해당합니다. 제거하거나 별도 test 파일로 분리하는 것을 권장합니다.

2. 신규 학습 카드 → reviewQueue 등록 로직 부재 (참조: lesson.js nextLessonCard, review.js judgeReview, EN-core.md §8-A)

nextLessonCard()에서 _sessionNewCount++만 하고, 해당 신규 문장을 reviewQueue에 추가하는 코드가 없습니다. finishStudy()에서도 sessionLog 기록과 saveToServer만 수행하고 reviewQueue 등록은 하지 않습니다. 현재 구조에서는 Opus 세션에서 작업지시서를 통해 GAS JSON에 직접 등록하는 흐름이지만, 웹앱 내에서 학습을 완료했을 때 자동으로 reviewQueue에 들어가야 Day+1에 복습 대상으로 잡힙니다. 이것은 Phase 2의 핵심 누락입니다.

3. finishStudy() — sessionLog 미기록 (참조: app.js finishStudy, EN-core.md §8 로그 스키마)

finishStudy()가 renderStudySummary + showScreen('summary')만 호출하고, data.sessionLogs에 현재 세션 결과(newSentenceIds, reviewResults, durationMin 등)를 push하는 코드가 없습니다. stats 화면의 날짜별 학습 카드가 빈 상태로 남게 됩니다.

4. finishStudy() — meta/stats 미갱신 (참조: app.js finishStudy, EN-core.md §9)

totalDays, totalMinutes, currentStreak, longestStreak, totalSentences, reviewPending 등이 세션 종료 시 갱신되지 않습니다. 홈 화면의 요약 메시지("총 N개 표현을 배웠어요", "N일 연속")가 영원히 초기값을 표시합니다.

5. _flushBeforeUnload 미정의 (참조: app.js 하단 beforeunload/pagehide 이벤트)

window._flushBeforeUnload가 호출되지만 어디서도 정의되지 않았습니다. 현재는 에러를 삼키고 있겠지만, 이탈 시 비상 저장이 작동하지 않는 상태입니다. gym 앱에서 같은 문제(gym-13/14)를 겪고 해결한 이력이 있으므로 동일 패턴을 이식해야 합니다.

6. dailyPracticeStats 갱신 로직 부재 (참조: stats.js renderStatsMonthCal → getUtterancesForDate, app.js)

월간 캘린더와 발화량 차트에서 dailyPracticeStats[날짜].utterances를 읽지만, 실제로 이 값을 증가시키는 코드가 웹앱에 없습니다. practice.js(Phase 3, Azure Speech)가 아직 미구현이므로 현재는 빈 차트만 나옵니다. Phase 2에서 최소한 "세션 완료 시 학습한 문장 수"를 utterances 대체값으로 기록하는 것을 권장합니다.

7. getUtterancesForDate 미정의 (참조: stats.js renderStatsMonthCal에서 호출)

stats.js에서 getUtterancesForDate(data, dateStr)를 호출하지만, storage.js나 stats.js 어디에도 이 함수가 정의되어 있지 않습니다. 기록 화면 진입 시 에러가 발생합니다.

8. openPronModal 미정의 (참조: review.js revealAnswer, lesson.js renderLessonCard에서 호출)

"따라 말하기" 버튼 클릭 시 openPronModal(sentence)를 호출하지만, practice.js가 미구현이므로 에러가 발생합니다. Phase 3 전까지는 빈 함수 스텁이라도 필요합니다.

9. 복습 카드 스와이프 바인딩 누락 (참조: review.js renderReviewCard vs lesson.js bindLessonSwipe)

lesson 카드에는 bindLessonSwipe()로 터치 스와이프가 바인딩되지만, review 카드에는 스와이프 바인딩이 없습니다. swipe.js가 별도로 있지만 review 카드에 연결하는 코드를 확인할 수 없습니다.

10. progress.js / calendar.js — 실제 연동 미확인 (참조: AGENTS.md 파일 구조)

progress.js(발화량 프로그레스바)와 calendar.js(주간 캘린더)가 파일 구조에 있지만, 이번에 크롤링하지 않았습니다. renderWeekCal() 호출은 ui.js renderHome()에 있으나, _selectedWeekDate 변수가 calendar.js에서 관리되는지 확인이 필요합니다.

권장 백로그 등록 (우선순위순)
우선순위	제안 ID	작업	종류
🔴 즉시	study-A	finishStudy에서 sessionLog 기록 + meta/stats 갱신 + 신규→reviewQueue 등록	버그
🔴 즉시	study-B	_flushBeforeUnload 구현 (gym-13 패턴 이식)	버그
🔴 즉시	study-C	getUtterancesForDate, openPronModal 스텁 함수 추가 (에러 방지)	버그
🟡 단기	study-D	injectTestData 제거 또는 테스트 파일로 분리	정리
🟡 단기	study-E	dailyPracticeStats 최소 기록 (세션 완료 시 문장 수 기반)	개선
🟡 단기	study-F	복습 카드 터치 스와이프 바인딩	개선
⚪ 중기	study-G	EN-core.md §8-A 용량 관리 (reviewQueue 80/100 임계값) 클라이언트 구현	새 기능
study-A가 가장 치명적입니다. 현재 웹앱에서 학습을 완료해도 아무것도 기록되지 않는 상태입니다. | true |
| study-100 | 04/06 23:55 | study | 복습 판정 도형 변경 -> 텍스트로 | 개선 |  |  |
| keep-15 | 03/28 19:16 | keep | AI 댓글 체계 구축 (품질 개선 → 인용문 참조 → 지오 확장 → API 자동화) | 개선 | Phase 1 착수 가능 \ |  |
| book-18 | 03/28 19:16 | 서재_어구록 | 서재 발췌문 통합 | 새 기능 | book-01 완료, 착수 가능 \ |  |
| all-98 | 04/05 06:05 | — | mycodex 프로젝트 진행 | 신규 |  | true |
| opus-97 | 04/04 20:28 | opus | 검색/파일 업로드 트리거 작동 안함. 웹검색이라고 해야 하나? | 메모 |  |  |

## 🟡 대기
| ID | 등록일 | 프로젝트 | 작업 | 종류 | 메모 | ⭐ |
|---|---|---|---|---|---|---|
| keep-25 | 03/28 19:16 | keep | 파트너 모드 진입 시 로딩 시간 단축 | 개선 | \ |  |

## ⚪ 메모
| ID | 등록일 | 프로젝트 | 작업 | 종류 | 메모 | ⭐ |
|---|---|---|---|---|---|---|
| idea-05 | 03/28 19:16 | — | 개인 취향 기반 콘텐츠 추천 앱 | 아이디어 | MVP: NotebookLM/Claude로 취향 프로필 테스트 먼저 \ |  |

## ✅ 최근완료
| ID | 등록일 | 프로젝트 | 작업 | 종류 | 메모 | ⭐ |
|---|---|---|---|---|---|---|
| all-92 | 04/02 19:45 | 전체 (복합) | (Playwright toHaveCSS) | 운영 | 신규 \ (완료: 04/05 01:10) | true |
| all-91 | 04/02 19:44 | 전체 (복합) | CSS 정적 검사 | 신규 | \ (완료: 04/04 11:34) | true |
| study-80 | 04/02 01:21 | study | 테스트 | 메모 | (완료: 04/02 16:25) \ |  |
| finder-84 | 04/02 01:26 | Finder | 칼럼 간격은 재실행 후 기존 값이 저장되어야 함 | 버그 | (완료: 04/02 07:35) \ |  |
| backlog-83 | 04/02 01:26 | BackLog | 최근완료 항목이 changelog에 저장되는지 | 버그 | (완료: 04/02 07:34) \ |  |
