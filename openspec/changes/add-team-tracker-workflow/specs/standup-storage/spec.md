## ADDED Requirements

### Requirement: Standup note location and naming

The system SHALL write daily standup notes to `/mnt/c/Users/nnvie/Documents/team-tracker-bot-vault/standups/`. Each note SHALL be named `YYYY-MM-DD.md` using the date in Asia/Ho_Chi_Minh (GMT+7).

#### Scenario: New day, no existing note

- **WHEN** the heartbeat fires and today's standup note does not exist
- **THEN** the bot SHALL create `YYYY-MM-DD.md` in the standups directory before any capture occurs

### Requirement: Note creation from template

The system SHALL create new daily notes by rendering `STAND_UP_TEMP.md`, replacing `{{DATE}}` with today's date and emitting one `## {{MEMBER_NAME}}` block per active member from `MEMBERS.md`.

#### Scenario: Creating today's note

- **WHEN** the bot creates today's standup note
- **THEN** the file SHALL contain a section for each active member with empty "Tasks for today" and "EOD progress" subsections, and the Team Summary section SHALL be present and empty

### Requirement: Verbatim capture

The system SHALL write member replies verbatim into the appropriate subsection. The bot SHALL NOT paraphrase, summarize, correct, or interpret captured text.

#### Scenario: Member reply contains typos

- **WHEN** a member replies with typos or grammatical errors
- **THEN** the bot SHALL write the reply exactly as received

### Requirement: Multiple replies append

The system SHALL append to an existing subsection if a member sends additional messages within the same window. The bot SHALL NOT overwrite earlier captured text.

#### Scenario: Member sends a follow-up message in the morning window

- **WHEN** a member has already replied during the morning window and sends another message before 10:00
- **THEN** the bot SHALL append the new message to their "Tasks for today" subsection on a new line