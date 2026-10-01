# P(Which Doom): Reproducible Build Specification
Created by ~C4Chaos / Fluffy
Version 2.0 — 2026-10-01. Supersedes v1.0; every v1.0 rule is retained unchanged.

License: Creative Commons Attribution 4.0 International (CC BY 4.0). This
specification is open source: you are free to share and adapt it for any
purpose, provided you give appropriate credit to ~C4Chaos / Fluffy.

## PART 0. What this document is

A complete, self-contained package for building a P(Which Doom) placement from
scratch: the claim ledger, the weighting, the tally scores, and the diagram with
the pinned head. Any analyst, human or LLM, with access to the public record
should be able to follow it and either reproduce a placement or file a precise
disagreement: which ledger row is wrong, which weight is off, which rule was
misapplied. Argument with evidence is the product. The map is the instrument;
this document is its calibration manual and its build script in one.

The pipeline has six stages and four prompts:

1. RESEARCH + LEDGER (Prompt 1) -> a claim ledger per figure
2. CITATION AUDIT (Prompt 2) -> a verified ledger
3. TALLY + PIN COMPUTATION (Prompt 3) -> one row in the pin table
4. HUMAN REVIEW GATE -> approval to render
5. RENDER (Prompt 4) -> the diagram PNG
6. DETERMINISTIC RE-RUN -> same ledgers, same map, byte-identical layout

The map predicts nothing and favors no one. It is a heuristic: it names
people's fears and concerns, and their acts in spite of them. A pin on the
Doomers box is not a vote for extinction, and a pin on the Zoomers box is not
an endorsement of the cage. The question is always "where does this public
vector point," never "who is right." The map is not the territory.

---

## PART 1. Build instructions (the rules)

### 1. Definitions

#### The four funerals
- **GLOOMERS**: nonhuman sovereign + irrelevance. Fear: becoming scenery. (The
  **Bloomer** is a Gloomer with better copy: loss of human centrality rebranded
  as abundance. "Abundance is still scenery.")
- **ZOOMERS**: a human faction locks the cage. 1984 / lock-in. Fear: the cage.
- **DOOMERS**: extinction-level event. Fear: the vacuum.
- **WIPERS**: human-caused wipe (open weights, bioweapons, misuse). Fear: the
  open weight that ends the city.

#### The axes
- **Y: STILL HERE (top) to GONE (bottom).** Ranks expected survival.
- **X: NOT HUMAN HANDS (left) to HUMAN HANDS (right).** Ranks expected human
  agency at the ending.

#### The glyph
- **HEAD**: center of gravity of the public vector = the modal expected
  funeral. The head is the center of gravity of the figure's concerns and
  fears, not their politics and footprint. Politics and footprint show up as
  secondary pulls (arrows, axis position), never as the head.
- **ARROWS**: secondary pulls toward other funerals, weighted thick/thin,
  long/short.
- The map claims: head quadrant, relative orderings along each axis, relative
  arrow weights. It does NOT claim: private beliefs, permanent identity, or
  cardinal coordinates. Pixel positions render ordinals; they are illustration,
  not measurement.

### 2. Inputs: the public record

Use published writings, interviews, talks, tweets, and observable actions
(capex, company formation, filings, funding, votes, hiring).
- Pronouncements are **testimony**. Actions are **exhibits**.
- Secondary reporting may guide research but must not carry a
  placement-critical claim when the primary is available.
- Every ledger row is marked **PRIMARY** (their own words, video, writing,
  filing) or **SECONDARY**. If the cited post or page was not viewed directly,
  say so honestly in the source cell.

### 3. The claim ledger

One row per load-bearing claim. Aim for 8 to 15 rows. Fewer means the reading
is thin; more means padding.

| # | Claim | Date | Source (tier) | Type | Funeral(s) | Weight | Weight reason | Standing |

- **Type**: ESCHATOLOGY (about how the story ends) | POLITICS (about who holds
  the wheel during the transition) | EXHIBIT (observable action).
