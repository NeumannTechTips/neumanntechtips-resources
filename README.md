<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/NeumannTechTips_Lockup_Horizontal_Tagline_OnDark.png">
  <img alt="NeumannTechTips. Practical AI. Real results." src="assets/NeumannTechTips_Lockup_Horizontal_Tagline_OnLight.png" width="560">
</picture>

</div>

# 📚 neumanntechtips-resources

> Free prompt packs, checklists and reference material from the **[NeumannTechTips](https://www.youtube.com/@NeumannTechTips)** YouTube channel. Everything here has been used on a real task before it was published, and every prompt says where it falls over as well as where it works.

[![Licence](https://img.shields.io/badge/licence-CC%20BY%204.0-5DBBD6?labelColor=0B2039)](LICENSE)
[![YouTube channel](https://img.shields.io/badge/YouTube-%40NeumannTechTips-5DBBD6?labelColor=0B2039&logo=youtube&logoColor=white)](https://www.youtube.com/@NeumannTechTips)
[![Tagline](https://img.shields.io/badge/Practical%20AI.-Real%20results.-F2A93B?labelColor=0B2039)](#-what-this-is)
[![Packs](https://img.shields.io/badge/packs-1%20live%20·%204%20planned-2F5F7B?labelColor=0B2039)](#-packs)
[![Language](https://img.shields.io/badge/language-UK%20English-2F5F7B?labelColor=0B2039)](#-house-rules)
[![Cost](https://img.shields.io/badge/cost-free%2C%20no%20sign%20up-8FA3B8?labelColor=0B2039)](#-licence)

`#PracticalAI` `#PromptEngineering` `#DataQuality` `#AIGovernance` `#LocalAI` `#OpenWeight` `#Claude` `#ChatGPT` `#Gemini` `#Copilot` `#Ollama` `#NeumannTechTips`

---

## 📖 Table of contents

- [What this is](#-what-this-is)
- [Start here](#-start-here)
- [Packs](#-packs)
- [Latest additions](#-latest-additions)
- [How a prompt file is laid out](#-how-a-prompt-file-is-laid-out)
- [Model compatibility](#-model-compatibility)
- [Repository map](#-repository-map)
- [House rules](#-house-rules)
- [Contributing](#-contributing)
- [Licence](#-licence)

---

## 🎯 What this is

Most prompt collections are lists of clever one-liners with no indication of whether they survive contact with real work. This one is organised the other way round: around the job you are trying to finish, with the failure modes left in.

Each prompt records what it needs from you, what it gives back, which models it has actually been run on, and where it breaks. If a prompt only works properly on one model, that is stated rather than hidden.

Everything here accompanies a video on the channel. You do not need to watch it to use the material, and the link is there if you want to see the prompt applied to a real task.

> [!NOTE]
> **No sign up, no email capture, no upsell.** Clone it, copy it, adapt it. The licence asks only for attribution.

---

## 🚀 Start here

| If you want to... | Go to |
| --- | --- |
| Work out whether your data is fit to feed an AI system | [`packs/data-readiness/`](packs/data-readiness/) |
| Find prompts that work on the model you already have | [Model compatibility](#-model-compatibility) |
| See what has just been added | [Latest additions](#-latest-additions) |
| Understand how to read a prompt file | [How a prompt file is laid out](#-how-a-prompt-file-is-laid-out) |
| Report a prompt that failed, or fix something | [CONTRIBUTING.md](CONTRIBUTING.md) |

---

## 📦 Packs

Packs are organised by the job to be done, not by model. Most prompts here are portable across the frontier models, and the ones that are not say so in their own compatibility table.

| Pack | What it gives you | Prompts | Status |
| --- | --- | --- | --- |
| 🔍 [**Data readiness**](packs/data-readiness/) | Screen a dataset for the defects that disqualify it from model training, and produce an assessment a non-technical sponsor can act on | 6 | ✅ Live |
| 📄 **Document work** | Summarise, compare and extract from long documents without losing the figures | | 🟦 Planned |
| ⚖️ **Governance** | Answer the questions a risk committee, a procurement team or a vendor review will put to you | | 🟦 Planned |
| 🗄️ **SQL and data** | Describe data well enough that a model returns something you can trust, and verify what comes back | | 🟦 Planned |
| 💻 **Local and open-weight models** | Get useful work out of models running on hardware you already own | | 🟦 Planned |

Each pack has its own `README.md` listing every prompt in it, so you can browse a pack without opening each file.

---

## 🆕 Latest additions

| Date | Added | Pack |
| --- | --- | --- |
| 2026-09-12 | Six prompts, the full readiness sequence from profiling query to sponsor scorecard | Data readiness |

---

## 🧭 How a prompt file is laid out

Every prompt file follows the same shape, so you can judge whether it is worth your time in about ten seconds.

| Section | What it tells you |
| --- | --- |
| **Use it for** | The specific job, in one sentence |
| **You need** | What you must supply, such as a schema, a document or a list of columns |
| **Returns** | The shape of the output, so you know what you are getting before you run it |
| **The prompt** | A fenced block, ready to copy, with placeholders in `[square brackets]` |
| **Worked example** | Synthetic input and the output it produced |
| **Known limits** | Where it fails, and what to do instead |
| **Tested on** | The compatibility table described below |
| **Episode** | The video it comes from, where one exists |

> [!IMPORTANT]
> **Every example in this repository uses synthetic data.** Records shown are generated to reproduce a defect pattern. No real organisation, system, schema or record appears anywhere in it, and none ever will.

---

## 🤖 Model compatibility

Prompts are written to be portable, then tested. The table in each file uses three marks and nothing else.

| Mark | Meaning |
| --- | --- |
| ✅ | Works as written |
| ⚠️ | Works with a caveat, which is stated next to it |
| ❌ | Does not work reliably, and the file says why |
| ⏳ | Not yet tested on that model |

A mark is entered only after the prompt has actually been run on that model. Nothing is marked on expectation, which is why ⏳ appears often at launch and will keep appearing as new models arrive.

Models currently in the test set:

| Model | Where it runs |
| --- | --- |
| Claude | Frontier, hosted |
| ChatGPT | Frontier, hosted |
| Gemini | Frontier, hosted |
| Grok | Frontier, hosted |
| Microsoft Copilot | Frontier, hosted, inside Microsoft 365 |
| Open-weight models via Ollama | Local, on consumer hardware |

**Open-weight, not open-source.** Models such as Llama, Mistral and Qwen publish their weights but not their training code or data composition, so they do not meet the [Open Source AI Definition](https://opensource.org/ai/open-source-ai-definition). This repository uses the accurate term throughout.

---

## 🗂️ Repository map

<details>
<summary><strong>Full folder tree</strong></summary>

```
neumanntechtips-resources/
├── README.md                     this index
├── LICENSE                       Creative Commons Attribution 4.0
├── CONTRIBUTING.md               what helps, and the rule about real data
├── .gitignore                    a safety net against committing data or secrets
├── .gitattributes                line endings and binary handling
├── .github/
│   └── ISSUE_TEMPLATE/           forms that make a report actionable
├── assets/                       brand images used by this README
└── packs/
    └── data-readiness/
        ├── README.md             what is in this pack, and the order to use it in
        └── prompts/
            ├── 01-column-profile.md
            ├── 02-null-and-default-scan.md
            ├── 03-format-versus-correctness.md
            ├── 04-duplicates-beyond-exact-match.md
            ├── 05-semantic-drift-check.md
            └── 06-readiness-scorecard.md
```

</details>

| Path | Contents |
| --- | --- |
| [`packs/`](packs/) | One folder per pack, each with its own index |
| `packs/<pack>/prompts/` | One file per prompt, numbered in the order they are meant to be used |
| `assets/` | Images used by this page. Nothing here is required to use the prompts |

**Naming.** Lowercase kebab-case throughout, numbered within a pack: `01-column-profile.md`. Numbers indicate sequence, not importance.

---

## 📐 House rules

These apply to everything published here.

- **Tested before published.** A prompt appears only after it has been run on a real task, on at least one model, with the result recorded in its compatibility table.
- **Failures stated.** Where a prompt is unreliable, the file says so. A collection that only reports successes is not useful.
- **Synthetic examples only.** No real data, organisation, client or system, under any circumstances.
- **Sources given.** Factual claims carry a source. Anything unverified is labelled as opinion.
- **Plain English.** Professional UK English, no jargon for its own sake.
- **No lock-in.** No sign up, no email wall, no paid tier hiding behind a free sample.

---

## 🤝 Contributing

Issues are open. The most useful things you can raise:

- A prompt that failed on a model listed as ✅, with the model version and what happened.
- A defect pattern you have hit in real work that is not covered here. Describe the pattern, never the data.
- A correction. Errors get fixed and credited.

Please do not paste real data, client information or anything identifying into an issue. Describe the shape of the problem instead.

---

## 📄 Licence

[**Creative Commons Attribution 4.0 International**](LICENSE).

Use it, adapt it, build on it, including commercially and inside your organisation. The only condition is attribution: credit NeumannTechTips and link back to this repository or the channel.

---

<div align="center">

**[NeumannTechTips](https://www.youtube.com/@NeumannTechTips)** · Practical AI. Real results.

Questions or corrections: NeumannTechTips@pm.me

</div>
