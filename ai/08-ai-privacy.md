# Privacy and Data Safety — What Never to Put Into AI

## What You'll Learn

- What data you should NEVER put into AI tools
- How to use AI safely at work
- Understanding free vs paid data policies
- The Diligence skill from the 4D Framework

## The Golden Rule

**If you wouldn't post it on a public bulletin board, don't put it in a free AI tool.**

Free AI tools may use your conversations to train models. Paid/enterprise tools usually have agreements that protect your data. Always check before you type.

## Never Put Into AI (Any Tier)

- Customer personal data (names, emails, phone numbers, IDs)
- Financial data (account numbers, salary details, budgets with names)
- Health information (diagnosis, medications, insurance)
- Legal documents under attorney-client privilege
- Government/classified information
- Credentials (passwords, API keys, access tokens)

## Be Careful With (Depends on Tool)

| Data Type | Free AI | Paid/Enterprise | Safe Approach |
|-----------|---------|-----------------|---------------|
| Company strategy | Don't | Check policy | Anonymize first |
| Source code | Don't (public repos) | Check policy | Use private repos only |
| Meeting notes with names | Don't | Usually OK | Remove names |
| Financial reports | Don't | Check policy | Use percentages, not actuals |
| Client requirements | Don't | Check policy | Genericize details |

## How to Anonymize Before Using AI

Before pasting work content into AI:

```
Original: "Our client Siam Commercial Bank wants to migrate 
their core banking system from the legacy IBM mainframe 
by Q3 2026 with a budget of 50M THB"

Anonymized: "A large Thai bank wants to migrate their core 
system from a legacy mainframe by Q3 2026 with an 8-figure 
budget. What are the key risks and recommended approach?"
```

You get the same quality of advice without exposing sensitive data.

## Tool-Specific Privacy Settings

### Claude (Anthropic)
- Free/Pro: Conversations may be used for training. Opt out in Settings → Privacy
- Team/Enterprise: Data not used for training. Covered by BAA (Business Associate Agreement)
- Projects: Files stay within the project, not used for training

### ChatGPT (OpenAI)
- Free/Plus: Conversations used for training by default. Opt out in Settings → Data Controls
- Team/Enterprise: Data not used for training

### Google Gemini
- Free: Conversations may be reviewed by human annotators
- Workspace (business): Governed by your organization's Google Workspace agreement

## AI Use Policy Template for Teams

If your team doesn't have an AI policy, start with these rules:

1. **No customer data in AI tools** — ever
2. **No financial data in free AI tools** — paid tier with training opt-out only
3. **Anonymize before sharing** — remove names, IDs, and specifics
4. **Verify AI output before sharing externally** — AI can hallucinate facts and generate plausible but wrong information
5. **Disclose AI usage** — if AI helped create a deliverable, note it in your work

## Key Takeaway

AI is useful but your data is irreplaceable. When in doubt, anonymize. Use paid tiers for sensitive work. And always remember: AI output needs human review before it reaches clients or stakeholders.
