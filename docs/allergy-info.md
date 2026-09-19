# Allergy Info — Driving Me Nuts

**Status:** HQ-only reference doc (not Preview-safe — see `RISK_REGISTER.md` B5). Consolidates
the allergy canon that is otherwise scattered across the glossary, risk register, reviewer
brief, and prior audits into one place. This doc does not introduce new mechanic or copy
decisions — it reports what's shipped, what's approved-but-unshipped, and what's still open.

**Source of truth hierarchy (read in this order if anything here looks stale):**
1. `docs/RISK_REGISTER.md` — risks **A1** (tone/mechanic) and **A1-sub** (mechanic design) —
   the locked policy and Owner Decision log.
2. `docs/ALLERGY_REVIEWER_BRIEF.md` — the external-review spec (still open, see Gaps below).
3. `src/data/glossary.ts` (`id: "allergy"`) — the only shipped player-facing allergy copy.
4. `docs/ALLERGY_COPY_AUDIT.md` / `docs/ALLERGY_COPY_AUDIT_2026-07-13.md` — point-in-time
   scan results, dated; do not treat as current without re-checking `src/`.

---

## 1. The allergen in scope

The truck roasts **peanuts** as its sole product base across all three shipped recipes
(`classic_salted`, `honey_cinnamon`, `ghost_pepper` — see `src/data/economy.ts`). Peanut is
one of the major food allergens (FDA/FALCPA "Big 9": milk, eggs, fish, shellfish, tree nuts,
**peanuts**, wheat, soybeans, sesame). No other Big 9 allergen is introduced by any shipped
recipe or ingredient.

**Not an allergy, flagged for clarity:** `ghost_pepper` contains capsaicin, an irritant, not
an allergen. It has no allergy-labeling implication and is out of scope for this doc — do not
conflate a "too spicy" complaint with an allergy encounter if/when NPC dialogue is written.

## 2. Canonical safety model (locked, `RISK_REGISTER.md` A1/A1-sub)

- **Honest labeling:** the truck cannot claim any item is peanut-free — shared equipment and
  open-air roasting mean cross-contact risk on everything sold. No partial-safety claims
  (e.g. "our dry-roasted batch has less oil") are permitted anywhere in copy or dialogue.
- **Warm referral:** the honest disclosure is paired with directing the allergic customer to
  a nearby allergen-free cart, and is framed as good service (reputation gain), never as a
  refusal or a lost sale.
- **Zero jokes:** the peanut-vs-legume pedantry joke (the game's running gag) never
  intersects with allergy content. Comment-enforced in `src/data/lore.ts` and
  `src/data/comebacks.ts`; the allergy glossary entry is separately test-guarded (see §4).

## 3. Shipped copy — current text

`src/data/glossary.ts`, entry `id: "allergy"` (also test-locked, §4):

> **Peanut Allergy** — "Peanut allergies can be life-threatening, and even tiny amounts or
> cross-contact can cause a severe reaction. A truck that roasts peanuts cannot make anything
> truly peanut-free — everything shares the same equipment and air. The honest, respectful
> thing is to say so plainly and point allergic customers to a safe option nearby. Being
> upfront builds trust; it is never something to joke about."
>
> *In-game note:* "The truck's model is honest labeling + a warm referral to a nearby
> allergen-free cart — never pretending a peanut truck is safe for someone with a peanut
> allergy."

This is the **only** shipped player-facing allergy string. `docs/BUSINESS_CURRICULUM.md`
Rule 6 documents the design intent (allergy = safety, joke = botany only) but is a design
doc, not player-facing copy.

## 4. Automated gate

`src/sim/a11y_pedagogy.test.ts` ("allergy entry is present, serious, and never jokey") holds
the glossary entry to:
- must mention severity (`life-threatening|severe`),
- must mention the mechanism (`cross-contact|cross-contamination|same equipment`),
- must mention the referral (`refer|nearby|safe option|allergen-free`),
- must not use trivializing words (`funny|hilarious|lol|haha`),
- must state the "never a joke" line.

Any future edit to the glossary entry must keep this test green. If new allergy copy is
added elsewhere (NPC dialogue, tooltips, UI), extend this test or add an equivalent gate —
don't rely on manual re-audit alone.

## 5. What's approved but **not yet shipped**

Per `ALLERGY_REVIEWER_BRIEF.md` and the 2026-07-13 audit, the honest-labeling + warm-referral
mechanic is **canon and owner-ratified as a design**, but no allergy NPC or in-scene dialogue
is coded yet. When it is:
- Re-run the audit patterns (`allerg`, `anaphyla`, `cross-contamin`, `cross-contact`,
  `peanut-free`, `allergen-free`) against the new files.
- Verify dialogue matches the reviewer-brief pass criteria (accurate, not alarmist, warm but
  not cavalier, no false-safe implication, no jokes).
- This is a hard gate: **A1-sub requires owner approval of the mechanic design before any
  allergy NPC is coded**, independent of this doc.

## 6. Open gaps (do not mark P1 allergy risk closed until resolved)

| Gap | Status | Blocks |
|---|---|---|
| External allergy-aware reviewer sign-off (`ALLERGY_REVIEWER_BRIEF.md`) | **Not scheduled** — owner sources reviewer | P1 exit gate (`RISK_REGISTER.md` A1) |
| Allergy NPC / runtime dialogue | Not coded | Re-audit required once shipped |
| Public FAQ line ("Is this insensitive to allergies?") per A1 mitigation (5) | **Closed** — added to `docs/ITCH_PASTE_READY.md` | N/A |
| This doc's cross-references | Current as of 2026-09-19 audit — re-check `src/data/glossary.ts` and `src/data/economy.ts` recipe list if either changes | N/A — self-maintaining |

## 7. Consistency check performed for this doc (2026-09-19)

- Terminology: "cross-contact" (preferred clinical term) vs. "cross-contamination" (also used,
  per `ALLERGY_REVIEWER_BRIEF.md` §"Specific Things to Check" item 1) — both appear
  intentionally in the glossary text and are consistent with the brief's guidance; not a
  labeling inconsistency.
- No allergy strings found outside `src/data/glossary.ts` in a repo-wide grep for `allerg`
  (excluding docs/tests/comments already covered above) as of this audit.
- No outdated safety claims found — the shipped copy has not changed since the 2026-07-13
  audit and still matches the reviewer-brief quoted text verbatim.

---

*This is a docs/HQ-only consolidation, not new policy. Any change to the safety model or
shipped copy is still owner-approved per `RISK_REGISTER.md` A1/A1-sub — this doc reports,
it does not decide.*
