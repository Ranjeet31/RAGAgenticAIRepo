# ADR-002: Prompting Strategy for Job Description Extraction

**Status:** Accepted for Evaluation
**Date:** 2026-08-29
**Decision:** Benchmark four prompting strategies and select the best strategy based on accuracy, cost, latency, and reliability.

---

## Context

We need to extract structured information from unstructured job descriptions using an LLM.

The required output is:

```json
{
  "id": null,
  "company": null,
  "role": null,
  "years_experience_required": null,
  "notes": null
}
```

The solution must provide accurate extraction while minimizing hallucination, latency, token consumption, and API cost.

We will evaluate four prompting strategies using the same dataset and model (`gpt-4o-mini`).

---

## Decision

Evaluate the following strategies:

### 1. Zero-shot

Provide instructions and the job snippet without examples.

**Pros:** Simple, low cost, low latency, easy to maintain.
**Cons:** May have inconsistent interpretation of ambiguous data.

### 2. Few-shot

Provide instructions plus representative input/output examples.

**Pros:** Improves understanding of expected behavior and edge cases.
**Cons:** Higher token usage, cost, and prompt maintenance.

### 3. Structured / Role-based

Provide an expert persona, explicit schema, field definitions, and extraction rules.

**Pros:** Strong schema adherence, consistent extraction, clear business rules.
**Cons:** Larger prompt and higher token usage than zero-shot.

### 4. Reasoning-guided / CoT-style

Ask the model to internally reason through the extraction before returning the final JSON.

**Pros:** May improve handling of ambiguous or multi-step extraction.
**Cons:** Potentially higher latency and token consumption.

The model should **not expose its internal chain of thought**; only the final structured result is returned.

---

## Evaluation

For each strategy:

```text
10 job snippets × 4 strategies = 40 extraction calls
```

Each result captures:

* Accuracy
* Parse success
* LLM judge score
* Input tokens
* Output tokens
* Total tokens
* Cost
* Latency
* Errors

### Accuracy

Three fields are evaluated:

```text
company
role
years_experience_required
```

Score:

```text
0 = none correct
1 = one correct
2 = two correct
3 = all three correct
```

### LLM Judge

`gpt-4o` evaluates the extracted result against the original snippet and gold answer.

```text
4 = all three fields correct
3 = two correct, no fabrication
2 = one correct or fabricated information
1 = none correct or unparsable
```

---

## Decision Matrix

| Strategy         | Accuracy       | Cost   | Latency | Complexity |
| ---------------- | -------------- | ------ | ------- | ---------- |
| Zero-shot        | Baseline       | Low    | Low     | Low        |
| Few-shot         | Expected ↑     | Medium | Medium  | Medium     |
| Structured       | Expected ↑↑    | Medium | Medium  | Medium     |
| Reasoning-guided | Potentially ↑↑ | High   | High    | High       |

These are initial hypotheses and must be validated through benchmark results.

---

## Selection Criteria

The production strategy will be selected based on:

1. **Extraction accuracy**
2. **Hallucination/fabrication rate**
3. **Parse success**
4. **Latency**
5. **Token consumption**
6. **Cost**
7. **Prompt maintainability**

Accuracy will be prioritized, but a small accuracy improvement will not justify significantly higher cost or latency unless the business requirement demands it.

---

## Consequences

### Positive

* Provides measurable comparison of prompting techniques.
* Establishes a repeatable evaluation framework.
* Separates accuracy from operational metrics.
* Enables future models/prompts to be benchmarked using the same dataset.

### Negative

* Requires maintaining a trusted gold dataset.
* LLM-as-a-Judge introduces additional API cost.
* A small benchmark dataset may not represent all production scenarios.
* Prompt performance may change with different job-description formats.

---

## Initial Recommendation

Use **zero-shot as the baseline**.

Use **structured / role-based prompting as the leading production candidate**, subject to benchmark validation.

Few-shot and reasoning-guided prompting should only be selected if their accuracy improvement justifies their additional token usage, latency, and cost.

**Final production decision:** Pending benchmark results.

Benchmark results showed that Few-shot prompting achieved the highest deterministic accuracy (2.9/3) and tied Structured prompting for the highest LLM judge score (3.9/4). However, Structured prompting achieved the lowest p50 latency (2.209s) while maintaining 2.8/3 accuracy, a 3.9/4 judge score, and a 100% parse rate. Zero-shot performed surprisingly well at 2.8/3 accuracy with a 3.8/4 judge score, demonstrating that a simple prompt can be highly effective for well-defined extraction tasks. CoT-style prompting produced the lowest accuracy (2.7/3) and did not provide a measurable benefit despite additional reasoning guidance and higher latency. Based on the balance of accuracy, reliability, latency, and maintainability, Structured prompting is the preferred production candidate, while Few-shot remains a strong alternative if maximizing extraction accuracy is the primary objective.
