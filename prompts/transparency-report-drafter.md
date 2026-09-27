# Transparency report drafter

> **Can the public understand what you did to keep people safe, and does the report hold up to scrutiny?**

Turn your enforcement numbers into a clear, factual transparency report section, with what each number means and what a regulator would say is missing.

[Try it live](https://stevenmacchia.github.io/ts-workbench/#transparency) · runs on your own Claude account

## How it works

1. **Add your numbers.** Paste them, or pull them from your Metrics scorecard.
2. **Let Claude draft.** Highlights, sections, and the gaps a regulator would spot.
3. **Check every figure.** Nothing is published until your team verifies it.

## Inputs

| Input | Type | Required |
|---|---|---|
| Company or product | Text |  |
| Reporting period | Text |  |
| Rules that apply | Multiple choice |  |
| Main audience | Choice |  |
| Your numbers for this period | Long text | Yes |
| Previous period (optional) | Long text |  |
| What changed this period (optional) | Long text |  |

## The prompt

Shown with the worked example filled in. The tool asks for JSON only, then validates every field before showing anything.

```text
You are a Trust & Safety communications lead drafting part of a public transparency report.

COMPANY OR PRODUCT: Pixelry
REPORTING PERIOD: January to June 2026
RULES THAT APPLY: dsa
MAIN AUDIENCE: The public and your users
NUMBERS FOR THIS PERIOD (use only these; never invent or estimate figures):
Average monthly active users in the EU: 8.2 million
Content removed for breaking our rules: 1,240,000 (spam 870,000; nudity 190,000; harassment 110,000; hate speech 42,000; violent content 28,000)
Removed by automated tools before anyone reported it: 1,080,000
User reports received: 310,000
Appeals received: 21,500; content restored after appeal: 4,300
Accounts suspended: 96,000 (81,000 for spam)
Notices of illegal content from users and trusted flaggers: 5,800; median time to decision: 19 hours
Government requests for user data: 140; data disclosed in 88
PREVIOUS PERIOD:
Content removed: 980,000
Appeals received: 18,000; content restored: 3,100
WHAT CHANGED THIS PERIOD:
A new spam classifier launched in March.

INSTRUCTIONS
- Write clearly for the audience. Explain what each figure means. Explain why a number moved only where the inputs support it.
- Use only the numbers given. When you calculate a percentage or change, it must follow directly from the numbers given. If a figure is missing, list it under gaps rather than estimating it.
- If the EU Digital Services Act applies, list transparency items it requires (Articles 15 and 24) that are missing from the data, such as orders from authorities, notices by type of illegal content, own-initiative moderation, automated tools and their accuracy, complaint numbers and times, out-of-court disputes, suspensions, and average monthly active recipients.
- Factual, calm and non-defensive. No marketing language.

Return ONLY a JSON object with exactly these keys:
{"title": "string",
 "summary": "3 to 4 sentences",
 "highlights": [{"label": "string", "value": "string", "context": "string"}],
 "sections": [{"heading": "string", "body": "60 to 150 words"}],
 "gaps": [{"item": "string", "why": "string"}],
 "charts": ["string"],
 "review_before_publishing": ["string"]}
Give 3 to 6 highlights and 3 to 6 sections.
```

## Example output

The worked example result shipped with the tool, in the JSON shape the prompt asks for.

```json
{
  "title": "Pixelry transparency report: January to June 2026",
  "summary": "Between January and June 2026 we removed 1.24 million pieces of content for breaking our rules, 27% more than in the previous six months, mostly spam. 87% of removals happened before anyone reported the content. People appealed 21,500 decisions, and we restored content in 4,300 of those cases (20%).",
  "highlights": [
    {
      "label": "Pieces of content removed",
      "value": "1.24M",
      "context": "Up 27% from 980,000. Spam made up 70% of removals."
    },
    {
      "label": "Found before any report",
      "value": "87%",
      "context": "1,080,000 of 1,240,000 removals."
    },
    {
      "label": "Appeals that restored content",
      "value": "20%",
      "context": "4,300 of 21,500, up from 17% (3,100 of 18,000)."
    },
    {
      "label": "Illegal-content notices",
      "value": "5,800",
      "context": "Median time to a decision: 19 hours."
    },
    {
      "label": "Government data requests",
      "value": "140",
      "context": "Data disclosed in 88 cases (63%)."
    }
  ],
  "sections": [
    {
      "heading": "How much we removed, and why",
      "body": "We removed 1,240,000 pieces of content this period, up from 980,000. Spam accounted for 870,000 removals, followed by nudity (190,000), harassment (110,000), hate speech (42,000) and violent content (28,000). A new spam classifier launched in March, and it accounts for much of the increase. We are checking whether it also changed how much spam people actually saw, which is the number that matters most."
    },
    {
      "heading": "How we find harmful content",
      "body": "Automated tools found 1,080,000 of the removals, 87% of the total, before anyone reported them. People sent us 310,000 reports. Automated tools are most effective for spam and nudity. Harassment and hate speech depend more on context, so reports from people and review by our trained teams remain essential."
    },
    {
      "heading": "Appeals and our mistakes",
      "body": "People appealed 21,500 decisions and we restored content in 4,300 cases, or 20%, up from 17% in the previous period. Each restored decision is used to retrain reviewers and adjust our tools. A higher restore rate after a new classifier launch is something we are watching closely."
    },
    {
      "heading": "Reports of illegal content",
      "body": "We received 5,800 notices of illegal content from users and trusted flaggers, and made a decision in a median of 19 hours. Notices from trusted flaggers are handled in a separate, prioritized queue."
    },
    {
      "heading": "Requests from governments",
      "body": "We received 140 requests for user data from governments and law enforcement, and disclosed data in 88 cases (63%). Every request is reviewed by our legal team, and requests that are unlawful or too broad are narrowed or rejected."
    }
  ],
  "gaps": [
    {
      "item": "Accuracy of automated tools",
      "why": "The DSA asks for indicators of accuracy and possible error rates for automated moderation (Article 15)."
    },
    {
      "item": "Orders from EU authorities",
      "why": "Required by Article 15, broken down by member state and type of illegal content."
    },
    {
      "item": "Out-of-court disputes",
      "why": "Online platforms must report disputes submitted, their outcomes and time to resolve (Article 24)."
    },
    {
      "item": "Time to decide appeals",
      "why": "Report the median time to handle complaints, not only the counts."
    },
    {
      "item": "Notices by type of illegal content",
      "why": "Break notices down by category, with trusted-flagger notices shown separately."
    }
  ],
  "charts": [
    "Removals by policy area, this period against the previous one",
    "Share found by automated tools against user reports, by policy area",
    "Appeals received, restored and upheld"
  ],
  "review_before_publishing": [
    "Check every figure against the source dashboards for the same dates",
    "Legal review of the DSA sections and the wording on government requests",
    "Confirm the spam increase explanation with the detection team",
    "Confirm the monthly active user definition matches your DSA publication"
  ]
}
```

---

Part of [T&S Workbench](https://github.com/stevenmacchia/ts-workbench) by [Steven Macchia](https://www.linkedin.com/in/stevenmacchia). Content licensed [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/): reuse it freely with credit.
