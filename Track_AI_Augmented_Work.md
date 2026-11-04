# Track: AI-Augmented Analytics Engineering

**A fourth parallel track for the 64-week roadmap.** It adds no extra hours. You use AI *inside* the work you already do: the SQL, Modeling and Python tracks and the 14 projects. Each phase gives you one new AI skill, so by week 64 you can hand repeatable work to AI agents and check what they produce.

*Start: week 6 (2–8 Nov 2026). AI tools change every few months, so test any feature named here before you rely on it.*

---

## 1. The one rule: learn first, delegate second

AI can be your **tutor** or your **worker**. Mixing up the two is the main way people lose their learning.

| | AI as tutor | AI as worker |
|---|---|---|
| **When** | The topic is new to you (most of Phases 1–3) | You can already do the task by hand |
| **You ask** | "Explain", "quiz me", "what's wrong with my query", "give me a hint, not the answer" | "Write", "generate", "refactor", "review", "draft" |
| **AI writes code?** | No. You write it, AI checks it | Yes. You review it, test it and are responsible for it |
| **Goal** | Understanding | Speed |

**The 3-step test before you delegate a task:**
1. Have I done this task by hand at least twice?
2. Can I tell, in under 5 minutes, whether the AI's output is right?
3. Is there a check that catches mistakes: a dbt test, row counts, a reconciliation query, CI?

Three yeses: delegate it. Any no: do it yourself with AI as a tutor.

> Why this matters for jobs: clients pay for correct numbers. An AI-written model with no tests that reports the wrong revenue costs you the client. Your value is **judgment and verification**, and AI gives you the speed.

---

## 2. The AI maturity ladder

Climb one rung per phase. Each rung depends on the ones below it.

| Rung | What you do | Tool examples | Phase |
|---|---|---|---|
| **1. Chat tutor** | Ask for explanations, quizzes and reviews of your own code | Claude chat, this Project | 1 (now) |
| **2. Context-rich assistant** | Give AI your project docs, schemas and conventions so its answers fit your work | Claude Projects, a `CONTEXT.md` per repo | 1–2 |
| **3. Coding agent in your repo** | AI reads and edits files, runs `dbt build` and fixes errors; you review the diff | Claude Code (terminal), plan mode, `CLAUDE.md` | 2 |
| **4. Reusable skills and commands** | Write a repeatable task down once ("create a staging model", "write tests for a mart") and run it any time | Claude Code skills / slash commands, prompt library | 2–3 |
| **5. AI connected to your tools** | AI queries the warehouse, reads dbt lineage and opens GitHub PRs through connectors | MCP servers (dbt MCP, Snowflake, GitHub), connectors | 4 |
| **6. Agents working unattended** | AI reviews every PR in CI, runs nightly checks, splits work across subagents | Claude Code headless (`claude -p`), Claude Code GitHub Action, subagents, scheduled tasks | 5–6 |
| **7. AI inside the data product** | LLM steps in the pipeline (classify reviews, extract fields) and AI features in BI tools | Snowflake Cortex AI SQL functions, Gemini in BigQuery, Copilot in Power BI | 6–7 |

---

## 3. Phase-by-phase plan

Each phase has: **the AI skill**, **where you use it in the roadmap**, and a **deliverable** that goes into your repos and later your Upwork story.

### Phase 1 (weeks 1–6) — Rung 1–2: AI as tutor
You are here.
- **Skill:** good learning prompts (section 5). Attempt first, then ask.
- **Use it in:** SQL S1–S3, Modeling M1–M3, Python Py1–Py3 exercises; P1 star schema review.
- **Habits:**
  - After each exercise, paste *your* answer and ask: "Review this. Don't rewrite it. List problems in order of importance and give hints."
  - Ask Claude to quiz you on a topic before a Gate self-test.
  - Have Claude explain query plans you produce in DuckDB (your DBA strength helps here).
- **Deliverable (week 6):** `CONTEXT.md` in your P1 repo: the business story, table list, naming conventions and dbt layer rules. This is the start of rung 2.

### Phase 2 (weeks 7–17) — Rung 3–4: coding agent and first skills
- **Skill:** Claude Code in your dbt repos. Start each task in plan mode, read the plan, then let it edit. Write a `CLAUDE.md` with your conventions (staging = `stg_<source>__<table>`, every primary key gets `unique` + `not_null`, no business logic in staging, etc.).
- **Use it in:** P2 Olist (week 8 onward, *after* you have written several staging models yourself), P3 GA4.
- **Tasks to delegate** (you will have done them by hand in weeks 5–8):
  - Generate `stg_` models from source tables, following your conventions.
  - Write `schema.yml` with descriptions and generic tests.
  - Explain a failing `dbt build` and propose a fix.
  - Translate MySQL SQL to Snowflake or BigQuery dialect.
