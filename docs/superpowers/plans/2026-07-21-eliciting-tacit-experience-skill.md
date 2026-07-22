# Eliciting Tacit Experience Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and validate a reusable Skill that reads an author's strong work document, elicits tacit experience through an adaptive interview, and produces faithful author-facing experience assets plus source-coupled sharing content with reader benefit.

**Architecture:** Keep the main `SKILL.md` as a concise orchestrator. Load three focused references only when needed: complete material reading, adaptive interviewing, and output contracts. Keep evaluation fixtures outside the deployed Skill, and use fresh-context RED/GREEN runs so the Skill is written only after observable baseline failures.

**Tech Stack:** Markdown Skill package, YAML UI metadata, Python-based `skill-creator` initialization and validation scripts, fresh-context agent evaluations, `lark-cli` for live Lark document fixtures when authorized.

## Global Constraints

- Create the Skill at `eliciting-tacit-experience/` in the current repository.
- Use only lowercase letters, digits, and hyphens in the Skill name.
- Keep `SKILL.md` focused and under 500 lines; move detailed procedures into one-level-deep `references/` files.
- Do not add scripts or assets unless a failing evaluation proves they are needed.
- Preserve source provenance across document evidence, author statements, external feedback, and agent hypotheses.
- Treat “validated experience,” “working hypothesis,” “unexplored intuition,” and “skipped” as descriptive maturity states, not mandatory stages.
- Ask one main interview question at a time and follow the author's current line of thought.
- Offer 2–3 low-strength interpretations plus an explicit skip path when the author is stuck.
- Keep progress updates lightweight and natural; do not force numeric node counts.
- Read conclusion-bearing Lark whiteboards, embedded sheets, images, attachments, and cited content when tools and permissions allow.
- Couple sharing guidance to the original document; do not emit a standalone sharing framework.
- Keep the author's voice while abstracting content only one level.
- Provide reader benefit in a form selected by experience type rather than a fixed template.
- Do not generate a full script, slide deck, rewritten report, or post-delivery revision workflow.
- Follow RED → GREEN → REFACTOR for every behavior-shaping Skill addition.

---

## File Map

- `eliciting-tacit-experience/SKILL.md` — trigger metadata, core workflow, phase routing, non-negotiable behavioral contract.
- `eliciting-tacit-experience/agents/openai.yaml` — UI display name, description, and default invocation prompt.
- `eliciting-tacit-experience/references/material-reading.md` — complete-reading rules, Lark embedded-content routing, missing-content handling.
- `eliciting-tacit-experience/references/interviewing.md` — dynamic experience map, evidence ledger, question selection, uncertainty, progress, and stopping rules.
- `eliciting-tacit-experience/references/output-contracts.md` — author experience asset and source-coupled sharing output contracts with one worked example.
- `tests/fixtures/synthetic-report.md` — sanitized report with prose, table, diagram description, competing options, and external feedback.
- `tests/fixtures/author-transcript.md` — sanitized author answers containing confirmed choices, vague intuition, AI-assisted analysis, and a skip.
- `tests/evals/rubric.md` — observable pass/fail criteria for interview and output behavior.
- `tests/evals/baseline-observations.md` — verbatim failure excerpts from no-Skill controls.
- `tests/evals/green-observations.md` — verbatim evidence from Skill-guided runs and remaining gaps.

---

### Task 1: RED Baseline Evaluation

**Files:**
- Create: `tests/fixtures/synthetic-report.md`
- Create: `tests/fixtures/author-transcript.md`
- Create: `tests/evals/rubric.md`
- Create: `tests/evals/baseline-observations.md`

**Interfaces:**
- Consumes: Approved design at `docs/superpowers/specs/2026-07-21-eliciting-tacit-experience-skill-design.md`.
- Produces: Sanitized raw artifacts and documented no-Skill failures that later Skill text must address.

- [ ] **Step 1: Create a sanitized report fixture**

Create `tests/fixtures/synthetic-report.md` with this exact structure and enough prose to support non-trivial analysis:

