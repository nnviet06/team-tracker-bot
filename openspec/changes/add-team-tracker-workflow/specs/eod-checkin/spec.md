## ADDED Requirements

### Requirement: EOD window timing

The system SHALL run the EOD check-in between 17:30 and 18:00 in Asia/Ho_Chi_Minh (GMT+7). Outside this window, no EOD prompts SHALL be sent.

#### Scenario: Heartbeat fires at 17:45 GMT+7

- **WHEN** the heartbeat fires at 17:45 GMT+7 and a member has no "EOD progress" entry yet
- **THEN** the bot SHALL send the EOD prompt to that member

### Requirement: EOD prompt format

The system SHALL send the prompt: `"Hey [name], how did today go? What did you get done?"` where `[name]` is the member's name from `MEMBERS.md`.

#### Scenario: Sending EOD prompt

- **WHEN** the bot sends an EOD prompt to a member named "Mi"
- **THEN** the message text SHALL be exactly `"Hey Mi, how did today go? What did you get done?"`

### Requirement: Single follow-up policy

The system SHALL send at most one follow-up per member per EOD window. A follow-up SHALL be sent if the member has not replied within 15 minutes of the initial prompt.

#### Scenario: Member silent for 15 minutes after EOD prompt

- **WHEN** 15 minutes have passed since the initial EOD prompt and the member has not replied
- **THEN** the bot SHALL send exactly one follow-up message and SHALL NOT send further messages in the EOD window

### Requirement: No-response handling

The system SHALL write `"No update"` under a member's "EOD progress" subsection if the member has not replied by the end of the EOD window.

#### Scenario: Window closes with member silent

- **WHEN** the EOD window closes at 18:00 and a member has not replied
- **THEN** the bot SHALL write `"No update"` under that member's "EOD progress" subsection

### Requirement: Team summary at end of window

The system SHALL append a Team Summary section to today's standup note when the EOD window closes at 18:00. The summary SHALL contain two parts per member: a verbatim quote of their EOD reply, followed by a one-sentence factual restatement.

#### Scenario: All members replied

- **WHEN** the EOD window closes and all active members have replied
- **THEN** the bot SHALL write a Team Summary section with one entry per member containing both the verbatim EOD reply and a one-sentence factual summary

#### Scenario: Member with no update

- **WHEN** a member's "EOD progress" is "No update" at window close
- **THEN** that member's Team Summary entry SHALL be `"[Name]: No update."` with no fabricated content

### Requirement: No fabrication

The system SHALL NOT invent progress, infer status, or add encouragement to the Team Summary. Summaries SHALL be derived only from the captured "EOD progress" text.

#### Scenario: Member reply is vague

- **WHEN** a member's EOD reply is vague (e.g., "stuff")
- **THEN** the one-sentence summary SHALL reflect that vagueness factually rather than inventing detail