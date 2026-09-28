# Policy stress-tester

> **Would two reviewers apply this rule the same way, and what does it forget?**

Paste a platform rule and get an instant clarity check in the browser, then an AI review: vague terms, missing exceptions, eight hard edge cases with decisions, enforcement risks, relevant laws, a reviewer checklist and a clearer rewrite.

[Try it live](https://stevenmacchia.github.io/ts-workbench/#policy) · the instant check runs anywhere; the AI review runs on your own Claude account

## The prompt

Shown with the complete example (a live-streaming platform with teen users) filled in.

```text
You are a senior trust and safety policy lead reviewing a platform rule before it goes live. Stress-test it: find where two reviewers would disagree, what it leaves out, and how it holds up against realistic hard cases on THIS platform.

CONTEXT
Company or product: Not provided
Platform type: Video and live streaming
Product description: A live-streaming and chat app for gamers. Streamers broadcast gameplay while viewers chat in real time. Many streamers are teenagers, and banter and trash talk are a big part of the culture.
Audience: Teens allowed
Regions: United States, United Kingdom, European Union
How the rule is enforced: User reports, Automated detection, Human reviewers
Actions reviewers can take: Remove content, Warn the user, Temporary suspension, Permanent ban
Known concerns and gray areas from the team: Heated trash talk between rival teams keeps getting reported as harassment. Viewers sometimes coordinate raids on smaller streamers. We're unsure how to treat jokes about a streamer's appearance, and whether repeated one-word insults count.

RULE
"""
Users must not harass, bully or intimidate other users. Content that is abusive or offensive will be removed.
"""

INSTRUCTIONS
- Base every finding on the rule text and the context. Where context is "Not provided", make a sensible assumption and list it under "assumptions".
- If you recognize the company or product named in the context, use what you know about how that platform works and how people use it to make the hard cases realistic. Don't invent specifics you aren't sure of.
- Edge cases must be realistic for this platform and audience. Turn the team's concerns into edge cases where relevant. Across the set, include at least one case each of news or documentary use, satire or humour, counter-speech, and, if under-18s may be present, a case involving a minor.
- Use "escalate" for cases a reviewer could not decide from the rule text alone.
- Tie enforcement risks to the enforcement methods and actions listed.
- Only include laws that clearly bear on this rule in the listed regions. Say "may" where uncertain. Never invent a law.
- Write in plain English with short sentences, for product managers and policy teams.

Reply with only a JSON object with exactly these keys:
{"score": integer 0-100 for how clear and consistently enforceable the rule is,
 "summary": "one-sentence verdict",
 "strengths": ["what the rule already does well"] (up to 3),
 "vague_terms": [{"term": "...", "why": "why reviewers could disagree", "suggest": "clearer wording"}] (up to 6),
 "gaps": [{"gap": "missing definition, exception, scope, consequence or appeal", "why": "why it matters"}] (up to 6),
 "edge_cases": [{"case": "a realistic piece of content or situation", "decision": "allow" or "remove" or "escalate", "reasoning": "one or two sentences"}] (exactly 8),
 "enforcement_risks": ["..."] (up to 4),
 "legal": [{"law": "law or regulation", "note": "how it bears on this rule"}] (up to 4, empty if none clearly apply),
 "assumptions": ["what you assumed because context was missing"] (up to 4, empty if none),
 "open_questions": ["a policy decision the team still needs to make"] (up to 4),
 "reviewer_checklist": ["a short yes/no check a moderator applies, in order"] (3 to 5),
 "rewrite": "an improved version of the rule in plain language with a definition, examples, exceptions, consequences and how to appeal, under 200 words"}
```

## Example rules to test

**Harassment.** Users must not harass, bully or intimidate other users. Content that is abusive or offensive will be removed.

**Hate speech.** We do not allow hate speech or content that attacks people based on who they are.

**Scams.** Don't post scams, fraudulent offers or misleading content designed to trick people out of money or information.

**Nudity.** Nudity and sexual content are not allowed, except in appropriate contexts.

**Dangerous acts.** Content that promotes dangerous or harmful activities will be removed.

---

Part of [T&S Workbench](https://github.com/stevenmacchia/ts-workbench) by [Steven Macchia](https://www.linkedin.com/in/stevenmacchia). Content licensed [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/): reuse it freely with credit.
