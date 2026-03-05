# ProPrompt – The Guide to Effective Prompting

> **Target Audience:** Anyone who wants to use GitHub Copilot, Copilot Studio Agents, or AI-powered toolchains productively – no prior AI experience required.

---

## Table of Contents

1. [Markdown Quick Start](#1-markdown-quick-start)
2. [Prompting Fundamentals](#2-prompting-fundamentals)
3. [Dos & Don'ts – Overview](#3-dos--donts--overview)
4. [Prompting in Daily Work (Chat & Edit)](#4-prompting-in-daily-work-chat--edit)
5. [Prompting in Agent Mode](#5-prompting-in-agent-mode)
6. [Building Copilot Studio Agents](#6-building-copilot-studio-agents)
7. [Designing Agent Toolchains](#7-designing-agent-toolchains)
8. [Office Files → LLM-Friendly Structures](#8-office-files--llm-friendly-structures)
9. [Files & Formats – Best Practices](#9-files--formats--best-practices)
10. [Instruction Files & Custom Instructions](#10-instruction-files--custom-instructions)
11. [Cheat Sheet](#11-cheat-sheet)

---

## 1 Markdown Quick Start

Markdown is the standard format for documentation and AI context files. Here are the essentials:

### Text Formatting

```markdown
# Heading 1
## Heading 2
### Heading 3

**Bold**  
*Italic*  
~~Strikethrough~~  
`Inline Code`
```

### Lists

```markdown
- Bullet point 1
- Bullet point 2
  - Sub-item

1. Numbered list
2. Second item
```

### Code Blocks

````markdown
```python
def hello():
    print("Hello World")
```
````

### Links & Images

```markdown
[Link text](https://example.com)
![Alt text](image.png)
```

### Tables

```markdown
| Column A | Column B |
|----------|----------|
| Value 1  | Value 2  |
```

### Why Markdown for AI?

- LLMs understand Markdown structure natively
- Headings create clear hierarchy → better context
- Code blocks are recognized as code (syntax highlighting)
- Tables convey structured data compactly

---

## 2 Prompting Fundamentals

### What Is a Prompt?

A prompt is the **instruction** you send to an AI model. The clearer and more structured your prompt, the better the output.

### The 4 Pillars of a Good Prompt

| Pillar | Description | Example |
|--------|-------------|---------|
| **Role** | Who should the AI be? | *"You are an experienced C# developer."* |
| **Context** | What background does the AI need? | *"We're building a .NET 8 Web API."* |
| **Task** | What exactly should be done? | *"Create a controller for User CRUD."* |
| **Format** | How should the output look? | *"Output the code with XML comments."* |

### The RICE Framework

> **R**ole → **I**nstruction → **C**ontext → **E**xpected Output

```
Role:        You are a Senior DevOps Engineer.
Instruction: Create a Dockerfile for a Node.js 20 app.
Context:     The app uses pnpm, has a /src directory, and needs port 3000.
Expected:    Multi-stage Dockerfile with comments.
```

---

## 3 Dos & Don'ts – Overview

### ✅ DOs

| # | Do | Why |
|---|-----|-----|
| 1 | **Be specific** | "Write a TypeScript function that sorts an array" > "write me code" |
| 2 | **Provide context** | Include language, framework, version, architecture |
| 3 | **Define output format** | "Return JSON", "Use bullet points", "Create a table" |
| 4 | **Work iteratively** | Start with skeleton, then refine in follow-up prompts |
| 5 | **Use examples (Few-Shot)** | Show 1–2 examples of desired output |
| 6 | **Limit the scope** | One prompt = one clear task |
| 7 | **Use Markdown in prompts** | Headings, lists, and code blocks for structure |
| 8 | **Reference files** | Use `#file:src/service.ts` in Copilot Chat |
| 9 | **Review the output** | Always review AI output, never blindly accept |
| 10 | **Use Custom Instructions** | `.github/copilot-instructions.md` for project-wide rules |

### ❌ DON'Ts

| # | Don't | Why |
|---|-------|-----|
| 1 | **Vague prompts** | "Make it better" → No clear goal |
| 2 | **Too much at once** | "Build me a complete app" → Overwhelming |
| 3 | **Forget context** | Without language/framework, the AI guesses |
| 4 | **Blind copy-paste** | Always read and understand the code |
| 5 | **Enter sensitive data** | No real passwords, API keys, or customer data |
| 6 | **Expect perfection first try** | Iterative prompting is normal |
| 7 | **Use negations** | "Don't use var" → Better: "Use const and let" |
| 8 | **Overload the context window** | Don't paste entire codebases into one prompt |
| 9 | **Switch prompt languages** | Stay in one language per conversation |
| 10 | **Agent mode for trivial tasks** | Simple edits don't need an agent |

---

## 4 Prompting in Daily Work (Chat & Edit)

### Copilot Chat – Best Practices

**Use Slash Commands:**

| Command | Function |
|---------|----------|
| `/explain` | Get code explanations |
| `/fix` | Fix errors |
| `/tests` | Generate tests |
| `/doc` | Create documentation |
| `/new` | Scaffold new projects/files |

**Context Variables:**

| Variable | Description |
|----------|-------------|
| `#file` | Reference a specific file |
| `#selection` | Reference selected code |
| `#editor` | Current editor content |
| `#codebase` | Search entire project |
| `#terminalLastCommand` | Reference last terminal command |

### Example Prompts for Daily Work

**Code Review:**
```
Review #selection for:
1. Potential bugs
2. Performance issues
3. Best practice violations
Provide improvement suggestions as a diff.
```

**Refactoring:**
```
Refactor the function in #file:src/utils/parser.ts:
- Extract validation logic into a separate function
- Use early returns instead of nested if-blocks
- Keep the existing signature
```

**Debugging:**
```
The following error occurred: #terminalLastCommand
Analyze the error in the context of #file:src/app.ts and suggest a fix.
```

---

## 5 Prompting in Agent Mode

### What Is Agent Mode?

Agent mode in VS Code allows Copilot to **autonomously** perform multiple steps:
- Read, create, and edit files
- Run terminal commands
- Work across multiple files
- Detect and self-correct errors

### When to Use Agent Mode?

| Scenario | Agent ✅ | Chat ❌ |
|----------|---------|--------|
| New feature across multiple files | ✅ | |
| Refactoring an entire module | ✅ | |
| Debugging with terminal access | ✅ | |
| Writing a single function | | ❌ Chat suffices |
| Quick explanation | | ❌ Chat suffices |

### Effective Agent Prompting

**Structure for Agent Prompts:**

```markdown
## Goal
[What should be achieved at the end?]

## Context
[Relevant architecture, technologies, constraints]

## Steps
1. [First step]
2. [Second step]
3. [Third step]

## Requirements
- [Non-functional requirement 1]
- [NFR 2]

## Do Not
- [Explicit exclusions]
```

**Practical Example:**

```markdown
## Goal
Create a REST API route for user registration.

## Context
- Express.js with TypeScript
- Prisma ORM with PostgreSQL
- Existing structure in /src/routes/ and /src/services/

## Steps
1. Create the Prisma schema for User (email, passwordHash, createdAt)
2. Create the service in /src/services/userService.ts
3. Create the route in /src/routes/auth.ts
4. Add input validation with zod
5. Write tests in /src/__tests__/auth.test.ts

## Requirements
- Hash passwords with bcrypt
- Email validation
- Follow existing code conventions

## Do Not
- No changes to existing routes
- Do not introduce a new ORM
```

### Agent Mode Tips

1. **Use instruction files** – `.github/copilot-instructions.md` is loaded automatically
2. **Scope tasks clearly** – 3 focused agent sessions beat one massive one
3. **Set checkpoints** – Review changes after each step
4. **Watch terminal output** – Agent runs commands that may have side effects
5. **Use undo** – VS Code can revert agent changes

---

## 6 Building Copilot Studio Agents

### What Is Copilot Studio?

Microsoft Copilot Studio lets you build custom AI agents without code – for Teams, SharePoint, Web, and more.

### Prompt Design for Studio Agents

**Structure the System Prompt:**

```markdown
# Role
You are [Name], an assistant for [Purpose].

# Capabilities
- You can [Capability 1]
- You can [Capability 2]
- You have access to [Data Source]

# Behavior
- Always respond in [Language]
- Use a [formal/informal] tone
- Maximum [X] sentences per response

# Boundaries
- Do NOT answer questions about [Topic]
- When uncertain, say: "[Fallback Text]"

# Output Format
- Use bullet points for lists
- Link to [Sources] when possible
```

**Practical Example – IT Helpdesk Agent:**

```markdown
# Role
You are IT-Helper, the internal IT support assistant for Contoso Corp.

# Capabilities
- Resolve common IT issues (password reset, VPN, printers)
- Search the internal knowledge base
- Create support tickets

# Behavior
- Respond in English
- Use friendly, patient language
- Ask clarifying questions when the issue is unclear
- Provide step-by-step instructions

# Boundaries
- No changes to production systems
- No access to personal data
- For hardware issues: escalate to physical support

# Escalation
If you cannot resolve the issue, create a ticket with:
- Problem description
- Steps taken so far
- Priority (Low/Medium/High)
```

### Topics & Trigger Phrases

| Topic | Trigger Phrases |
|-------|----------------|
| Password Reset | "forgot password", "reset password", "can't log in" |
| VPN Issues | "VPN not working", "network connection", "remote access" |
| Printer | "printer not printing", "add printer", "paper jam" |

---

## 7 Designing Agent Toolchains

### What Is an Agent Toolchain?

A toolchain connects multiple agents and tools into an automated workflow.

### Architecture Patterns

```
[User Request]
       ↓
[Orchestrator Agent]
       ↓
  ┌────┼────┐
  ↓    ↓    ↓
[Agent A] [Agent B] [Agent C]
(Research) (Code)   (Review)
  ↓    ↓    ↓
  └────┼────┘
       ↓
[Summary]
       ↓
[User Response]
```

### Designing Toolchain Prompts

**Orchestrator Prompt:**
```markdown
You are the Orchestrator. Your task:
1. Analyze the user request
2. Decide which agents are needed
3. Invoke agents in the correct order
4. Combine results into a coherent response

Available Agents:
- @research – Searches data sources
- @code – Writes and reviews code
- @review – Checks quality and security
```

**Specialist Agent Prompt:**
```markdown
You are the Code Agent. You receive tasks from the Orchestrator.

Input Format:
- Task: [Description]
- Context: [Relevant files/info]
- Constraints: [Limitations]

Output Format:
- Status: [Success/Error]
- Result: [Code/Explanation]
- Confidence: [High/Medium/Low]
```

### Practical: Power Automate + Copilot Studio

```
[Teams Message]
  → [Copilot Studio Agent] (Classification)
    → [Power Automate Flow]
      → [Update SharePoint List]
      → [Send Email]
      → [Post Teams Channel Message]
```

---

## 8 Office Files → LLM-Friendly Structures

### The Problem

LLMs cannot directly read `.docx`, `.xlsx`, or `.pptx` files. These must be converted to text-based formats.

### Conversion Guide

#### Word (.docx) → Markdown

**Manual:**
1. Open the document in Word
2. File → Save As → **"Plain Text (.txt)"** or use Pandoc
3. Add Markdown heading structure afterward

**With Pandoc (recommended):**
```bash
pandoc input.docx -t markdown -o output.md
```

**With Python:**
```python
from docx import Document

def docx_to_markdown(filepath):
    doc = Document(filepath)
    md_lines = []
    for para in doc.paragraphs:
        if para.style.name.startswith('Heading 1'):
            md_lines.append(f"# {para.text}")
        elif para.style.name.startswith('Heading 2'):
            md_lines.append(f"## {para.text}")
        elif para.style.name.startswith('Heading 3'):
            md_lines.append(f"### {para.text}")
        elif para.style.name == 'List Bullet':
            md_lines.append(f"- {para.text}")
        else:
            md_lines.append(para.text)
    return "\n\n".join(md_lines)
```

#### Excel (.xlsx) → Markdown/CSV

**Export as CSV:**
1. File → Save As → **CSV (Comma delimited)**

**As Markdown table (Python):**
```python
import pandas as pd

def xlsx_to_markdown(filepath, sheet_name=0):
    df = pd.read_excel(filepath, sheet_name=sheet_name)
    return df.to_markdown(index=False)
```

**Tip:** For large tables, extract only relevant columns/rows:
```python
df = pd.read_excel("data.xlsx")
# Only relevant columns
subset = df[["Name", "Status", "Date"]].head(50)
context = subset.to_markdown(index=False)
```

#### PowerPoint (.pptx) → Markdown

```python
from pptx import Presentation

def pptx_to_markdown(filepath):
    prs = Presentation(filepath)
    md_lines = []
    for i, slide in enumerate(prs.slides, 1):
        md_lines.append(f"## Slide {i}")
        for shape in slide.shapes:
            if shape.has_text_frame:
                for para in shape.text_frame.paragraphs:
                    if para.text.strip():
                        md_lines.append(para.text)
        md_lines.append("")  # Blank line
    return "\n".join(md_lines)
```

#### PDF → Text

```bash
# With pdftotext (poppler-utils)
pdftotext input.pdf output.txt

# Or with Python
pip install PyPDF2
```

```python
from PyPDF2 import PdfReader

def pdf_to_text(filepath):
    reader = PdfReader(filepath)
    text = ""
    for page in reader.pages:
        text += page.extract_text() + "\n"
    return text
```

### Quality Checklist After Conversion

- [ ] Headings correctly transferred?
- [ ] Tables readable and formatted?
- [ ] Lists preserved?
- [ ] Images described as alt text?
- [ ] Footnotes/references carried over?
- [ ] Special characters correct?

---

## 9 Files & Formats – Best Practices

### Project Structure for AI Context

```
my-project/
├── .github/
│   └── copilot-instructions.md    ← Project-wide AI rules
├── .copilot/
│   ├── context.md                 ← Architecture & tech stack
│   ├── conventions.md             ← Code conventions
│   └── glossary.md                ← Domain terminology
├── docs/
│   ├── architecture.md            ← System architecture
│   ├── api.md                     ← API documentation
│   └── decisions/                 ← Architecture Decision Records
│       └── 001-database-choice.md
├── src/
│   └── ...
└── README.md
```

### copilot-instructions.md – Example

```markdown
# Project: Contoso Web App

## Tech Stack
- Frontend: React 18 + TypeScript 5
- Backend: .NET 8 Web API
- Database: PostgreSQL 16
- ORM: Entity Framework Core

## Code Conventions
- Use PascalCase for C# classes and methods
- Use camelCase for TypeScript variables and functions
- All API endpoints return `ApiResponse<T>`
- Error handling via global exception middleware

## Architecture
- Clean Architecture (Domain → Application → Infrastructure → API)
- CQRS with MediatR for commands and queries
- Repository pattern for data access

## Rules
- Write unit tests for all new services
- Do not use `var` in C# – prefer explicit types
- All DTOs are `record` types
- API versioning via URL path (/api/v1/)
```

### Context Files for Agents

**For Copilot Studio – Structure Knowledge Bases:**

```markdown
# FAQ – Product XY

## Common Questions

### How do I reset my password?
1. Go to https://portal.contoso.com
2. Click "Forgot Password"
3. Enter your email address
4. Follow the link in the confirmation email

### How do I request a new laptop?
1. Open a ticket in the IT portal
2. Category: "Hardware Request"
3. Describe your requirements
4. Manager approval required
```

---

## 10 Instruction Files & Custom Instructions

### Levels of Configuration

```
┌────────────────────────────────────┐
│  1. VS Code Settings (global)       │  → Applies to all projects
├────────────────────────────────────┤
│  2. .github/copilot-instructions.md │  → Applies to the project
├────────────────────────────────────┤
│  3. .copilot/*.md                   │  → Context files per topic
├────────────────────────────────────┤
│  4. Inline prompt context           │  → Applies to the single request
└────────────────────────────────────┘
```

### VS Code Custom Instructions

In `settings.json`:
```json
{
  "github.copilot.chat.codeGeneration.instructions": [
    { "text": "Always use TypeScript strict mode." },
    { "text": "Prefer functional programming." },
    { "file": ".copilot/conventions.md" }
  ]
}
```

### Instruction File Best Practices

1. **Keep it short and precise** – One rule per line
2. **Phrase positively** – "Use X" instead of "Don't use Y"
3. **Prioritize** – Most important rules first
4. **Keep it current** – Review and update regularly
5. **Team consensus** – Involve all team members

---

## 11 Cheat Sheet

### Prompt Templates – Ready to Copy

**Explain Code:**
```
Explain #selection step by step. Focus on:
- What does the code do?
- What edge cases exist?
- How could it be improved?
```

**Find Bugs:**
```
Analyze #file for potential bugs:
1. Null reference errors
2. Race conditions
3. Missing error handling
4. Memory leaks
```

**Write Tests:**
```
Write unit tests for #file:
- Use [Jest/xUnit/pytest]
- Test happy path and error cases
- Use Arrange-Act-Assert pattern
- Mock external dependencies
```

**API Documentation:**
```
Create an OpenAPI 3.0 documentation for #file.
Include:
- All endpoints with methods
- Request/response schemas
- Example payloads
- Error codes
```

**Agent – New Feature:**
```
## Goal
[Feature description]

## Context
- Project: [Name]
- Tech Stack: [Technologies]
- Relevant files: #file:... #file:...

## Task
1. [Step 1]
2. [Step 2]
3. [Write tests]
4. [Update documentation]

## Rules
- Follow existing architecture
- No breaking changes
- All tests must pass
```

---

## Further Reading

- [GitHub Copilot Docs](https://docs.github.com/en/copilot)
- [Copilot Studio Docs](https://learn.microsoft.com/en-us/microsoft-copilot-studio/)
- [Prompt Engineering Guide](https://www.promptingguide.ai/)
- [Markdown Guide](https://www.markdownguide.org/)
- [Pandoc](https://pandoc.org/)

---

> **License:** MIT – Free to use and modify.  
> **Contributing:** Pull requests and issues are welcome!
