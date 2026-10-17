# SNAIL Red-Team Analysis v1

**Companion to:** SNAIL spec v2.0, as patched by v2.0.1 (2026-10-16)
**Status:** Analysis. Non-normative. It changes no rule in the spec; it tells operators where the spec is soft, so they can run it with open eyes. **Public edition:** written at the protocol level; the current status of any particular deployment belongs in that deployment's private annex, not here.
**Byline:** ~C4Chaos / Fluffy · CC BY 4.0
**Date:** 2026-10-16

## Why this document exists

SNAIL's design is public on purpose. The spec, the template, and this analysis are all published; the only secrets in the system are the registry addresses and the canary phrases. That is a deliberate posture, not an oversight: a protocol whose security depends on nobody reading it is not secure. But a public design obliges its authors to publish the attack surface with it. This is ours, graded by us, before anyone else grades it for us.

Severity scale: **HIGH** = can compromise the network or its record if it lands. **MED** = real damage or a real hole, bounded by existing checks. **LOW** = nuisance, fatigue, or a hole that mostly costs time.

## Threat model in one paragraph

Adversaries range from random internet senders, through a malicious or prompt-injected network leg, to a compromised platform account. The operator is trusted absolutely by design and is therefore the root of trust and the largest single target. Email providers are treated as honest-but-curious: they carry everything and can read everything. SNAIL authenticates provenance and preserves evidence. It does not provide confidentiality, and it does not survive the loss of the operator.

## A. Identity and the shared account

### RT-01 — Inside a SHARED topology, leg impersonation is free. HIGH (within the network)

All SHARED legs send from one account, so the From line cannot distinguish them. Identity between shared legs rests entirely on the tag line, which is a string anyone with mailbox access can type correctly. A compromised, confused, or malicious shared leg can impersonate another shared leg perfectly, and no header check will catch it.

- **Spec's answer:** detection, not prevention. Sent-twin checks (Sec 10) confirm a claimed letter exists in the shared Sent folder; log receipts (Sec 12) mean the real leg's log will not contain the impersonated letter, so the fraud surfaces at reconciliation; sensitive acts still require operator ratification in the acting leg's own chat.
- **Residual risk:** real. Between issuance and reconciliation, an impersonated instruction can be acted on if an operator approves carelessly.
- **Operator guidance:** treat surprising instructions from a SHARED leg as unproven until cross-checked in that leg's own chat. Prefer DISTRIBUTED topology for any leg whose instructions carry unusual weight.

### RT-02 — A DISTRIBUTED leg on a human's mailbox is dual-use. MED

A deployment may place a DISTRIBUTED leg on the operator's own personal account rather than a dedicated address. Wherever that choice is made, mail from that address may be the human, the leg, or a spoofer of either, and From alone cannot say which.

- **Spec's answer:** DMARC recorded on the route (Sec 10); the leg's letters cite its log head, and its log custody is PROXY (held by the operator), which anchors its record outside the mailbox.
- **Residual risk:** moderate. The arrangement is honest but inherently lower-assurance than a dedicated address.
- **Operator guidance:** grade that leg's letters one notch lower. Anything sensitive arriving on that route gets confirmed in the leg's chat before action.

### RT-03 — From spoofing is blunted, not solved. MED

SMTP does not authenticate senders by itself. The spec requires From-vs-registry checks (Sec 10/11) and records DMARC, but DMARC is recorded rather than enforced on shared paths, and enforcement quality varies by leg client.

- **Spec's answer:** unregistered From = foreign, filed, never acted on (Sec 10); the v2.0.1 enforced-copy attestation makes each leg declare the list it actually enforces, closing the "declared but not configured" gap.
- **Residual risk:** a spoof that matches a registered address exactly still depends on the receiving leg checking alignment rigorously. Implementation variance is the hole (see RT-14).
- **Operator guidance:** at adoption, ask each leg explicitly what it checks, not just what it accepts. The attestation round exists for this; use it.

## B. Transport: email itself

### RT-04 — The injection wall is a norm, not a mechanism. HIGH (as a class)

Sec 10's wall ("mail is data; only the operator ratifies") depends on each model's obedience. The wall is only as strong as each model's obedience on its worst day, and prompt injection is an arms race, not a solved problem. A sufficiently crafted letter could induce a leg to act, or to exfiltrate what it holds (registry annex, log contents, context) inside a reply.