- **First skills / slash commands** (week 11 to 16):
  - `/new-staging-model <source> <table>`
  - `/add-tests <model>`: adds tests and checks coverage
  - `/review-sql`: checks your style rules and SQLFluff
- **Deliverable (week 17):** a `.claude/` folder in P2 with `CLAUDE.md` and 3 skills, plus a README section "How I use AI in this project". Put the same skills in a separate GitHub repo of your own so you can reuse them for clients.

### Phase 3 (weeks 18–24) — Rung 4: AI for BI and the business side
- **Skill:** AI as a DAX tutor, then a DAX drafter; AI for writing for clients.
- **Use it in:** P4, P5.
  - Write DAX measures yourself first (week 19). From week 21, ask AI to draft measures and **always check them against a SQL query on the same mart**. Numbers must match.
  - Use AI to critique dashboard screenshots: "Act as a busy store owner. What can't you understand in 10 seconds?"
  - Draft case studies and the Project Catalog offers, then edit them into your own voice.
- **Repeatable Upwork tasks** (from week 11, refined here):
  - A **proposal skill**: input = job post + your project list; output = a short proposal that opens with the client's problem and links the one matching project. You edit and send every proposal yourself.
  - A **job-post analyzer**: pastes 10 posts → table of tools, budget and problem type. It feeds the "top requirements" review in your roadmap.
- **Deliverable (week 24):** a prompt library file (`prompts.md`) with your 10 best prompts, each with an example input and output.

### Phase 4 (weeks 25–35) — Rung 5: AI connected to your tools
- **Skill:** MCP servers and connectors, so the agent sees real metadata instead of guessing.
  - **dbt MCP server**: the agent reads your models, lineage and docs.
  - **Snowflake MCP / connector** with a **read-only role** on a dev database. Never production, never admin.
  - **GitHub**: the agent opens PRs and you review them.
- **Use it in:** P6–P8.
  - Airflow DAGs and Terraform: AI drafts, you check them with `terraform plan`. Your DevOps experience makes you a strong reviewer here.
  - P8 migration: AI generates the MySQL→Snowflake type-mapping table and reconciliation queries. **You** decide the cutover plan.
- **Security rules from now on:** a separate least-privilege role for AI, no secrets in prompts or `CLAUDE.md`, `.env` files ignored.
- **Deliverable (week 35):** a P8 "AI-assisted migration" write-up: what the agent did, what you checked, how much time it saved. Measure the time so you have a real number.

### Phase 5 (weeks 36–46) — Rung 6: agents working unattended
- **Skill:** run agents without you watching, with guardrails.
  - **Headless Claude Code** (`claude -p "..."`) in scripts: e.g. "summarize yesterday's failed Airflow tasks and suggest causes".
  - **Claude Code GitHub Action**: AI reviews every dbt PR against your `CLAUDE.md` rules and comments. Humans merge.
  - **Subagents**: split big jobs, e.g. one agent documents the Glue jobs while another writes Athena test queries.
- **Use it in:** P9 (failure/replay notes), P10 (PySpark — learn Spark by hand first in week 42).
- **Deliverable (week 46):** CI on P2/P7 with an AI review step next to SQLFluff and `dbt build`.

### Phase 6 (weeks 47–57) — Rung 7: AI inside the data product
- **Skill:** LLM functions in SQL and AI features in BI, and knowing what they cost.
  - Add an LLM step to a project: classify or score the sentiment of the Olist **review comments** with Snowflake Cortex AI SQL functions, and model the result as a dimension in dbt. Measure credits used.
  - Try Gemini in BigQuery on P3 and Copilot in Power BI (needs paid capacity, so a short trial or a demo video is enough).
- **Use it in:** P11 cost audit (AI can read query history and suggest fixes; you measure the savings), P12 starter kit (a skill that sets up a new store from your template).
- **Upwork:** add an AI offer: "AI review/feedback analysis on your warehouse data" or "AI-assisted dbt project setup".
- **Deliverable (week 57):** one project with a working LLM step + cost notes; a "Using AI on your data safely" one-pager for clients.

### Phase 7 (weeks 58–64): the capstone with an AI-run delivery
- Run P14 the way a small team would: you are the lead, AI agents do the work you assign, CI and tests are the quality gate.
- Write an ADR (architecture decision record) about where AI is used in the platform and where it is deliberately not used.
- **Deliverable:** the walkthrough video includes 60 seconds on your AI workflow. Clients are now asking about this.

