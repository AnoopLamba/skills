---
name: problem-handoff
description: >
  Create self-contained, evidence-first problem handoffs when an existing
  problem needs to be transferred to another human, LLM, coding agent,
  research agent, reviewer, or new session. Use when the user or an agent
  asks to share, package, hand off, delegate, escalate, or prepare an
  already-known problem for someone else to investigate or continue.
  Also use when requesting an independent second opinion or delegating a
  specific hypothesis for investigation. Preserve verified evidence,
  reproduction details, relevant context, prior experiments, constraints,
  current state, and unknowns. For independent investigation, avoid
  transmitting the current investigator's preferred diagnosis, solution,
  or search direction. For explicitly directed investigation, preserve
  the requested hypothesis or approach but label it as a question to
  evaluate rather than an established fact.
---

# Problem Handoff

Create a self-contained problem statement that allows another human or agent to understand and investigate an existing problem without needing the full originating conversation, session, or investigation history.

The handoff should preserve useful investigation progress while avoiding accidental distortion, unsupported certainty, and unnecessary solution anchoring.

## Core principle

**Transfer the problem, evidence, and investigation state — not merely the current investigator's conclusion.**

A good handoff enables the recipient to understand:

- what is being attempted;
- what is going wrong;
- what evidence exists;
- what has actually been verified;
- what has already been tried;
- what constraints must remain true;
- what remains unresolved;
- what they are being asked to investigate.

Do not force the recipient to reconstruct the problem from conversation history.

Do not convert assumptions into facts.

## When to use this skill

Use this skill when an already-known problem needs to be transferred to another person, agent, model, or session.

Typical triggers include:

- "Share the problem we're facing."
- "Prepare this issue so I can send it to another LLM."
- "Write this up for another developer."
- "Give this problem to a research agent."
- "Delegate this issue to another agent."
- "Prepare a neutral brief for a second opinion."
- "Summarize the current problem for a new Codex/Claude session."
- "Escalate this issue with enough context for someone else to investigate."
- "Prepare the problem without telling the next agent our theory."
- "Ask another agent to investigate this specific hypothesis."

The skill may be used for debugging, architecture, production incidents, API failures, infrastructure issues, data inconsistencies, performance problems, security investigations, source/research retrieval problems, implementation disagreements, technical research questions, and other existing problems that need to be handed off.

## Do not use this skill when

Do not invoke this skill merely because a problem exists.

Do not use it for ordinary debugging, normal research, implementation work, code review, feature specification, implementation planning, architecture design, initial problem discovery, initial problem framing, generic "what is the problem?" questions, or rewriting an already complete problem statement for grammar or tone — unless the problem is specifically being **packaged, transferred, delegated, escalated, or prepared for another human, agent, model, or session**.

Examples that should normally **not** trigger this skill:

> "Why is this endpoint returning 500?"

> "Fix this race condition."

> "Research the best database for this application."

> "Review this implementation plan."

> "Write a PRD for this feature."

Handle those tasks directly unless a handoff is explicitly required.

## 1. Determine the handoff intent

Before writing the problem statement, determine which mode applies.

### Mode A — Independent investigation

Use this when the recipient is expected to independently diagnose, research, or solve the problem.

Examples:

- "Write this problem so I can ask another LLM."
- "Give me this issue to send to another developer."
- "Delegate this to another agent and see what it finds."
- "Get a second opinion."
- "Prepare a neutral problem statement."
- "Send this to a research agent for independent investigation."

In this mode, preserve evidence and investigation progress while avoiding conclusions that could unnecessarily anchor the recipient.

Normally exclude:

- preferred solution;
- preferred architecture;
- unverified root-cause conclusion;
- instructions about which component must be responsible;
- instructions about what solution to search for;
- previous LLM recommendations;
- speculative explanations from the current investigator.

If an existing hypothesis is necessary to understand an experiment, include only the minimum context needed to explain the experiment.

Bad:

> We believe the cache invalidation system is causing the bug. Investigate how to fix the cache.

Better:

> Disabling the cache during one experiment prevented the observed failure. It is not yet established whether the difference was caused by caching, invalidation behavior, timing, or another correlated component.

