## ADDED Requirements

### Requirement: Roster file format

The system SHALL store the team roster in `MEMBERS.md` at the repo root. Each member SHALL be listed with four fields: name, Telegram handle, role, and active flag.

#### Scenario: Reading a valid member entry

- **WHEN** the bot reads `MEMBERS.md` during a heartbeat window
- **THEN** each member entry yields a name, a Telegram handle, a role, and an active flag (true/false)

#### Scenario: Member with missing handle

- **WHEN** a member entry is missing a Telegram handle
- **THEN** the bot SHALL skip that member for the current window and log the issue without messaging the team

### Requirement: Roster modification

The system SHALL support roster modification via two paths only: manual file edits, and bot-issued read operations. The bot SHALL NOT write to `MEMBERS.md`.

#### Scenario: Manual edit between windows

- **WHEN** `MEMBERS.md` is edited manually between heartbeat windows
- **THEN** the next heartbeat window SHALL read the updated roster without restart

### Requirement: Active flag governs messaging

The system SHALL message only members whose active flag is true. Inactive members SHALL be logged in the day's standup note as inactive but receive no Telegram message.

#### Scenario: Inactive member during morning window

- **WHEN** the bot processes the morning window and a member's active flag is false
- **THEN** the bot SHALL write "inactive" under that member's section in today's standup note and SHALL NOT send a Telegram message