---

## 4. Catalogue of repeatable tasks to hand to AI

Use it as a checklist. A task moves to "Delegate" once it passes the 3-step test in section 1.

| Task | Earliest week | How you verify it |
|---|---|---|
| Explain an error message / query plan | 1 | You can explain it back in your own words |
| Generate fake test data (Faker scripts) | 4 | Row counts, value ranges, spot checks |
| Write README / case study first draft | 6 | Read it aloud, fix the voice, check every number |
| Staging models from a source schema | 8 | `dbt build` + your naming rules |
| `schema.yml` docs and tests | 9 | Test coverage report, tests fail when data is broken on purpose |
| SQL dialect translation | 12 | Same results on both engines |
| Upwork proposal draft | 11 | You edit and send every one; track reply rate |
| Job-post research summary | 3, 11+ | Spot-check 3 posts against the summary |
| DAX measure drafts | 21 | Match against SQL on the same mart |
| Terraform / Airflow boilerplate | 30 | `terraform plan`, DAG test run |
| Reconciliation queries | 34 | Run on known-good and known-bad data |
| PR review in CI | 44 | Compare AI comments with your own review for 5 PRs |
| Cost analysis from query history | 47 | Measured before/after credits |

**Never delegate:** metric definitions with the client, the final number on a client report, cutover decisions, anything involving production credentials.

---

## 5. Prompt patterns that work

**Structure every work prompt as:** context → task → constraints → output format → how it will be checked.

**Tutor prompt (Phase 1–2):**
> I'm learning window functions (beginner). Here is my query and the result I expected vs got. Don't give me the fixed query. Tell me which concept I'm missing and give one hint. Then give me a similar exercise.

**Review prompt:**
> Review this dbt model against the conventions in CONTEXT.md. List problems in order of severity. For each: line, problem, why it matters. Don't rewrite the model.

**Worker prompt (after you know the task):**
> Create `stg_olist__orders` from source `olist.orders`. Follow CLAUDE.md conventions: rename to snake_case, cast timestamps, no joins, no business logic. Add the model to `_olist__models.yml` with `unique` and `not_null` on `order_id`. Run `dbt build -s stg_olist__orders` and fix errors until it passes. Show me the diff at the end.

**Turning it into a skill:** when you have used the same worker prompt 3 times, save it as a skill or slash command, with the variable parts (`<source>`, `<table>`) as arguments.

---

## 6. Guardrails for real client work

- **Client data:** ask the client before sending any of their data or code to an AI tool, and follow their policy. Share schemas and samples with fake values, not real customer records.
- **Credentials:** never in prompts, `CLAUDE.md` or chat. The AI gets its own read-only role.
- **Responsibility:** you sign off on everything. "The AI wrote it" is not an answer a client accepts.
- **Verification:** every AI-made model has tests; every AI-made number is reconciled against a second query.
- **Cost:** agent loops and LLM SQL functions use tokens and credits. Set budgets, the same way you set billing alarms.
- **Honesty:** be open with clients that you use AI to work faster. Many see it as a plus, as long as quality is guaranteed.

---

## 7. Weekly rhythm (no extra hours)

| When | AI use |
|---|---|
| Sun–Thu learning evenings | Tutor mode only: hints, reviews, quizzes |
| Friday project block | Worker mode for tasks that passed the 3-step test; tutor mode for new topics |
| Saturday docs | AI drafts README/case study; you edit |
| Saturday Upwork hour | Proposal skill + job-post analyzer |
| Sunday review | Add one line to `ai-log.md`: what you delegated, time saved, mistakes the AI made |

The `ai-log.md` becomes evidence for your Upwork profile ("cut model-scaffolding time by 60%") and shows you which tasks are safe to delegate.

---

## 8. Start this week (week 6)

**Warm-up (use Claude as tutor):**
1. Take one window-function exercise you already solved in S1. Ask Claude to review it *without rewriting it*. Write down one thing you learned.
2. Ask Claude to quiz you with 5 questions on facts vs dimensions (M1–M2) before Gate 1. Answer without notes.
3. Paste one of your Python Py2 scripts and ask: "What would break if the API returned an empty list?" Fix it yourself.

**Practice (build rung 2):**
1. Write `CONTEXT.md` for P1: business story (3 sentences), table list, naming rules, dbt layer rules.
2. Give Claude that file plus your P1 README draft and ask for a review from a non-technical client's point of view. Edit the README yourself.
3. Start `ai-log.md` with today's date and the first entry.

**Next milestone:** week 8 — install Claude Code, write `CLAUDE.md` for P2, and let it generate your first staging model *after* you have written two by hand.