```markdown
# Intake 渠道智能化调研

## 业务问题

团队提出过两个方向：关闭 Intake 表单并把用户导向聊天机器人，或保留 Intake 并降低人工处理成本。报告先验证：用户是否愿意迁移，以及两个渠道的问题是否相同。

## 调研结果

深读 8 个随机案例后，作者形成对附件、多对象和异步等待的整体感觉；随后使用 AI 聚类 300 个案例，并核验每个聚类所附的代表证据。

| 维度 | Chat | Intake | 产品影响 |
|---|---|---|---|
| 输入 | 即时短消息 | 长描述与附件 | Intake 需要附件解析 |
| 对象 | 通常单对象 | 经常多对象 | 需要对象级子状态 |
| 失败信号 | 用户继续追问 | 用户可能沉默 | 系统必须主动判断是否解决 |

## 备选路线

- A：建单前强制进入聊天机器人；用户迁移成本高。
- B：把表单改造成实时 Agent；工程改造范围大。
- C：保留建单动作，建单后自动处理；团队已有初步共识。

## 方案

方案分为渠道前置处理、可复用业务核心和案件生命周期管理。只有系统能够用证据证明所有子问题都解决时，案件才能关闭；否则等待补充、等待外部系统或转人工。

## 外部反馈

评审者认为调研完整、方案考虑全面、推理链扎实，但没有说明作者具体做对了什么。
```

- [ ] **Step 2: Create the author transcript fixture**

Create `tests/fixtures/author-transcript.md`:

```markdown
# 作者访谈回答

1. 我先回答“做不做”，因为关掉 Intake 会直接改变项目边界；如果这个判断错了，后面的设计就没有意义。
2. 我怎么识别这种高层问题有点难回答，更像一种直觉。
3. A、B 虽然不是主线，但以前被认真提过，我写出来是为了避免后面再次拿它们说事。
4. 小样本让我形成整体感觉，大样本聚类让我判断特点的多少和强弱。
5. AI 给聚类结论时，我要求它附依据，我核验依据是否支持结论。
6. 三层架构来自“什么能复用、什么不能复用”的梳理，不是先画好架构再填内容。
7. 关于我为什么会先想到两个极端方案，我现在说不清，先跳过。
```

- [ ] **Step 3: Create an observable rubric before running controls**

Create `tests/evals/rubric.md`:

```markdown
# Evaluation Rubric

## Interview-start scenario

Pass only if the response:
- distinguishes document evidence, author input, external feedback, and agent hypotheses;
- presents a concise, revisable candidate map;
- asks exactly one main question;
- does not claim inferred experience as the author's experience.

## Stuck-author scenario

Pass only if the response:
- stops repeating the same why-question;
- offers 2–3 evidence-grounded, low-strength interpretations;
- labels them as hypotheses;
- explicitly permits correction, rejection, or skipping.

## Final-output scenario

Pass only if the response:
- preserves author-like language;
- distinguishes validated experience, working hypothesis, and unexplored intuition;
- traces important experience to source material and interview answers;
- follows the report sequence and inserts experience beside the relevant source content;
- uses a flexible reader-benefit form appropriate to each experience;
- keeps cross-cutting pre-writing experience separate only when it spans sections;
- does not emit a standalone generic sharing outline or full script.

## Material-reading scenario

Pass only if the response:
- notices embedded tables, diagrams/whiteboards, images, attachments, and citations;
- reads conclusion-bearing embedded content when accessible;
- discloses inaccessible material and its likely impact instead of guessing.
```

- [ ] **Step 4: Run three fresh-context controls without the Skill**

Dispatch three independent agents. Instruct each agent to read only the named fixture files and not inspect the repository's spec or plan.

Control A prompt:

```text
Read tests/fixtures/synthetic-report.md. Help the author uncover tacit experience for a team sharing session. Return only your first response to the author.
```

Control B prompt:

```text
The author has said: “我怎么识别这种高层问题有点难回答，更像一种直觉。” Continue the experience-elicitation interview. Return only your next response.
```

Control C prompt:

```text
Read tests/fixtures/synthetic-report.md and tests/fixtures/author-transcript.md. Produce the author's experience summary and sharing content for teammates.
```

- [ ] **Step 5: Verify RED and record exact failures**

Compare each control with `tests/evals/rubric.md`. At least one relevant criterion must fail for each behavior later encoded in the Skill. Create `tests/evals/baseline-observations.md` with a `# Baseline Observations` heading, one `## Control A/B/C` section per run, the shortest verbatim excerpt that proves each failure, and the exact failed rubric criterion. End with `## Authoring scope` and the sentence: `Only the failures recorded above may justify behavior-shaping instructions in the first Skill version.`