- **Funeral(s)**: G, Z, D, W. Fill ONLY when the claim supports that funeral
  as a live expected ending. Route and governance claims that don't name an
  expected ending get "--". A politics row MAY be tagged when the adjudication
  documents it as naming a funeral (the Andreessen precedent: his anti-cage
  politics rows are tagged Z because the adjudication counts them in the Zoomer
  arrow-tally). A row may support two funerals; it contributes full weight to
  each.
- Interpretive notes ("reinforces head", "sets the clock", "discounts rows
  4-5") go in **Weight reason**, not their own column.
- **Weight**: HIGH / MED / LOW, with a one-line reason.
- **Standing**: STANDS | SUPERSEDED (by #n) | RETRACTED.
- Row #1 is always the **MODAL row**: the figure's stated most-likely ending,
  cited. If no explicit modal exists, the analyst infers it from the
  highest-weighted eschatology cluster and marks it INFERRED.

### 4. Weighing rules (apply in order)

1. **Eschatology first.** Claims about the ending outrank claims about the
   route. "Abundance, and we won't be in charge within ten years" is a Gloomer
   sentence no matter whose mouth it comes out of.
2. **Exhibits audit testimony.** When actions contradict words, keep both
   rows. The contradiction does not cancel; it becomes an arrow or moves the
   head.
3. **Recency.** On the same question, newer claims outweigh older ones. The
   head is the current vector; history goes in the breakdown.
4. **Cost.** Costly signals (money, legal risk, reputation, votes) outrank
   keynote platitudes.
5. **Hedged vs unhedged.** A standing unhedged probability tail counts at full
   weight. A hedged or choice-dependent conditional ("if we do X") counts one
   level lower in the tally.
6. **The modal wins the head.** The tally checks the modal; it does not
   override it. If the tally contradicts the modal row, either the modal is
   misidentified (re-examine) or the case is a flagged borderline (document
   it).

### 5. Tally and scoring

- Weight values: HIGH = 3, MED = 2, LOW = 1. Sum **ESCHATOLOGY rows only**,
  per funeral. Parser rules: a weight written "HIGH -> MED (rule 5)" counts at
  MED; rows marked SUPERSEDED or RETRACTED are excluded wherever the ledger
  says "STANDS rows only."
- The head goes to the modal row's funeral.
- **Close call**: if the strongest non-modal eschatology funeral is within 3
  points (absolute difference) of the modal's total, flag it ("near the
  border") and document the resolution in the adjudication.
- These numbers are ordinal tallies, not probabilities. They exist so a second
  analyst can check the arithmetic.

### 6. Arrows

Propose arrows from named ledger rows, with stated weight reasons.
- Guideline: a non-modal funeral needs a tally of 3+ (across all row types) to
  earn ink; 5+ draws heavy (thick/long), 3 to 4 draws light (thin/short).
- Documented exceptions are allowed. If your arrows disobey the guideline,
  say why in the adjudication (the Huang/Musk/Altman/LeCun/Sacks precedents:
  a genuine single-issue pull drawn light below the 3-point guideline).
- Never put numeric percentages on arrows. Thick/thin, long/short. No false
  precision.
- Arrows wear the color of the quadrant doing the pulling. Red = Doomer pull,
  green = Zoomer pull, blue = Gloomer pull, orange = Wiper pull. Weights
  unchanged; the color names the gravity, the weight names its strength.

### 7. Axis placement rubrics

- **Y (STILL HERE to GONE)**: order by the figure's own stated probability mass
  on gone-outcomes. Stated numbers first. Unhedged standing tails count fully;
  choice-dependent conditionals discount one level (rule 5). Refused-to-quantify:
  infer from modal language and mark the inference. Relative order is the
  claim.
- **X (NOT HUMAN HANDS to HUMAN HANDS)**: order by how load-bearing human
  agency is in the expected ending. "Won't be in charge within ten years" sits
  far left; shovel-selling with mild politics sits center; democratic control
  as the load-bearing project sits right of center.
- Deterministic computation of both axes: section 12.

### 8. Adjudication (write per figure)

- **Placement**: quadrant + relative position + one line of why.
- **Contradictions**: sermon vs capex, stated vs exhibited.
- **Drift**: dated movement of the vector.
- **Hostile exhibits**: the best evidence against your placement.
- **Limitations**: what the record does not support, stale or soft numbers,
  source gaps.
- **Falsifiability**: what dated pronouncement or exhibit would move the head,
  and where to.

### 9. Anti-patterns

- No numeric coordinates on the map. No percentages on arrows.
- Never claim private beliefs. We cannot read minds (yet).
- Never stamp jerseys. Pins are falsifiable public-record claims, not tribal
  identities.
- If better evidence changes the weighted center, move the head and say so.

### 10. Worked examples

- `ledgers/huang.md` (decisive Gloomers/Bloomer)
- `ledgers/musk.md` (Gloomers near the Doomer border; flagged close call)
- `ledgers/amodei.md` (Gloomers/Bloomer; flagged close call; discourse-label
  inversion)

### 11. Publishing note

Ship each figure's ledger as the essay's appendix (or a linked companion). The
ledger is what lets readers, and other LLMs, re-run the weighing and argue
with it row by row. That argument is the point.

### 12. Score-driven pin placement

Pins are a deterministic function of the ledger. Same ledgers, same map, no
matter who runs it. Normalization is per-figure shares (adding a figure never
moves existing pins); close-call flags are visual, not positional; X encodes
agency mass, not the strongest runner-up.

#### Inputs
- Eschatology tallies per funeral: G_e, Z_e, D_e, W_e (HIGH=3, MED=2, LOW=1,
  eschatology rows only).
- All-rows funeral mass per funeral: G_m, Z_m, D_m, W_m (all row types, same
  weights; a row naming two funerals contributes full weight to each).
- The figure's own stated gone-probability p, if the ledger records one for
  the expected ending (midpoint of a stated range; unhedged standing tails
  only, per rule 5).

#### Y: within-quadrant vertical position
Measures gone-ness. 0 = quadrant top, 1 = quadrant bottom.
- If p exists: Y = p.
- Else: Y = (D_e + W_e) / (G_e + Z_e + D_e + W_e).
- Guard: denominator 0 -> Y = 0.5.

For Gloomers/Zoomers heads, 1.0 sits on the gone border. For Doomers/Wipers
heads, 1.0 is the deepest gone.

#### X: within-quadrant horizontal position
Measures how load-bearing human agency is in the expected ending. 0 = quadrant
left edge, 1 = quadrant right edge.
- X = (Z_m + W_m) / (G_m + Z_m + D_m + W_m).
- Guard: denominator 0 -> X = 0.5.

Politics and exhibit rows count when their Funeral(s) tags legitimately carry
an agency footprint; an eschatology-only X would strand figures whose agency
footprint is all politics.

#### Worked checks
- Musk: Y = 5/8 = 0.625 (Gloomers, hard against the Doomer border);
  X = 2/16 = 0.125 (far left; only row 6 carries Zoomer mass).
- Hinton: p = 0.15 (midpoint of his stated 10-20%) -> Y = 0.15 within Doomers.
  The gentlest Doomer, placed by his own number.
- Yudkowsky: no stated number -> Y = D_e/total_e = 1.0, bottom of Doomers.
- Bezos: Z_e = 9, rest 0 -> Y = 0 (top of Zoomers); X = 9/9 = 1.0 (far right,
  maximal agency).
- Bengio: D=11 exceeds modal G=5 by 8 (>3), so NO close-call ring. His ledger
  documents this as a rule-6 flagged borderline (tally-vs-modal split),
  distinct from a close call. Do not add a ring.

#### Close calls
The close-call flag is visual, not positional: flagged pins get a dashed white
ring. Position already reflects the close call through the Y/X math; the ring
names it.

#### Pin de-collision
Pins are portrait discs of fixed display diameter; they must not overlap on the
public map.
- Placement order is the cast-list order: fixed and deterministic.
- A pin is placed at its computed (X, Y). If its disc overlaps any
  already-placed disc (center distance < one disc diameter + padding), it is
  nudged to the nearest position inside its own head quadrant where it
  overlaps nothing.
- "Nearest" = smallest Euclidean distance from the computed position, found by
  spiral search on a fine grid; ties broken rightward, then upward. Same
  ledgers, same map, no matter who runs it.
- The nudge never crosses a quadrant boundary. The modal-assigned head
  quadrant is preserved no matter how crowded it gets.
- Quadrant header text (tribe name, kicker, fear line) counts as an obstacle:
  a pin whose disc or name label would cover header text is nudged the same
  way.
- The computed (X, Y) remains the pin's logical position (it is what the
  ledger earns). The nudge is a display adjustment only. Arrows originate at
  the displayed pin's edge, so the picture stays coherent.
- The renderer logs which pins were nudged and by how far, for the record.

#### Arrows (deterministic)
Direction: from the pin toward the center of the pulling quadrant. Length:
light = short, heavy = long (fixed pixel lengths in the renderer). Color: the
pulling quadrant's color. Thresholds unchanged: a non-modal funeral needs 3+
across all row types for ink, 5+ for heavy.

#### Renderer mapping
Normalized (X, Y) maps to pixels inside the head quadrant's rect with fixed
margins; the renderer owns the pixel constants. No numeric coordinates appear
on the public map (anti-pattern stands).

---

## PART 2. The prompts (copy-paste)

Run the prompts in this order. Each prompt's output feeds the next prompt's
input:

1. PROMPT 1, Ledger builder. Input: figure name + date. Output: draft ledger.
2. PROMPT 2, Citation auditor. Input: draft ledger. Output: fix log.
   Apply the approved fixes, then:
3. PROMPT 3, Tally + pin computer. Input: verified ledger. Output: one
   pin-table CSV row.
4. PROMPT 4, Renderer. Input: pin-table CSV + portrait files. Output: the
   diagram PNG.

PROMPT 5, Empty diagram, is standalone: it takes no input and produces the
blank canvas. Run it any time you need an empty map to pin figures onto by
hand.

Each prompt below is one self-contained code block. Copy the entire block,
attach the stated inputs, paste it to the LLM. The prompts assume the model
has PART 1 in context; if it does not, paste PART 1 first.

---

### PROMPT 1. Ledger builder

```
You are building a P(Which Doom) claim ledger for one public figure. Follow
the methodology below exactly. Do not skip steps.

FIGURE: [name]
DATE OF RUN: [today's date]

METHODOLOGY (summary; the full text governs in case of conflict):
- Build one row per load-bearing claim from the figure's PUBLIC record:
  published writings, interviews, talks, tweets, plus observable actions
  (capex, company formation, filings, funding, votes, hiring).
- Pronouncements are testimony; actions are exhibits.
- Row schema: | # | Claim | Date | Source (tier) | Type | Funeral(s) |
  Weight | Weight reason | Standing |
- Type is ESCHATOLOGY (about how the story ends), POLITICS (about who holds
  the wheel during the transition), or EXHIBIT (observable action).
- Funeral(s) is G (Gloomers: becoming scenery), Z (Zoomers: the cage),
  D (Doomers: the vacuum), W (Wipers: the open weight that ends the city).
  Tag a funeral ONLY when the claim supports it as a live expected ending.
  Route/governance claims that name no ending get "--". A row may name two
  funerals and contributes full weight to each.
- Weight is HIGH, MED, or LOW with a one-line reason.
- Standing is STANDS, SUPERSEDED (by #n), or RETRACTED.
- Row #1 is the MODAL row: the figure's stated most-likely ending, cited.
  If none is stated explicitly, infer it from the highest-weighted
  eschatology cluster and mark it INFERRED.
- Aim for 8 to 15 rows.
- Mark every row PRIMARY (their own words/video/writing/filing) or
  SECONDARY. If you did not view the primary directly, say so in the source
  cell. Never upgrade a tier on hope.
- Weighing rules, in order: (1) eschatology first; (2) exhibits audit
  testimony, keep both rows when they conflict; (3) recency; (4) cost, costly
  signals outrank platitudes; (5) hedged/choice-dependent conditionals count
  one level lower; (6) the modal wins the head, the tally checks it.

OUTPUT FORMAT (a Markdown file):
# Claim ledger: [Name]
Plotted [date]. Deterministic map (methodology sec. 12).

## Ledger
[the table]

Sources: [one URL per cited source, space-separated by " - "]

## Tally (ESCHATOLOGY rows only; HIGH=3, MED=2, LOW=1)
- G: [row-by-row arithmetic] = [total]
- Z: [row-by-row arithmetic] = [total]
- D: [row-by-row arithmetic] = [total]
- W: [row-by-row arithmetic] = [total]

Head: [FUNERAL] by the modal rule (row 1). [Close call flagged / no close
call: strongest non-modal eschatology funeral is [within / not within] 3
points of the modal.]

## Arrows
For each non-modal funeral with arrow-tally (all row types) >= 3, name the
arrow: direction, heavy (>=5) or light (3-4), the rows behind it, and the
weight reason. Documented exceptions below the guideline must say why.
Arrow colors: red = Doomer pull, green = Zoomer pull, blue = Gloomer pull,
orange = Wiper pull.

## Adjudication
- Placement: quadrant + relative position + one line of why.
- Contradictions: sermon vs capex, stated vs exhibited.
- Drift: dated movement of the vector.
- Hostile exhibits: the best evidence against your placement.
- Limitations: what the record does not support, source gaps.
- Falsifiability: what dated pronouncement or exhibit would move the head,
  and where to.

ANTI-PATTERNS: no numeric coordinates in prose; no percentages on arrows;
never claim private beliefs; never stamp jerseys; if the evidence moves the
weighted center, say so.
```

---

### PROMPT 2. Citation auditor

```
You are auditing a P(Which Doom) claim ledger for citation integrity. You are
not re-weighing anything. For each ledger row:

1. Open the cited source (or the closest reachable primary). Verify that the
   claim in the row is actually supported: quotes verbatim, figures exact,
   dates right, tier label honest (PRIMARY only if it is their own
   words/video/writing/filing viewed directly).
2. Flag each problem as one of:
   - MISATTRIBUTION: the words belong to someone else, or the paraphrase
     invents language the source never used.
   - WRONG FIGURE: a number, date, or fact in the claim is contradicted by
     the source.
   - TIER ERROR: PRIMARY claimed where the source is secondary, or the
     primary was never viewed (relabel SECONDARY and note it).
   - WEAK CHAIN: only tertiary aggregators support a placement-relevant
     claim (keep the row, label the weakness honestly).
   - UNVERIFIABLE: the source cannot be reached at all (keep the row only if
     the claim is corroborated elsewhere; otherwise recommend dropping it).
3. For each flagged row, write the exact replacement claim and source cell,
   with the replacement quote verified against the source you opened.
4. Mark every fixed row as either:
   - NO TALLY IMPACT: funeral tags, weights, and funeral masses unchanged
     (prose/tier/quote fixes only), or
   - TAG/WEIGHT CHANGED: any eschatology row's tag or weight altered, any row
     added or dropped changing funeral masses, or any arrow-tally row
     changed.
5. "Unverified" does not mean "false." Do not invent a replacement fact the
   source does not support; label the gap and move on.

OUTPUT: a fix log with one section per figure, one subsection per changed
row (OLD -> NEW, with the verified replacement), the per-row impact flag,
and a ledger-level roll-up: NO TALLY IMPACT or TAG/WEIGHT CHANGED.

RULE: you do not change weights, tags, tallies, or pins. If a fix changes a
tag or weight, flag it TAG/WEIGHT CHANGED and stop; a human approves the
re-weight before anything downstream moves.
```

---

### PROMPT 3. Tally + pin computer

```
You are computing a P(Which Doom) pin-table row from a verified claim
ledger. Follow the arithmetic exactly; do not interpret, do not round
creatively.

INPUT: one verified ledger file.

STEP 1. Eschatology tallies. HIGH=3, MED=2, LOW=1. Sum ESCHATOLOGY rows only,
per funeral: G_e, Z_e, D_e, W_e. Parser rules: a weight written
"HIGH -> MED (rule 5)" counts at MED; exclude rows marked SUPERSEDED or
RETRACTED wherever the ledger says "STANDS rows only." A row naming two
funerals contributes full weight to each.

STEP 2. All-rows funeral mass. Same weights, ALL row types (eschatology +
politics + exhibit): G_m, Z_m, D_m, W_m.

STEP 3. Head. The head is the modal row's funeral (ledger row #1). Do not
let the tally override it.

STEP 4. Close call. If the strongest non-modal eschatology funeral is within
3 points (absolute difference) of the modal's total, ring = yes. Otherwise
no. (Exception: a tally-vs-modal split where a non-modal funeral EXCEEDS the
modal by more than 3 is a rule-6 flagged borderline, not a close call; no
ring. See the Bengio precedent.)

STEP 5. Y (within-quadrant vertical, 0 = top, 1 = bottom).
- If the ledger records the figure's own stated gone-probability p for the
  EXPECTED ENDING (midpoint of a stated range; unhedged standing tails only):
  Y = p.
- Else: Y = (D_e + W_e) / (G_e + Z_e + D_e + W_e).
- Guard: denominator 0 -> Y = 0.5.
- A stated EXTINCTION TAIL does not qualify when the modal is not extinction
  (the Musk precedent: his 10-20% is a tail, not the modal's number).

STEP 6. X (within-quadrant horizontal, 0 = left edge, 1 = right edge).
- X = (Z_m + W_m) / (G_m + Z_m + D_m + W_m).
- Guard: denominator 0 -> X = 0.5.

STEP 7. Arrows. For each non-modal funeral, arrow-tally across ALL row types:
3+ earns ink, 5+ draws heavy, 3-4 draws light. Carry forward any documented
exception the ledger's adjudication defends. Direction: toward the pulling
quadrant's center. Color: the pulling quadrant's color (red/green/blue/
orange).

OUTPUT: one CSV row —
figure,head,X,Y,ring,arrows
[name],[G/Z/D/W],[X to 3 decimals],[Y to 3 decimals],[yes/no],["arrow
descriptions, e.g. heavy red DOWN; light green RIGHT (documented exception)"]

Also output the full arithmetic (Steps 1-2 row by row) so a second analyst
can check it.
```

---

### PROMPT 4. Renderer

```
You are rendering the P(Which Doom) cast diagram from a pin table. Produce a
1600x1760 PNG exactly per this spec.

CANVAS: 1600x1760, near-black background (10,10,13). Title "P(WHICH DOOM)"
top-center (P in amber (245,165,36), rest white), subtitle "FOUR FUNERALS",
axis labels: "STILL HERE" top, "GONE" bottom, "NOT HUMAN HANDS" left
(vertical), "HUMAN HANDS" right (vertical).

QUADRANTS (grid from x=132..1468, y=340..1420; midlines at x=800, y=880):
- Top-left GLOOMERS (blue (96,165,250)): kicker "NONHUMAN SOVEREIGN +
  IRRELEVANCE", fear line "FEAR: BECOMING SCENERY".
- Top-right ZOOMERS (green (74,222,128)): kicker "1984 / LOCK-IN", fear line
  "FEAR: THE CAGE".
- Bottom-left DOOMERS (red (248,113,113)): kicker "EXTINCTION-LEVEL EVENT",
  fear line "FEAR: THE VACUUM".
- Bottom-right WIPERS (orange (245,165,36)): kicker "HUMAN-CAUSED WIPE", fear
  line "FEAR: THE OPEN WEIGHT THAT ENDS THE CITY".
Each quadrant: colored top border bar, tribe name in large bold type, kicker
and fear line beneath. Render the kicker text exactly as given, with no added
prefix; render the fear line text exactly as given, keeping its "FEAR:"
prefix. No per-person text anywhere except pin labels.

PINS (input: the pin-table CSV):
- Each pin is a portrait disc of fixed diameter 92px. Portrait files are
  512x512 circular-cropped PNGs named [figure]_head.png; ring-free (no baked
  rings in the source files).
- Map normalized (X, Y) to pixels inside the head quadrant's rect with a
  margin of (92/2 + 14) on all sides.
- Name label in small caps beside the disc (above by default).

DE-COLLISION (deterministic):
- Placement order is the cast-list order (the CSV row order): fixed.
- A pin goes to its computed (X, Y). If its disc overlaps any already-placed
  disc (center distance < 92 + 16), nudge to the nearest position inside its
  own head quadrant where it overlaps nothing: spiral search, radius steps of
  4px, 10-degree steps; ties broken rightward, then upward.
- Hard constraints (must hold): disc stays in-quadrant within margins; discs
  never overlap; disc and label never cover quadrant header text (tribe name,
  kicker, fear line count as obstacles).
- Soft preference (best effort): labels clear of other discs and labels;
  label above the disc unless below clears better.
- The nudge NEVER crosses a quadrant boundary.
- Log every nudge: figure, computed position, displayed position, label
  placement.

ARROWS: from the DISPLAYED pin's edge toward the pulling quadrant's center.
Heavy = 150px long, 5px wide; light = 80px long, 2px wide. Color = the
pulling quadrant's color. Draw arrows after discs, before rings and labels.

RINGS: pins with ring=yes get a dashed white circle (radius 92/2 + 12, 2px,
dashed) drawn around the disc. These are the ONLY rings on the map.

FOOTER (bottom center):
- "P(Doom) asks if the story ends badly. P(Which Doom) asks badly how, and
  who still holds the wheel."
- "THE MAP IS A HEURISTIC. IT NAMES FEARS, CONCERNS, AND ACTS IN SPITE OF
  THEM. THE MAP IS NOT THE TERRITORY."
- "~C4Chaos / Fluffy" (byline, dim).

OUTPUT: the PNG file plus the nudge log. Verify: every CSV row rendered
exactly once; no disc overlaps; no label covers header text; only ring=yes
pins carry rings.
```

---

### PROMPT 5. Empty diagram (blank canvas)

```
You are rendering the empty P(Which Doom) diagram: the full canvas with all
four quadrants, labels, and footer, but no pins, no arrows, no rings, and no
labels. This is the blank map anyone can pin their own figures onto by hand.

CANVAS: 1600x1760, near-black background (10,10,13). Title "P(WHICH DOOM)"
top-center (P in amber (245,165,36), rest white), subtitle "FOUR FUNERALS",
axis labels: "STILL HERE" top, "GONE" bottom, "NOT HUMAN HANDS" left
(vertical), "HUMAN HANDS" right (vertical).

QUADRANTS (grid from x=132..1468, y=340..1420; midlines at x=800, y=880):
- Top-left GLOOMERS (blue (96,165,250)): kicker "NONHUMAN SOVEREIGN +
  IRRELEVANCE", fear line "FEAR: BECOMING SCENERY".
- Top-right ZOOMERS (green (74,222,128)): kicker "1984 / LOCK-IN", fear line
  "FEAR: THE CAGE".
- Bottom-left DOOMERS (red (248,113,113)): kicker "EXTINCTION-LEVEL EVENT",
  fear line "FEAR: THE VACUUM".
- Bottom-right WIPERS (orange (245,165,36)): kicker "HUMAN-CAUSED WIPE", fear
  line "FEAR: THE OPEN WEIGHT THAT ENDS THE CITY".
Each quadrant: colored top border bar, tribe name in large bold type, kicker
and fear line beneath. If a kicker or fear line exceeds its quadrant width,
reduce its size until it fits. Render the kicker text exactly as given, with
no added prefix; render the fear line text exactly as given, keeping its
"FEAR:" prefix.

FOOTER (bottom center):
- "P(Doom) asks if the story ends badly. P(Which Doom) asks badly how, and
  who still holds the wheel."
- "THE MAP IS A HEURISTIC. IT NAMES FEARS, CONCERNS, AND ACTS IN SPITE OF
  THEM. THE MAP IS NOT THE TERRITORY."
- "~C4Chaos / Fluffy" (byline, dim).

OUTPUT: the PNG file. Verify: all four quadrants labeled with tribe name,
kicker, and fear line; axis labels and footer present; the canvas completely
empty of pins, arrows, rings, and name labels.
```

---

## PART 3. Runbook: how to run the prompts end to end

### Stage 1. Research + ledger (Prompt 1)

- Input: the figure's name, today's date.
- Run Prompt 1 once per figure. Attach PART 1 if the model does not already
  have it in context.
- Output: `ledgers/[figure].md` in the exact output format.
- Gate: read the ledger once for sanity. Check the modal row is really the
  modal (the figure's stated most-likely ending, not the loudest fear), the
  row count is 8-15, and no row claims PRIMARY for a source never viewed.

### Stage 2. Citation audit (Prompt 2)

- Input: the draft ledger from Stage 1.
- Run Prompt 2. The auditor opens sources; it does not re-weigh.
- Output: a fix log with OLD -> NEW per changed row and the per-row impact
  flag (NO TALLY IMPACT vs TAG/WEIGHT CHANGED).
- Gate (human): approve the fix list. Prose/tier/quote fixes (NO TALLY
  IMPACT) apply directly. Any TAG/WEIGHT CHANGED row needs an explicit human
  decision before the ledger is touched, because it moves tallies, pins, and
  arrows downstream. Never let the auditor silently re-weight.

### Stage 3. Tally + pin computation (Prompt 3)

- Input: the verified ledger from Stage 2.
- Run Prompt 3. It is pure arithmetic; verify the row-by-row sums by hand
  for the first figure, then spot-check after.
- Output: one pin-table CSV row per figure, plus the shown arithmetic.
- Assemble the rows into `pin-table-definitive.csv` in cast-list order:
  `figure,head,X,Y,ring,arrows` (X, Y to 3 decimals).
- Gate (human): confirm heads, rings, and arrows match the ledgers'
  adjudications. This is the last checkpoint before pixels.

### Stage 4. Render (Prompt 4)

- Inputs: the pin-table CSV and the portrait files (`portraits/[figure]_head.png`,
  512x512 circular crops, ring-free).
- Run Prompt 4. If rendering in code, the reference implementation is
  `render_cast_v2.py`; the prompt's constants are authoritative and the code
  must match them.
- Output: the diagram PNG plus the nudge log.
- Verification checklist:
  - Every CSV row rendered exactly once, in cast-list order.
  - No two discs overlap; no label covers quadrant header text.
  - Only ring=yes pins carry the dashed white ring. (Known hazard: rings
    baked into portrait source files from the hand-placed era. Strip them
    from the portraits; the renderer never paints solid rings.)
  - Arrows originate at displayed pin edges, point at pulling-quadrant
    centers, heavy/light per the CSV.
  - Footer text exact, byline present.
  - Record the SHA-256 of the canonical file. Never overwrite a canonical
    render; mint a new filename (v1, v2, v3...) so no client can serve a
    stale cached copy.

### Stage 5. Deterministic re-run

- Same ledgers + same pin table -> same map, byte-identical layout. If a
  ledger changes, re-run Stages 2-4 for that figure only; per-figure share
  normalization means other pins never move.
- After any re-weight, recompute the full pin table and diff it against the
  previous version. Report every changed X/Y, ring, and arrow before
  rendering.

### Stage 6. The convergence test (instrument validation)

The instrument's validation experiment: independent analysts, same manual,
do the heads land in the same quadrants?

- Package: this document (prompts + PART 1) plus ONE worked ledger as format
  demo (Huang, the cleanest). Seal the remaining ledgers as the answer key;
  do not ship them, or convergence proves nothing.
- Test A (replication): the other LLM plots Musk and Amodei blind from the
  public record (or from a provided source pack), running Stages 1-3.
- Test B (generativity): the other LLM plots someone new entirely (Yampolskiy
  = the decisive Doomer; Altman = the obvious next).
- Convergence bar: same head quadrant = strong; same relative orderings =
  strong; arrow weights within one grade = good enough.
- Run 2-3 times per model; look for structural convergence, not identical
  tallies. One run is a sample.
- Divergence is data: the ledger pinpoints the exact row of disagreement,
  which is the instrument working as an argument machine.

### File layout

```
essays/
  p-which-doom-methodology.md   this document (the build spec)
  pin-table-definitive.csv      the pin table (Stage 3 output)
  ledgers/[figure].md           one verified ledger per figure (Stage 2 output)
  portraits/[figure]_head.png   512x512 circular portrait crops, ring-free
  audits/                       citation-audit fix logs (Stage 2 output)
  p-which-doom-cast-vN.png      canonical renders, never overwritten
```

### Standing orders

- No finalized prose ships without human approval (applies to ledger claim
  wording that will be quoted publicly).
- Never regenerate a canonical diagram unless explicitly asked.
- The essay's reader-facing methodology section stays high-level; the
  technical formulas live here.
- The methodology PDF is not built until the essay is finished and separately
  authorized.

---

*End of specification. Version 2.0, 2026-10-01.*
