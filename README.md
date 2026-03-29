# Professional Career Notebook

**AI-powered Brag Document** — a personal repository that transforms career records into a structured knowledge base, ready to be consumed by AI agents.

> The Brag Document concept was popularized by [Julia Evans (2019)](https://jvns.ca/blog/brag-documents/) as a living document to record achievements, contributions, and professional impact. This repository takes the idea further: by structuring content in Markdown within a Git repo, it becomes **rich context for AI agents** (like [Cursor](https://cursor.com) and [Claude Code](https://claude.ai/code)), enabling automations that save hours of work.

---

## What this repository does

Instead of maintaining a static document, this repository organizes your entire professional trajectory — experiences, skills, storytellings, technical content, and hiring processes — so that an AI agent can cross-reference information and generate high-quality outputs.

**Real use cases:**

- **Hyper-customized resume per job** — the AI cross-references the job description with your experiences and generates a resume emphasizing the relevant matches
- **Interview preparation** — behavioral question suggestions with answers based on real experiences, and technical concepts to review with references to previously studied content
- **Performance self-assessment** — fill evaluation forms with concrete evidence and metrics, in minutes
- **Learning management** — records of courses, events, and studies that feed back into all other use cases

---

## Repository structure

```
professional-career-notebook/
│
├── source-of-truth/              # Primary source of information
│   ├── career-tracking/          # Career tracking by company
│   │   └── {company-name}/       # One directory per company (kebab-case)
│   │       ├── achievements/     # Quarterly achievements
│   │       ├── performance-cycles/ # Performance review cycles
│   │       └── 3ps/              # Weekly Progress/Plans/Problems
│   ├── personal-professional-profile.md
│   ├── work-experience.md
│   ├── relevant-experiences.md
│   ├── storytellings.md
│   ├── personal-projects.md
│   └── academic-projects-experiences.md
│
├── knowledge-base/               # Technical and study content
│   ├── algorithms/               # Algorithm exercises and notes
│   ├── architecture/             # Software architecture
│   ├── artificial-intelligence/  # AI and AI workflows
│   ├── courses/                  # Course and workshop notes
│   ├── english-training/         # English practice
│   ├── software-engineering/     # General software engineering
│   ├── system-design-interview/  # System design preparation
│   └── tech-leads-club/          # Tech Leads Club community content
│
├── hiring-processes/             # Hiring processes
│   ├── in-progress/              # Active processes
│   └── completed/                # Completed (history)
│
├── interview-preparation/        # Interview preparation (templates and guides)
├── guidelines/                   # Resume tips, copywriting, job search, and recruiter communication
├── salary-compensations/         # Compensation references
├── linkedin-posts/               # Drafts and published posts
├── resumes/                      # PDF resumes and presentation materials
├── resume-generator/             # Tool to generate PDF resumes from YAML
│
├── .cursor/
│   ├── rules/                    # Always-apply rules for AI behavior
│   └── skills/                   # Conditional skills (specific automations)
│
└── README.md
```

### Key folders

| Folder | Purpose |
|---|---|
| `source-of-truth/` | **Canonical data** about profile, experiences, and storytellings. This is the source the AI prioritizes to generate any output. |
| `source-of-truth/career-tracking/` | **Career tracking by company** — quarterly achievements, performance review cycles, and weekly progress reports (3Ps). One directory per company. |
| `knowledge-base/` | **Technical knowledge base.** Everything studied (courses, system design, algorithms) is recorded here and automatically reused in interview preparation and resume generation. |
| `hiring-processes/` | **Complete record of each hiring process** — job description, fit analysis, generated resume, interview preparation, and outcome. |
| `.cursor/rules/` | **Global rules** that define the AI's tone, format, and behavior in every interaction. |
| `.cursor/skills/` | **Conditional automations** — workflows the AI executes for specific tasks (register hiring process, generate resume, etc.). |

---

## How it works

The general flow follows this cycle:

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│   Experience logging              Study logging             │
│   (source-of-truth/)              (knowledge-base/)         │
│         │                                │                  │
│         └──────────┐    ┌────────────────┘                  │
│                    ▼    ▼                                   │
│              ┌──────────────┐                               │
│              │ AI Agent     │                               │
│              └──────┬───────┘                               │
│                     │                                       │
│         ┌───────────┼───────────┐                           │
│         ▼           ▼           ▼                           │
│    Customized   Interview   Performance                     │
│    resume       preparation evaluation                      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

1. **Feed the repository** with professional experiences, storytellings, projects, and study content
2. **The AI uses the context** from rules and skills to understand how to process each type of task
3. **Generate outputs** — resumes, interview preparations, evaluation responses — all based on real and verifiable data

---

## How to use as a template

If you want to create your own AI-powered Brag Document, the minimum structure to get started is:

```
my-brag-document/
├── source-of-truth/
│   ├── profile.md                # Your professional profile (summary, skills, languages)
│   ├── work-experience.md        # Work experiences with metrics and impact
│   └── storytellings.md          # Stories formatted in STAR (Situation, Task, Action, Result)
├── knowledge-base/               # Optional: study notes and technical content
├── hiring-processes/             # Optional: hiring process records
└── AGENTS.md                    # Behavioral instructions for AI agents
```

**Tips to get started:**

- Start with `source-of-truth/` — list your experiences with **concrete metrics and impact**
- Use the **STAR method** (Situation, Task, Action, Result) for storytellings
- Update biweekly — each update takes less than 1 minute
- Record "invisible work": mentoring, code review, refactoring, process improvements
- Share with your manager — it makes evaluations and career conversations easier

---

## References

- [Get your work recognized: write a brag document — Julia Evans (2019)](https://jvns.ca/blog/brag-documents/)
- [Career tip: create a brag document — Elton Minetto (2022)](https://eltonminetto.dev/post/2022-04-14-brag-document/)
- [Boost your career with a brag sheet — Erica Pisani (2023)](https://ericapisani.dev/boost-your-career-with-a-brag-sheet/)
- [Get promoted faster with an AI-written brag doc — The AI-Augmented Engineer (2025)](https://www.augmentedswe.com/p/how-to-write-a-brag-doc-using-ai)
