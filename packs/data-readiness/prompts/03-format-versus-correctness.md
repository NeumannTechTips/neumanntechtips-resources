# 03 · Format versus correctness

[⬅ Back to the pack](../README.md) · Pack 01, Data readiness

---

**Use it for** · Building checks that tell you whether contact data is usable, not merely whether it is shaped correctly.

**You need** · The column list for your contact fields, your SQL dialect, and the countries the data covers.

**Returns** · A tiered set of checks, from cheap format validation to the checks that actually predict usability, with the cost of each stated.

---

## The distinction that matters

A regular expression tells you an email address is correctly shaped. It tells you nothing about whether anybody reads it. `finance@company.invalid` passes every syntax check ever written and has never delivered a message.

The same applies to telephone numbers. A string of eleven digits beginning with the right country code is well formed. Whether it belongs to a live handset is a different question, and it is the only question that matters to anyone downstream.

Most data quality dashboards report the first and imply the second.

---

## The prompt

```
I need to assess contact data quality for a dataset being prepared for AI
use. I will not paste records to you.

Dialect: [dialect]
Table: [schema.table]
Contact columns: [email, mobile, landline, postcode, address_line_1, ...]
Countries covered: [e.g. Australia, United Kingdom]

Give me checks in three tiers. For each tier state what it catches, what it
misses, and what it costs to run.

Tier 1, structural. Free, runs in SQL. Syntax, length, character class,
country-code plausibility, checksum where one exists.

Tier 2, semantic. Still SQL, no external service. Domain plausibility,
known-placeholder detection, internal consistency between related fields,
distribution anomalies such as one value repeating.

Tier 3, external verification. Needs a service or a live test. State what
kind of service, roughly what it costs per record, and what it can and
cannot confirm. Do not recommend a specific vendor.

For tiers 1 and 2 give runnable SQL. For tier 3 describe only.

Finish with one sentence on which tier I should stop at if the data is
going into model training rather than into a live campaign.
```

---

## Worked example

The output's most useful section was Tier 2, which included a check most teams never run: comparing the count of distinct email domains against the row count, and flagging any single domain holding more than a threshold share. On synthetic data seeded with a defect, this surfaced 4.1% of rows sharing one internal domain, which is the signature of a bulk import that filled a mandatory field with a system address.

Its closing sentence was that for training data, tier 2 is the sensible stopping point, because external verification tells you about deliverability today, whereas training data needs to be internally consistent rather than currently reachable.

---

## Known limits

| Limit | What to do |
| --- | --- |
| Country-specific numbering rules change, and models are not reliably current on them | Verify the rules against the national numbering authority before trusting the ranges |
| Models will sometimes produce a regular expression that rejects valid addresses | Test every pattern against a list of known-good awkward cases first |
| Tier 3 cost estimates should be treated as order-of-magnitude only | Get a quote |

> [!IMPORTANT]
> Never send real contact data to an external verification service without checking your data protection position first. That is a processing decision, not a technical one.

---

## Tested on

| Model | Result | Note |
| --- | --- | --- |
| Claude | ✅ | Tiering held, and the stop-at-tier-2 reasoning for training data was sound |
| ChatGPT | ⏳ | Not yet tested |
| Gemini | ⏳ | Not yet tested |
| Grok | ⏳ | Not yet tested |
| Microsoft Copilot | ⏳ | Not yet tested |
| Open-weight, local | ⏳ | Not yet tested |

---

**Episode** · EP-001, *Six checks before you call your data AI-ready*
