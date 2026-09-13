# 🌀 05 · Semantic drift

> Finds columns no longer holding what their name claims

[![Pack](https://img.shields.io/badge/pack-data%20readiness-5DBBD6?labelColor=0B2039)](../README.md)
[![Step](https://img.shields.io/badge/step-05%20of%2006-2F5F7B?labelColor=0B2039)](../README.md#-use-them-in-order)
[![Claude](https://img.shields.io/badge/Claude-tested-5DBBD6?labelColor=0B2039)](#-tested-on)
[![Others](https://img.shields.io/badge/other%20models-not%20yet%20tested-8FA3B8?labelColor=0B2039)](#-tested-on)
[![Episode](https://img.shields.io/badge/episode-EP--001-F2A93B?labelColor=0B2039)](#)

`#DataQuality` `#AIReadiness` `#SQL` `#PromptEngineering` `#NeumannTechTips`

[🏠 Home](../../../README.md) · [📦 Pack index](../README.md) · [⬅️ 04 Duplicates beyond exact match](04-duplicates-beyond-exact-match.md) · [06 Readiness scorecard ➡️](06-readiness-scorecard.md)

---

| | |
| :-- | :-- |
| 🎯 **Use it for** | Finding columns that stopped holding what their name says, usually years ago, usually without anybody recording it. |
| 📋 **You need** | The profile output from [prompt 01](01-column-profile.md), the data dictionary if one exists, and a date column to slice by. |
| 📤 **Returns** | Columns where the name, the declared type, the documentation and the actual contents disagree, with a query to demonstrate each. |

---

## 🧠 What drift looks like

A `notes` column that quietly became a status field because somebody needed one and a change request was going to take six weeks. A `customer_type` column where the values before 2019 mean something entirely different from the values after it. A `region` column carrying three different regional schemes stacked on top of each other from three system migrations.

None of these breaks a report. All of them corrupt a model, because the model treats the column as one variable and it is three.

---

## 💬 The prompt

```
I am checking a dataset for semantic drift before AI use. I will not paste
records to you. Aggregate profile output and documentation only.

Profile output:
[paste from prompt 01]

Data dictionary or column documentation:
[paste, or write: none exists]

Date column available for slicing: [column name]

Find the disagreements. Specifically:

1. Columns whose actual value distribution contradicts their name.
2. Columns whose declared type is wider than their real content, which
   usually means the column is doing a second job.
3. Columns where distinct_count suggests a coded set but the top values
   look like free text, or the reverse.
4. Columns where the documentation describes something the profile does
   not support.
5. Anything suggesting more than one coding scheme in one column.

For each finding give me: the column, what it claims to be, what the profile
suggests it actually holds, and a query that slices the column by the date
column so I can see when the meaning changed.

Where no documentation exists, say what you can and cannot conclude without
it. Do not fill the gap with assumptions.
```

---

## 🧪 Worked example

On synthetic profile output, the highest-value finding was a `region_code` column declared as `varchar(10)` with 31 distinct values, where the top five were two-letter codes and the tail included several six-character codes. The model inferred two coding schemes rather than dirty data, and the date-sliced query it produced would show the changeover point.

It also flagged a `notes` column declared `varchar(4000)` where the profile showed a cardinality ratio of 0.004, meaning a handful of values repeating across a large table. Its conclusion was that a free text field with that little variety is not free text, and is almost certainly being used as an undeclared status flag.

Both are the kind of finding that takes an experienced person an afternoon and a newcomer several weeks.

---

## ⚠️ Known limits

| Limit | What to do |
| --- | --- |
| Without documentation, the model is inferring intent from a column name | Treat every finding as a hypothesis to test with the query, never as a conclusion |
| It cannot see values below the top five, so a rare third scheme stays hidden | Ask for the top fifty on any column it flags |
| Perfectly legitimate domain conventions will be flagged as drift | Expect false positives. They are cheap; a missed one is not |

---

## 🤖 Tested on

| Model | Result | Note |
| --- | --- | --- |
| Claude | ✅ | Distinguished two coding schemes from dirty data, which is the harder call |
| ChatGPT | ⏳ | Not yet tested |
| Gemini | ⏳ | Not yet tested |
| Grok | ⏳ | Not yet tested |
| Microsoft Copilot | ⏳ | Not yet tested |
| Open-weight, local | ⏳ | Not yet tested |

---

**Episode** · EP-001, *Six checks before you call your data AI-ready*

<div align="center">

[⬅️ 04 Duplicates beyond exact match](04-duplicates-beyond-exact-match.md) · [📦 Pack index](../README.md) · [06 Readiness scorecard ➡️](06-readiness-scorecard.md)

**[NeumannTechTips](https://www.youtube.com/@NeumannTechTips)** · Practical AI. Real results.

</div>
