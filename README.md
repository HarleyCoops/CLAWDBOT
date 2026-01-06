# 🦞 CLAWDBOT — Personal AI Assistant

<p align="center">
  <strong>EXFOLIATE! EXFOLIATE!</strong>
</p>

---

## ⚠️ CRITICAL: WSL + Bun Setup Issues & Solutions

**If you're running on WSL (Windows Subsystem for Linux), READ THIS FIRST to avoid hours of debugging:**

### 🚨 Known Issues & Solutions

#### 1. **pnpm Permission Errors on Windows Mounts (`/mnt/c/`)**
```
ERR_PNPM_EACCES  EACCES: permission denied, rename
```
**Solution:** Move project to WSL native filesystem (`~/clawdbot`) instead of `/mnt/c/`. Much faster performance too.
```bash
cp -r /mnt/c/Users/YOUR_USER/clawdbot ~/clawdbot
cd ~/clawdbot
```

#### 2. **Node Version Mismatch**
```
WARN Unsupported engine: wanted: {"node":">=22.12.0"} (current: {"node":"v20.x.x"})
```
**Solution:** Install Node.js v22+ using nvm:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
nvm install 22 && nvm use 22 && nvm alias default 22
```

#### 3. **Skills Blocked: "Error: brew not installed"**
Most skills require Homebrew. Install it in WSL:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.bashrc
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```
**⚠️ IMPORTANT:** After installing Homebrew, you MUST restart the gateway for it to detect the new PATH.

#### 4. **Homebrew gcc Download Failures**
If Homebrew fails downloading gcc due to network issues:
```bash
# Fix DNS resolution
sudo rm /etc/resolv.conf
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
# Retry Homebrew install
```

#### 5. **Using Bun Instead of pnpm**
This repo was built for pnpm, but Bun works great and is faster. Replace all `pnpm` commands with `bun`:
```bash
# Install Bun
curl -fsSL https://bun.sh/install | bash
export BUN_INSTALL="$HOME/.bun" && export PATH="$BUN_INSTALL/bin:$PATH"

# Use Bun for everything
bun install          # instead of pnpm install
bun run build        # instead of pnpm build
bun run clawdbot ... # instead of pnpm clawdbot ...
```

#### 6. **Build Errors: "Cannot find module './cjs/index.cjs'"**
You forgot to build the project first:
```bash
bun run build        # Compile TypeScript
bun run ui:build     # Build Control UI
```

#### 7. **"tsx: command not found"**
Dependencies not installed:
```bash
bun install  # This installs tsx and all other deps
```

### 🎯 Recommended WSL Setup (Start Here)

**Complete setup from scratch:**
```bash
# 1. Install Node.js v22+ with nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
nvm install 22 && nvm use 22 && nvm alias default 22

# 2. Install Bun
curl -fsSL https://bun.sh/install | bash
export BUN_INSTALL="$HOME/.bun" && export PATH="$BUN_INSTALL/bin:$PATH"
echo 'export BUN_INSTALL="$HOME/.bun"' >> ~/.bashrc
echo 'export PATH="$BUN_INSTALL/bin:$PATH"' >> ~/.bashrc

# 3. Install Homebrew (required for skills)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.bashrc
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"

# 4. Copy project to WSL filesystem (NOT /mnt/c/)
cp -r /mnt/c/Users/YOUR_USER/clawdbot ~/clawdbot
cd ~/clawdbot

# 5. Install deps and build
bun install
bun run build
bun run ui:build

# 6. Run onboarding
bun run clawdbot onboard

# 7. Start gateway
bun run clawdbot gateway --port 18789 --verbose
```

### 📋 Outstanding Issues (TODO)

- [ ] **Fix skills PATH detection** - Gateway doesn't pick up Homebrew PATH on first run (workaround: restart gateway after installing Homebrew)
- [ ] **Document Bun-specific quirks** - Some scripts may still reference pnpm, need to test all commands with Bun
- [ ] **WSL-specific docs** - Add dedicated WSL troubleshooting guide
- [ ] **Improve error messages** - Better error messages for missing dependencies (Homebrew, Node version, etc.)
- [ ] **Auto-detect environment** - Detect WSL vs native Linux and warn about `/mnt/c/` issues
- [ ] **Skills installation** - Simplify Homebrew requirement or provide apt alternatives for common tools

