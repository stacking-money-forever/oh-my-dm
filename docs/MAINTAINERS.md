# Maintainer 가이드 / Maintainer guide

[한국어](#한국어) · [English](#english)

## 한국어

### Triage

- 재현 여부를 확인하고 실수로 포함된 개인정보는 즉시 제거하세요.
- `status: needs-triage`를 `status: confirmed` 또는 `status: blocked`로 바꾸거나, 명확한 이유를 남기고 issue를 닫으세요.
- `priority: high`는 데이터 노출, 메시지 손상, 광범위한 connector 장애 또는 release blocker에만 사용하세요.
- 사용 질문은 Discussions로, 보안 신고는 private advisory로 안내하세요.
- 실제 대화 내용 대신 개인정보를 제거한 최소 fixture를 요청하세요.

### Pull request

- 병합 전에 CI, Conventional Commit 형식의 제목과 review를 요구하세요.
- PR 제목이 release commit이 되도록 squash merge를 권장합니다.
- 사용자에게 보이는 모든 문구가 한국어와 영어로 함께 제공되는지 확인하세요.
- Connector 자동화, 세션 처리, shell 실행과 filesystem 변경은 보안에 민감한 변경으로 취급하세요.
- 대량 메시지 전송, stealth escalation, rate-limit 우회 또는 영구 메시지 archive 기능을 받지 마세요.

### 저장소 설정

저장소에 commit된 workflow와 template을 기준으로 관리합니다. GitHub branch/ruleset은 CI, PR title, CodeQL, dependency review, conversation resolution, linear history와 최소 1명의 approving review를 요구해야 합니다. Discussions와 private vulnerability reporting을 활성화된 상태로 유지하세요.

### 문서와 현지화

사람이 직접 관리하는 Markdown 문서는 한국어 section을 먼저, 동일한 범위의 English section을 다음에 둡니다. 한쪽 언어에 절차나 제한을 추가했다면 같은 PR에서 다른 언어도 갱신하세요. `CHANGELOG.md`의 release 항목은 Release Please가 관리하므로 직접 version이나 생성 항목을 편집하지 않습니다.

## English

### Triage

- Confirm reproduction and remove accidentally included personal data immediately.
- Replace `status: needs-triage` with `status: confirmed` or `status: blocked`, or close the issue with a clear reason.
- Use `priority: high` only for data exposure, message corruption, widespread connector failure, or release blockers.
- Redirect usage questions to Discussions and security reports to private advisories.
- Ask for a minimal sanitized fixture instead of real conversation content.

### Pull requests

- Require CI, a Conventional Commit title, and review before merge.
- Prefer squash merge so the PR title becomes the release commit.
- Confirm that all user-facing copy exists in both Korean and English.
- Treat connector automation, session handling, shell execution, and filesystem changes as security-sensitive.
- Do not accept bulk messaging, stealth escalation, rate-limit bypasses, or persisted message archives.

### Repository settings

The checked-in workflows and templates are the source of truth. GitHub branch rulesets should require CI, PR titles, CodeQL, dependency review, conversation resolution, linear history, and at least one approving review. Keep Discussions and private vulnerability reporting enabled.

### Documentation and localization

Human-maintained Markdown documents place the Korean section first and an English section with the same scope second. If a procedure or restriction is added in one language, update the other language in the same pull request. Release entries in `CHANGELOG.md` are managed by Release Please; do not manually edit versions or generated entries.
