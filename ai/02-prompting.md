# Writing So AI Understands You — Systematic Prompting

## What You'll Learn

- The 4 elements of every good prompt: Role, Context, Task, Format
- How to iterate when the output isn't right
- Common mistakes that produce bad results
- The Description skill from the 4D Framework

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

**Chain of thought:** Ask AI to think step by step before giving the answer.
```
Think step by step. First identify the core problem, 
then list possible solutions, then recommend the best one 
with reasoning.
```

**Few-shot examples:** Show AI what you want with examples.
```
Here are examples of good email subject lines:
- "Q3 Report: Action Required by Friday"
- "Update: Project Phoenix Timeline Change"

Now write a subject line for: [your situation]
```

**Constraint-based:** Tell AI what NOT to do.
```
Write a product description. Do NOT use superlatives 
(best, amazing, incredible). Do NOT mention pricing.
Focus on features and use cases only.
```

## Key Takeaway

Good prompting is good communication. If a human colleague wouldn't understand your request, AI won't either. Be specific, provide context, and iterate.