### 🐛 Don't Run as Root
If you see `root@PC`, you're running as root. Create a proper user:
```bash
# As root, create a user
adduser yourname
usermod -aG sudo yourname
# Exit and log back in as yourname
```

---

<p align="center">
  <a href="https://github.com/clawdbot/clawdbot/actions/workflows/ci.yml?branch=main"><img src="https://img.shields.io/github/actions/workflow/status/clawdbot/clawdbot/ci.yml?branch=main&style=for-the-badge" alt="CI status"></a>
  <a href="https://github.com/clawdbot/clawdbot/releases"><img src="https://img.shields.io/github/v/release/clawdbot/clawdbot?include_prereleases&style=for-the-badge" alt="GitHub release"></a>
  <a href="https://discord.gg/clawd"><img src="https://img.shields.io/discord/1456350064065904867?label=Discord&logo=discord&logoColor=white&color=5865F2&style=for-the-badge" alt="Discord"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge" alt="MIT License"></a>
</p>

**Clawdbot** is a *personal AI assistant* you run on your own devices.
It answers you on the surfaces you already use (WhatsApp, Telegram, Slack, Discord, Signal, iMessage, WebChat), can speak and listen on macOS/iOS/Android, and can render a live Canvas you control. The Gateway is just the control plane — the product is the assistant.

If you want a personal, single-user assistant that feels local, fast, and always-on, this is it.