### Mode B — Directed investigation

Use this when the caller explicitly wants the recipient to investigate a particular hypothesis, technology, component, architecture, or approach.

Examples:

- "Ask the research agent whether Playwright could solve this."
- "Investigate whether PostgreSQL connection pooling is involved."
- "Have another agent assess whether Redis is responsible."
- "Research whether this API can be replaced with GraphQL."

Do not remove the requested direction in the name of neutrality.

Instead, clearly distinguish it from established evidence.

Use labels such as:

- **Assigned research question**
- **Hypothesis to evaluate**
- **Approach under consideration**
- **Component under investigation**

Never rewrite:

> Investigate whether X causes this.

as:

> X causes this.

The receiving agent must remain free to conclude that the assigned hypothesis is unsupported or inferior to another explanation.

### Mode C — Human communication

Use this when the primary recipient is another person rather than an autonomous agent, such as a developer, teammate, maintainer, vendor, support team, consultant, or reviewer.

Preserve the same factual discipline, but optimize the wording for human readability.

Do not assume that the recipient has repository, tool, or conversation access.

## 2. Gather the minimum sufficient context

Use information already available from the current investigation.

When repository access, logs, files, tests, source material, or development tools are available, inspect relevant evidence before producing the handoff when necessary to avoid relying on uncertain recollection.

Prefer direct evidence over summaries.

Relevant evidence may include:

- exact errors;
- logs;
- stack traces;
- test failures;
- commands and outputs;
- API responses;
- screenshots;
- source URLs;
- database observations;
- configuration;
- code paths;
- relevant diffs or commits;
- reproduction inputs;
- environment details;
- performance measurements;
- source artifacts.

Do not include irrelevant implementation detail merely because it is available.

The handoff should be **self-contained but bounded**.

### Preserve concrete evidence when summarizing

**Do not generalize away concrete evidence.** A concise handoff is not useful if the next investigator loses the specific observations needed to reproduce or reason about the problem.

When a concrete failure case, error, source, input, output, experiment, or implementation behavior is materially useful, preserve it even when the handoff also states the broader generalized problem.

In particular, preserve when relevant:

- the exact reproduction input or affected entity;
- exact error messages or status codes;
- source-by-source or case-by-case outcomes;
- specific URLs, IDs, filenames, functions, commands, or test names;
- environment-specific differences;
- counts, timestamps, versions, or measurements that materially affect interpretation;
- verified current implementation behavior that prevents the recipient from recommending something the system already does.

Do not replace:

```text
Official product page → access challenge
Marketplace listing → HTTP 403
YouTube video A → captions unavailable
YouTube video B → access challenge
```

with only:

> Some sources fail to retrieve.

The generalized statement may be useful, but it should accompany rather than replace the concrete evidence.

Likewise, if the current system has already been verified to retain blocked sources, isolate source failures, preserve provenance, or fail closed, include those facts when they are relevant. Otherwise the recipient may waste time proposing behavior that already exists.

Apply a **minimum sufficient evidence** test: remove detail only when losing it would not materially reduce the recipient's ability to reproduce the issue, distinguish competing explanations, understand the current implementation, or avoid repeating completed investigation.

## 3. Separate evidence from interpretation

When the distinction matters, classify important information as:

### Verified

Directly supported by reproduction, code inspection, logs, tests, measurements, retrieved source material, tool output, or other concrete evidence.

### Inferred

A reasonable interpretation of available evidence that has not been directly established.

### Unknown

Not yet determined.

Never silently promote **Unknown → Inferred → Verified** because a previous investigator or LLM stated something confidently.

Statements from another model or agent are not evidence by themselves.

## 4. Preserve contradictory evidence

Do not selectively include only evidence supporting the current investigator's theory.

Include relevant observations that contradict the leading explanation, behaved differently in another environment, reproduce only intermittently, changed after an experiment, suggest multiple plausible causes, or failed to behave as expected.

Contradictory evidence is often especially useful to an independent investigator.

## 5. Preserve completed investigation work

Neutrality must not mean discarding useful work.

Include relevant experiments already performed. For each experiment, record when useful:

