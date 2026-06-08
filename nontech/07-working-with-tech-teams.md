# Working with Tech Teams

## What

Effective collaboration between business and technical teams is a skill. The gap isn't about intelligence — it's about language, context, and expectations. This guide bridges that gap.

## Communication

### Be Specific About What You Want

Vague: "We need a better login experience."
Specific: "Users should be able to log in with email and password, and optionally with Google. Password reset should take under 2 minutes. We're losing 30% of users at the login step."

The second version gives the tech team something concrete to design and build toward. The first version could mean anything.

### Explain the Why, Not Just the What

"We need a dashboard" tells developers what to build but not why. The why shapes the solution.

"The sales team needs to see daily revenue by region so they can identify underperforming markets and adjust their approach weekly. Currently they export to Excel and spend 3 hours every Monday on manual calculations."

Now the team can evaluate options: a full dashboard, a scheduled email report, or an improved export might all solve the problem.

### One Decision Maker Per Topic

If three stakeholders give conflicting requirements, the tech team stops and waits for alignment. Assign one person to own each feature's requirements.

## Writing Requirements

### Good Requirements Are

- **Specific** — "The search should return results within 2 seconds for queries up to 100 characters"
- **Measurable** — "Support 500 concurrent users" not "handle a lot of traffic"
- **Prioritized** — Must have vs. nice to have. Not everything is priority 1.
- **Complete** — Include edge cases: "What happens if the payment fails? What if the user is offline?"

### Bad Requirements Are

- "Make it like [competitor]" — Which part? All of it? Your business is different.
- "Fast" — Fast means different things to different people. Give a number.
- "User-friendly" — To whom? A developer? A first-time user? Your grandmother?
- "Just figure it out" — This leads to building the wrong thing and reworking it later.

### Use Examples

Instead of describing abstract rules, walk through scenarios:

"A customer adds 3 items to their cart: a $20 shirt, a $50 jacket, and a $10 pair of socks. They apply a 20% off coupon. The discount applies to the shirt and jacket only (socks are excluded). Their total is $64. They pay with a credit card. The order is confirmed and they get an email."

This is more useful than: "Implement cart with coupon support and payment."

## Understanding Estimates

### Why Estimates Are Hard

Software development involves unknowns. The team is estimating work that hasn't been done before, in a codebase they may not fully know, with requirements that may change.

### How to Read Estimates

- **1-2 days** — Well-understood, straightforward work
- **1-2 weeks** — Standard feature, some unknowns
- **1+ months** — Complex feature or significant unknowns. Ask for a breakdown.
- **"We need to investigate first"** — The team doesn't know enough to estimate. That's honest, not evasive.

### How to Help

- Provide clear, complete requirements
- Make decisions promptly when the team asks questions
- Don't change requirements mid-sprint
- Accept that estimates are estimates, not guarantees

## Common Misunderstandings

- "Can't you just..." — Adding a feature is rarely "just" anything. It touches design, backend, database, testing, and deployment.
- "It works for [competitor], why can't we do it?" — You see the feature. You don't see the years of infrastructure, the team of 50 engineers, and the technical debt they're managing.
- "Why does fixing a bug take longer than building the feature?" — Building is creating something new with known rules. Debugging is finding where reality doesn't match your expectations, in a system of thousands of interacting parts.
