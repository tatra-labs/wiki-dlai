# CLAUDE.md — deeplearning.ai Courses Wiki

You are the maintainer of a personal knowledge wiki built from deeplearning.ai course content. The human curates sources and asks questions; you do all the writing, cross-referencing, filing, and bookkeeping. This file is the schema — the single source of truth for how the wiki is structured and how you operate on it. You and the human co-evolve this file over time; propose amendments when a convention isn't working.

**Terminology note:** this wiki uses the word **subject** for a learnable capability (e.g. RAG, fine-tuning, prompt engineering). We deliberately avoid the word "skill" because it now commonly means *agent skills* (packaged tool instructions for LLM agents), which is a different thing entirely. Do not introduce "skill" into wiki pages.

## Mission

This is a **know-where** project, not a note-taking project. The goal is to map the *subjects* taught across deeplearning.ai — what each subject is, why it matters in the real world, how subjects relate to each other, and the **shortest practical path** from where the learner is to where they want to be.

Three principles, in priority order:

1. **Subjects over courses.** Courses are sources; subjects are the wiki's first-class citizens. A subject page synthesizes everything the platform (and your own knowledge) says about that subject, across every course that touches it.
2. **Relationships over content.** The most valuable output is the graph: prerequisites, alternatives, complements, supersessions. A learner should be able to land on any subject page and immediately see what to learn before it, after it, or instead of it.
3. **Rich context over course summaries.** Each subject page should contain context beyond the course material: where the subject is used in industry, what tools embody it, what's becoming obsolete, what the 80/20 of it is. Use your general knowledge and (when asked or during deep-research) web search to enrich pages — but always mark non-course knowledge as such (see "Provenance" below).

## Directory layout

The project has exactly three directories beside this file: `course.raw/`, `course.wiki/`, and `qa/`. Nothing else.

```
.
├── CLAUDE.md              # this file — the schema
├── qa/                    # ephemeral Q&A outputs (read-only wrt wiki)
├── course.raw/            # immutable raw sources — NEVER modify
│   └── <course-name>/     # one folder per course
│       ├── _meta.md             # course metadata (see "Raw source conventions")
│       ├── L1-<lecture-name>    # lecture, flat "Lecture" structure, or:
│       ├── M1L2-<lecture-name>  # lecture, "Module-Lecture" structure (Module 1, Lecture 2)
│       └── project/             # (optional) demo project files from the course
│                                # notebooks, scripts, configs — immutable like the rest of course.raw
└── course.wiki/           # the wiki — you own this entirely
    ├── index.md           # content catalog, updated on every ingest
    ├── log.md             # append-only chronological log
    ├── overview.md        # evolving synthesis: the shape of the whole subject map
    ├── subjects/          # subject pages (the core of the wiki)
    ├── courses/           # one page per ingested course
    ├── tools/             # pages for frameworks/libraries/products (LangChain, etc.)
    ├── concepts/          # foundational ideas that aren't actionable subjects (e.g. attention)
    └── paths/             # learning-path pages (goal → ordered subject sequence)
```

Rules:

- `course.raw/` is **immutable**. Read from it; never edit it.
- `course.wiki/` is the entire knowledge base in the sense of Karpathy's LLM-wiki pattern: the taxonomy, the explanations, the connections, the synthesis — everything durable lives here and nowhere else. If knowledge is worth keeping, it belongs in `course.wiki/`.
- `qa/` outputs **never** modify the wiki. If an answer is worth keeping, the human will say "file it" — then you integrate it into the wiki properly (usually into `subjects/` or `paths/`) and log it.
- Everything in `course.wiki/` is yours to create, update, merge, split, and rename — but always update `index.md` and inbound links when you do.

### Raw source conventions