- what was tested or changed;
- the observed result;
- whether the change remains present;
- important limitations of the experiment.

Example:

### Experiment

Disabled response caching for the failing request.

### Result

The failure was not reproduced in three subsequent local runs.

### Interpretation status

This establishes an observed correlation only. It does not establish caching as the root cause.

This prevents the next investigator from unnecessarily repeating work while preserving their ability to interpret the result independently.

## 6. Prefer pointers when the recipient has the same workspace

When another agent has access to the same repository or development environment, avoid copying large quantities of code into the problem statement.

Prefer precise references such as:

```text
Relevant code:
- src/retrieval/fetcher.ts → fetchSource()
- src/retrieval/validator.ts → validateResponse()
- tests/retrieval/fetcher.test.ts
```

Include code snippets only when the recipient will not have repository access, a small snippet is necessary to understand the issue, or preserving an exact implementation state is important.

Do not summarize code in a way that embeds an unverified diagnosis.

## 7. Preserve current state

When relevant, state whether the system or repository changed during the investigation.

Include:

- whether the issue is currently reproducible;
- temporary fixes;
- debugging instrumentation;
- feature flags;
- experimental configuration;
- uncommitted changes;
- changed dependencies;
- whether the current state differs from the state where the issue originally occurred.

This prevents the recipient from unknowingly investigating a modified system.

## 8. Preserve constraints and invariants

State what a valid outcome must preserve.

Examples include existing behavior, API compatibility, data integrity, security properties, offline operation, performance requirements, latency limits, memory limits, deployment constraints, backwards compatibility, human approval boundaries, source provenance, and regulatory requirements.

Do not turn implementation preferences into constraints unless they are genuinely mandatory.

Bad:

> Must use Playwright.

Better:

> Must obtain usable evidence from supported dynamic sources without implicitly relying on a user's private authenticated browser session.

## 9. Generalize without erasing the reproduction case

When one concrete example exposed a broader issue:

1. state the general problem;
2. preserve the concrete case as reproduction evidence;
3. explicitly distinguish the example from the intended scope.

Example:

> The retrieval architecture must support heterogeneous public product-research sources. The Cosmic Byte keyboard case below is one reproducible example of the broader issue and must not be treated as the scope of the architecture.

Do not allow the handoff to accidentally turn "This is where we observed the problem" into "This is the only case the solution must support."

Do not move so far in the opposite direction that the concrete case disappears. The broader problem and the reproduction case should normally coexist.

This principle applies equally to one customer revealing a general bug, one website revealing a retrieval limitation, one PDF revealing a parser weakness, one API revealing an architectural issue, one operating system revealing a portability problem, or one product revealing a product-research limitation.

## 10. Use a problem-specific structure

Do not mechanically fill every possible section.

Choose only the sections useful for the actual problem.

A typical technical investigation handoff may contain:

- Problem
- Objective / Expected Behavior
- Observed Behavior
- Reproduction
- Evidence
- Relevant System Context
- Relevant Code / Components
- Verified Observations
- Experiments Already Performed
- Current State
- Constraints / Invariants
- Unknowns
- Investigation Request

An architecture investigation may instead use:

- Objective
- Current Architecture
- Observed Limitations
- Evidence
- Relevant Components
- Existing Constraints
- Previous Experiments
- Unknowns
- Investigation Request

A directed research handoff may use:

- Research Objective
- Assigned Research Question
- Existing Context
- Evidence
- Constraints
- Known Unknowns
- Required Output

Use the structure that best communicates the problem.

## 11. Write the investigation request carefully

The ending of the handoff strongly influences the receiving investigator. Match it to the caller's actual intent.

### Independent investigation

Prefer wording such as:

> Independently investigate this problem using the evidence and context above. Verify relevant claims yourself where possible. Do not assume a particular root cause or solution. Determine the most likely explanation, consider credible alternatives, and recommend an approach based on your own investigation.

Adapt this wording to the task.

Do not request implementation when the delegated task is only diagnosis or research.

### Research only

> Research the available approaches to this problem, compare their trade-offs against the stated constraints, and report your findings. Do not implement changes.

