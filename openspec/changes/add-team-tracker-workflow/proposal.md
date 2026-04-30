## Why

Rockship take-home: build a Telegram-based standup bot that runs daily morning and EOD check-ins, captures replies verbatim into Obsidian, and produces a factual team summary. The bot must operate without interpretation, fabrication, or scope creep — capture is the product.

Specs are needed to lock in the workflow contract before implementation: what each window does, what gets stored where, and what the bot must never do.

## What Changes

- Define a 4-capability workflow: member roster, standup storage, morning check-in, EOD check-in.
- Establish file-format contracts for `MEMBERS.md` and daily standup notes (rendered from `STAND_UP_TEMP.md`).
- Establish behavioral contracts for both check-in windows (timing, follow-up policy, capture rules).
- No code or model integration changes in this proposal — these are workflow specs that the agent and HEARTBEAT loop will read at runtime.

## Capabilities

### New Capabilities

- `member-management`: Roster file (`MEMBERS.md`) format, fields per member, and read rules during a heartbeat window.
- `standup-storage`: Daily standup note location, filename convention, creation-from-template behavior, and per-member section structure.
- `morning-checkin`: 09:30–10:00 GMT+7 window — prompt format, single follow-up policy, verbatim capture into the day's standup note.
- `eod-checkin`: 17:30–18:00 GMT+7 window — prompt format, single follow-up policy, verbatim capture, and end-of-window team summary generation.

### Modified Capabilities

None. Greenfield project, no existing specs.

## Impact

- **Files added:** `openspec/specs/{member-management,standup-storage,morning-checkin,eod-checkin}/spec.md` (after archive).
- **Runtime files affected:** `HEARTBEAT.md` will reference these specs instead of inlining behavior.
- **External systems:** Telegram bot binding (team-tracker agent), Obsidian vault at `/mnt/c/Users/nnvie/Documents/team-tracker-bot-vault/`.
- **No code dependencies, no schema changes, no migrations.**