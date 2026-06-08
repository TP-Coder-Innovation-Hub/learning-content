# AI Privacy

## What

Everything you type into an AI tool potentially becomes data the provider can use, store, or expose. Understanding what to never share is critical for protecting yourself, your company, and your customers.

## The Golden Rule

If you wouldn't post it on a public forum, don't put it into an AI tool unless you have explicit confirmation that your data is not used for training and is adequately protected.

## What to Never Put Into AI

### Company Confidential Information

- Proprietary code that gives your company a competitive advantage
- Internal system architecture details
- Business strategies and roadmaps
- Financial data and projections
- Customer lists and data

### Personally Identifiable Information (PII)

- Names, addresses, phone numbers
- Social security or government ID numbers
- Email addresses (unless your own for testing)
- Health information
- Financial account numbers

### Authentication Secrets

- Passwords
- API keys
- Access tokens
- Private keys
- Database connection strings with credentials

### Legal and Compliance Sensitive Data

- Contract terms and negotiations
- Pending litigation details
- Regulated data (healthcare HIPAA data, financial data under SOX/PCI)
- Export-controlled information

## How to Work Safely with AI

### Sanitize Before Sharing

Replace sensitive values with placeholders before pasting into AI.

```
# Bad
const dbPassword = "xK9$mP2vL5nQ";

# Good (what you share with AI)
const dbPassword = process.env.DB_PASSWORD;
```

```
# Bad
User Alice Smith (alice@company.com, SSN: 123-45-6789) reported an error.

# Good
A user reported an error when trying to update their profile.
The error message is: [error message]
```

### Use Company-Approved Tools

Many organizations have approved AI tools with enterprise agreements that:
- Do not train on your data
- Provide data isolation
- Offer audit logging
- Comply with relevant regulations

Use these instead of consumer-grade AI tools for work tasks.

### Check the Terms

Before using any AI tool for work:
1. Does it train on your inputs?
2. Can you opt out?
3. Where is data processed?
4. How long is data retained?
5. Is there an enterprise option with better privacy?

### Use Local Models When Appropriate

For highly sensitive code or data, consider running AI models locally. No data leaves your machine. Trade-off: local models are typically less capable than cloud-hosted ones.

## Company Policies

If your company has an AI usage policy, follow it. If it doesn't, advocate for one. A clear policy protects everyone.

A good policy covers:
- Which tools are approved
- What data can and cannot be shared
- Who to ask for guidance
- What to do if you accidentally share sensitive data

## Common Mistakes

- Pasting error logs that contain real user data, stack traces with internal hostnames, or environment variables. Always sanitize.
- Sharing code that includes hardcoded credentials "just for context." Remove them first.
- Assuming free tools have the same privacy protections as paid enterprise plans. They usually don't.
- Not checking whether your company has a policy. Ignorance of the policy is not a defense.
