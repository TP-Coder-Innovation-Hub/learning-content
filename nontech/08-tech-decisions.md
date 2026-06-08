# Tech Decisions

## What

Technology decisions — whether to build or buy, which vendor to choose, which technology to adopt — have lasting impact. This guide provides a framework for making these decisions with your eyes open.

## Build vs Buy

### When to Build

- The software is core to your competitive advantage
- No existing solution fits your specific requirements
- You need full control over the technology and roadmap
- You have the team to build and maintain it long-term

### When to Buy

- The problem is solved well by existing products
- The software is not your competitive advantage (email, accounting, CRM)
- Speed to market matters more than customization
- Your team is small and needs to focus on core product

### The Hidden Cost of Building

Building is not a one-time cost. You also pay for:
- Ongoing maintenance and bug fixes
- Security updates and dependency upgrades
- Feature requests and changes
- Onboarding new developers onto the custom system
- Infrastructure and hosting
- Opportunity cost (what your team could have built instead)

Rule of thumb: Building costs 3-5x the initial development over 5 years.

## Evaluating Vendors

### Functional Fit

- Does it solve your actual problem? (Not the demo problem — your problem)
- Can you test it with your real data before committing?
- What's missing from the demo that you'll need?
- How customizable is it?

### Technical Fit

- Does it integrate with your existing systems?
- What are the API capabilities?
- What's the data migration path?
- What's the vendor's uptime SLA?
- Where is data stored? (Important for compliance)

### Financial Fit

- Total cost over 3 years (not just the first year discount)
- Cost at scale (pricing often changes as you grow)
- Hidden costs (implementation, training, add-ons)
- Exit costs (how do you get your data out?)

### Vendor Health

- Are they profitable or burning investor money?
- What's their customer retention rate?
- How responsive is their support? (Test this before buying)
- What's their product roadmap? Does it align with your needs?
- What happens if they get acquired or go out of business?

## Understanding Trade-offs

Every technology choice involves trade-offs. There is no best option — only the best option for your specific context.

### Common Trade-offs

| Trade-off                    | Question to Ask                                    |
|------------------------------|----------------------------------------------------|
| Speed vs Quality             | Can we ship fast and iterate, or does it need to be right the first time? |
| Flexibility vs Simplicity   | Do we need to handle many scenarios or just the common ones? |
| Cost vs Reliability          | What's the cost of downtime?                        |
| Custom vs Standard           | Does customization give us a real advantage?        |
| New vs Proven                | Are we comfortable with the risk of new technology? |

### How to Decide

1. **List the options** — Usually 2-4 realistic choices
2. **Define criteria** — What matters most? Cost, speed, reliability, flexibility?
3. **Weight the criteria** — Not everything is equally important
4. **Score each option** — Against each criterion
5. **Check for deal-breakers** — Any option that fails a non-negotiable requirement is out
6. **Make the call** — With imperfect information. Waiting for perfect information is itself a decision.

## Common Mistakes

- Choosing technology because it's trendy, not because it fits the problem.
- Not planning for the exit. If you can't migrate away from a vendor, they have leverage over pricing and roadmap.
- Over-optimizing for the current state. Choose for where you'll be in 2 years, not where you are today. But don't over-engineer for hypothetical scale either.
- Deciding by committee. Collective input is good. Collective decision-making is slow. Assign a decision maker.