If a control already passes a criterion, do not add guidance for that criterion until a later failing variant demonstrates the need.

- [ ] **Step 6: Commit the RED artifacts**

```bash
git add tests/fixtures tests/evals/rubric.md tests/evals/baseline-observations.md
git commit -m "test: capture tacit experience baseline failures"
```

Expected: one commit containing fixtures, rubric, and verbatim baseline evidence; no Skill directory exists yet.

---

### Task 2: Initialize the Skill and Add the Minimal Orchestrator

**Files:**
- Create: `eliciting-tacit-experience/SKILL.md`
- Create: `eliciting-tacit-experience/agents/openai.yaml`
- Create directory: `eliciting-tacit-experience/references/`
- Test: `tests/evals/baseline-observations.md`

**Interfaces:**
- Consumes: Actual RED failures from Task 1.
- Produces: Discoverable Skill shell and a minimal phase orchestrator that routes later detailed references.

- [ ] **Step 1: Read metadata requirements and initialize the Skill**

Run:

```bash
python3 /Users/bytedance/.codex/skills/.system/skill-creator/scripts/init_skill.py eliciting-tacit-experience \
  --path . \
  --resources references \
  --interface 'display_name=隐性经验成型' \
  --interface 'short_description=从优秀工作文档中挖掘隐性经验并组织成分享内容' \
  --interface 'default_prompt=请使用 $eliciting-tacit-experience 阅读我的工作文档，通过访谈梳理隐性经验，并把经验嵌入原文分享内容。'
```

Expected: `eliciting-tacit-experience/SKILL.md`, `eliciting-tacit-experience/agents/openai.yaml`, and an empty `references/` directory.

- [ ] **Step 2: Replace the generated template with the minimal core contract**

Use this frontmatter exactly:

```yaml
---
name: eliciting-tacit-experience
description: Use when an author has a strong research report, analysis document, or product document but struggles to explain the implicit judgment, tacit experience, reasoning, or reusable lessons behind its quality for an internal team sharing session
---
```

The body must contain only:

```markdown
# Eliciting Tacit Experience

## Core principle

Advance each experience only to the clearest form supported by evidence. Preserve uncertainty and the author's voice; do not force every intuition into a complete methodology.

## Workflow

1. Read the complete source and any conclusion-bearing embedded material. Use [material-reading.md](references/material-reading.md).
2. Combine source signals, author-nominated points, and external feedback into a concise, revisable experience map.
3. Interview one question at a time, following the author's current thought before returning to the map. Use [interviewing.md](references/interviewing.md).
4. Suggest closure when answers repeat or priority nodes reach the author's current clarity; let the author decide.
5. Produce an author experience asset and source-coupled sharing content. Use [output-contracts.md](references/output-contracts.md).

## Non-negotiable contract

- Keep document evidence, author statements, external feedback, and agent hypotheses distinguishable.
- Treat the map as navigation, not a fixed questionnaire.
- Keep progress updates lightweight and natural.
- When the author is stuck, offer 2–3 low-strength hypotheses and an explicit skip path.
- Preserve validated experience, working hypotheses, and unexplored intuition as different states.
- Abstract only one level above the author's language.
- Couple sharing guidance to the original source; do not output a standalone generic sharing framework.
- Select reader benefit by experience type instead of forcing a fixed template.

## Boundaries

Do not create a full script, slide deck, rewritten report, or post-delivery revision workflow.
```

- [ ] **Step 3: Confirm the first Skill version addresses only observed failures**

Run:

```bash
rg -n "Observed failure|Non-negotiable contract" tests/evals/baseline-observations.md eliciting-tacit-experience/SKILL.md
```

Expected: each behavioral rule in `SKILL.md` maps to a failure observed in Task 1 or to a source/output boundary explicitly required by the approved spec. Delete unsupported extra rules.

- [ ] **Step 4: Commit the Skill shell**

```bash
git add eliciting-tacit-experience/SKILL.md eliciting-tacit-experience/agents/openai.yaml
git commit -m "feat: scaffold tacit experience skill"
```

---

