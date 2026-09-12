# 04 · Duplicates beyond exact match

[⬅ Back to the pack](../README.md) · Pack 01, Data readiness

---

**Use it for** · Designing a duplicate detection strategy that finds the records an exact-match check will never see.

**You need** · Your column list, your dialect, and an honest statement of how the records were created.

**Returns** · A blocking and scoring strategy, the SQL to implement it, and a threshold you can defend to somebody else.

---

## Why exact matching finds the easy ones

Deduplicating on a key finds the records that were inserted twice. That is the smallest category and the least interesting one.

The duplicates that matter are the ones a human created: the same customer entered through a different channel, with a shortened first name, a moved-in address, a typo in the surname and a mobile number that has since changed. Nothing matches exactly. Everything matches nearly. A model trained on that data learns that this person is two people, and every per-customer aggregate you calculate is wrong in a direction nobody can see.

---

## The prompt

```
I need to find non-exact duplicates in a dataset before it is used for AI.
I will not paste records to you. Aggregates and column names only.

Dialect: [dialect]
Table: [schema.table]
Available columns: [list]
Approximate row count: [n]
How the records were created: [e.g. three channels merged in 2019, no
  deduplication at the point of entry, free text address fields]

Design a matching strategy:

1. BLOCKING. Which keys should I block on to avoid comparing every record
   with every other? Give me at least two blocking passes with different
   keys so a record missed by one is caught by the other. State the
   expected comparison count for each.

2. SCORING. Per-field comparison rules, with a weight for each and the
   reasoning for that weight. Say which fields are strong evidence of a
   match and which are almost worthless.

3. THRESHOLD. Where to set the match cut-off, and what I lose at that
   setting in each direction.

4. SQL. Runnable implementation for the blocking passes and the scoring.

5. VALIDATION. How to check the strategy is working without a labelled set,
   since I do not have one.

Be explicit about what this approach will still miss.
```

---

## Worked example

The most valuable part of the output was section 2, specifically the weighting reasoning. It rated a shared mobile number as strong evidence but noted two failure cases: shared household landlines, and the placeholder numbers found by [prompt 03](03-format-versus-correctness.md), which will match thousands of unrelated records to each other. It recommended excluding any value appearing more than a small number of times from the matching entirely, which is a correction most naive implementations miss.

On weighting, it rated date of birth as strong, then immediately qualified it: worthless if the sentinel date from [prompt 02](02-null-and-default-scan.md) has not been excluded first, because every record defaulted to 1900-01-01 will match every other one.

That dependency between prompts is real. Run 02 and 03 before this one.

---

## Known limits

| Limit | What to do |
| --- | --- |
| Fuzzy matching at billions of rows is an engineering problem, not a prompting one | Use the strategy as a specification, then implement it in a tool built for the scale |
| Models over-recommend Levenshtein distance, which is poor on names | Ask specifically about phonetic and token-based alternatives |
| Without a labelled set, precision and recall are estimates | Hand-check a sample at the threshold, above it and below it |
| A threshold that is right for marketing is wrong for a financial record | State the consequence of a false merge in your prompt |

---

## Tested on

| Model | Result | Note |
| --- | --- | --- |
| Claude | ✅ | Caught the placeholder-number and sentinel-date interactions without being prompted |
| ChatGPT | ⏳ | Not yet tested |
| Gemini | ⏳ | Not yet tested |
| Grok | ⏳ | Not yet tested |
| Microsoft Copilot | ⏳ | Not yet tested |
| Open-weight, local | ⏳ | Not yet tested |

---

**Episode** · EP-001, *Six checks before you call your data AI-ready*
