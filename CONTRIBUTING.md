# Contributing

Thank you for considering it. This repository is small, opinionated and maintained by one person, so here is exactly what helps and what does not.

---

## 🛑 Read this first

> [!WARNING]
> **Never paste real data into an issue, a pull request or a comment.** No customer records, no production schemas, no internal system names, no screenshots of live tables. Describe the shape of the problem instead. Contributions containing real data will be deleted rather than edited, because edits do not remove content from the history.

If you are unsure whether something counts, assume it does.

---

## ✅ What helps most

| Contribution | Why it is valuable |
| --- | --- |
| **A prompt that failed** on a model marked ✅ | The compatibility tables are the point of this repository. A wrong mark is worse than no mark |
| **A defect pattern** you have hit that is not covered | Real failure modes are the hardest thing to invent at a desk |
| **A correction** to anything factual | Errors get fixed and credited |
| **A dialect translation** of a query, for a database not yet covered | Widens the audience at low cost |

## 🤔 What is less likely to be merged

- A new prompt that has not been run on a real task. Everything here has been used before it was published, and that rule does not bend.
- A prompt that requires pasting records into a model. See [the rule the data readiness pack is built on](packs/data-readiness/README.md#the-rule-this-pack-is-built-on).
- Style rewrites. The house style is deliberate, including the British spelling.
- Additions that only work on one model, unless the file says so clearly and the limitation is genuinely worth the space.

---

## 📝 Raising an issue

Use one of the templates. They exist to make sure a report contains enough to act on:

- **Prompt failure** for a prompt that did not behave as the file says.
- **Correction or suggestion** for everything else.

Include the model and its version. "It did not work on ChatGPT" cannot be acted on; "GPT-5.2 returned Oracle syntax when asked for Postgres" can.

---

## 🧱 If you are opening a pull request

1. One change per pull request.
2. Follow the existing file structure. Every prompt file carries the same sections in the same order.
3. Lowercase kebab-case file names, numbered within a pack.
4. Professional UK English. No em dash character.
5. If you are adding a compatibility result, say which model version you tested and what you observed. Never enter a mark on expectation.

---

## 📄 Licence on contributions

By contributing you agree that your contribution is licensed under [CC BY 4.0](LICENSE), the same terms as the rest of the repository.

---

## 💬 Anything else

NeumannTechTips@pm.me, or a comment on the relevant video on the [channel](https://www.youtube.com/@NeumannTechTips).
