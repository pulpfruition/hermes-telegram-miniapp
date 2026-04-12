# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.2] — 2026-04-12

### Added
- **File attachments** — attach images, PDFs, and text files to chat messages via the 📎 button
  - Sends files as OpenAI multimodal content arrays (`image_url` + `text`)
  - Backend saves attachments to temp files and injects system context so the agent uses `vision_analyze`, `read_file`, or local OCR
  - Supports PNG, JPEG, GIF, WebP, PDF, CSV, TXT, JSON
  - Attachment preview badge shows filename with remove button
  - 50 MB file size limit
- **Cron job creation** — floating `+` FAB button opens bottom sheet modal to create new jobs
  - Fields: Name, Schedule, Prompt, Repeat count
  - Quick-select schedule chips: 30m, 1h, 2h, 9am daily, weekly
- **Cron job editing** — pencil button on each card opens pre-filled edit modal (PATCH)
- **Cron job deletion** — trash button with inline confirmation prompt (DELETE)
- **Local vision server integration** — LFM2-VL-450M on port 8080 for image analysis
- **Local OCR server integration** — GLM-OCR on port 8081 for document/text extraction

### Fixed
- Gateway `_handle_chat_completions` now parses OpenAI multimodal content arrays (was only handling plain strings)
- `cron/jobs.py` `update_job` crashed when schedule was a plain string — now handles both string and dict schedules
- Attachment temp files are cleaned up after agent response completes

## [1.0.1] — 2026-04-12

### Fixed
- Fixed 404 on miniapp routes — merged gateway feature branch (`feat/telegram-miniapp-ed25519-auth`) into main, enabling static file serving at `/miniapp/*`
- Gateway now properly serves `~/.hermes/miniapp/index.html` and redirects `/miniapp` and `/miniapp/` to `/miniapp/index.html`

### Added
- Gateway merge includes Ed25519 auth, `/api/model-info`, `/api/session-usage`, and all miniapp backend endpoints

## [1.0.0] — 2026-04-12

### Added
- Terminal-style chat interface with streaming responses
- TUI-style header bar showing model name, context usage bar, and session duration
- Ed25519 signature validation for Telegram initData (no bot token needed for auth)
- Bearer token fallback for non-Telegram access (browser testing, API clients)
- Owner-only access control via Telegram user ID
- Haptic feedback on first response chunk
- Streaming send button with abort/cancel support
- Animated typing indicator held until first content arrives
- Status tab with system health, quick actions, and account/debug info
- Cron tab for viewing and managing scheduled jobs
- Drawer menu with slash command palette
- CSS containment + visualViewport handler for smooth keyboard transitions
- `/api/model-info` endpoint returning live model metadata from Hermes
- `/api/session-usage` endpoint for cumulative token tracking
- Cloudflare tunnel setup guide
- Systemd service template