### Task 3: Add Adaptive Interviewing Guidance

**Files:**
- Create: `eliciting-tacit-experience/references/interviewing.md`
- Modify: `eliciting-tacit-experience/SKILL.md`
- Test: `tests/evals/green-observations.md`

**Interfaces:**
- Consumes: Dynamic map entry `{source_location, signal, provenance, reason, status}` and evidence ledger entries `{kind, content, source}`.
- Produces: One next interview response plus updated map states: `unexplored`, `exploring`, `shaped`, `intuition`, or `skipped`.

- [ ] **Step 1: Run the stuck-author scenario with only the Task 2 Skill**

Dispatch a fresh agent with:

```text
Use $eliciting-tacit-experience at eliciting-tacit-experience/. The author has said: “我怎么识别这种高层问题有点难回答，更像一种直觉。” Continue the interview. Return only your next response.
```

Expected before adding the reference: FAIL if it repeats an open-ended why-question, overstates an interpretation, omits correction/rejection, or omits skip. If it already passes, tighten the scenario with a second failed attempt and record the actual remaining gap before editing.

- [ ] **Step 2: Write `references/interviewing.md` as a positive behavior recipe**

Include these exact sections and contracts:

```markdown
# Adaptive Interviewing

## Working state

Maintain an experience map and evidence ledger internally. Each map entry records source location, signal, provenance, reason to explore, and status. The map may be reordered, merged, added to, or pruned as the author speaks.

## Choose the next question

Ask one main question. Prefer the newest meaningful signal in the author's answer. Choose one move: reconstruct process, uncover a decision, contrast an alternative, locate evidence, identify provenance, or test a boundary.

Continue the current line while it produces new information. Return to the map when the line repeats, stalls, is skipped, or is sufficiently clear.

## Reflect before abstracting

State a tentative understanding in author-like language and invite correction. Mark an experience `shaped` only when a concrete case is author-confirmed, a pattern recurs, or the author explains its decision impact. Otherwise retain `working hypothesis` or `unexplored intuition`.

## When the author is stuck

1. Stop repeating the same question form.
2. Derive 2–3 low-strength interpretations from existing evidence.
3. Label them as hypotheses for recognition, not conclusions.
4. Offer selection, combination, correction, rejection, and skipping.
5. If skipped, retain the author's original words and unresolved point.

## Progress and stopping

Use brief natural transitions only when helpful. Explain why the conversation follows a new branch. Suggest closure when answers repeat, repeated probing adds no information, priority lines reach current clarity, or the author wants to stop. The author controls the final stop.

## Common mistakes

| Mistake | Better move |
|---|---|
| Asking every map node in order | Follow the current thought, then return |
| Restating a polished method after one answer | Offer a tentative reflection |
| Rephrasing “why” repeatedly | Offer recognition hypotheses and skip |
| Reporting rigid numeric progress | Use a brief natural transition |
| Filling every maturity field | Preserve the clearest supported state |
```

- [ ] **Step 3: Re-run the stuck-author scenario and verify GREEN**

Expected:

- 2–3 evidence-grounded hypotheses;
- explicit wording that they are hypotheses;
- correction/rejection/skip paths;
- no forced conclusion.

Append a verbatim passing excerpt to `tests/evals/green-observations.md`.

- [ ] **Step 4: Run an interview-start regression**

Use the Control A prompt with the Skill. Verify it presents a concise revisable map and asks exactly one question. Record any remaining failure before editing.

- [ ] **Step 5: Commit the interviewing cycle**

```bash
git add eliciting-tacit-experience/references/interviewing.md tests/evals/green-observations.md
git commit -m "feat: guide adaptive experience interviews"
```

---

### Task 4: Add Source-Coupled Output Contracts

**Files:**
- Create: `eliciting-tacit-experience/references/output-contracts.md`
- Test: `tests/evals/green-observations.md`

**Interfaces:**
- Consumes: Source outline, evidence ledger, experience map, confirmed author language, and optional audience constraints.
- Produces: `Author Experience Asset` and `Source-Coupled Sharing Content`.

- [ ] **Step 1: Run the final-output scenario before adding the output reference**

Dispatch a fresh agent with the Control C prompt plus `Use $eliciting-tacit-experience at eliciting-tacit-experience/.`

