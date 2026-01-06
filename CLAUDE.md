# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Clawdbot is a personal AI assistant platform with a TypeScript Gateway server that connects to multiple messaging surfaces (WhatsApp, Telegram, Slack, Discord, Signal, iMessage, WebChat). It includes native companion apps for macOS, iOS, and Android that connect as "nodes" to the Gateway.

## Build & Development Commands

```bash
# Install dependencies
pnpm install

# Type-check and build
pnpm build

# Build Control UI (required for web dashboard)
pnpm ui:build

# Run CLI in development
pnpm clawdbot <command>          # tsx entry point
pnpm dev                          # runs src/entry.ts

# Start Gateway
pnpm clawdbot gateway --port 18789 --verbose
pnpm gateway:watch                # auto-reload on changes

# Lint and format
pnpm lint                         # biome check + oxlint
pnpm format                       # biome format
pnpm lint:fix                     # auto-fix

# Tests
pnpm test                         # vitest (watch mode)
pnpm test <pattern>               # run specific tests
pnpm test:coverage                # with V8 coverage (70% threshold)
```

## Committing Changes

Use the commit helper script instead of manual git add/commit:
```bash
scripts/committer "<message>" <file> [<file>...]
```
This keeps staging scoped to specific files—do not use "." as a file argument.

## Architecture Overview

### Gateway (`src/gateway/`)
Single HTTP + WebSocket server (default port 18789) that owns all messaging provider connections. All clients connect via WebSocket with a mandatory `connect` handshake. The server validates every frame against TypeBox schemas via AJV.

Key files:
- `server.ts` - Main gateway server implementation
- `protocol/` - TypeBox schema definitions (source of truth)
- `server-bridge.ts` - Node bridge for device pairing (TCP JSONL, port 18790)
- `server-providers.ts` - Provider lifecycle management

### Agent System (`src/agents/`)
Pi agent runtime that handles AI model interactions with tool streaming.

Key files:
- `pi-embedded*.ts` - Agent runner and streaming
- `tools/` - Tool implementations (browser, canvas, nodes, sessions, discord, slack, cron)
- `skills.ts` - Skills platform for extending agent capabilities
- `system-prompt.ts` - System prompt construction
- `model-selection.ts`, `model-auth.ts` - Model configuration and auth profiles

### CLI (`src/cli/`, `src/commands/`)
Commander-based CLI with commands for gateway, agent, send, onboard, doctor, etc.

### Messaging Providers
Each surface has its own directory with provider-specific code:
- `src/telegram/` - grammY
- `src/discord/` - discord.js
- `src/slack/` - Bolt
- `src/signal/` - signal-cli
- `src/imessage/` - imsg

WhatsApp uses Baileys, wired directly in the gateway.

### Native Apps (`apps/`)
- `apps/macos/` - Swift Package (Swift 6.2, macOS 15+) - menu bar app with voice wake, talk mode, WebChat
- `apps/ios/` - XcodeGen project (iOS 17+) - node with canvas, camera, voice wake
- `apps/android/` - Kotlin/Compose (SDK 31+) - node with canvas, camera, screen capture
- `apps/shared/ClawdbotKit/` - Shared Swift package for iOS/macOS

### Infrastructure (`src/infra/`)
- Bonjour/mDNS discovery
- Node pairing protocol
- Tailscale integration
- Heartbeat runner
- System presence tracking

## Configuration

Config file: `~/.clawdbot/clawdbot.json`
Credentials: `~/.clawdbot/credentials/`
Sessions: `~/.clawdbot/sessions/`
Workspace: `~/clawd/` (configurable via `agent.workspace`)

## Testing Guidelines

- Tests colocated as `*.test.ts` alongside source files
- Use Vitest with V8 coverage
- Run `pnpm test` before pushing when touching logic
- For mobile testing, prefer connected real devices over simulators

## Code Style

- TypeScript ESM with strict typing—avoid `any`
- Biome for formatting/linting
- Keep files under ~700 LOC; split when it improves clarity
- SwiftUI: prefer `Observation` framework (`@Observable`) over `ObservableObject`

## Multi-Agent Safety Rules

When working in a multi-agent environment:
- Do NOT create/apply/drop git stash entries unless explicitly asked
- Do NOT create/remove git worktrees unless explicitly asked
- Do NOT switch branches unless explicitly asked
- When pushing, you may `git pull --rebase` but never discard others' work
- Commit only your changes unless explicitly told to "commit all"

## Protocol Codegen

```bash
pnpm protocol:gen              # Generate JSON Schema from TypeBox
pnpm protocol:gen:swift        # Generate Swift Codable models
pnpm protocol:check            # Verify generated files are up to date
```

## macOS Development

```bash
# Rebuild and relaunch macOS app
./scripts/restart-mac.sh

# Query unified logs
./scripts/clawlog.sh
```

Do not rebuild the macOS app over SSH—rebuilds must run directly on the Mac.

## Key Subsystems Documentation

- Gateway protocol: `docs/architecture.md`
- Configuration reference: `docs/configuration.md`
- Skills platform: `docs/skills.md`
- Node devices: `docs/nodes.md`
- Canvas/A2UI: `docs/mac/canvas.md`
- Voice wake: `docs/voicewake.md`
