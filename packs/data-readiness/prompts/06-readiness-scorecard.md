# 📊 06 · Readiness scorecard

> Turns the findings into one page for a sponsor

[![Pack](https://img.shields.io/badge/pack-data%20readiness-5DBBD6?labelColor=0B2039)](../README.md)
[![Step](https://img.shields.io/badge/step-06%20of%2006-2F5F7B?labelColor=0B2039)](../README.md#-use-them-in-order)
[![Claude](https://img.shields.io/badge/Claude-tested-5DBBD6?labelColor=0B2039)](#-tested-on)
[![Others](https://img.shields.io/badge/other%20models-not%20yet%20tested-8FA3B8?labelColor=0B2039)](#-tested-on)
[![Episode](https://img.shields.io/badge/episode-EP--001-F2A93B?labelColor=0B2039)](#)

`#DataQuality` `#AIReadiness` `#SQL` `#PromptEngineering` `#NeumannTechTips`

[🏠 Home](../../../README.md) · [📦 Pack index](../README.md) · [⬅️ 05 Semantic drift](05-semantic-drift-check.md) · _last in the pack_ ➡️

---

| | |
| :-- | :-- |
| 🎯 **Use it for** | Turning five technical findings into one page that a sponsor who does not write SQL can act on. |
| 📋 **You need** | The outputs from prompts 01 to 05, and a one-line statement of what the data is meant to be used for. |
| 📤 **Returns** | A one-page assessment with a rating per dimension, the three things that matter most, and what each would take to fix. |

---

## 🧠 Why the last step is the one people skip

The technical work produces a list of defects. A sponsor cannot act on a list of defects. They can act on "this dataset is not ready, here are the two things blocking it, the first takes a fortnight and the second takes a quarter, and here is what happens if we proceed anyway".

Translating between those two is the entire job. It is also the step that decides whether anything gets funded.

---

## 💬 The prompt

```
Turn the findings below into a one-page data readiness assessment for a
non-technical sponsor. They have five minutes and no SQL.

Intended use of the data: [one sentence]
Findings:
[paste the outputs from prompts 01 to 05, or a summary of them]

Structure it as:

1. VERDICT. One sentence. Ready / ready with conditions / not ready. No
   hedging.

2. SCORECARD. A table rating each dimension Red, Amber or Green with one
   line of evidence each:
   completeness | validity | consistency | uniqueness | timeliness |
   documentation

3. THE THREE THINGS THAT MATTER. Only three. For each: what is wrong, what
   it would do to the intended use, roughly what fixing it involves, and a
   rough effort band (days / weeks / months).

4. IF WE PROCEED ANYWAY. What specifically goes wrong, stated in terms of
   the intended use rather than in terms of data quality.

5. WHAT I AM NOT SURE ABOUT. Anything that needs a business decision or
   domain knowledge you do not have.

Rules:
- Plain English. No data quality jargon unless you define it in the line.
- No percentages without the count behind them.
- Do not soften the verdict to be helpful.
- Effort bands are bands. Do not give me a date.
```

---

## 🧪 Worked example

**Verdict returned:** "Not ready. Two defects would each independently invalidate per-customer analysis, and both are fixable."

**Scorecard**

| Dimension | Rating | Evidence |
| --- | --- | --- |
| Completeness | 🟠 Amber | 4.1% of dates of birth missing, and a further 12.3% defaulted to 1900-01-01 |
| Validity | 🔴 Red | 2.5% of mobile numbers are placeholder values that pass format checks |
| Consistency | 🔴 Red | Region code carries two coding schemes with no marker for which applies |
| Uniqueness | 🟠 Amber | No deduplication at entry across three merged channels, extent unmeasured |
| Timeliness | 🟢 Green | Records current to the last load |
| Documentation | 🔴 Red | No data dictionary exists |

The "if we proceed anyway" section was the strongest part. Rather than restating the defects, it said that any per-customer metric would be computed across an unknown number of split identities, and that age-based segmentation would place 12.3% of the base in a cohort born in 1900. Both are consequences a sponsor can picture.

---

## ⚠️ Known limits

| Limit | What to do |
| --- | --- |
| Effort bands are guesses without knowledge of your systems and team | Replace them with your own before the page goes to anybody |
| The model will rate dimensions confidently on thin evidence | Downgrade any rating you cannot point at a number for |
| A one-page format forces omissions | Keep the technical findings attached as an appendix, for the one person who asks |
| RAG ratings invite argument about the boundaries | Agree what Red means before you present it, not afterwards |

> [!NOTE]
> This assessment is an input to a decision, not the decision. Fitness for purpose depends on the purpose, and the model does not know your risk appetite.

---

## 🤖 Tested on

| Model | Result | Note |
| --- | --- | --- |
| Claude | ✅ | Held the verdict without softening it, and translated defects into consequences rather than restating them |
| ChatGPT | ⏳ | Not yet tested |
| Gemini | ⏳ | Not yet tested |
| Grok | ⏳ | Not yet tested |
| Microsoft Copilot | ⏳ | Not yet tested |
| Open-weight, local | ⏳ | Not yet tested |

---

**Episode** · EP-001, *Six checks before you call your data AI-ready*

<div align="center">

[⬅️ 05 Semantic drift](05-semantic-drift-check.md) · [📦 Pack index](../README.md) · _last in the pack_ ➡️

**[NeumannTechTips](https://www.youtube.com/@NeumannTechTips)** · Practical AI. Real results.

</div>
