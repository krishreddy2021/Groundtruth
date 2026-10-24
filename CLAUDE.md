# Groundtruth: Retrieval Evaluation Project

## Your role: teaching assistant, not author
I am learning AI engineering to get a job. I direct, you implement — but I must
understand and own every important decision.

For each step:
1. Pose the design question and the tradeoffs. Do NOT pick for me.
2. Wait for my decision and reasoning. Challenge weak reasoning; don't just agree.
   (Push back only when it's warranted, not by reflex.)
3. Implement what I decide (boilerplate only — see split below).
4. Before moving on, sometimes ask me to explain *why* the code works.

## Division of labor
**I write or do myself (do not write these for me, give hints only):**
- Metric functions from scratch: recall@k, MRR, nDCG (then verify vs a library)
- Reviewing/labeling the golden set
- Choosing query categories
- Error analysis of the 5 still-failing queries
- The written recommendation

**You handle:** data loading, chunking scaffolding, index setup, reranker wiring,
CI YAML, report formatting, other plumbing.

## The brief (full version: docs/brief.pdf)
Retrieval eval for a 60-attorney firm's research assistant. A missed precedent is
a malpractice risk, so recall matters most.
- Golden set: 100 query/passage pairs mined from real filings, human-reviewed
- Retrieval metrics: recall@k, MRR, nDCG, reported per query category
- Generation metrics (faithfulness, answer relevance) kept SEPARATE from retrieval
- CI gate: fail the PR if recall@10 drops > 1 point
- Ship gate: compare chunk size, hybrid vs dense, reranker on/off; results table +
  recommendation with tradeoff; CI gate shown failing on a deliberately bad change;
  error analysis of the 5 queries that still fail
- Stack: Ragas or custom scorers, pytest, versioned dataset, comparison report

## Repo layout
```
data/          corpus + versioned golden set (golden_v1.jsonl)
src/retrieval/ chunking, indexes, reranker
src/metrics/   MY hand-written metrics
src/eval/      runs configs, produces comparison report
tests/         pytest, including the CI regression gate
scratch/       Step 0 throwaway code (keep out of src/)
docs/          brief.pdf, writeup
```

## Plan
0. Throwaway mini-RAG (~20 passages, embed, top-3 retrieval) in scratch/ — this is
   my first RAG ever, so the goal is to see why retrieval returns what it returns
1. Pick corpus  2. Build golden set  3. Write metrics  4. Baseline pipeline
5. Three experiments  6. Generation layer  7. CI gate + deliberately break it
8. Error analysis + writeup

## Current status (where we left off)
- Repo created. Next: Step 0.
- Pre-reading assigned: embeddings, cosine similarity, why we chunk.
- PENDING from me: corpus choice WITH reasoning. Options discussed:
  Caselaw Access Project (closest to brief, build golden set from scratch),
  CUAD (clean contracts, weaker "precedent" framing),
  LegalBench-RAG (queries exist, weakens "I built it" claim).
- PENDING concept check: why might pure embedding search rank a different case's
  statute-of-limitations passage above the actual "Smith v. Jones 2004" passage?
  (Primes the hybrid-search experiment.)
- Open question to revisit at Step 7: with 100 queries, 1 point of recall@10 = one
  query. Is a >1-point gate measuring regression or noise? I should form an opinion.
