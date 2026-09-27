# Appeal reviewer

> **Did we get this decision right, and would we make the same call again?**

Get a structured second opinion on a user's appeal: each part of the rule tested against the facts, the user's arguments weighed fairly, and a suggested reply.

[Try it live](https://stevenmacchia.github.io/ts-workbench/#appeal) · runs on your own Claude account

## How it works

1. **Share the case.** The rule, the decision, the content and the user's appeal.
2. **Let Claude review.** Each element of the rule is tested against the facts.
3. **You decide.** Claude recommends. A person makes the final call.

## Inputs

| Input | Type | Required |
|---|---|---|
| The rule that was applied | Long text | Yes |
| Original decision | Text | Yes |
| Reviewer's reason | Text |  |
| The content or behavior | Long text | Yes |
| What the user said in their appeal | Long text | Yes |
| Other context (optional) | Long text |  |

## The prompt

Shown with the worked example filled in. The tool asks for JSON only, then validates every field before showing anything.

```text
You are a senior Trust & Safety appeals reviewer giving a structured second opinion. A person will make the final decision.

THE RULE THAT WAS APPLIED
Violent speech: We remove content that threatens violence against a person or group. We allow clearly hyperbolic or joking statements between friends where no real threat is intended, and statements about fictional or game violence.

ORIGINAL DECISION
Action: Comment removed and 24-hour posting restriction
Reviewer's reason: Threat of violence

THE CONTENT OR BEHAVIOR
A comment on a friend's post about a football match: "I'm going to kill you when I see you Saturday 😂 you owe me £20 for that bet". The friend replied "haha bring it".

THE USER'S APPEAL
It was a joke with my best mate about a bet on the match. He replied laughing. We play five-a-side every Saturday.

OTHER CONTEXT
Account 6 years old with no previous violations. The two accounts follow each other and have messaged regularly for years. Flagged by a classifier, not reported by the friend.

INSTRUCTIONS
- Break the rule into the elements that must all be true for a violation (including that no exception applies), and test each one against the facts.
- Take the user's arguments seriously and assess each one fairly.
- Consider satire, news value, counter-speech, quoting to condemn, education and the user's history, but only where the inputs support it.
- If key facts are missing, list them and lower your confidence. Recommend escalation if a specialist (legal, child safety, self-harm) should decide.
- Never invent facts and never identify real people.

Return ONLY a JSON object with exactly these keys:
{"recommendation": "uphold" or "overturn" or "modify" or "escalate",
 "confidence": "low" or "medium" or "high",
 "summary": "2 to 3 sentences",
 "elements": [{"element": "string", "met": "yes" or "no" or "unclear", "reasoning": "string"}],
 "user_arguments": [{"argument": "string", "assessment": "string", "persuasive": true}],
 "mitigating": ["string"], "aggravating": ["string"],
 "missing_information": ["string"],
 "suggested_action": "string",
 "reply_to_user": "under 150 words, plain language",
 "note_for_record": "1 to 2 sentences for the case file",
 "policy_feedback": "how the rule or guidance could be clearer, or an empty string"}
```

## Example output

The worked example result shipped with the tool, in the JSON shape the prompt asks for.

```json
{
  "recommendation": "overturn",
  "confidence": "high",
  "summary": "The comment uses violent words but, read in context, is a joke between friends about a bet. The rule explicitly allows hyperbolic statements between friends where no real threat is intended, and the friend's reply shows they didn't take it as one.",
  "elements": [
    {
      "element": "The content refers to violence against a person",
      "met": "yes",
      "reasoning": "\"I'm going to kill you\" literally refers to violence."
    },
    {
      "element": "It is a genuine threat rather than hyperbole",
      "met": "no",
      "reasoning": "The laughing emoji, the bet and the friend's \"haha bring it\" all point to banter."
    },
    {
      "element": "No exception applies",
      "met": "no",
      "reasoning": "The exception for joking statements between friends fits these facts."
    }
  ],
  "user_arguments": [
    {
      "argument": "It was a joke with a close friend about a bet.",
      "assessment": "Supported by the emoji, the reference to the bet and years of messaging between the accounts.",
      "persuasive": true
    },
    {
      "argument": "They play football together every Saturday.",
      "assessment": "Plausible and explains \"when I see you Saturday\", though it can't be verified.",
      "persuasive": true
    }
  ],
  "mitigating": [
    "No violations in 6 years",
    "The friend replied playfully",
    "Clear reference to a bet"
  ],
  "aggravating": [
    "Out of context, the wording reads as a threat, which is why the classifier flagged it"
  ],
  "missing_information": [
    "Whether the friend reported the comment at any point. If they did, look again."
  ],
  "suggested_action": "Restore the comment, lift the posting restriction and remove the strike from the account.",
  "reply_to_user": "Thanks for explaining. We've looked again and agree this was a joke between friends about a bet, which our rules allow. We've restored your comment and lifted the posting restriction, and it won't count against your account. Sorry for the trouble.",
  "note_for_record": "Overturned: banter between long-standing mutual connections; the exception for jokes between friends applies. Not reported by the recipient.",
  "policy_feedback": "Add examples of sports and betting banter to reviewer guidance, so the friends-joking exception is applied consistently."
}
```

---

Part of [T&S Workbench](https://github.com/stevenmacchia/ts-workbench) by [Steven Macchia](https://www.linkedin.com/in/stevenmacchia). Content licensed [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/): reuse it freely with credit.