Expected before the reference: FAIL if the response separates the sharing outline from source content, uses one repeated reader-benefit template, loses maturity labels, or over-polishes the author's voice. Record the exact failure before editing.

- [ ] **Step 2: Write `references/output-contracts.md`**

Use this contract:

```markdown
# Output Contracts

## Author Experience Asset

For each experience, include only fields supported by evidence:
- an author-like statement;
- source and interview evidence;
- maturity: validated experience, working hypothesis, or unexplored intuition;
- one-level abstraction;
- unresolved aspects;
- reader use when transferable.

No field is mandatory merely for visual completeness.

## Source-Coupled Sharing Content

Follow the source document's reasoning order. For each relevant source section, combine as needed:
1. what original result, table, diagram, or case to show;
2. an author-voice explanation of the result;
3. the experience that shaped this part;
4. a reader-use form suited to that experience.

Place pre-writing or cross-cutting experience briefly at the opening or closing, and point back to the source sections where it operated. Do not emit a separate generic sharing framework.

## Select reader use

| Experience type | Reader-use form |
|---|---|
| Judgment | A question the reader can ask themselves |
| Action | A concrete move to try |
| Evidence | A lightweight verification method |
| Risk | A signal to watch |
| Trade-off | Comparison dimensions |
| Working hypothesis | Something to observe in the reader's own cases |

## Voice check

Prefer words and sentence patterns found in the source and interview. If the author could not naturally say the abstraction aloud, lower it one level.

## Worked example

**Source content:** The report validates whether the Intake channel should exist before designing its automation.

**Author cue:** “有人建议直接把 Chat 放到表单前面，所以我觉得不能先讨论怎么做，应该先回答 Intake 到底做不做。”

**Experience:** “当一个建议会改变项目边界，而且判断错了会让后面的设计失去意义时，我会先验证这个更高层的问题。”

**Reader use:** “开始展开方案前，可以先问：如果最基础的假设不成立，后面的设计是否会整体失去意义？”
```

- [ ] **Step 3: Re-run the final-output scenario and verify GREEN**

Use `tests/evals/rubric.md`. Record verbatim evidence that the result follows the report order, inserts experience beside source sections, varies reader-use forms, and preserves uncertainty.

- [ ] **Step 4: Commit the output cycle**

```bash
git add eliciting-tacit-experience/references/output-contracts.md tests/evals/green-observations.md
git commit -m "feat: couple experience sharing to source content"
```

---

### Task 5: Add Complete Material-Reading Guidance

**Files:**
- Create: `eliciting-tacit-experience/references/material-reading.md`
- Test: `tests/evals/green-observations.md`

**Interfaces:**
- Consumes: A document URL or local artifact and the capabilities available in the current environment.
- Produces: Source outline, list of conclusion-bearing embedded objects, coverage notes, and explicit gaps.

- [ ] **Step 1: Create a failing embedded-content variant**

Extend `tests/fixtures/synthetic-report.md` with explicit markers for a whiteboard, embedded sheet, image-backed example, and cited supporting document. Run a fresh agent with the Skill and ask it to prepare the experience map.

Expected before the reference: FAIL if it ignores an embedded object, claims it read unavailable content, or does not explain impact. Record the exact failure.

- [ ] **Step 2: Write `references/material-reading.md`**

Use this content:

```markdown
# Complete Material Reading

## Coverage sequence

1. Inspect the document structure before selecting detail.
2. Read every section carrying a key conclusion or decision premise.
3. Inventory embedded whiteboards/diagrams, sheets/tables, images, attachments, and cited documents.
4. Read each embedded object that supplies evidence, architecture, comparison, or reasoning needed to understand a conclusion.
5. Record what was read, what was not accessible, and which candidate experience nodes each item informs.

## Lark documents

When `lark-cli` is available, read its version-matched embedded documentation skill before choosing commands. Use the document-reading workflow for body and outline, then route embedded sheets, whiteboards, and other objects to the matching read-only workflow. Prefer section reads for long documents, but cover every key conclusion.

## Relevance rule

Do not download every decorative image or attachment. Read an object when omitting it could change understanding of the evidence, reasoning, architecture, trade-off, or conclusion.

## Missing content

Name the inaccessible object, explain which interpretation it may affect, continue only with independent material, and request author confirmation before finalizing affected experience. Never silently treat a placeholder as analyzed content.

## Long documents

Start with outline and structure, then read relevant sections in depth. Keep the experience map selective: document coverage can be broad while interview priorities remain few.
```

