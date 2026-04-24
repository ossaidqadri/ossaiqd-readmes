# Qwen Code — Remote Control Feature

**Repository:** https://github.com/QwenLM/qwen-code (forked from `ossaidqadri/qwen-code`)

## My Contribution

Implemented the **Remote Control feature** — a WebSocket-based server that lets you connect to a live Qwen Code CLI session from a browser or mobile device on the same network.

### Commit

`c52858a5a` — `feat: add remote-control feature for browser-based CLI interaction`

Addressed PR #2330 security review comments, implementing:

- **Token removal from HTML/QR** — auth tokens sent via WebSocket message body, not embedded in HTML source or QR code URLs
- **Rate limit IP bypass fix** — `clientIp` captured at WebSocket connection time, not per-message (prevents IP spoofing via headers)
- **Proxy-aware IP detection** — respects `X-Forwarded-For`, `X-Real-IP` for correct IP behind reverse proxies
- **Host header validation** — validates `Host` header before constructing URLs (prevents host header injection)
- **Secure flag removal** — `secure` flag was misleading (default is `ws://`, not `wss://`); removed and documented
- **`/remote-control stop` fix** — server was unreachable after stop command
- **ESM `require.main` fix** — switched to proper ESM `import.meta.url` for main module detection
- **Unused htmlEscapes entry** — removed erroneous `/` from escape mapping

### Files Added

| File | Lines | Purpose |
|---|---|---|
| `packages/cli/src/remote-control/server/RemoteControlServer.ts` | 1167 | Core WebSocket server with auth, rate limiting, connection management |
| `packages/cli/src/remote-control/server/RemoteControlServer.test.ts` | 416 | Unit tests for server logic |
| `packages/cli/src/remote-control/types.ts` | 282 | TypeScript types for messages, config, protocol |
| `packages/cli/src/commands/remote-control/index.ts` | 238 | CLI subcommand (`qwen remote-control`) |
| `packages/cli/src/ui/commands/remoteControlCommand.ts` | 207 | Interactive slash command (`/remote-control`) |
| `docs/remote-control.md` | 298 | User documentation |

### Architecture

```
Browser/Mobile (ws://) ←→ RemoteControlServer (port 7373) ←→ Qwen Code Core
                              ↓
                    Web UI served on /
                    Token auth per connection
                    Rate limiting (5 attempts/min/IP)
                    Max 5 concurrent connections
                    30-min idle timeout
```

### Security Features

- 64-char hex token per session (sent in WebSocket message, not URL)
- Connection-time IP capture (prevents `X-Forwarded-For` spoofing)
- Host header validation before URL construction
- Max 1MB message size limit
- No tokens in `/api/connect` or `/api/qr-data` endpoints
- HTML escaping on all user input (XSS prevention)