Website: [https://clawdbot.com](https://clawdbot.com) · Docs: [https://docs.clawdbot.com](https://docs.clawdbot.com/) · Showcase: [https://docs.clawdbot.com/showcase](https://docs.clawdbot.com/showcase) · FAQ: [https://docs.clawdbot.com/faq](https://docs.clawdbot.com/faq) · Wizard: [https://docs.clawdbot.com/wizard](https://docs.clawdbot.com/wizard) · Nix: [https://github.com/clawdbot/nix-clawdbot](https://github.com/clawdbot/nix-clawdbot) · Docker: [https://docs.clawdbot.com/docker](https://docs.clawdbot.com/docker) · Discord: [https://discord.gg/clawd](https://discord.gg/clawd)

Preferred setup: run the onboarding wizard (`clawdbot onboard`). It walks through gateway, workspace, providers, and skills. The CLI wizard is the recommended path and works on **macOS, Windows, and Linux**.
Works with npm, pnpm, or bun.

**Subscriptions (OAuth):**
- **Anthropic** (Claude Pro/Max)
- **OpenAI** (ChatGPT/Codex)

Model note: while any model is supported, I strongly recommend **Anthropic Pro/Max (100/200) + Opus 4.5** for long‑context strength and better prompt‑injection resistance. See [Onboarding](https://docs.clawdbot.com/onboarding).

## Recommended setup (from source)

Do **not** download prebuilt binaries. Build from source.

```bash
# Clone this repo
git clone https://github.com/clawdbot/clawdbot.git
cd clawdbot

pnpm install
pnpm build
pnpm ui:build
pnpm clawdbot onboard
```

## Quick start (from source)

Runtime: **Node ≥22** + **pnpm**.

```bash
pnpm install
pnpm build
pnpm ui:build

# Recommended: run the onboarding wizard
pnpm clawdbot onboard

# Link WhatsApp (stores creds in ~/.clawdbot/credentials)
pnpm clawdbot login

# Start the gateway
pnpm clawdbot gateway --port 18789 --verbose

# Dev loop (auto-reload on TS changes)
pnpm gateway:watch

# Send a message
pnpm clawdbot send --to +1234567890 --message "Hello from Clawdbot"

# Talk to the assistant (optionally deliver back to WhatsApp/Telegram/Slack/Discord)
pnpm clawdbot agent --message "Ship checklist" --thinking high
```

Upgrading? `clawdbot doctor`.

If you run from source, prefer `pnpm clawdbot …` (not global `clawdbot`).

## Highlights

- **[Local-first Gateway](https://docs.clawdbot.com/gateway)** — single control plane for sessions, providers, tools, and events.
- **[Multi-surface inbox](https://docs.clawdbot.com/surface)** — WhatsApp, Telegram, Slack, Discord, Signal, iMessage, WebChat, macOS, iOS/Android.
- **[Voice Wake](https://docs.clawdbot.com/voicewake) + [Talk Mode](https://docs.clawdbot.com/talk)** — always-on speech for macOS/iOS/Android with ElevenLabs.
- **[Live Canvas](https://docs.clawdbot.com/mac/canvas)** — agent-driven visual workspace with [A2UI](https://docs.clawdbot.com/refactor/canvas-a2ui).
- **[First-class tools](https://docs.clawdbot.com/tools)** — browser, canvas, nodes, cron, sessions, and Discord/Slack actions.
- **[Companion apps](https://docs.clawdbot.com/macos)** — macOS menu bar app + iOS/Android [nodes](https://docs.clawdbot.com/nodes).
- **[Onboarding](https://docs.clawdbot.com/wizard) + [skills](https://docs.clawdbot.com/skills)** — wizard-driven setup with bundled/managed/workspace skills.

## Everything we built so far

### Core platform
- [Gateway WS control plane](https://docs.clawdbot.com/gateway) with sessions, presence, config, cron, webhooks, [Control UI](https://docs.clawdbot.com/web), and [Canvas host](https://docs.clawdbot.com/refactor/canvas-a2ui).
- [CLI surface](https://docs.clawdbot.com/agent-send): gateway, agent, send, [wizard](https://docs.clawdbot.com/wizard), and [doctor](https://docs.clawdbot.com/doctor).
- [Pi agent runtime](https://docs.clawdbot.com/agent) in RPC mode with tool streaming and block streaming.
- [Session model](https://docs.clawdbot.com/session): `main` for direct chats, group isolation, activation modes, queue modes, reply-back. Group rules: [Groups](https://docs.clawdbot.com/groups).
- [Media pipeline](https://docs.clawdbot.com/images): images/audio/video, transcription hooks, size caps, temp file lifecycle. Audio details: [Audio](https://docs.clawdbot.com/audio).

### Surfaces + providers
- [Providers](https://docs.clawdbot.com/surface): [WhatsApp](https://docs.clawdbot.com/whatsapp) (Baileys), [Telegram](https://docs.clawdbot.com/telegram) (grammY), [Slack](https://docs.clawdbot.com/slack) (Bolt), [Discord](https://docs.clawdbot.com/discord) (discord.js), [Signal](https://docs.clawdbot.com/signal) (signal-cli), [iMessage](https://docs.clawdbot.com/imessage) (imsg), [WebChat](https://docs.clawdbot.com/webchat).
- [Group routing](https://docs.clawdbot.com/group-messages): mention gating, reply tags, per-surface chunking and routing. Surface rules: [Surface routing](https://docs.clawdbot.com/surface).

### Apps + nodes
- [macOS app](https://docs.clawdbot.com/macos): menu bar control plane, [Voice Wake](https://docs.clawdbot.com/voicewake)/PTT, [Talk Mode](https://docs.clawdbot.com/talk) overlay, [WebChat](https://docs.clawdbot.com/webchat), debug tools, [remote gateway](https://docs.clawdbot.com/remote) control.
- [iOS node](https://docs.clawdbot.com/ios): [Canvas](https://docs.clawdbot.com/mac/canvas), [Voice Wake](https://docs.clawdbot.com/voicewake), [Talk Mode](https://docs.clawdbot.com/talk), camera, screen recording, Bonjour pairing.
- [Android node](https://docs.clawdbot.com/android): [Canvas](https://docs.clawdbot.com/mac/canvas), [Talk Mode](https://docs.clawdbot.com/talk), camera, screen recording, optional SMS.
- [macOS node mode](https://docs.clawdbot.com/nodes): system.run/notify + canvas/camera exposure.

### Tools + automation
- [Browser control](https://docs.clawdbot.com/browser): dedicated clawd Chrome/Chromium, snapshots, actions, uploads, profiles.
- [Canvas](https://docs.clawdbot.com/mac/canvas): [A2UI](https://docs.clawdbot.com/refactor/canvas-a2ui) push/reset, eval, snapshot.
- [Nodes](https://docs.clawdbot.com/nodes): camera snap/clip, screen record, [location.get](https://docs.clawdbot.com/location-command), notifications.
- [Cron + wakeups](https://docs.clawdbot.com/cron); [webhooks](https://docs.clawdbot.com/webhook); [Gmail Pub/Sub](https://docs.clawdbot.com/gmail-pubsub).
- [Skills platform](https://docs.clawdbot.com/skills): bundled, managed, and workspace skills with install gating + UI.

### Ops + packaging
- [Control UI](https://docs.clawdbot.com/web) + [WebChat](https://docs.clawdbot.com/webchat) served directly from the Gateway.
- [Tailscale Serve/Funnel](https://docs.clawdbot.com/tailscale) or [SSH tunnels](https://docs.clawdbot.com/remote) with token/password auth.
- [Nix mode](https://docs.clawdbot.com/nix) for declarative config; [Docker](https://docs.clawdbot.com/docker)-based installs.
- [Doctor](https://docs.clawdbot.com/doctor) migrations, [logging](https://docs.clawdbot.com/logging).

## How it works (short)

```
WhatsApp / Telegram / Slack / Discord / Signal / iMessage / WebChat
               │
               ▼
┌───────────────────────────────┐
│            Gateway            │  ws://127.0.0.1:18789
│       (control plane)         │  bridge: tcp://0.0.0.0:18790
└──────────────┬────────────────┘
               │
               ├─ Pi agent (RPC)
               ├─ CLI (clawdbot …)
               ├─ WebChat UI
               ├─ macOS app
               └─ iOS/Android nodes
```

## Key subsystems

- **[Gateway WebSocket network](https://docs.clawdbot.com/architecture)** — single WS control plane for clients, tools, and events (plus ops: [Gateway runbook](https://docs.clawdbot.com/gateway)).
- **[Tailscale exposure](https://docs.clawdbot.com/tailscale)** — Serve/Funnel for the Gateway dashboard + WS (remote access: [Remote](https://docs.clawdbot.com/remote)).
- **[Browser control](https://docs.clawdbot.com/browser)** — clawd‑managed Chrome/Chromium with CDP control.
- **[Canvas + A2UI](https://docs.clawdbot.com/mac/canvas)** — agent‑driven visual workspace (A2UI host: [Canvas/A2UI](https://docs.clawdbot.com/refactor/canvas-a2ui)).
- **[Voice Wake](https://docs.clawdbot.com/voicewake) + [Talk Mode](https://docs.clawdbot.com/talk)** — always‑on speech and continuous conversation.
- **[Nodes](https://docs.clawdbot.com/nodes)** — Canvas, camera snap/clip, screen record, `location.get`, notifications, plus macOS‑only `system.run`/`system.notify`.

## Tailscale access (Gateway dashboard)

Clawdbot can auto-configure Tailscale **Serve** (tailnet-only) or **Funnel** (public) while the Gateway stays bound to loopback. Configure `gateway.tailscale.mode`:

- `off`: no Tailscale automation (default).
- `serve`: tailnet-only HTTPS via `tailscale serve` (uses Tailscale identity headers by default).
- `funnel`: public HTTPS via `tailscale funnel` (requires shared password auth).

Notes:
- `gateway.bind` must stay `loopback` when Serve/Funnel is enabled (Clawdbot enforces this).
- Serve can be forced to require a password by setting `gateway.auth.mode: "password"` or `gateway.auth.allowTailscale: false`.
- Funnel refuses to start unless `gateway.auth.mode: "password"` is set.
- Optional: `gateway.tailscale.resetOnExit` to undo Serve/Funnel on shutdown.

Details: [Tailscale guide](https://docs.clawdbot.com/tailscale) · [Web surfaces](https://docs.clawdbot.com/web)

## Remote Gateway (Linux is great)

It’s perfectly fine to run the Gateway on a small Linux instance. Clients (macOS app, CLI, WebChat) can connect over **Tailscale Serve/Funnel** or **SSH tunnels**, and you can still pair device nodes (macOS/iOS/Android) to execute device‑local actions when needed.

- **Gateway host** runs the bash tool and provider connections by default.
- **Device nodes** run device‑local actions (`system.run`, camera, screen recording, notifications) via `node.invoke`.
In short: bash runs where the Gateway lives; device actions run where the device lives.

Details: [Remote access](https://docs.clawdbot.com/remote) · [Nodes](https://docs.clawdbot.com/nodes) · [Security](https://docs.clawdbot.com/security)

## macOS permissions via the Gateway protocol

The macOS app can run in **node mode** and advertises its capabilities + permission map over the Gateway WebSocket (`node.list` / `node.describe`). Clients can then execute local actions via `node.invoke`:

- `system.run` runs a local command and returns stdout/stderr/exit code; set `needsScreenRecording: true` to require screen-recording permission (otherwise you’ll get `PERMISSION_MISSING`).
- `system.notify` posts a user notification and fails if notifications are denied.
- `canvas.*`, `camera.*`, `screen.record`, and `location.get` are also routed via `node.invoke` and follow TCC permission status.

Elevated bash (host permissions) is separate from macOS TCC:

- Use `/elevated on|off` to toggle per‑session elevated access when enabled + allowlisted.
- Gateway persists the per‑session toggle via `sessions.patch` (WS method) alongside `thinkingLevel`, `verboseLevel`, `model`, `sendPolicy`, and `groupActivation`.

Details: [Nodes](https://docs.clawdbot.com/nodes) · [macOS app](https://docs.clawdbot.com/macos) · [Gateway protocol](https://docs.clawdbot.com/architecture)

## Agent to Agent (sessions_* tools)

- Use these to coordinate work across sessions without jumping between chat surfaces.
- `sessions_list` — discover active sessions (agents) and their metadata.
- `sessions_history` — fetch transcript logs for a session.
- `sessions_send` — message another session; optional reply‑back ping‑pong + announce step (`REPLY_SKIP`, `ANNOUNCE_SKIP`).

Details: [Session tools](https://docs.clawdbot.com/session-tool)

## Skills registry (ClawdHub)

ClawdHub is a minimal skill registry. With ClawdHub enabled, the agent can search for skills automatically and pull in new ones as needed.

https://ClawdHub.com

## Chat commands

Send these in WhatsApp/Telegram/Slack/WebChat (group commands are owner-only):

- `/status` — health + session info (group shows activation mode)
- `/new` or `/reset` — reset the session
- `/think <level>` — off|minimal|low|medium|high
- `/verbose on|off`
- `/restart` — restart the gateway (owner-only in groups)
- `/activation mention|always` — group activation toggle (groups only)

## macOS app (optional)

The Gateway alone delivers a great experience. All apps are optional and add extra features.

If you plan to build/run companion apps, initialize submodules first:

```bash
git submodule update --init --recursive
./scripts/restart-mac.sh
```

### macOS (Clawdbot.app) (optional)

- Menu bar control for the Gateway and health.
- Voice Wake + push-to-talk overlay.
- WebChat + debug tools.
- Remote gateway control over SSH.

Note: signed builds required for macOS permissions to stick across rebuilds (see `docs/mac/permissions.md`).

### iOS node (optional)

- Pairs as a node via the Bridge.
- Voice trigger forwarding + Canvas surface.
- Controlled via `clawdbot nodes …`.

Runbook: [iOS connect](https://docs.clawdbot.com/ios).

### Android node (optional)

- Pairs via the same Bridge + pairing flow as iOS.
- Exposes Canvas, Camera, and Screen capture commands.
- Runbook: [Android connect](https://docs.clawdbot.com/android).

## Agent workspace + skills

- Workspace root: `~/clawd` (configurable via `agent.workspace`).
- Injected prompt files: `AGENTS.md`, `SOUL.md`, `TOOLS.md`.
- Skills: `~/clawd/skills/<skill>/SKILL.md`.

## Configuration

Minimal `~/.clawdbot/clawdbot.json` (model + defaults):

```json5
{
  agent: {
    model: "anthropic/claude-opus-4-5"
  }
}
```

[Full configuration reference (all keys + examples).](https://docs.clawdbot.com/configuration)

## Security model (important)

- **Default:** tools run on the host for the **main** session, so the agent has full access when it’s just you.
- **Group/channel safety:** set `agent.sandbox.mode: "non-main"` to run **non‑main sessions** (groups/channels) inside per‑session Docker sandboxes; bash then runs in Docker for those sessions.
- **Sandbox defaults:** allowlist `bash`, `process`, `read`, `write`, `edit`; denylist `browser`, `canvas`, `nodes`, `cron`, `discord`, `gateway`.

Details: [Security guide](https://docs.clawdbot.com/security) · [Docker + sandboxing](https://docs.clawdbot.com/docker) · [Sandbox config](https://docs.clawdbot.com/configuration)

### [WhatsApp](https://docs.clawdbot.com/whatsapp)

- Link the device: `pnpm clawdbot login` (stores creds in `~/.clawdbot/credentials`).
- Allowlist who can talk to the assistant via `whatsapp.allowFrom`.

### [Telegram](https://docs.clawdbot.com/telegram)

- Set `TELEGRAM_BOT_TOKEN` or `telegram.botToken` (env wins).
- Optional: set `telegram.groups` (with `telegram.groups."*".requireMention`), `telegram.allowFrom`, or `telegram.webhookUrl` as needed.

```json5
{
  telegram: {
    botToken: "123456:ABCDEF"
  }
}
```

### [Slack](https://docs.clawdbot.com/slack)

- Set `SLACK_BOT_TOKEN` + `SLACK_APP_TOKEN` (or `slack.botToken` + `slack.appToken`).

### [Discord](https://docs.clawdbot.com/discord)

- Set `DISCORD_BOT_TOKEN` or `discord.token` (env wins).
- Optional: set `discord.slashCommand`, `discord.dm.allowFrom`, `discord.guilds`, or `discord.mediaMaxMb` as needed.

```json5
{
  discord: {
    token: "1234abcd"
  }
}
```

### [Signal](https://docs.clawdbot.com/signal)

- Requires `signal-cli` and a `signal` config section.

### [iMessage](https://docs.clawdbot.com/imessage)

- macOS only; Messages must be signed in.

### [WebChat](https://docs.clawdbot.com/webchat)

- Uses the Gateway WebSocket; no separate WebChat port/config.

Browser control (optional):

```json5
{
  browser: {
    enabled: true,
    controlUrl: "http://127.0.0.1:18791",
    color: "#FF4500"
  }
}
```

## Docs

Use these when you’re past the onboarding flow and want the deeper reference.
- [Start with the docs index for navigation and “what’s where.”](https://docs.clawdbot.com/)
- [Read the architecture overview for the gateway + protocol model.](https://docs.clawdbot.com/architecture)
- [Use the full configuration reference when you need every key and example.](https://docs.clawdbot.com/configuration)
- [Run the Gateway by the book with the operational runbook.](https://docs.clawdbot.com/gateway)
- [Learn how the Control UI/Web surfaces work and how to expose them safely.](https://docs.clawdbot.com/web)
- [Understand remote access over SSH tunnels or tailnets.](https://docs.clawdbot.com/remote)
- [Follow the onboarding wizard flow for a guided setup.](https://docs.clawdbot.com/wizard)
- [Wire external triggers via the webhook surface.](https://docs.clawdbot.com/webhook)
- [Set up Gmail Pub/Sub triggers.](https://docs.clawdbot.com/gmail-pubsub)
- [Learn the macOS menu bar companion details.](https://docs.clawdbot.com/mac/menu-bar)
- [Platform guides: Windows](https://docs.clawdbot.com/windows), [Linux](https://docs.clawdbot.com/linux), [macOS](https://docs.clawdbot.com/macos), [iOS](https://docs.clawdbot.com/ios), [Android](https://docs.clawdbot.com/android)
- [Debug common failures with the troubleshooting guide.](https://docs.clawdbot.com/troubleshooting)
- [Review security guidance before exposing anything.](https://docs.clawdbot.com/security)

## Advanced docs (discovery + control)

- [Discovery + transports](https://docs.clawdbot.com/discovery)
- [Bonjour/mDNS](https://docs.clawdbot.com/bonjour)
- [Gateway pairing](https://docs.clawdbot.com/gateway/pairing)
- [Remote gateway README](https://docs.clawdbot.com/remote-gateway-readme)
- [Control UI](https://docs.clawdbot.com/control-ui)
- [Dashboard](https://docs.clawdbot.com/dashboard)

## Operations & troubleshooting

- [Health checks](https://docs.clawdbot.com/health)
- [Gateway lock](https://docs.clawdbot.com/gateway-lock)
- [Background process](https://docs.clawdbot.com/background-process)
- [Browser troubleshooting (Linux)](https://docs.clawdbot.com/browser-linux-troubleshooting)
- [Logging](https://docs.clawdbot.com/logging)

## Deep dives

- [Agent loop](https://docs.clawdbot.com/agent-loop)
- [Presence](https://docs.clawdbot.com/presence)
- [TypeBox schemas](https://docs.clawdbot.com/typebox)
- [RPC adapters](https://docs.clawdbot.com/rpc)
- [Queue](https://docs.clawdbot.com/queue)

## Workspace & skills

- [Skills config](https://docs.clawdbot.com/skills-config)
- [Default AGENTS](https://docs.clawdbot.com/AGENTS.default)
- [Templates: AGENTS](https://docs.clawdbot.com/templates/AGENTS)
- [Templates: BOOTSTRAP](https://docs.clawdbot.com/templates/BOOTSTRAP)
- [Templates: IDENTITY](https://docs.clawdbot.com/templates/IDENTITY)
- [Templates: SOUL](https://docs.clawdbot.com/templates/SOUL)
- [Templates: TOOLS](https://docs.clawdbot.com/templates/TOOLS)
- [Templates: USER](https://docs.clawdbot.com/templates/USER)

## Platform internals

- [macOS dev setup](https://docs.clawdbot.com/mac/dev-setup)
- [macOS menu bar](https://docs.clawdbot.com/mac/menu-bar)
- [macOS voice wake](https://docs.clawdbot.com/mac/voicewake)
- [iOS node](https://docs.clawdbot.com/ios)
- [Android node](https://docs.clawdbot.com/android)
- [Windows app](https://docs.clawdbot.com/windows)
- [Linux app](https://docs.clawdbot.com/linux)

## Email hooks (Gmail)

[Gmail Pub/Sub wiring (gcloud + gogcli), hook tokens, and auto-watch behavior are documented here.](https://docs.clawdbot.com/gmail-pubsub)

Gateway auto-starts the watcher when `hooks.enabled=true` and `hooks.gmail.account` is set; `clawdbot hooks gmail run` is the manual daemon wrapper if you don’t want auto-start.

```bash
clawdbot hooks gmail setup --account you@gmail.com
clawdbot hooks gmail run
```

## Clawd

Clawdbot was built for **Clawd**, a space lobster AI assistant. 🦞  
by Peter Steinberger and the community.

- https://clawd.me
- https://soul.md
- https://steipete.me

## Community

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines, maintainers, and how to submit PRs.  
AI/vibe-coded PRs welcome! 🤖

Thanks to all clawtributors:

<p align="left">
  <a href="https://github.com/steipete"><img src="https://avatars.githubusercontent.com/u/58493?v=4&s=48" width="48" height="48" alt="steipete" title="steipete"/></a> <a href="https://github.com/thewilloftheshadow"><img src="https://avatars.githubusercontent.com/u/35580099?v=4&s=48" width="48" height="48" alt="thewilloftheshadow" title="thewilloftheshadow"/></a> <a href="https://github.com/mcinteerj"><img src="https://avatars.githubusercontent.com/u/3613653?v=4&s=48" width="48" height="48" alt="mcinteerj" title="mcinteerj"/></a> <a href="https://github.com/joshp123"><img src="https://avatars.githubusercontent.com/u/1497361?v=4&s=48" width="48" height="48" alt="joshp123" title="joshp123"/></a> <a href="https://github.com/joaohlisboa"><img src="https://avatars.githubusercontent.com/u/8200873?v=4&s=48" width="48" height="48" alt="joaohlisboa" title="joaohlisboa"/></a> <a href="https://github.com/petter-b"><img src="https://avatars.githubusercontent.com/u/62076402?v=4&s=48" width="48" height="48" alt="petter-b" title="petter-b"/></a> <a href="https://github.com/mukhtharcm"><img src="https://avatars.githubusercontent.com/u/56378562?v=4&s=48" width="48" height="48" alt="mukhtharcm" title="mukhtharcm"/></a> <a href="https://github.com/dan-dr"><img src="https://avatars.githubusercontent.com/u/6669808?v=4&s=48" width="48" height="48" alt="dan-dr" title="dan-dr"/></a> <a href="https://github.com/Nachx639"><img src="https://avatars.githubusercontent.com/u/71144023?v=4&s=48" width="48" height="48" alt="Nachx639" title="Nachx639"/></a> <a href="https://github.com/jeffersonwarrior"><img src="https://avatars.githubusercontent.com/u/89030989?v=4&s=48" width="48" height="48" alt="jeffersonwarrior" title="jeffersonwarrior"/></a>
  <a href="https://github.com/mbelinky"><img src="https://avatars.githubusercontent.com/u/132747814?v=4&s=48" width="48" height="48" alt="mbelinky" title="mbelinky"/></a> <a href="https://github.com/julianengel"><img src="https://avatars.githubusercontent.com/u/10634231?v=4&s=48" width="48" height="48" alt="julianengel" title="julianengel"/></a> <a href="https://github.com/CashWilliams"><img src="https://avatars.githubusercontent.com/u/613573?v=4&s=48" width="48" height="48" alt="CashWilliams" title="CashWilliams"/></a> <a href="https://github.com/omniwired"><img src="https://avatars.githubusercontent.com/u/322761?v=4&s=48" width="48" height="48" alt="omniwired" title="omniwired"/></a> <a href="https://github.com/jverdi"><img src="https://avatars.githubusercontent.com/u/345050?v=4&s=48" width="48" height="48" alt="jverdi" title="jverdi"/></a> <a href="https://github.com/Syhids"><img src="https://avatars.githubusercontent.com/u/671202?v=4&s=48" width="48" height="48" alt="Syhids" title="Syhids"/></a> <a href="https://github.com/meaningfool"><img src="https://avatars.githubusercontent.com/u/2862331?v=4&s=48" width="48" height="48" alt="meaningfool" title="meaningfool"/></a> <a href="https://github.com/rafaelreis-r"><img src="https://avatars.githubusercontent.com/u/57492577?v=4&s=48" width="48" height="48" alt="rafaelreis-r" title="rafaelreis-r"/></a> <a href="https://github.com/wstock"><img src="https://avatars.githubusercontent.com/u/1394687?v=4&s=48" width="48" height="48" alt="wstock" title="wstock"/></a> <a href="https://github.com/vsabavat"><img src="https://avatars.githubusercontent.com/u/50385532?v=4&s=48" width="48" height="48" alt="vsabavat" title="vsabavat"/></a>
  <a href="https://github.com/scald"><img src="https://avatars.githubusercontent.com/u/1215913?v=4&s=48" width="48" height="48" alt="scald" title="scald"/></a> <a href="https://github.com/sreekaransrinath"><img src="https://avatars.githubusercontent.com/u/50989977?v=4&s=48" width="48" height="48" alt="sreekaransrinath" title="sreekaransrinath"/></a> <a href="https://github.com/ratulsarna"><img src="https://avatars.githubusercontent.com/u/105903728?v=4&s=48" width="48" height="48" alt="ratulsarna" title="ratulsarna"/></a> <a href="https://github.com/osolmaz"><img src="https://avatars.githubusercontent.com/u/2453968?v=4&s=48" width="48" height="48" alt="osolmaz" title="osolmaz"/></a> <a href="https://github.com/conhecendocontato"><img src="https://avatars.githubusercontent.com/u/82890727?v=4&s=48" width="48" height="48" alt="conhecendocontato" title="conhecendocontato"/></a> <a href="https://github.com/hrdwdmrbl"><img src="https://avatars.githubusercontent.com/u/554881?v=4&s=48" width="48" height="48" alt="hrdwdmrbl" title="hrdwdmrbl"/></a> <a href="https://github.com/jayhickey"><img src="https://avatars.githubusercontent.com/u/1676460?v=4&s=48" width="48" height="48" alt="jayhickey" title="jayhickey"/></a> <a href="https://github.com/jamesgroat"><img src="https://avatars.githubusercontent.com/u/2634024?v=4&s=48" width="48" height="48" alt="jamesgroat" title="jamesgroat"/></a> <a href="https://github.com/gtsifrikas"><img src="https://avatars.githubusercontent.com/u/8904378?v=4&s=48" width="48" height="48" alt="gtsifrikas" title="gtsifrikas"/></a> <a href="https://github.com/djangonavarro220"><img src="https://avatars.githubusercontent.com/u/251162586?v=4&s=48" width="48" height="48" alt="djangonavarro220" title="djangonavarro220"/></a>
  <a href="https://github.com/azade-c"><img src="https://avatars.githubusercontent.com/u/252790079?v=4&s=48" width="48" height="48" alt="azade-c" title="azade-c"/></a> <a href="https://github.com/andranik-sahakyan"><img src="https://avatars.githubusercontent.com/u/8908029?v=4&s=48" width="48" height="48" alt="andranik-sahakyan" title="andranik-sahakyan"/></a>
</p>
