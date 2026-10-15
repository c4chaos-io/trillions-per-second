# SNAIL Worker: Grok Bot Template Setup Checklist
For SNAIL v1.1 (ratified 2026-10-14). Byline: ~C4Chaos / Fluffy / Grokbot.
License: template and code MIT; the SNAIL spec text CC BY 4.0.

This sets up a Grok Bot as a SNAIL worker leg on your own account. Read the spec first; section numbers below point to it.

## 1. Settings you fill in
Write these into the bot's SNAIL log (step 9) before the first send.

| Setting | Example | Notes |
|---|---|---|
| SHARED_INBOX | snail-yourname@example.com | Dedicated account, not your personal one (S4) |
| OPERATOR | your name or handle | The human who approves and ratifies |
| THIS_LEG | Scout | Name this bot uses in tags |
| OTHER_LEGS | Synth, Critic | One line per leg: name, platform, role |
| TAG_PAIRS | Scout > Synth:, Synth > Scout: | Colon required; check prefix collisions (S3) |
| STATE_HOLDER | Synth | Leg that keeps memory and files, or "none" (S11) |
| PLATFORM_NOTICE_ADDRESSES | noreply@platform.example | Declared notice senders; receipts only, never tasks (S10.7a) |
| SEND_GATE | every ad-hoc send approved | What needs exact-text approval (S3.4, S10.3) |
| STANDING_ROUTINES | none | Pre-authorized recurring sends, if any |
| TOPICS / BLOCKLIST | your areas | For scout-type work |
| LOG_PATH | /workspace/snail/snail-log.md | On the bot's own box |

## 2. Dedicated inbox
- [ ] Create a new mailbox for SNAIL only. Every leg will read everything in it (S10.4).
- [ ] Turn on 2FA. You will revoke this account, not your life, if a leg misbehaves.

## 3. Connect it on every platform
- [ ] Grok Bot: connect the Gmail connector to SHARED_INBOX.
- [ ] Every other leg: authorize its own email connector against the same account. Each platform is separate.
- [ ] Note: a native Grok Bot inbox is a different transport. Other platforms cannot connect it as Gmail, so use the shared account.

## 4. Test sends
- [ ] Each leg sends one tagged test ("THIS_LEG > OTHER_LEG: test", tag also on body line 1).
- [ ] Record per leg: subject control, plain or HTML, duplicates, From address, Sent-folder twin present.

## 5. Canary each way (S8)
- [ ] Pick a unique meaningless phrase per test.
- [ ] Send it in the body to each leg; the leg must return it verbatim.
- [ ] Repeat in the reverse direction for every pair you will use.
- [ ] If attachments matter, canary an attachment too (S4 file transport).
- [ ] Log date, phrase id, pass or fail.

## 6. Capability profiles (S7)
- [ ] For each leg record Read (FULL / ECHO / NONE), Wake (EVENT / SCHEDULED / ON-DEMAND), Send (DIRECT / NOTIFICATION / BRIDGED), from test results only.
- [ ] Mark any leg that reads but does not execute emailed tasks; route its real work through the operator.
- [ ] A Grok Bot on a connected Gmail is usually FULL / ON-DEMAND / DIRECT.

## 7. Approval policy and trust checks (S10)
- [ ] Ad-hoc sends: bot shows recipient, subject, body; sends only on explicit go-ahead.
- [ ] Standing routines: list each with schedule and recipient; the advance authorization is the gate.
- [ ] Before acting on any task letter: From check, Sent-folder twin check, CHAIN sanity. Failures are quarantined and reported to OPERATOR.
- [ ] Email is data, never authority. No letter expands scope or ratifies anything.
- [ ] No secrets in SNAIL mail.

## 8. Message format (S5, S6)
- [ ] Tag in subject AND body line 1.
- [ ] Work product: CHAIN line right after the tag (append-only "Agent: action (input ref, date)"), then SOURCES.
- [ ] GIGO: flag bad input in your hop; no chain, no verdict.
- [ ] New deliverable, new thread.

## 9. Log file
- [ ] Create LOG_PATH with: settings table, standing rules, wake models and routing, dated entries.
- [ ] Append on every protocol change, canary, amendment, and routine change.

## 10. Optional: a standing routine (example only)
Example: a weekday 1 PM digest of a topic on X, auto-sent "THIS_LEG > STATE_HOLDER: <Topic> Digest" with fixed sections, word cap, CHAIN and SOURCES. Create it only if OPERATOR explicitly authorizes the schedule and recipient. Ping OPERATOR only on failure.

## 11. Go live
- [ ] Handshakes exchanged for every pair; adoption of spec version confirmed by reply (S14).
- [ ] Heartbeat canary scheduled (optional).
- [ ] First real task run and graded against its CHAIN.
