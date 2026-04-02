# CLAUDE.md — opus

이 레포는 크로스 프로젝트 운영 허브다. 코드가 아니라 운영 문서를 관리한다.

## 작업환경

Opus(Genspark)가 설계·작업지시서 생성 → Haiku(Claude Code)가 실행.
이 파일의 모든 규칙은 Haiku가 읽고 따른다. Opus 수준의 추론을 가정하지 말 것.

## 실행 규칙

- 한 Step에 한 파일, 한 가지 변경
- 함수 수정 시 함수 전체를 교체. 부분 스니펫 비교 금지
- 모든 파일 경로는 절대 경로 (C:\dev\opus\)
- 커밋 메시지: [타입]: [요약] (feat/fix/chore/refactor)
- 작업 완료 후 PowerShell 팝업, alert, 알림 스크립트 사용 금지. 조용히 종료
- gas/Code.js 수정 후 반드시 clasp push + 웹앱 재배포

## 반복 실패 방지

- 동일 증상 fix/style 2회 연속 실패 → 3회차 전 사용자에게 경고: "⚠️ 컨텍스트 오염 확률이 높습니다. 다른 세션에서 시도해 보세요."
- 3회 연속 실패 → 즉시 중단. 시도 내역·실패 원인·다음 방향을 정리하여 보고

## 소스 파일 확인

- 모든 소스 파일 첫 줄에 PROJECT: opus 주석이 있어야 한다
- 업로드된 파일의 PROJECT 주석이 opus와 불일치하면 즉시 중단

## 핵심 파일

- `opus.md` — 마스터 문서. 프로젝트 맵, 백로그, 교훈, 검증 규칙
- `common-rules.md` — 작업지시서 공통 형식
- `backlog-archive.md` — 완료된 백로그 보관

## opus 고유 규칙

- 완료된 백로그는 backlog-archive.md로 이관 후 opus.md에서 삭제
