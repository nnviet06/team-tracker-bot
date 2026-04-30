## 1. Workspace alignment

- [ ] 1.1 Verify HEARTBEAT.md references all 4 capability specs by name
- [ ] 1.2 Verify SOUL.md, AGENTS.md, TOOLS.md still align with spec requirements (no contradictions)
- [ ] 1.3 Confirm STAND_UP_TEMP.md placeholders match standup-storage spec exactly

## 2. Telegram binding

- [ ] 2.1 Bind Telegram bot to the team-tracker agent (currently bound to main)
- [ ] 2.2 Unbind Telegram from main agent
- [ ] 2.3 Verify team-tracker receives a test message when sent to the bot

## 3. Heartbeat configuration

- [ ] 3.1 Configure heartbeat schedule on team-tracker agent only (morning + EOD windows GMT+7)
- [ ] 3.2 Confirm main agent has heartbeat disabled (already set "0m")
- [ ] 3.3 Verify timezone resolution: heartbeat fires at correct GMT+7 wall-clock times

## 4. Roster setup

- [ ] 4.1 Update MEMBERS.md to include all 4 fields per member: name, handle, role, active flag
- [ ] 4.2 Add at least one inactive member entry to verify inactive-handling spec

## 5. Live test: single window

- [ ] 5.1 Trigger morning window manually (or wait for scheduled fire)
- [ ] 5.2 Verify today's standup note is created from STAND_UP_TEMP.md with correct sections
- [ ] 5.3 Reply as a member; verify verbatim capture under "Tasks for today"
- [ ] 5.4 Verify inactive member shows "inactive" in their section, no message sent
- [ ] 5.5 Verify follow-up sends after 15 min of silence (test with one silent member)
- [ ] 5.6 Verify "No update" written for member who never replies

## 6. Live test: full day cycle

- [ ] 6.1 Run a real morning window with all members
- [ ] 6.2 Run the EOD window same day
- [ ] 6.3 Verify Team Summary appended at 18:00 with verbatim + one-sentence summary per member
- [ ] 6.4 Verify no fabrication: members with "No update" show "No update" in summary, no invented progress

## 7. Validation and archive

- [ ] 7.1 Run `openspec validate add-team-tracker-workflow` — must pass
- [ ] 7.2 Run `openspec archive add-team-tracker-workflow` to merge deltas into openspec/specs/
- [ ] 7.3 Commit archived state