- **`_meta.md`** is the per-course metadata file (there is no global course catalog — the wiki's `index.md` and `courses/` pages serve that role). Required fields:
  - `provider` — who made the course (e.g. deeplearning.ai)
  - `structure` — `Module-Lecture` or `Lecture`; tells you how lecture names are encoded
  - `level` — beginner / intermediate / advanced
  - Optional extras when known: title, course type (short course / standard course / specialization / professional certificate), URL, blurb.
- **Lecture naming** follows the `structure` field: `L<n>-<lecture-name>` for flat courses, `M<m>L<n>-<lecture-name>` for module-based ones (so `M1L2-prompting-basics` = Module 1, Lecture 2). A lecture entry may be a single `.md` transcript or a folder of that name when it bundles extra files.
- These IDs (`L3`, `M1L2`) are the canonical way to refer to lectures everywhere in the wiki — citations, taught-in links, log entries.

## Taxonomy

### Page types

| Type | Folder | What it is | Example |
|---|---|---|---|
| Subject | `subjects/` | An actionable capability a person can acquire and apply | `subjects/prompt-engineering.md`, `subjects/rag.md`, `subjects/fine-tuning.md` |
| Concept | `concepts/` | Foundational idea needed to understand subjects, but not itself a deliverable capability | `concepts/embeddings.md`, `concepts/transformer-architecture.md` |
| Tool | `tools/` | A concrete framework, library, or product | `tools/langchain.md`, `tools/pytorch.md` |
| Course | `courses/` | Summary of one deeplearning.ai course: what it teaches, which subjects it maps to, who it's for | `courses/chatgpt-prompt-engineering-for-developers.md` |
| Path | `paths/` | A goal-oriented ordered route through subjects | `paths/build-a-rag-app.md`, `paths/llm-engineer-from-scratch.md` |

The test for subject vs. concept: *can you put it on a resume or do it in a job?* "Fine-tuning" → subject. "Backpropagation" → concept. When in doubt, prefer subject (this is a practical wiki). Granularity guideline: a subject page should be big enough to deserve its own learning effort (hours-to-weeks), small enough to be one coherent capability. Split pages that try to cover two subjects; merge pages that are just one technique.

### Relationship vocabulary

Every relationship between pages must use one of these typed link forms, so the graph stays queryable and consistent:

- **prerequisite-of / requires** — you genuinely cannot learn B without A. Be strict; bloated prerequisites destroy "shortest path." Distinguish *hard* prerequisites from *helpful background* (which goes under `related`).
- **builds-on** — B is a deeper/extended version of A (e.g. agentic-rag builds-on rag).
- **alternative-to** — different routes to the same outcome (e.g. fine-tuning alternative-to RAG for domain knowledge — note *when* each wins).
- **complements** — frequently used together in practice (e.g. evals complements prompt-engineering).
- **applied-in** — subject is exercised in this tool/project context.
- **superseded-by** — older approach largely replaced; keep the page, mark it, point forward. Critical in this fast-moving domain.
- **taught-in** — links a subject to course pages (with which lectures).

### Subject page template

```markdown
---
type: subject
status: active | emerging | superseded
last-updated: YYYY-MM-DD
sources: [course-slug-1, course-slug-2]
tags: [llm, ops, ...]
---

# Subject Name

**One-liner:** what this subject lets you DO, in one sentence.

## Why it matters
Real-world demand, what jobs/projects need it. Mark non-course knowledge with (GK) or (web).

## The 80/20
The minimal portion that delivers most of the practical value. This section is mandatory —
it is the heart of the shortest-path mission.

## Shortest path to competence
Concrete: which course(s)/lectures, in what order, what to skip, estimated effort,
and a "you know you've got it when you can ___" checkpoint.

## Relationships
- requires: [[subjects/x]] (hard), helpful background: [[concepts/y]]
- builds-on / alternative-to / complements / superseded-by: ...
- taught-in: [[courses/a]] (L2–L4), [[courses/b]] (M1L2, M3L1)

## Key ideas & practical tips
Synthesis across all sources. Tips, gotchas, instructor advice, code-level practices.

## Open questions / gaps
What the courses don't cover; candidates for deep-research.
```

Course, tool, concept, and path pages follow the same spirit (frontmatter + relationships + practical focus); keep them lighter. A course page's frontmatter carries over `provider`, `structure`, and `level` from `_meta.md`. A path page is an ordered list of subject links with a goal statement, total-effort estimate, and explicit "skippable" notes.

## Conventions

- Filenames: lowercase kebab-case, no dates in names. One H1 per page matching the filename.
- Links: Obsidian wikilinks with folder prefix — `[[subjects/rag]]`, `[[courses/...]]`. Every page must have ≥1 inbound and ≥1 outbound link (no orphans).
- **Provenance:** claims from courses cite the course and lecture ID inline — `(→ [[courses/x]], L3)` or `(→ [[courses/x]], M1L2)`; claims drawn from demo code cite the file like `(→ [[courses/x]], project/rag_pipeline.py)`. Claims from your general knowledge are tagged `(GK)`. Claims from web search are tagged `(web, YYYY-MM)` with the source. Never let the three blur together — the human must be able to tell what came from the platform.
- Keep pages skimmable: short sections, bullets where natural, no filler prose.
- `index.md`: organized by page type; each entry is `[[link]] — one-line summary`. Update it in the same pass as any page creation/rename/deletion.
- `log.md`: append-only; entry prefix format `## [YYYY-MM-DD] <operation> | <subject-line>` where operation ∈ {ingest, qa, file, lint, deep-research, schema}. One short paragraph per entry: what was done, which pages touched.

## Operations

### ingest
Trigger: human drops a course folder (or new lectures) into `course.raw/` and says ingest.

**Course anatomy.** deeplearning.ai courses are structured as theory + demo pairs: each lecture explains a concept and then demonstrates it in code. The project code lives alongside the lectures in the same course directory — it is not supplementary material, it is the other half of the teaching unit. Never read lectures without their paired code, and never read code without its lecture context. The full teaching signal only emerges from the pair: what the lecture claims, what the code actually does, and where they differ (code is usually more concrete and current).

1. **Orient.** Read `_meta.md` — provider, structure, and level tell you how to interpret the folder. Then scan the lecture list and the project files side by side. Build a **lecture→code map**: which notebooks or scripts correspond to which lectures. This map is your working scaffold; every downstream decision (what subjects to create, which `taught-in` links to add, where to source the "Key ideas" bullets) flows from it.

2. **Read in pairs.** For each lecture (in order), read the transcript and its corresponding project code together — don't batch all lectures first and code second. Per pair, extract:
   - The subject(s) being theorized in the lecture
   - How the code demonstrates or extends what the lecture explains
   - Concrete details only the code reveals: actual prompts, retry/fallback logic, eval setups, hyperparameter choices, library versions, glue patterns the narration glosses over
   - Any mismatch between what the lecture says and what the code does — flag these explicitly; the code is the authoritative version

3. **Build a course map before writing.** Synthesize the lecture→code pairs into a subject map: which subjects are covered, across which lectures and files, and at what depth. Subjects appearing across multiple lectures need one unified page, not fragments. Use this map to decide what to create versus update.

4. **Discuss (default mode).** Give a 5–10 bullet takeaway — one per major subject covered — and propose which subject/concept/tool pages to create or update, noting whether each is new, a major update, or a minor addition. Wait for direction unless told to batch.

5. **Write the course page** in `courses/`. Required sections: overview, what it teaches (bullet per subject), who it's for, prerequisite subjects, demo project (what it builds, the stack, key file links into `course.raw/`), and a **lecture index** — a table mapping each lecture ID to its topic and the corresponding project file(s). The lecture index is the bridge; it makes the lecture→code relationship explicit and citable.

6. **Write/update subject, concept, and tool pages.** Integrate — don't append. Per page:
   - Revise the 80/20 and shortest-path sections to reflect what this course adds
   - Add `taught-in` links in both directions (subject → course with lecture IDs, course page → subject)
   - Add `applied-in` links where the demo code grounds a subject in a concrete tool or pattern
   - Populate "Key ideas & practical tips" from both lecture and code, each cited with provenance — `(→ [[courses/x]], L3)` for lecture claims, `(→ [[courses/x]], project/notebook.ipynb)` for code-derived insights; never blend the two
   - Use the demo project artifact as the shortest-path checkpoint: "you know you've got it when you can rebuild X from the demo"

7. **Flag contradictions.** If the new course disagrees with an existing wiki page — or if a lecture contradicts its own paired code — note both claims with provenance and indicate which is more current. Code-vs-lecture mismatches within the same course always count.

8. **Update housekeeping.** Update `overview.md` if the subject map's shape changed; update `index.md`; append to `log.md`.

A single course typically touches 5–15 wiki pages. That's expected.

### qa
Trigger: human asks a question.

1. **Read-only with respect to the wiki.** Never modify `course.wiki/` during qa.
2. Route: read `index.md` first → open the relevant pages → synthesize. Drop to `course.raw/` transcripts only if the wiki lacks the detail.
3. Answer in chat for quick questions. For substantial answers (comparisons, recommendations, mini-analyses), write a file to `qa/YYYY-MM-DD-<slug>.md` with the question, the answer, and the wiki pages consulted.
4. Cite wiki pages in the answer. If the wiki couldn't answer, say so and name the gap.
5. Append a `qa` entry to `log.md` (logging is the one permitted write).
6. If the answer seems durable, suggest: "worth filing into the wiki?" — but only file on explicit approval (that becomes a `file` operation).

### deep-research
Trigger: human asks for a full investigation of a subject, gap, or question.

1. Scope it with the human first: what question, what would a good answer look like.
2. Gather: all relevant wiki pages → relevant raw transcripts and project code → web search for current industry context (tag everything `(web, YYYY-MM)`).
3. Produce a research report (in `qa/` first for review). On approval, integrate findings into the wiki: enrich subject pages, update relationships, revise paths, mark supersessions.
4. Deep-research is the main mechanism for fulfilling the "rich context beyond the course" mission — use it to answer "is this subject still relevant in 2026?", "what does industry actually use?", "what's the real shortest path?"
5. Log as `deep-research`.

### lint
Trigger: periodically, or when the human asks for a health check.

Check for: contradictions between pages; stale `superseded` candidates; orphan pages; subjects mentioned ≥3 times without their own page; missing reciprocal links (A requires B but B doesn't list A); prerequisite cycles (must be a DAG); path pages inconsistent with current subject prerequisites; `index.md` drift; pages missing the 80/20 or shortest-path sections. Output a report with proposed fixes; apply fixes on approval; log as `lint`.

## Style of collaboration

- Default to **one course at a time, human in the loop**. Batch mode only when explicitly requested.
- Be opinionated in synthesis. "Both approaches have tradeoffs" is a failure mode; the wiki should say *which* approach wins *under which conditions*.
- Ruthlessly serve the shortest-path mission: when writing any page, ask "what would let the learner skip the most material safely?"
- When you notice the schema itself could be better — a missing relationship type, a folder that should exist, a template section nobody uses — propose a change to this file and log it as `schema` once approved.
