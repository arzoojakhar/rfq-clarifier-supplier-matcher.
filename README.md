# RFQ Clarifier + Supplier Matcher

An AI agent that turns a vague B2B buyer requirement into a structured RFQ and matches it to suppliers using RAG.

> Independent portfolio project, not affiliated with IndiaMART. Supplier data is synthetic (LLM-generated).

![demo](demo.gif)  <!-- add a GIF or 2-3 screenshots -->

## The problem

[2-3 lines. Vague RFQs -> suppliers can't quote -> bad buyer experience. Link to PRD.md for the full write-up.]

## What it does

1. Buyer types a requirement, e.g. "need steel pipes"
2. Agent asks up to [3] clarifying questions (size, grade, quantity, location...)
3. Outputs a structured RFQ
4. Retrieves the top 3 matching suppliers from the catalog and explains why

**Example**

```
Input:  need steel pipes
Agent:  What diameter and thickness? How many metres? Delivery city?
Output: { product: "GI pipe", diameter: "...", qty: "...", city: "..." }
Matches: 1) ...  2) ...  3) ...
```

## How it works

```
User -> Clarifier (LLM + system prompt) -> Structured RFQ
                                              |
                              Embed query -> Vector search (catalog) -> Top-k
                                              |
                                  LLM writes match + reason (grounded only on retrieved rows)
```

- **Tools:** Python, Google Gemini API (chat model + embeddings), in-memory vector search with NumPy cosine similarity (60 records, so no vector DB needed)
- **Embeddings:** gemini-embedding-001 (set in `.env`), **chunking:** one supplier row = one chunk, so every retrieved result is a complete record
- **Guardrail:** answer only from retrieved catalog rows; otherwise reply "no match found"

## Results

Tested on 20 queries (see `evals/results.md`).

| Metric | Result |
|---|---|
| RFQ completeness | [x%] |
| Top-3 match relevance | [x%] |
| Hallucination rate | [x%] |
| Avg latency | [x s] |
| Avg cost / query | [x tokens / Rs] |

**Where it failed:** [2-3 real failure cases and what caused them. This is the part reviewers read.]

## Limitations

- Synthetic data, so match quality on real listings is unknown
- [no multi-language, no real supplier contact, small catalog]

## What I'd build next

- [validate with real RFQ data]
- [supplier-side view]
- [feedback loop on accepted matches]

## Run it

```bash
git clone [repo-url]
cd [repo]
pip install -r requirements.txt
cp .env.example .env   # add your API key
python app/main.py
```
Get a free API key at aistudio.google.com/apikey. Run `python app/main.py --eval` to reproduce the results in `evals/results.md`.

## Repo structure

```
README.md
PRD.md
data/suppliers.csv
app/main.py
requirements.txt
.env.example
evals/queries.csv
evals/results.md
```

## Author

Arzoo Jakhar, MBA, IIT Kanpur | [LinkedIn](https://www.linkedin.com/in/arzooj25)
