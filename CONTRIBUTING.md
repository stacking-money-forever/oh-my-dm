# oh-my-dm 기여 가이드 / Contributing to oh-my-dm

[한국어](#한국어) · [English](#english)

## 한국어

oh-my-dm 개선에 참여해 주셔서 감사합니다. 범위가 작고 목적이 분명하며 테스트가 포함된 변경일수록 빠르게 검토할 수 있습니다.

### 시작하기 전에

- 중복 작업을 피하도록 기존 issue와 pull request를 먼저 검색해 주세요.
- 버그와 기능 제안은 GitHub issue를 이용하세요. 작은 문서 수정은 issue 없이 pull request를 열어도 됩니다.
- 보안 취약점은 공개 issue에 작성하지 말고 [SECURITY.md](SECURITY.md)의 비공개 제보 절차를 따르세요.
- 쿠키, 브라우저 프로필, 실제 대화 내용, 개인정보가 포함된 스크린샷 또는 카카오톡 접근성 덤프를 절대 첨부하지 마세요.

### 로컬 개발

Node.js 22 이상과 npm이 필요합니다. Instagram 테스트에는 Playwright Chromium을 사용하고, 카카오톡 개발과 수동 테스트에는 macOS가 필요합니다.

```bash
git clone git@github.com:stacking-money-forever/oh-my-dm.git
cd oh-my-dm
npm ci
npm test
npm run typecheck
npm run build
```

Connector를 수동 테스트할 때는 격리된 데이터 디렉터리를 사용하세요.

```bash
OH_MY_DM_DATA="$PWD/.oh-my-dm" npm run dev
```

`.oh-my-dm/`은 Git에서 제외됩니다. 강제로 추가하지 마세요.

### 브랜치와 커밋

- 최신 `main`에서 브랜치를 만드세요.
- `feat/language-picker`, `fix/kakao-timeout`, `docs/setup`처럼 짧고 설명적인 이름을 사용하세요.
- 커밋과 PR 제목은 [Conventional Commits](https://www.conventionalcommits.org/) 형식을 따르세요: `feat:`, `fix:`, `perf:`, `docs:`, `test:`, `refactor:`, `build:`, `ci:`, `chore:`.
- 호환되지 않는 변경은 `!` 또는 `BREAKING CHANGE:` footer로 표시하세요.
- 가능하면 동작 변경과 관련 없는 refactor를 별도 커밋이나 PR로 분리하세요.

예시:

```text
feat: add English interface
fix(kakao): preserve message order while paging
docs: explain Accessibility permissions
```

### 코드 규칙

- strict type checking을 사용하는 TypeScript와 `.js` 확장자가 붙은 ESM import를 사용합니다.
- parsing, layout, state transition은 작고 순수한 함수로 분리하는 방식을 선호합니다.
- Connector별 동작은 `src/connectors/`에 둡니다.
- 사용자에게 보이는 문구는 i18n 계층에 한국어와 영어를 함께 추가하고, 한 언어만 inline으로 작성하지 마세요.
- 터미널 너비 처리, 한글 IME 입력, grapheme 단위 cursor 이동을 유지하세요.
- 대화방이나 메시지 내용을 디스크에 저장하지 마세요.
- 공격적인 재시도, 대량 메시지 전송, 탐지 회피 또는 서비스 제한 우회 기능을 추가하지 마세요.
- 외부에서 관찰되는 동작을 변경했다면 `node:test` 테스트를 추가하거나 수정하세요.

### Pull request 확인 목록

PR을 열기 전에 다음 명령을 실행하세요.

```bash
npm test
npm run typecheck
npm run build
npm pack --dry-run
```

PR template을 작성하고 관련 issue를 연결하며 수동 테스트 방법과 결과를 설명하세요. 터미널 스크린샷은 개인정보를 제거한 뒤에만 첨부하세요. 큰 제안은 검토 과정에서 더 작은 PR로 나누도록 요청받을 수 있습니다.

### 검토와 릴리스

PR은 CI와 review를 통과해야 합니다. PR 제목이 release commit이 되도록 squash merge를 사용합니다. Release Please는 병합된 Conventional Commit을 모아 release PR을 만들고, version과 changelog를 갱신합니다. Release PR이 병합되면 GitHub Release가 생성되고 npm Trusted Publishing workflow가 package를 배포합니다.

## English

Thanks for helping improve oh-my-dm. Small, focused changes with tests are the easiest to review.

### Before you start

- Search existing issues and pull requests before opening a duplicate.
- Use a GitHub issue for bugs and feature proposals. For small documentation fixes, a pull request is enough.
- Do not report security vulnerabilities in a public issue; follow [SECURITY.md](SECURITY.md).
- Never include cookies, browser profiles, chat content, screenshots containing personal information, or KakaoTalk accessibility dumps.

### Local development

Node.js 22 or later and npm are required. Instagram tests use Playwright Chromium. KakaoTalk development and manual testing require macOS.

```bash
git clone git@github.com:stacking-money-forever/oh-my-dm.git
cd oh-my-dm
npm ci
npm test
npm run typecheck
npm run build
```

Use an isolated data directory for manual connector testing:

```bash
OH_MY_DM_DATA="$PWD/.oh-my-dm" npm run dev
```

The `.oh-my-dm/` directory is ignored. Do not force-add it.

### Branches and commits

- Branch from the latest `main`.
- Use short, descriptive names such as `feat/language-picker`, `fix/kakao-timeout`, or `docs/setup`.
- Follow [Conventional Commits](https://www.conventionalcommits.org/) for commits and PR titles: `feat:`, `fix:`, `perf:`, `docs:`, `test:`, `refactor:`, `build:`, `ci:`, or `chore:`.
- Add `!` or a `BREAKING CHANGE:` footer for incompatible behavior.
- Keep unrelated refactors separate from behavior changes when practical.

Examples:

```text
feat: add English interface
fix(kakao): preserve message order while paging
docs: explain Accessibility permissions
```

### Code conventions

- Use TypeScript with strict type checking and ESM imports ending in `.js`.
- Prefer small pure functions for parsing, layout, and state transitions.
- Keep connector-specific behavior inside `src/connectors/`.
- Keep user-facing strings in the i18n layer; do not add inline Korean- or English-only UI copy.
- Preserve terminal width handling, Korean IME behavior, and grapheme-safe cursor movement.
- Do not persist message or conversation content.
- Avoid aggressive retries, bulk messaging, anti-detection, or service-bypass behavior.
- Add or update `node:test` coverage for observable behavior changes.

### Pull request checklist

Before opening a pull request, run:

```bash
npm test
npm run typecheck
npm run build
npm pack --dry-run
```

Complete the pull request template, link the issue, and explain manual testing and its result. Include terminal screenshots only after removing private data. A maintainer may ask that a large proposal be split into smaller pull requests.

### Review and release

Pull requests require passing CI and review. Squash-merge so the PR title becomes the release commit. Release Please collects merged Conventional Commits into a release pull request, updates the version and changelog, and creates a GitHub Release when that pull request is merged. The npm Trusted Publishing workflow then publishes the package.
