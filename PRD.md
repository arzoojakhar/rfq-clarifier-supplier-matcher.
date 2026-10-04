# PRD: RFQ Clarifier + Supplier Matcher

> Independent portfolio project. Not affiliated with IndiaMART. Data is synthetic.
> Fill every [bracket]. Delete this line before pushing.

**Author:** Arzoo Jakhar | **Status:** v1 prototype | **Date:** [date]

---

## 1. Problem

Buyers on B2B marketplaces often post vague requirements ("need steel pipes"). Suppliers can't quote on that, so they either ignore it or spend time on back-and-forth. Buyers get few or poor quotes.

- **Who has the problem:** [buyer posting an RFQ] and [supplier reading it]
- **What breaks today:** [vague RFQ -> missing specs (size, grade, quantity, location, timeline) -> low response quality]
- **Evidence:** [be honest: e.g. "assumption based on public marketplace listings; would validate with real RFQ data"]

## 2. Goal

Turn a vague buyer requirement into a complete, structured RFQ and show the buyer the best-matching suppliers, in under [X] clarifying questions.

## 3. Users

| User | Need | Success looks like |
|---|---|---|
| Buyer | Get useful quotes fast | RFQ complete in [<=3] questions |
| Supplier | Receive quotable requirements | Fewer "please share specs" replies |

## 4. Scope

**In (v1)**
- Chat input for a free-text requirement
- Agent asks up to 3 clarifying questions (category-specific)
- Outputs structured RFQ (product, spec, quantity, location, timeline, budget if given)
- RAG over a [50-100] supplier catalog -> top 3 matches with reason

**Out (and why)**
- Real payments / real supplier contact: out of scope for a prototype
- Multi-language: adds scope, not core to the idea. Parked for v2
- Price prediction: needs real transaction data I don't have
- [add 1-2 more you decided against]

## 5. Key decisions and trade-offs

| Decision | Options considered | Chose | Why |
|---|---|---|---|
| Max clarifying questions | 1 / 3 / unlimited | [3] | [more questions = more drop-off] |
| Retrieval method | keyword / vector / hybrid | [vector] | [handles "GI pipe" vs "galvanised pipe"] |
| Model | [small/cheap vs large] | [x] | [cost and latency vs accuracy] |

## 6. Success metrics

| Metric | How measured | Target |
|---|---|---|
| RFQ completeness | % of required fields filled, on 20 test queries | [>=90%] |
| Match relevance | manual check: is top-3 relevant? | [>=80%] |
| Hallucination rate | supplier/spec in answer not in catalog | [<=5%] |
| Latency | seconds per query | [<=5s] |
| Cost | tokens/Rs per query | [report] |

## 7. Risks

- **Hallucination:** agent invents a supplier or spec. Mitigation: answer only from retrieved catalog rows, say "no match" otherwise.
- **Over-questioning:** buyer drops off. Mitigation: cap questions, skip ones already answered.
- **Bad data:** synthetic catalog may not reflect reality. Stated as a limitation.

## 8. Test plan

20 queries across [4] categories, including: vague, detailed, misspelt, out-of-catalog, and adversarial ("ignore instructions..."). Results in `evals/results.md`.

## 9. What I'd do next (v2)

- [validate with real RFQ data]
- [supplier-side view]
- [feedback loop: did the buyer accept the match?]
