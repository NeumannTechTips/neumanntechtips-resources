# 🔍 Pack 01: Data readiness

Six prompts that take a dataset from "somebody says we should train a model on this" to a one-page assessment a sponsor can act on.

**Companion episode:** EP-001, *Six checks before you call your data AI-ready*
**Licence:** [CC BY 4.0](../../LICENSE) · **Status:** Live

---

## The rule this pack is built on

**Your data never goes into the model. The query does.**

Every prompt here asks the model to write a query, or to interpret the summary a query returned. None of them asks you to paste records into a chat window. That design is deliberate for three reasons.

| Reason | Detail |
| --- | --- |
| **Scale** | A table with billions of rows cannot be pasted anywhere. Aggregate output can |
| **Confidentiality** | Personal and commercially sensitive records must not leave your environment, and a chat window is leaving your environment |
| **Accuracy** | A model reasoning over a hundred sampled rows will tell you about those hundred rows. A model reading `COUNT(DISTINCT ...)` over the whole table tells you about the table |

If a prompt collection asks you to paste customer records into a model to check their quality, it was written by somebody who has never had to clear it with a risk function.

---

## Use them in order

Each prompt feeds the next. Running them out of sequence works, but you will do more thinking yourself.

| # | Prompt | What it does | Feeds |
| --- | --- | --- | --- |
| 01 | [Column profile](prompts/01-column-profile.md) | Writes the profiling query for your SQL dialect | 02, 05 |
| 02 | [Null and default scan](prompts/02-null-and-default-scan.md) | Reads the profile and separates genuine values from defaults and sentinels | 06 |
| 03 | [Format versus correctness](prompts/03-format-versus-correctness.md) | Builds checks that distinguish well-formed from actually usable | 06 |
| 04 | [Duplicates beyond exact match](prompts/04-duplicates-beyond-exact-match.md) | Designs a matching strategy for the duplicates a key check never finds | 06 |
| 05 | [Semantic drift](prompts/05-semantic-drift-check.md) | Finds columns that are no longer holding what their name claims | 06 |
| 06 | [Readiness scorecard](prompts/06-readiness-scorecard.md) | Turns the findings into a one page assessment for a non-technical sponsor | |

---

## What you need before you start

- Read access to the table or an extract of it, and permission to run aggregate queries against it.
- The data dictionary, or whatever passes for one. Its absence is itself a finding.
- A rough idea of what the data is meant to be used for. "Fit for purpose" has no meaning without a purpose.

---

## What this pack will not do

- It will not tell you a dataset is fit for a given use. It tells you what is wrong with it, and how badly. The decision remains yours.
- It will not clean anything. Remediation is a separate problem, and a much larger one.
- It will not substitute for knowing your own business rules. A value that looks impossible may be entirely normal in your domain, and only you know that.

---

## Compatibility at a glance

| Prompt | Claude | ChatGPT | Gemini | Grok | Copilot | Open-weight, local |
| --- | --- | --- | --- | --- | --- | --- |
| 01 | ✅ | ⏳ | ⏳ | ⏳ | ⏳ | ⏳ |
| 02 | ✅ | ⏳ | ⏳ | ⏳ | ⏳ | ⏳ |
| 03 | ✅ | ⏳ | ⏳ | ⏳ | ⏳ | ⏳ |
| 04 | ✅ | ⏳ | ⏳ | ⏳ | ⏳ | ⏳ |
| 05 | ✅ | ⏳ | ⏳ | ⏳ | ⏳ | ⏳ |
| 06 | ✅ | ⏳ | ⏳ | ⏳ | ⏳ | ⏳ |

✅ works as written · ⚠️ works with a stated caveat · ❌ not reliable · ⏳ not yet tested

Results are filled in as each model is tested. A mark is never entered on expectation.
