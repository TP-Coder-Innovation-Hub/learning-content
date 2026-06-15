# Writing So AI Understands You — Systematic Prompting

## What You'll Learn

- The 4 elements of every good prompt: Role, Context, Task, Format
- Prompting patterns: zero-shot, few-shot, chain of thought, constraints
- How to break down complex tasks into chained prompts
- How to iterate when the output isn't right

## The RCTF Prompt Framework

Every effective prompt has four elements. You don't need all four every time, but the more you include, the more predictable the output.

### Role — Who should AI be?

```
You are a senior product manager at a tech startup.
You are a Thai language teacher correcting student essays.
You are a financial analyst reviewing quarterly earnings.
```

### Context — What background does AI need?

```
Our company sells B2B SaaS to banks in Southeast Asia.
I'm writing an email to a client who complained about delayed delivery.
This is a follow-up to last week's meeting where we agreed on 3 action items.
```

### Task — What exactly should AI do?

```
Write a 3-paragraph email apologizing and offering a 10% discount.
Summarize this article into 5 bullet points.
Compare option A and option B in a table format.
```

### Format — How should the output look?

```
Respond in Thai. Use professional tone.
Output as a markdown table with columns: Feature, Option A, Option B.
Keep it under 200 words. Use bullet points, not paragraphs.
```

## Putting It Together

**Bad prompt:**
```
Write me an email about the project delay.
```

**Good prompt:**
```
You are a project manager at a software consultancy. 

Context: Our team is 2 weeks behind on the mobile app project 
for Client X. The delay was caused by a third-party API change 
that required unexpected rework.

Task: Write an email to the client informing them about the delay, 
explaining the cause, and proposing a revised timeline.

Format: Professional but warm tone. Under 300 words. 
End with a clear ask for a 15-minute call this week. 
Write in English.
```

## Iteration — When the Output Isn't Right

Don't rewrite the whole prompt. Fix one thing at a time:

| Problem | Fix |
|---------|-----|
| Too vague | Add more context |
| Wrong tone | Specify tone explicitly |
| Too long/short | Set word count or bullet limit |
| Off-topic | Narrow the task scope |
| Wrong format | Specify table, list, paragraph, code |

Example iteration:
```
First attempt: "Summarize this article"
→ Too long

Second attempt: "Summarize this article in 5 bullet points"
→ Missing key points

Third attempt: "Summarize this article in 5 bullet points, 
focusing on the financial impact and next steps"
→ Good
```

## Prompting Patterns That Work

These patterns come from AI research but are practical for anyone. Use them when a simple prompt isn't enough.

### Zero-shot — Ask Directly

No examples, just instructions. Works for simple, common tasks where AI already knows the pattern.

```
Classify this email as "urgent" or "normal":
[paste email]
```

If the result is wrong or inconsistent, switch to few-shot.

### Few-shot — Show, Don't Tell

Give 2-3 examples so AI learns the exact pattern you want. This is the single most effective way to improve output quality.

```
Classify these emails:

Email: "Server down, all users affected"
Label: Urgent

Email: "Weekly newsletter subscription"
Label: Normal

Email: "Payment failed for order #1234"
Label: Urgent

Email: [paste your email]
Label:
```

**Rules for good examples:**
- Keep the same format across all examples
- Use 2-3 examples (too many can confuse the model)
- Make examples different enough to show the pattern, not just copies

### Chain of Thought — Think Step by Step

Ask AI to reason before answering. Dramatically improves accuracy for complex questions, math, planning, and analysis.

```
Think step by step.

1. First, identify the root cause of the problem
2. Then, list 3 possible solutions
3. Then, compare pros and cons of each
4. Finally, recommend the best one with reasoning

Problem: [your problem]
```

You can also just add "Think step by step before answering" to any prompt.

### Constraints — Tell AI What NOT to Do

Sometimes telling AI what to avoid is more effective than telling it what to do.

```
Write a product description.
- Do NOT use superlatives (best, amazing, incredible)
- Do NOT mention pricing
- Do NOT exceed 100 words
- Focus on features and use cases only
```

### Break Down Complex Tasks

Don't ask AI to do everything in one prompt. Chain prompts for multi-step work:

```
Step 1 prompt: "Analyze this report and list the 3 key findings"
Step 2 prompt (paste findings): "For each finding, suggest one action item"
Step 3 prompt (paste action items): "Write a summary email to the team"
```

Each step gives AI focused context, producing better results than one giant prompt.

## Key Takeaway

Good prompting is good communication. If a human colleague wouldn't understand your request, AI won't either. Be specific, provide context, and iterate.

## Further Reading

- [Prompt Design Strategies — Google Gemini Guide](https://ai.google.dev/gemini-api/docs/prompting-strategies) — Official Google guide with more advanced patterns: response format control, partial input completion, model parameters (temperature, topK), and agentic workflows.
