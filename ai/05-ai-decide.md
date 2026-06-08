# AI Decide

## What

AI is useful for decision-making — not as the decision maker, but as a thinking partner. It can play devil's advocate, surface considerations you missed, and help you structure your analysis.

## AI as Devil's Advocate

Ask AI to argue against your position. This tests the strength of your thinking.

```
I'm deciding to use PostgreSQL for our new microservice. 
Here's my reasoning: [your reasoning].

Argue against this decision. What am I missing? What could go wrong? 
What would make this a bad choice?
```

The AI will surface concerns you haven't considered. Some will be valid. Some won't. The value is in forcing yourself to defend your position.

## AI as Brainstorming Partner

Generate options you haven't thought of.

```
We need to reduce our API response time from 800ms to under 200ms. 
Current stack: Node.js, PostgreSQL, Redis cache.

Brainstorm 10 different approaches. Include unconventional options.
For each, note: expected impact, implementation effort, risks.
```

You get a breadth of ideas fast. Then you evaluate them with your domain knowledge.

## Decision Frameworks with AI

### Pros/Cons Analysis

```
Compare using Kafka vs RabbitMQ for our order processing pipeline.

Format as a table with columns:
- Criterion
- Kafka
- RabbitMQ
- Winner

Criteria: throughput, latency, ordering guarantees, operational complexity,
ecosystem maturity, cost at our scale (10K messages/sec)
```

### Scenario Planning

```
I'm choosing between hiring a senior engineer vs promoting a junior engineer 
to tech lead. Describe what happens in 6 months under each scenario if:
1. Things go well
2. Things go poorly
3. The person leaves
```

### Reversibility Check

```
I'm considering migrating from REST to gRPC for internal services.
Is this decision easily reversible? What would a rollback look like?
What are the one-way doors?
```

## What AI is Good For

- **Generating alternatives** — You think of 3 options. AI gives you 7.
- **Challenging assumptions** — "What if the opposite is true?"
- **Structuring analysis** — Frameworks, comparison tables, scoring matrices
- **Stress-testing plans** — "What could go wrong with this approach?"

## What AI is Bad For

- **Making the final call** — AI doesn't know your team, your culture, your constraints
- **Intuition and judgment** — These come from experience, not pattern matching
- **Understanding context** — The real reasons behind a decision are often unwritten
- **Accountability** — AI cannot take responsibility for a bad decision

## The Rule

AI informs decisions. People make them. Use AI to widen your perspective, not to narrow your accountability.

If you find yourself saying "the AI said we should," you're using it wrong. The right framing is: "After considering multiple perspectives, including AI-generated analysis, I recommend..."

## Common Mistakes

- Asking AI to make the decision for you. "Should we use React or Vue?" leads to a generic non-answer. "Help me think through React vs Vue for our specific case" is productive.
- Accepting AI's analysis without adding your own expertise. AI doesn't know your team's skills, your timeline, or your business constraints.
- Only using AI to confirm what you already believe. If you only ask for supporting arguments, you're seeking validation, not insight.
