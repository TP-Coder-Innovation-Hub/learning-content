# Using Claude Cowork — Your AI That Does Work For You

## What You'll Learn

- What Claude Cowork is and when to use it (vs regular chat)
- Setting up Cowork: folders, instructions, permissions
- Real workflows: scheduled reports, file organization, research briefs
- Safety and permissions

## What Is Claude Cowork

Regular Claude chat: you ask, Claude answers, one turn at a time.

Claude Cowork: you describe an outcome, Claude works on it for minutes — reading files, creating documents, running multi-step tasks — while you step away.

Think of it as the difference between asking a colleague a question vs assigning them a task.

```mermaid
graph LR
    A["You: describe task"] --> B["Cowork: plans steps"]
    B --> C["Cowork: reads files"]
    C --> D["Cowork: creates output"]
    D --> E["You: review result"]
```

## When to Use Cowork vs Chat

| Use Chat When | Use Cowork When |
|---------------|-----------------|
| Quick question, one answer | Multi-step task with files |
| Brainstorming, ideation | Creating documents, reports |
| Learning, explaining | Organizing files, batch processing |
| Simple edits | Research across multiple documents |

## Setup: 5 Minutes

### Requirements
- Claude Desktop app (not web)
- Paid plan: Pro, Max, Team, or Enterprise
- macOS or Windows

### Step 1: Open Cowork

In Claude Desktop, click the **Cowork** icon in the left sidebar.

### Step 2: Set Global Instructions

Go to Settings → Cowork → Global Instructions. This tells Claude how you like to work:

```
Language: Always respond in Thai unless I ask for English.
Tone: Professional but conversational.
Format: Use bullet points and tables when possible.
Output: Always save files to /Users/[you]/Documents/claude-output/
```

These apply to every Cowork session automatically.

### Step 3: Grant Folder Access

When starting a session, select which folders Claude can access. Claude can only see folders you explicitly grant access to.

- Grant: your Documents, project folders, desktop
- Don't grant: system folders, password managers, sensitive client data

## Real Workflows

### Workflow 1: Weekly Report Generation

```
Read all files in /Reports/week-27/ and create a weekly summary with:
1. Key metrics table (revenue, users, conversion)
2. Top 3 wins this week
3. Top 3 challenges
4. Action items for next week

Save as weekly-report-W27.md in /Reports/output/
```

Claude reads each file, extracts data, and generates a formatted report. Takes 2-3 minutes instead of 30.

### Workflow 2: File Organization

```
In /Downloads/, find all PDF invoices from 2026.
Organize them into folders by month: 2026-01, 2026-02, etc.
Create a summary spreadsheet with: filename, date, amount, vendor.
Save as invoice-tracker-2026.xlsx
```

### Workflow 3: Research Brief

```
Read all 5 PDF files in /Research/competitor-analysis/.
Create a research brief that:
1. Summarizes each document in 3 sentences
2. Creates a comparison table of features
3. Identifies 3 gaps in our knowledge
4. Recommends next research steps

Save as competitor-brief.md
```

## Scheduled Tasks (Automation)

You can schedule tasks that run automatically:

1. In any Cowork chat, type `/schedule`
2. Describe the task and schedule (daily, weekly, monthly)
3. Claude runs it automatically when your computer is on

Example: "Every Monday at 9 AM, check the /Reports/incoming/ folder and create a summary of new files."

Note: Scheduled tasks only run when your computer is awake and Claude Desktop is open.

## Permissions

Cowork has two modes:

| Mode | Behavior | Best For |
|------|----------|----------|
| **Plan** | Claude asks before making changes | Important files, first-time tasks |
| **Auto** | Claude executes without asking | Repeated tasks, non-critical files |

Start in Plan mode. Switch to Auto only after you trust the workflow.

## Cost Warning

Cowork uses significantly more usage than chat. Complex file tasks can consume your daily allocation quickly. Reserve Cowork for multi-step tasks that genuinely benefit from file access and extended execution.

## Key Takeaway

Cowork is for tasks, not conversations. Give it a clear outcome, grant it access to the right files, and review the result. Start with Plan mode, schedule recurring work, and upgrade to Auto mode only when you trust the output.