### Root-cause investigation

> Determine the most likely root cause using the available evidence. Consider credible alternative explanations and identify what additional evidence would distinguish between them. Do not implement a fix unless requested.

### Architecture investigation

> Evaluate viable architectures against the stated requirements and constraints. Do not assume the current architecture or any previously proposed replacement is correct.

## 12. Directed investigation requests

When the caller intentionally specifies a hypothesis or approach, preserve that scope.

Example:

> **Assigned research question:** Determine whether Playwright is an appropriate retrieval mechanism for the affected source classes.

Then clarify:

> Treat Playwright as an approach being evaluated, not as an established solution. Identify evidence for and against its suitability and compare relevant alternatives where necessary.

Do not unnecessarily broaden a tightly scoped research assignment.

## 13. Avoid common failure modes

### Diagnosis disguised as context

Bad:

> The worker has a race condition that causes duplicate processing.

If not verified, better:

> Duplicate processing has been observed when multiple workers handle related events concurrently. The mechanism responsible for the duplication has not been established.

### Solution disguised as requirement

Bad:

> We need to replace direct HTTP with Playwright.

Better:

> The current direct-HTTP retrieval path cannot obtain usable evidence from the reproduction sources. The appropriate retrieval mechanism remains unresolved.

### Search anchoring

Bad:

> Search for Cloudflare bypass solutions.

Better:

> The returned response contains an access challenge. The mechanism producing the challenge and appropriate retrieval options remain to be investigated.

### Historical observation presented as permanent behavior

Bad:

> Flipkart blocks our scraper.

Better:

> During the recorded reproduction attempt, the Flipkart URL returned HTTP 403 through the current retrieval path.

### Previous-agent authority

Bad:

> Claude confirmed that the parser is broken.

Better:

> A previous investigator proposed the parser as a possible cause. This has not been independently verified.

In independent mode, omit this entirely unless required to explain previous experiments.

### Excessive context dumping

Do not paste full conversation transcripts, entire source files, unrelated logs, complete repository architecture, every failed command, or every thought from the originating investigator.

Extract the context necessary for the recipient to continue productively.

### Overcompression

Do not reduce a complex problem to:

> API sometimes fails. Find out why.

Preserve enough evidence and context for the recipient to begin useful investigation without reconstructing the original session.

Do not remove concrete reproduction cases, exact failures, verified implementation behaviors, or completed experiments merely to make the handoff shorter.

## 14. Protect provenance

When the problem involves external evidence, research, documents, datasets, retrieved content, or source material, preserve provenance.

Where relevant include:

- original URL or source identifier;
- retrieval timestamp;
- source role;
- artifact/file identifier;
- version;
- commit;
- environment;
- integrity/hash information.

Do not merge evidence from different sources into one unattributed statement.

For example, do not collapse "Manufacturer states X" and "Reviewer observed Y" into a generic statement that loses which source supports which information.

## 15. Final quality check

Before returning the handoff, verify:

- Can the recipient understand the problem without the original conversation?
- Is the objective or expected behavior clear?
- Is observed behavior separated from interpretation?
- Are important claims supported by evidence?
- Are verified facts distinguishable from inference?
- Are useful previous experiments preserved?
- Is contradictory evidence retained?
- Are constraints actual constraints rather than preferred solutions?
- Is current repository/environment state clear when relevant?
- Is the concrete reproduction case preserved with enough detail to be useful?
- Are exact errors, status codes, source-by-source outcomes, IDs, inputs, or measurements retained when they materially affect investigation?
- Are verified implementation behaviors preserved when omitting them could cause the recipient to recommend behavior that already exists?
- Is broader scope clear when the example is only one instance?
- Does the handoff preserve both the generalized problem and the concrete evidence instead of choosing one at the expense of the other?
- Does the investigation request match the caller's intent?
- For independent investigation, have unnecessary diagnoses, preferred solutions, and search directions been removed?
- For directed investigation, is the requested hypothesis clearly labeled rather than treated as fact?
- Could another agent begin useful work from this handoff without first asking for the entire originating conversation?

If not, revise the handoff before returning it.