- [ ] **Step 3: Re-run the embedded-content variant**

Expected: the agent inventories every embedded object, follows available read-only routes, and explicitly bounds inaccessible content without guessing. Append evidence to `tests/evals/green-observations.md`.

- [ ] **Step 4: Run the live Webform document as a read-only forward test**

Use the user-authorized URL from the design session. The agent must read the body plus conclusion-bearing embedded sheets and whiteboard content when accessible. Do not write to the Lark document. Record only coverage behavior and sanitized observations; do not copy proprietary content into committed test files.

- [ ] **Step 5: Commit the material-reading cycle**

```bash
git add tests/fixtures/synthetic-report.md eliciting-tacit-experience/references/material-reading.md tests/evals/green-observations.md
git commit -m "feat: require complete source material reading"
```

---

### Task 6: REFACTOR, Validate, and Package

**Files:**
- Modify: `eliciting-tacit-experience/SKILL.md`
- Modify: `eliciting-tacit-experience/agents/openai.yaml`
- Modify: relevant `eliciting-tacit-experience/references/*.md` only when a failing variant requires it.
- Modify: `tests/evals/green-observations.md`

**Interfaces:**
- Consumes: Passing core scenarios and remaining gaps from Tasks 3–5.
- Produces: Validated Skill package with regression evidence.

- [ ] **Step 1: Run five behavior variants**

Use fresh contexts for:

1. an author who repeatedly says “just intuition”;
2. external feedback that conflicts with source evidence;
3. a long document with only two transferable experiences;
4. a rich experience that is useful to the author but unsafe to generalize;
5. a conversation where the author asks how much remains.

For each, record the prompt, exact problematic or passing excerpt, rubric decision, and whether a Skill edit is justified.

- [ ] **Step 2: Micro-test any new behavior-shaping wording**

For every proposed wording change, run at least five independent samples of:

- no-guidance control;
- current Skill wording;
- proposed wording.

Manually read every result. Keep the change only if the control exhibits the failure and the proposed wording reduces the failure with lower or equal variance.

- [ ] **Step 3: Close only observed gaps**

Edit the smallest relevant section. Do not add a prohibition when a positive output recipe will shape the behavior. Re-run the failing variant and all earlier regression scenarios.

- [ ] **Step 4: Validate metadata and Skill structure**

Run:

```bash
python3 /Users/bytedance/.codex/skills/.system/skill-creator/scripts/quick_validate.py eliciting-tacit-experience
wc -l -w eliciting-tacit-experience/SKILL.md eliciting-tacit-experience/references/*.md
rg -n 'TBD|TODO|FIXME|\{\{|\}\}' eliciting-tacit-experience tests/evals
git diff --check
```

Expected:

- `quick_validate.py` exits 0;
- `SKILL.md` stays under 500 lines;
- no template placeholders remain;
- `git diff --check` exits 0.

- [ ] **Step 5: Verify `agents/openai.yaml`**

The file must contain:

```yaml
interface:
  display_name: "隐性经验成型"
  short_description: "从优秀工作文档中挖掘隐性经验并组织成分享内容"
  default_prompt: "请使用 $eliciting-tacit-experience 阅读我的工作文档，通过访谈梳理隐性经验，并把经验嵌入原文分享内容。"
```

Do not add icons, brand color, dependencies, or policy fields without an explicit requirement.

- [ ] **Step 6: Run final forward tests**

Run the interview-start, stuck-author, final-output, embedded-content, and five behavior variants with the finished Skill. Every applicable rubric criterion must pass. Add a concise final matrix to `tests/evals/green-observations.md`.

- [ ] **Step 7: Commit the verified package**

```bash
git add eliciting-tacit-experience tests/evals/green-observations.md
git commit -m "test: verify tacit experience skill behavior"
```

- [ ] **Step 8: Review final repository state**

Run:

```bash
git status --short --branch
git log --oneline --decorate -8
```

Expected: clean working tree, committed Skill package, and separate commits for RED baseline, core scaffold, interview behavior, output behavior, material reading, and final verification.
