# Prompting

## What

Prompting is how you communicate with AI. It is not about finding magic words. It is about structuring your request so the AI has the context and constraints to give you useful output.

## The Thinking Framework

Every effective prompt has four parts:

### 1. Role

Tell the AI what perspective to take.

```
You are a senior backend engineer reviewing code for production readiness.
You are a technical writer creating documentation for junior developers.
You are a security auditor looking for vulnerabilities.
```

The role sets the tone, depth, and focus of the response.

### 2. Context

Give the AI the background it needs.

```
We have a Node.js Express API with PostgreSQL. The /users endpoint is 
returning 500 errors intermittently. We see "connection timeout" in the 
logs. Our database has a connection pool of 10.
```

Without context, the AI gives generic advice. With context, it gives specific advice.

### 3. Constraints

Define boundaries.

```
- Use only the dependencies already in package.json
- Solution must work with PostgreSQL 14
- Do not suggest switching to a different database
- Keep the response under 500 words
- Format as a numbered list
```

Constraints prevent the AI from going off on tangents.

### 4. Output Format

Tell the AI what shape you want the answer in.

```
Return a JSON object with fields: diagnosis, steps, code_changes.
Write a bullet list of 3-5 action items.
Provide a before/after code comparison.
```

## Techniques

### Few-Shot Examples

Show the AI what good output looks like.

```
Classify these support tickets:

Ticket: "I can't log in" → Category: AUTH
Ticket: "Page loads slowly" → Category: PERFORMANCE
Ticket: "My order didn't arrive" → Category: ?
```

One or two examples dramatically improve accuracy.

### Chain-of-Thought

Ask the AI to reason step by step.

```
Debug this error. Think through:
1. What the error message means
2. What could cause it
3. How to verify each cause
4. The most likely fix
```

This produces more accurate answers because the AI generates reasoning before the conclusion.

### Iterative Refinement

Your first prompt will not be perfect. Refine based on the output.

```
Prompt 1: "Write a function to validate emails"
Output: Too simple, misses edge cases

Prompt 2: "Write an email validation function that handles: 
international domains, plus addressing, subdomains. 
Return specific error messages for invalid formats."
Output: Much better
```

## Anti-Patterns

- **Wall of text prompts.** Break complex requests into steps. Ask one thing at a time.
- **Vague instructions.** "Make it better" is not actionable. "Reduce the function from 50 lines to 20 by extracting helper functions" is.
- **Not specifying what you don't want.** Negative constraints are powerful: "Do not use external libraries."
- **Asking once and accepting.** Treat the first response as a draft. Iterate.

## The Real Skill

Prompting is not a separate skill. It is clear thinking expressed in writing. If you cannot explain what you want to a colleague, you cannot prompt an AI for it either. The AI just responds faster.
