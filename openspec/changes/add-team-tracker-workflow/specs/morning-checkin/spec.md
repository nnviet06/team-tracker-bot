## ADDED Requirements

### Requirement: Morning window timing

The system SHALL run the morning check-in between 09:30 and 10:00 in Asia/Ho_Chi_Minh (GMT+7). Outside this window, no morning prompts SHALL be sent.

#### Scenario: Heartbeat fires at 09:45 GMT+7

- **WHEN** the heartbeat fires at 09:45 GMT+7 and a member has no "Tasks for today" entry yet
- **THEN** the bot SHALL send the morning prompt to that member

#### Scenario: Heartbeat fires at 10:30 GMT+7

- **WHEN** the heartbeat fires at 10:30 GMT+7
- **THEN** the bot SHALL NOT send any morning prompts regardless of unanswered members

### Requirement: Morning prompt format

The system SHALL send the prompt: `"Good morning [name]. What are your tasks for today?"` where `[name]` is the member's name from `MEMBERS.md`.

#### Scenario: Sending prompt to active member

- **WHEN** the bot sends a morning prompt to a member named "Mi"
- **THEN** the message text SHALL be exactly `"Good morning Mi. What are your tasks for today?"`

### Requirement: Single follow-up policy

The system SHALL send at most one follow-up per member per morning window. A follow-up SHALL be sent if the member has not replied within 15 minutes of the initial prompt.

#### Scenario: Member silent for 15 minutes after initial prompt

- **WHEN** 15 minutes have passed since the initial morning prompt and the member has not replied
- **THEN** the bot SHALL send exactly one follow-up message and SHALL NOT send further messages in the morning window

### Requirement: No-response handling

The system SHALL write `"No update"` under a member's "Tasks for today" subsection if the member has not replied by the end of the morning window.

#### Scenario: Window closes with member silent

- **WHEN** the morning window closes at 10:00 and a member has not replied to either the initial prompt or the follow-up
- **THEN** the bot SHALL write `"No update"` under that member's "Tasks for today" subsection