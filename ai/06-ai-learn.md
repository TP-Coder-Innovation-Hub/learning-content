# AI Learn

## What

AI is a personalized tutor available 24/7. It can explain concepts at your level, answer follow-up questions instantly, and adapt to how you learn. The key is asking the right questions.

## How to Learn Anything with AI

### Step 1: Get a Conceptual Overview

Start broad. Understand what the thing is before diving into how it works.

```
Explain distributed tracing to me as if I'm a mid-level developer who 
understands HTTP APIs and basic monitoring but has never used tracing.
What is it? Why does it exist? What problem does it solve?
```

Adjust the "as if" to your actual level. If you're a junior, say so. If you know some related concepts, mention them.

### Step 2: Ask for Analogies

```
Give me a real-world analogy for how a message queue works. 
Then show me how each part of the analogy maps to the technical concept.
```

Analogies create mental hooks. Once you understand the analogy, the technical details stick better.

### Step 3: Go Deep with Follow-ups

```
In your explanation of Kafka, you mentioned consumer groups. 
I don't understand why a consumer would join a group vs. being standalone. 
Explain with a specific example.
```

This is where AI outperforms static documentation. You can ask "why" as many times as you need. No tutorial will judge you for not understanding on the third explanation.

### Step 4: Learn by Doing

Ask for exercises, not just explanations.

```
I understand the concept of database indexes. Give me 3 practice problems 
where I need to decide: should I add an index? What columns? What type?
Make them progressively harder. Don't give me the answers — let me try first.
```

### Step 5: Connect to What You Know

```
I understand thread pools from Java. How do goroutines in Go differ? 
What's the same? What's different? What mental model should I adjust?
```

## Questions That Work Well

- "Explain X and then give me a concrete example"
- "I understand [A]. How does [B] relate to or differ from [A]?"
- "What are the most common mistakes people make when learning X?"
- "What should I learn before X? What should I learn after?"
- "Explain X in 3 levels of detail: 1 paragraph, 1 page, deep dive"
- "I tried to implement X and got this error: [error]. What concept am I missing?"

## Questions That Don't Work Well

- "Tell me everything about X." Too broad. You get a textbook dump.
- "Is X good?" No context. You get a generic "it depends."
- "How do I master X?" Unanswerable in one response. Break it into steps.

## The Feynman Technique with AI

1. Study a concept
2. Explain it to the AI as if teaching it
3. The AI identifies gaps in your explanation
4. Go back and fill those gaps
5. Repeat until you can explain it clearly

```
I'm going to explain database normalization to you. 
Tell me what I got wrong, what I'm missing, and what's imprecise.

[Your explanation]
```

## Common Mistakes

- Reading AI explanations without practicing. Understanding is not the same as ability. Build something.
- Not asking follow-up questions. If something is unclear, ask again in a different way.
- Only using AI for syntax and API lookups. Use it for concepts and mental models — that's where it adds the most value.
- Treating AI explanations as authoritative. Cross-reference with documentation and experienced practitioners.