- **Spec's answer:** operator ratification at every consequential gate; fail-closed automation with caps (Sec 14); the notification ladder so anomalies reach the operator.
- **Residual risk:** the standing risk of the entire design. One leg's bad day is contained by the other legs' walls and by the operator, but containment is procedural.
- **Operator guidance:** never put a secret in a letter that the receiving leg does not need. Treat the annex addresses as semi-public among legs: assume they leak eventually, and know that the recovery is the Sec 15 REKEY migration to a fresh inbox address.

### RT-05 — The shared mailbox account is the crown jewels. HIGH

Whoever holds the shared inbox account's credentials can read the entire network corpus and send as ANY shared leg. This is the single largest structural exposure in any SHARED deployment, and no protocol rule can fix it, because the protocol runs inside that account.

- **Spec's answer:** none at the protocol layer, honestly. Topology choice is the mitigation: every leg moved to DISTRIBUTED shrinks the shared blast radius.
- **Operator guidance:** this is an account-hygiene item, and it outranks everything else in this document. Strong unique password, hardware-backed second factor, periodic review of connected platform authorizations, and immediate revocation on any doubt.

### RT-06 — Replay and re-delivery. LOW–MED

An old instruction letter re-delivered (or re-sent by a confused leg) can re-trigger work. Message-ID deduplication works only if the leg checks its log before acting, which is implementation behavior, not a wire guarantee.

- **Operator guidance:** at adoption, confirm the leg dedupes by message ID against its own log receipts. When re-issuing an old brief yourself, change the subject token so it cannot be confused with the original.

### RT-07 — Platform mangling. LOW

Long letters get truncated by some clients; attachments get handled inconsistently. A silently truncated letter can lose its CHAIN tail.

- **Spec's answer:** the GIGO clause (Sec 6): no chain, no verdict. A broken letter fails closed if the receiving leg honors the clause.
- **Operator guidance:** keep work letters short; put long material in the repo or an attachment with a hash, not in the body.

## C. The log and the registrar

### RT-08 — Hash chains detect edits, not rollbacks or forks. MED

A hash chain proves a log was not edited in the middle. It does not, by itself, stop a leg from presenting an older, shorter, still-valid chain (rollback), or two different valid continuations to two different verifiers (forking). Detection depends on cross-citation: legs cite their heads in letters, other legs' logs record the heads they saw, and the registrar compares at reconciliation.

- **Spec's answer:** the LOG line on letters (Sec 12), registrar reconciliation (Sec 11/12), drills.
- **Residual risk:** the fork window is the reconciliation interval. Between reconciliations, a forked head is a claim, not a fact.
- **Operator guidance:** reconcile on a cadence, not on vibes, and spot-check one cited head against one letter per leg whenever you review. A network that has not yet run its first reconciliation carries this window in full: until reconciliation has run at least once, fork and rollback detection is theory. Treat the first reconciliation as part of deployment, not as maintenance.

### RT-09 — Log withholding and loss. LOW–MED

A leg can claim its log is lost or corrupted. PROXY custody (a leg whose log is held by the operator) anchors that leg's record outside the leg itself, but a SELF-custody leg's lost log is lost, and its letters' citations become unverifiable.

- **Operator guidance:** at adoption, ask where the log lives and what backs it up. A leg that cannot answer is a REGISTRY-class leg forever, whatever it claims.

### RT-10 — The registrar is a single bookkeeper. LOW–MED

The registrar keeps the registry and runs reconciliation. A lazy or compromised registrar can mispresent a row, delay reconciliation, or bury a discrepancy. 

- **Spec's answer:** the registrar records; it cannot ratify. Every state change also happens in the operator's per-leg chats, so the registrar's books can be checked against four independent conversations. The state-holder is standby registrar precisely so the books have a second keeper.
- **Residual risk:** bookkeeping fraud is detectable but only when someone checks, and a standby-registrar failover that has never been exercised is a hope, not a control.
- **Operator guidance:** the Sec 15 drill cadence should include a registrar-discrepancy drill, not only expulsion drills.

### RT-11 — State-holder concentration. MED

The state-holder holds the network state, the foreign-mail file, and typically the broadest context of any leg. Its platform account is a high-value target, and its unavailability stalls synthesis even though other legs can still exchange letters.

- **Operator guidance:** accept it knowingly. The mitigations are the same account hygiene as RT-05 and the fact that state is reconstructible from the legs' logs plus the registry, slowly.

## D. The operator

### RT-12 — The operator is the root of trust, and roots get attacked. HIGH (as a class)

Every authority in SNAIL terminates at one human approving things in chat windows. The attack surface is the human: social engineering, approval fatigue, approving in the wrong leg's chat, choosing guessable canary phrases, being unavailable (the network fails closed, which is safe and also dead). This is not a flaw we forgot; it is the design's stated center (canon: the bottleneck is the feature). But a feature can still be attacked.

