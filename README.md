# Team Tracker Bot

A Telegram-based daily standup bot. Sends morning task reminders and end-of-day check-ins to team members on a fixed schedule, captures replies verbatim, and stores them as Markdown notes in an [Obsidian](https://obsidian.md) vault.

Built on [OpenClaw](https://docs.openclaw.ai) (agent runtime) with workflow specs managed by [OpenSpec](https://github.com/Fission-AI/OpenSpec).

## What it does

- **09:30–10:00 GMT+7** — DMs each active team member: *"Good morning [name]. What are your tasks for today?"*
- **17:30–18:00 GMT+7** — DMs each member: *"Hey [name], how did today go? What did you get done?"*
- Captures replies verbatim into `standups/YYYY-MM-DD.md` in your Obsidian vault.
- One follow-up per missed prompt, then logs `"No update"` and moves on.
- Appends a factual Team Summary at end of EOD window.

No interpretation, no encouragement, no fabrication. Capture is the product.

## Requirements

- WSL2 or native Linux (OpenClaw does not support Windows natively or macOS without adjustments)
- Node.js 22 LTS or 24 (via `nvm` is fine)
- An Obsidian vault on the host filesystem
- A Telegram bot (created via [@BotFather](https://t.me/BotFather))
- A Gemini API key ([Google AI Studio](https://aistudio.google.com/apikey))

## Setup

### 1. Install OpenClaw and OpenSpec

```bash
npm install -g openclaw
npm install -g @fission-ai/openspec
```

### 2. Clone and enter the repo

```bash
git clone https://github.com/nnviet06/team-tracker-bot.git
cd team-tracker-bot
```

### 3. Configure env

```bash
cp .env.example .env
```

Edit `.env` and fill in `GEMINI_API_KEY` and `TELEGRAM_BOT_TOKEN`.

### 4. Set up the Obsidian vault path

The bot writes daily standup notes to a folder called `standups/` inside your Obsidian vault. Update the vault path in `.openclaw/TOOLS.md` to point to your vault location.

### 5. Onboard the agent

```bash
openclaw onboard
```

Follow the prompts to register the Gemini key, Telegram bot, and skills (`obsidian`, `github`, `session-logs`).

### 6. Bind Telegram to the team-tracker agent

```bash
openclaw agents bind --agent team-tracker --bind telegram
```

DM your bot once on Telegram to trigger pairing. The bot will reply with a pairing code; approve it:

```bash
openclaw pairing approve telegram <code>
```

### 7. Configure the heartbeat schedule

```bash
openclaw config set agents.list[1].heartbeat.every "5m"
openclaw config set agents.list[1].heartbeat.activeHours.start "09:30"
openclaw config set agents.list[1].heartbeat.activeHours.end "18:00"
openclaw config set agents.list[1].heartbeat.activeHours.timezone "Asia/Ho_Chi_Minh"
openclaw config set agents.list[1].heartbeat.target "last"
openclaw config set agents.list[1].heartbeat.lightContext true
openclaw gateway restart
```

### 8. Add team members

Edit `.openclaw/MEMBERS.md` to add each team member (name, Telegram handle, role, active flag).

### 9. Verify

```bash
openclaw gateway status
openclaw agents bindings
openclaw config get agents.list[1].heartbeat
```

All three should look healthy. Done.

## Running

The OpenClaw gateway must be running for the bot to fire on schedule.

```bash
openclaw gateway start
```

The gateway is registered as a `systemd --user` service, so it auto-starts when your Linux session starts. **However**, if running on WSL2, the gateway only stays alive while WSL2 itself is running. To keep WSL2 alive across terminal closures, set `vmIdleTimeout=-1` in `C:\Users\<you>\.wslconfig`:

```ini
[wsl2]
vmIdleTimeout=-1
```

Then `wsl --shutdown` from PowerShell once to apply.

## Architecture

```
.openclaw/          OpenClaw agent workspace
├── SOUL.md         Bot personality + behavioral rules
├── AGENTS.md       Workflow conventions
├── HEARTBEAT.md    Per-fire decision logic (the scheduler reads this)
├── IDENTITY.md     Bot identity
├── USER.md         Operator context
├── TOOLS.md        Local config (vault path, etc.)
├── MEMBERS.md      Team roster
└── STAND_UP_TEMP.md  Daily note template

openspec/           OpenSpec workflow specs
├── changes/        Active and archived spec proposals
└── specs/          Canonical specs (after archive)
```

## Specs

The bot's behavior is defined by four capability specs under `openspec/specs/`:

- `member-management` — roster file format and read rules
- `standup-storage` — daily note location, naming, and capture rules
- `morning-checkin` — 09:30–10:00 window: prompt, follow-up, no-response handling
- `eod-checkin` — 17:30–18:00 window: prompt, follow-up, team summary generation

To inspect or modify the workflow:

```bash
openspec list
openspec status --change <name>
```
