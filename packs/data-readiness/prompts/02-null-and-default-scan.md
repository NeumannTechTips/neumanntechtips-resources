# 02 · Null and default scan

[⬅ Back to the pack](../README.md) · Pack 01, Data readiness

---

**Use it for** · Separating real values from defaults, placeholders and sentinels that a null check will never catch.

**You need** · The profile output from [prompt 01](01-column-profile.md). Aggregates only.

**Returns** · A ranked list of suspected default and sentinel values, what each one probably is, and the query to confirm it.

---

## Why this is not just counting nulls

A null is honest. It tells you something is missing. The dangerous values are the ones that look like data: a date of birth of 1900-01-01, a mobile number of 0000000000, a surname of "Unknown", a status code that appears in 94% of rows. None of them is null. All of them are absence wearing a costume, and every one will be learned as signal by a model that cannot tell the difference.

---

## The prompt

```
Below is aggregate profile output for a table being considered for AI
model training. It contains no individual records.

[paste the profile output from prompt 01]

Identify values that are probably NOT genuine data. Look for:
- Sentinel dates such as 1900-01-01, 1970-01-01, 9999-12-31
- Repeated-digit or obviously placeholder numbers
- Placeholder strings: "Unknown", "N/A", "TEST", "XXX", ".", "-", empty string
- Any single value holding an implausible share of non-null rows
- Columns where distinct_count is far lower than the column name implies
- Columns where distinct_count is close to row_count but the name implies a
  small set

For each finding give me:
  column | suspect value | share of non-null rows | why you think it is not
  genuine | confidence (high/medium/low) | a confirming query

Rank by how much damage it would do if it reached training, not by frequency.

State explicitly where you are guessing, and where a value could be
legitimate in some domains. Do not assume my business rules.
```

---

## Worked example

**Aggregate input** (synthetic)

> Every value below is fabricated. The example telephone numbers are deliberately malformed, because an Australian number never carries a `0` immediately after the `+61` country code, so neither string can correspond to a real subscriber. The email domain used elsewhere in this pack is under `.invalid`, a top-level domain [reserved by the IETF](https://www.rfc-editor.org/rfc/rfc2606) precisely so that it can never resolve.

```
column         | nulls  | null_rate | distinct | top_5_values_with_counts
date_of_birth  | 41,208 | 0.0412    | 19,447   | 1900-01-01:118,004 | 1970-01-01:9,110 | ...
mobile         | 96,455 | 0.0965    | 812,330  | 0000000000:22,417 | +61000000000:7,905 | ...
status_code    |      0 | 0.0000    | 4        | A:942,113 | I:48,220 | P:8,004 | X:1,663
last_name      |  1,004 | 0.0010    | 240,881  | Unknown:31,660 | .:2,904 | ...
```

**What came back** (abbreviated, ranked)

| Column | Suspect value | Share | Why | Confidence |
| --- | --- | --- | --- | --- |
| date_of_birth | `1900-01-01` | 12.3% | Classic system default. A real cohort born on one day at that scale is not plausible | High |
| last_name | `Unknown` | 3.2% | Placeholder written into a NOT NULL column | High |
| mobile | `0000000000` | 2.5% | Repeated-digit placeholder satisfying a format check | High |
| status_code | `A` | 94.2% | Not necessarily wrong, but a four-value column at 94% concentration carries almost no information | Medium |
| last_name | `.` | 0.3% | Single character entered to satisfy a mandatory field | High |

The model added, without being asked, that `1970-01-01` at 0.9% is the Unix epoch and typically indicates a failed conversion rather than a default, which is a different fault with a different fix.

---

## Known limits

| Limit | What to do |
| --- | --- |
| A model cannot know your domain. A 94% concentration may be entirely correct | Treat every medium and low confidence finding as a question, not a conclusion |
| It will miss defaults it has never seen, such as a company-specific code | Add your own known sentinels to the prompt |
| Confidence ratings are the model's self-assessment and are not calibrated | Use them for ordering, not for deciding |

---

## Tested on

| Model | Result | Note |
| --- | --- | --- |
| Claude | ✅ | Ranked by damage rather than frequency as asked, and distinguished the epoch date from a default |
| ChatGPT | ⏳ | Not yet tested |
| Gemini | ⏳ | Not yet tested |
| Grok | ⏳ | Not yet tested |
| Microsoft Copilot | ⏳ | Not yet tested |
| Open-weight, local | ⏳ | Not yet tested |

---

**Episode** · EP-001, *Six checks before you call your data AI-ready*
