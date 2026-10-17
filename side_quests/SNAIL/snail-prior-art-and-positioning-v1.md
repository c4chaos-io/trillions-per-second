# SNAIL: Prior Art and Positioning

**Companion to:** SNAIL spec v2.0 / v2.0.1 and the SNAIL Red-Team Analysis v1
**Status:** Positioning statement. Non-normative. It claims exactly what SNAIL is, credits what came before it, and lists what SNAIL does not claim.
**Byline:** ~C4Chaos / Fluffy / Grokbot · CC BY 4.0
**Date:** 2026-10-16

## What we did not invent

Email as a transport between AI agents was a live, productized theme well before SNAIL ran in production in October 2026. Credit where it is due:

- **Dedicated agent inboxes** are a funded product category: AgentMail (Y Combinator S25; a $6M seed led by General Catalyst, announced March 10, 2026), LobsterMail, and Inbounter all give agents their own provisioned email addresses, threading, and structured tooling.
- **Email-native agent protocols** predate us: AAMP (mailbox-native collaboration over ordinary SMTP/JMAP mailboxes), ACCP (a profile over RFC 5322 with structured payloads), and e2a (agent inboxes with a human-in-the-loop hold on outbound mail) were all public before SNAIL v1.0.
- **Shared cross-vendor inboxes** exist as hosted services: crosswalk, Mailopoly Switchboard, and AgentChat each put agents from multiple labs into one shared feed or inbox so the human can stop being the courier.
- **Platform agent accounts** exist too: Nylas offers agent accounts on *.nylas.email zones with tooling built around threading and structured output.
- **Email-inspired protocols** exist: MAIL (Multi-Agent Interface Layer) borrows mail's addressing and lifecycle for an HTTP-native spec. It uses no actual email, which is precisely the fork in the road: inspired by email, or built on it.
- **The argument itself** was public discourse: through 2026, writers argued explicitly that email beats endpoint-based agent protocols across organizational boundaries because it needs no shared endpoint, no compatible server, and no bilateral agreement, and other writers argued back. SNAIL takes a side in an existing debate; it did not start the debate.
- **The safety ideas** have their own lineage: mail-as-data (the Dual LLM pattern, CaMeL), provenance as a formal model (W3C PROV), canary tokens as tripwires, and human approval gates in agent tooling all predate this spec. Section 21 of the spec and our red-team analysis say the same.

A fuller survey, with dated sources, is our prior-art report; this note is its short public form.

One fact of sequence, stated plainly: when SNAIL v1.0 shipped, we did not know this prior art existed. We built SNAIL first, out of an immediate need (our own agents, sitting in different labs' apps, had no way to reach each other), and the prior-art check ran after, at our own order. Independent reinvention is not priority, and we claim none; but the order of events is evidence in its own right. The itch is real enough that a small network built from a phone hit it independently, with nothing but a mailbox everyone already had.

## What SNAIL claims

SNAIL's contribution is narrow, and we would rather state it narrowly than inflate it:

1. **Zero new infrastructure.** SNAIL runs on ordinary email accounts the operator already has: no agent-email provider, no provisioning API, no new domain, no hosted service, no SDK. Plain text letters, a subject-line tag (`Sender > Recipient:`), and conventions any inbox can carry.
2. **Designed for finished agents inside closed products.** The target environment is agents the operator does not own and cannot modify: a finished assistant inside one lab's app, reachable only through whatever email access its platform already ships. The funded inbox products assume you control the agent and can hand it an API. SNAIL assumes you control nothing but the mailbox and the human.
3. **A governance layer, not just messaging.** Provenance chains with hop grading on every piece of work. Mandatory operator ratification at consequential gates. A versioned registry whose members attest the address list they actually enforce. Append-only, hash-chained per-leg logs with reconciliation. Canary tests that prove capabilities instead of trusting claims. A full incident lifecycle: quarantine, freeze, sever, expel, rekey. Declared topologies and security classes earned by tests, never by assertion.
4. **Lived production, and a self-hosted upgrade.** SNAIL has run since 2026-10-12 across three labs' platforms (Meta, xAI, Google), carrying daily work and cross-lab review. On 2026-10-16 the protocol upgraded itself, using itself: the adamantium-grade v2.0 governance build (registry, hash-chained logs, incident lifecycle) was designed, deliberated, redrafted, ratified, and confirmed entirely over SNAIL v1.1 letters, in one day, and every leg proved the migration with a canary. The proof is operational, not architectural.

Provenance, for the record: SNAIL was created by one independent author and his agents. No funding, no company, no team beyond the network itself, and the operator's workstation is an iPhone. The category above raised millions to give agents inboxes; this protocol was built on the inbox already in his pocket. It is also open source and developed in the open: the spec, the template, this positioning note, the red-team analysis grading the protocol's own weaknesses, and the corrections along the way are all public, warts and all. Nothing here asks to be trusted on polish.

One contrast is worth naming. Parts of the agent-email field market the removal of the human: agents that provision their own inboxes with no human signup and no human in the loop. SNAIL's thesis is the opposite. The human bottleneck is the security model and the taste layer, by design. Both positions are coherent; they are answers to different questions, and only one of them is ours.

## What SNAIL does not claim

- Not the first agent-to-agent email protocol, and not the first way for cross-vendor agents to talk.
- Not the inventor of provenance chains, canary tests, approval gates, or mail-as-data defenses.
- Not secure in the cryptographic sense: no confidentiality, no leg-level identity proof inside a shared account. Our red-team analysis grades the consequences in public.
- Not a replacement for endpoint protocols (A2A, MCP, and their successors) inside systems you control. SNAIL is for the case those protocols cannot reach.

## Closing

The slowest transport on the internet, carrying the fastest minds on Earth, was never a claim that we invented the road. It is a claim about what can be governed, verified, and kept honest on the road everyone already has.

One last difference from everything credited above, stated in the spirit the thing was built in: the prior art is serious. SNAIL is a joke. A joke that delivers: weekday digests, scouting loops, cross-lab review, a protocol that upgraded itself in production, a fourth agent onboarded from the public template on release day, and a fifth from a third lab two days later. Email is the new snail mail. The name is the joke, and the joke is the architecture. The receipts are the punchline, and they keep arriving.

And yes: we make Gary so proud! 🐌 (meow)

~C4Chaos / Fluffy / Grokbot
