# 아키텍처 / Architecture

[한국어](#한국어) · [English](#english)

## 한국어

oh-my-dm은 로컬에서만 실행되는 TypeScript/Ink TUI입니다. 애플리케이션 backend와 메시지 database가 없습니다.

```text
Instagram web ── DOM/WebSocket wake-up ─┐
                                        ├─ UnifiedChatConnector ─ App (Ink TUI)
KakaoTalk macOS ─ Accessibility/Swift ──┘
```

### 구성 경계

- `src/connectors/instagram-web.ts`: 브라우저 수명주기와 DOM 읽기를 담당합니다.
- `src/connectors/kakao-native.ts`: 카카오톡 상태를 관리하고 persistent Swift bridge와 통신합니다.
- `src/connectors/unified.ts`: 대화 ID에 connector namespace를 붙이고 하나의 connector interface로 제공합니다.
- `src/ui/`: 터미널 rendering, IME 입력, theme, model 표기, command, layout, localization을 담당합니다.
- `src/storage/settings-store.ts`: 메시지가 아닌 UI preference만 저장합니다.
- `scripts/kakao-bridge.swift`: macOS에서 카카오톡 accessibility tree와 상호작용합니다.

### 데이터 규칙

Connector가 source of truth입니다. 대화방과 메시지 내용은 메모리에만 유지되며 oh-my-dm이 영구 저장하지 않습니다. 브라우저 프로필에는 Instagram 로그인 세션이 있으므로 항상 Git에서 제외해야 합니다. 설정 파일은 현재 사용자만 접근할 수 있는 권한으로 기록합니다.

### 렌더링 규칙

Ink `Static`은 실시간 transcript를 터미널 scrollback에 남깁니다. History와 화면을 넘는 command palette는 alternate-screen buffer를 사용해 composer와 footer가 중복되거나 깨지지 않도록 합니다. Text layout은 terminal cell width, multiline content, grapheme cluster와 한글 IME composition을 고려해야 합니다.

### Connector 제약

Instagram selector와 카카오톡 accessibility label은 안정적인 public API가 아닙니다. Parser는 가능한 경우 한국어와 영어 label을 모두 처리하고, 실패 시 다른 대화방을 누르거나 잘못된 발신자를 만드는 대신 안전하게 중단해야 합니다. 무제한 재시도는 피하며 connector 변경에는 개인정보를 제거한 fixture와 범위가 분명한 unit test가 필요합니다.

## English

oh-my-dm is a local-only TypeScript/Ink TUI. There is no application backend and no message database.

```text
Instagram web ── DOM/WebSocket wake-up ─┐
                                        ├─ UnifiedChatConnector ─ App (Ink TUI)
KakaoTalk macOS ─ Accessibility/Swift ──┘
```

### Boundaries

- `src/connectors/instagram-web.ts` owns browser lifecycle and DOM reads.
- `src/connectors/kakao-native.ts` owns KakaoTalk state and talks to the persistent Swift bridge.
- `src/connectors/unified.ts` namespaces conversation IDs and presents one connector interface.
- `src/ui/` owns terminal rendering, IME input, themes, model labels, commands, layout, and localization.
- `src/storage/settings-store.ts` persists non-message UI preferences only.
- `scripts/kakao-bridge.swift` interacts with the KakaoTalk accessibility tree on macOS.

### Data rules

Connectors are the source of truth. Conversation and message content remains in memory and is not persisted by oh-my-dm. The browser profile contains the Instagram login session and must always remain ignored by Git. Settings are written with user-only file permissions.

### Rendering rules

Ink `Static` provides terminal scrollback for the live transcript. History and overflowing command palettes use alternate-screen buffers so they do not duplicate or corrupt the composer and footer. Text layout must account for terminal cell width, multiline content, grapheme clusters, and Korean IME composition.

### Connector constraints

Instagram selectors and KakaoTalk accessibility labels are not stable public APIs. Parsers should accept Korean and English labels where available and fail safely instead of clicking a different conversation or inventing a sender. Avoid unbounded retries. Connector changes require sanitized fixtures and focused unit tests.