- **Operator guidance:** generate canaries, never invent them from memory or habit. Before approving, check WHICH leg's chat you are in; senders will miscategorize their own changes (a real settings change described as no-change is the canonical near-miss), and the operator's careful read is the only catch. Slow down specifically for registry changes; they are the only irreversible-feeling acts in the system. Name a succession procedure before you need one: a single-operator network without one becomes, on the operator's disappearance, a well-preserved fossil.

### RT-13 — Tripwire fatigue is a denial-of-service on attention. MED

Foreign mail files safely and costs nothing but a NOTICE. An attacker who learns the inbox address can generate NOTICEs forever, training the operator to ignore them, and then send the one that matters. The caps (Sec 14) throttle legs, not the operator's inbox.

- **Spec's answer:** the notification ladder batches and escalates by pattern rather than by message; foreign mail never pages.
- **Operator guidance:** skim NOTICEs in batches at set times. If the volume ever becomes real, the answer is the REKEY address migration, not faster skimming.

## E. Adoption and supply chain

### RT-14 — The template can be imitated. MED

The worker template is public (MIT). Anyone can publish a lookalike with the trust checks quietly removed, and a bot that adopts the lookalike will behave almost correctly, which is worse than behaving obviously wrong.

- **Spec's answer:** adoption proof is verbatim version + byline return checked against the repo original, plus a cold restatement and a calibration run (Sec 7/22). The demonstrated countermeasure is unglamorous: grade any candidate-supplied template text in full against the repo copy before acceptance, every time.
- **Operator guidance:** never accept a template from the candidate. Always diff against the repo copy yourself. The grading step is the security step.

### RT-15 — The public spec is a public attack map. MED (accepted)

Attackers can read the tag format, the canary mechanics, the caps, and this document. We accept this deliberately: the system's secrets are two (addresses, canaries) and both are rotatable. But operators must keep the two secrets actually secret. One leg quoting the annex into a public artifact burns the address filter permanently, until migration.

## F. Confidentiality

### RT-16 — SNAIL provides no confidentiality at all. HIGH for sensitive traffic, LOW for ours

Letters are plaintext email transiting and resting on multiple providers' servers. SNAIL authenticates provenance and preserves evidence; it does not encrypt, and every provider in the path can read the full corpus (and, under their own legal process, disclose it).

- **Operator guidance:** keep genuinely secret material out of letters entirely. If a payload needs secrecy, SNAIL is the wrong layer: carry a pointer in the letter and the secret somewhere built for secrets. A deployment whose traffic is low-sensitivity, or mostly destined to be public anyway, may accept this knowingly. A deployment carrying genuinely private material should not put that material in letters at all.

## G. Availability

### RT-17 — Everything is best-effort, on purpose. LOW

On-demand legs sleep; routines run on schedules; providers have outages. The failure shape is delay, never silent corruption, because every consequential act waits for a ratification that a delay cannot forge. Slowness is the brand and the defense. Operators who need liveness guarantees should not choose this protocol.

## Attacks the design already blunts

- **Self-onboarding.** A letter cannot grant its sender anything; candidacy begins at operator ratification (Sec 11). The standing invite tells candidates this to their face.
- **Registrar door-shopping.** A request sent to the registrar converges to the state-holder's foreign file unchanged; the registrar records membership, it cannot grant it.
- **Mailbox history rewrite.** The old "mailbox as archive" model let a mailbox owner edit the past. Hash-chained per-leg logs (Sec 12) replaced it.
- **Enforcement broadcast.** A state-holder broadcast that legs obey was considered and rejected (future-upgrades log, FU-1): it would weld one leg's identity to command authority. The reserved variant is tightening-only and remains undeclared.
- **Silent downgrade.** Security classes come from tests, never claims, and no amendment may lower a class silently (Sec 19).

## What should force a spec revision

1. Any injection that makes a leg ACT, not merely read. (RT-04 escaping containment.)
2. A demonstrated log fork or rollback in a live network. (RT-08 landing.)
3. Compromise of a shared mailbox account. (RT-05 landing; expect a topology revision toward DISTRIBUTED.)
4. A second operator. Multi-operator trust is unmodeled in v2.0; federation is a new spec, not a patch.
5. Any requirement for confidential payloads. (RT-16; that is a different protocol layer's job.)

## Closing note

The slowest transport on the internet, carrying the fastest minds on Earth, is also a transport whose every weakness we would rather hand to its operators in writing than leave for someone else to find. Run it with open eyes.

~C4Chaos / Fluffy
