# Prompt Engineering and Context Engineering for LLM Agents (applied to an autonomous pentest / bug-bounty hackbot)

Research date: 2026-10-27. Verification legend used on every resource:
- **[V-fetched]** = page/file fetched in this session (HTTP 200, title and/or date read from the page itself).
- **[V-indexed]** = URL confirmed by a search-engine result or by a hyperlink inside a primary file fetched in this session (e.g., a GitHub README), but the page body itself was not fetched (host blocked by the session's egress policy).
- **[UNVERIFIED]** = from prior knowledge; the host was blocked and the shared web-search budget ran out mid-session (200-call limit hit). URL/date are believed correct but were not confirmed today.

Method note for the report writer: Bash egress allowed anthropic.com, claude.com, platform.claude.com, code.claude.com and raw.githubusercontent.com; arxiv.org, openai.com, cookbook.openai.com, google.dev, kaggle.com, simonwillison.net, cognition.ai, philschmid.de, trychroma.com, owasp.org etc. were policy-blocked. OpenAI cookbook entries were verified from the cookbook's own GitHub `registry.yaml` (titles, dates, authors). arXiv IDs were cross-checked against curated GitHub paper lists and paper repos (Meirtz/Awesome-Context-Engineering, ace-agent/ace, gepa-ai/gepa, google-research/camel-prompt-injection, zou-group/textgrad, aliasrobotics/cai, ipa-lab/hackingBuddyGPT).

---

## Q1. What are the canonical official prompt-engineering guides as of late 2026, and what changed in 2025-2026 (reasoning models, extended/adaptive thinking, agents vs. chat)?

### Takeaway
Vendor docs are now model-versioned living documents: Anthropic maintains one "Prompting best practices" reference plus a per-model page for each current model (through Claude Fable 5.1 / Mythos 5.1 / Opus 5.5 / Sonnet 5.5 / Haiku 5.5), and OpenAI ships a per-model cookbook guide (GPT-4.1 -> GPT-5 -> 5.1 -> 5.2 -> Codex). The big 2025-2026 shift is away from "tricks" (prefill, ALL-CAPS "CRITICAL/MUST", "think step by step") toward calibrating effort/thinking, softer instructions for highly-steerable models, and explicit agentic guidance (state tracking across context windows, subagent control, reversibility/safety).

### Cited Findings

**Anthropic (read first if building on Claude)**
- **"Prompt engineering overview"** — Anthropic (Claude Platform Docs) — living doc, no date shown — Beginner. Entry point that links to all technique pages; old docs.claude.com/docs.anthropic.com URLs now redirect to platform.claude.com. [V-fetched] — [Source](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview)
- **"Prompting best practices"** — Anthropic — living doc (current as of 2026-10-27) — Beginner->Advanced. The single most important current reference; states it covers "Claude Fable 5.1, Claude Mythos 5.1, Claude Fable 5, Claude Mythos 5, Claude Opus 5.5, Claude Opus 5, Claude Opus 4.8, Claude Opus 4.7, Claude Opus 4.6, Claude Sonnet 5.5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Haiku 5.5, and Claude Haiku 4.5," organized as model-specific guidance -> techniques for all models -> migration. The old "claude-4-best-practices" URL redirects here. [V-fetched] — [Source](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
  - Section list (verified): Be clear and direct; Add context; Use examples; Structure prompts with XML tags; Give Claude a role; Long context prompting; Communication style/verbosity; Migrating away from prefilled responses; Tool usage; Optimize parallel tool calling; Overthinking and excessive thoroughness; Interleaved thinking; Agentic systems -> Long-horizon reasoning and state tracking, Context awareness and multiwindow workflows, State management best practices, Balancing autonomy and safety, Research and information gathering, Subagent orchestration, Overeagerness, Avoid focusing on passing tests and hardcoding, Minimizing hallucinations in agentic coding. — [Source](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
  - Long-context rule: put long documents "near the top of your prompt, above your query"; "Queries at the end can improve response quality by up to 30 percent in tests, especially with complex, multidocument inputs"; wrap each document in `<document>` with `<source>`/`<document_content>`. — [Source](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
  - Anti-shouting rule: newer models are "more responsive to the system prompt... might now overtrigger. The fix is to soften any aggressive language. Where you might have said 'CRITICAL: You MUST use this tool when...,' you can use more normal prompting such as 'Use this tool when...'" — [Source](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
  - Multi-window workflows: use a different prompt for the first context window (set up tests/scripts), track tests in structured `tests.json`, create `init.sh`, and "consider starting with a brand new context window rather than using compaction. Claude's latest models are extremely effective at discovering state from the local filesystem"; use git for state tracking. — [Source](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
  - Safety prompt template: "Consider the reversibility and potential impact of your actions... for actions that are hard to reverse, affect shared systems, or could be destructive, ask the user before proceeding." (Directly reusable for a hackbot's rules of engagement.) — [Source](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
  - Subagent overuse: Opus 4.6/Opus 5 "delegate to subagents more readily"; sample damping prompt: "Use subagents when tasks can run in parallel, require isolated context, or involve independent workstreams that don't need to share state." — [Source](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
- **"Prompting Claude Opus 5"** (and sibling per-model pages for Opus 5.5, Fable 5/5.1, Sonnet 5/5.5, Haiku 5.5, Opus 4.8) — Anthropic — living docs — Intermediate. Model deltas: verbosity, progress updates, task scope/over-verification, subagent control, effort calibration, "passing thinking blocks back unchanged" (Fable 5.1). [V-fetched for Opus 5 page; others V-indexed via links on the best-practices page] — [Source](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5)
- **"Extended thinking"** — Anthropic docs — living — Intermediate. API mechanics for thinking budgets/adaptive thinking that replace manual CoT prompting. [V-fetched] — [Source](https://platform.claude.com/docs/en/build-with-claude/extended-thinking)
- **"Prompt Engineering Interactive Tutorial"** — Anthropic (GitHub, anthropics/prompt-eng-interactive-tutorial) — first released 2024; still online — Beginner. 9 chapters + appendix with exercises; README still says it "uses... Claude 3 Haiku." FOUNDATIONAL BUT DATED: concepts hold; model references and prefill technique are superseded by the best-practices page's "Migrating away from prefilled responses." [V-fetched via raw.githubusercontent] — [Source](https://github.com/anthropics/prompt-eng-interactive-tutorial)
- **"The 'think' tool: Enabling Claude to stop and think in complex tool use situations"** (page title: "The 'think' tool for complex tool use") — Anthropic Engineering — 2025-03-20 — Intermediate. Pattern for mid-trajectory reasoning between tool calls; partly superseded by interleaved/adaptive thinking but still a useful policy-checking pattern. [V-fetched] — [Source](https://www.anthropic.com/engineering/claude-think-tool)

**OpenAI** (all verified from the openai/openai-cookbook `registry.yaml`; cookbook web host was blocked)
- **"GPT-4.1 Prompting Guide"** — OpenAI (nm-openai, julian-openai) — 2025-04-14 — Intermediate. First OpenAI guide aimed at agentic workflows (persistence, tool-calling, planning reminders); historically important. [V-fetched registry] — [Source](https://cookbook.openai.com/examples/gpt4-1_prompting_guide)
- **"GPT-5 prompting guide"** — OpenAI (anoop-openai, julian-openai, ericzakariasson, erinkav-openai) — 2025-08-07 — Intermediate. Agentic eagerness control, tool preambles, reasoning_effort, instruction-conflict hygiene (content details from prior knowledge). [V-fetched registry] — [Source](https://cookbook.openai.com/examples/gpt-5/gpt-5_prompting_guide)
- **"GPT-5.1 Prompting Guide"** — OpenAI (samarth-oai) — 2025-11-13 — Intermediate. [V-fetched registry] — [Source](https://cookbook.openai.com/examples/gpt-5/gpt-5-1_prompting_guide)
- **"GPT-5.2 Prompting Guide"** — OpenAI (msingh-openai, emreokcular) — 2025-12-11 — Intermediate. Latest general GPT prompting guide present in the cookbook registry. [V-fetched registry] — [Source](https://cookbook.openai.com/examples/gpt-5/gpt-5-2_prompting_guide)
- **"Codex Prompting Guide"** — OpenAI (nm-openai, bfioca-openai) — 2026-02-25 — Intermediate/Advanced. Coding-agent prompting; relevant to tool-heavy hackbot loops. [V-fetched registry] — [Source](https://cookbook.openai.com/examples/gpt-5/codex_prompting_guide)
- **"GPT-5 Prompt Migration and Improvement Using the New Optimizer"** — OpenAI — 2025-08-07; **"Prompt Migration Guide"** — 2025-06-26; **"Building resilient prompts using an evaluation flywheel"** (neelk-oai, hamel) — 2025-10-06 — Intermediate. Eval-driven prompt iteration. [V-fetched registry] — [Source](https://cookbook.openai.com/examples/evaluation/Building_resilient_prompts_using_an_evaluation_flywheel)
- **"Better performance from reasoning models using the Responses API"** (2025-05-11) and **"Handling Function Calls with Reasoning Models"** (2025-04-25) — OpenAI — Intermediate. How to pass reasoning items back across tool calls. [V-fetched registry] — [Source](https://cookbook.openai.com/examples/responses_api/reasoning_items)
- OpenAI "Prompt engineering" guide (platform docs) and "Reasoning best practices" — OpenAI — living docs — Beginner/Intermediate. [UNVERIFIED: host blocked] — [Source](https://platform.openai.com/docs/guides/prompt-engineering); [Source](https://platform.openai.com/docs/guides/reasoning-best-practices)
- "A practical guide to building agents" (PDF) — OpenAI — ~April 2025 — Beginner. [UNVERIFIED] — [Source](https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf)
- Secondary report: Decrypt says OpenAI published a GPT-5.6 ("Sol") prompting guide favoring leaner system prompts ("stop writing so much") and adding programmatic tool calling. NOT present in the cookbook registry fetched today (no gpt-5-4/5-5/5-6 prompting-guide files; 404s). Treat as unverified. — [Source](https://decrypt.co/373439/openai-new-gpt-5-6-prompt-guide-chatgpt); contradicted/unconfirmed by [registry](https://raw.githubusercontent.com/openai/openai-cookbook/main/registry.yaml)
- The cookbook appears to be served (also) at developers.openai.com/cookbook per search results. — [Source](https://developers.openai.com/cookbook)

**Google / Microsoft / community**
- "Prompt design strategies" — Google Gemini API docs — living — Beginner. [UNVERIFIED: host blocked] — [Source](https://ai.google.dev/gemini-api/docs/prompting-strategies)
- "Prompt Engineering" whitepaper — Lee Boonstra (Google) on Kaggle — Feb 2025 (~68 pp.) — Beginner. Good catalogue of classic techniques (zero/few-shot, CoT, ToT, ReAct, step-back) with config tips. [UNVERIFIED: host blocked] — [Source](https://www.kaggle.com/whitepaper-prompt-engineering)
- "Prompt engineering techniques" — Microsoft Learn (Azure AI Foundry / Azure OpenAI) — living — Beginner. [UNVERIFIED] — [Source](https://learn.microsoft.com/en-us/azure/ai-foundry/openai/concepts/prompt-engineering)
- Prompt Engineering Guide — DAIR.AI (promptingguide.ai) — continuously updated — Beginner. Linked from the dair-ai/Prompt-Engineering-Guide repo README. [V-indexed] — [Source](https://www.promptingguide.ai)
- Learn Prompting (learnprompting.org) — Sander Schulhoff et al. — Beginner. [UNVERIFIED] — [Source](https://learnprompting.org/)
- "Prompt Engineering" — Lilian Weng — 2023-03-15 — Intermediate. FOUNDATIONAL/OUTDATED for model specifics. [UNVERIFIED] — [Source](https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/)

### Inferences
- For a Claude-based hackbot, the authoritative stack is: per-model page for the exact model -> "Prompting best practices" (Agentic systems section) -> "Effective context engineering" (Q2). Older "tricks" (prefill, ALL CAPS, forced CoT) are actively discouraged for current models.
- The cross-vendor convergence for 2025-2026: (1) calibrate reasoning effort rather than prompting for reasoning; (2) be explicit about agent autonomy, scope and reversibility; (3) pass reasoning/thinking state back across tool calls unchanged; (4) use evals, not vibes, to iterate prompts.

### Gaps
- Could not fetch Google's Gemini 3-era prompting guidance, the Boonstra whitepaper, Microsoft Learn, or OpenAI platform docs in-session (egress blocked; search budget exhausted). Their existence/URLs are from prior knowledge.
- Could not confirm a GPT-5.5/5.6 prompting guide; only secondary reporting.
- No "last updated" timestamp is exposed on the Anthropic docs pages.

---

## Q2. What is "context engineering," who popularized it, and what are the definitive write-ups (incl. 2026 follow-ups)?

### Takeaway
"Context engineering" = curating the whole token state (system prompt, tools, examples, message history, retrieved data, memory) given to the model at each step under a finite "attention budget." It was popularized in June 2025 by Tobi Lütke and Andrej Karpathy on X, then codified by LangChain (write/select/compress/isolate), Drew Breunig (failure modes + fixes), Manus (KV-cache, filesystem-as-memory), Cognition (shared context, single-threaded agents), Chroma (Context Rot evidence) and Anthropic (Sep 29, 2025 canonical post). 2026 material shifts toward "harness engineering," context resets vs. compaction, and evolving playbooks.

### Cited Findings

**Origin**
- Andrej Karpathy, X post, 2025-06-25: "+1 for 'context engineering' over 'prompt engineering'," describing it as "the delicate art and science of filling the context window with just the right information for the next step." Status URL cited in multiple fetched repos (incl. via LangChain referral). [V-indexed] — [Source](https://x.com/karpathy/status/1937902205765607626); date per [secondary coverage](https://pureai.com/Articles/2025/09/23/Karpathy-Puts-Context-at-the-Core-of-AI-Coding.aspx)
- Tobi Lütke (@tobi), X post, June 2025: "I really like the term 'context engineering' over prompt engineering... the art of providing all the context for the task to be plausibly solvable by the LLM." Karpathy's post quoted it. [UNVERIFIED status URL; believed to be x.com/tobi/status/1935533422589399127, ~2025-06-19] — [Source](https://x.com/tobi/status/1935533422589399127); wording per [Simon Willison's write-up](https://feeds.simonwillison.net/2025/Jun/27/context-engineering/)
- "Context engineering" — Simon Willison — 2025-06-27 — Beginner. Short framing post on why the term stuck (its "inferred definition" is closer to the intended meaning than "prompt engineering"). [V-indexed] — [Source](https://simonwillison.net/2025/Jun/27/context-engineering/)

**Definitive write-ups (recommended order)**
1. **"Effective context engineering for AI agents"** — Anthropic Applied AI team — 2025-09-29 — Intermediate. THE canonical reference. Key claims (verified from page text): context is "a finite resource with diminishing marginal returns"; LLMs have an "attention budget"; cites "context rot"; system prompts at the "right altitude" (Goldilocks zone between brittle hardcoded logic and vague guidance); strive for "the minimal set of information that fully outlines your expected behavior" ("minimal does not necessarily mean short"); "just in time" retrieval via lightweight identifiers (file paths, stored queries, links) — Claude Code drops CLAUDE.md in up front but uses glob/grep just-in-time; long-horizon techniques = compaction, structured note-taking (agentic memory), sub-agent architectures; "tool result clearing" is "one of the safest lightest touch forms of compaction." [V-fetched] — [Source](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) (now redirects to /research/)
2. **"Context Engineering for AI Agents: Lessons from Building Manus"** — Yichao "Peak" Ji (Manus) — 2025-07-18 — Intermediate/Advanced. KV-cache hit rate is "the single most important metric for a production-stage AI agent"; average input:output token ratio ~100:1; keep prompt prefix stable/append-only; mask token logits instead of adding/removing tools; "file system as the ultimate context"; recitation via todo.md; keep errors in context; framework rebuilt four times ("Stochastic Graduate Descent"). [V-indexed] — [Source](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)
3. **"Context Engineering in Manus"** — Lance Martin — 2025-10-15 — Advanced. Webinar write-up with Peak Ji: tool results have "full" and "compact" representations (compact = file path reference); compaction swaps stale results for compact versions while newest stay full; schema-based summarization once compaction hits diminishing returns; todo.md consumed "roughly one-third of all actions," so Manus moved to a planner agent + executor sub-agents; "<20 atomic functions," MCP tools exposed via CLI through Bash; sub-agents "primarily to isolate context (not to divide labor by role)." [V-fetched via GitHub source] — [Source](https://rlancemartin.github.io/2025/10/15/manus/)
4. **"Context Engineering for Agents"** — LangChain (attributed by secondary sources to Lance Martin) — 2025-07-02 — Beginner/Intermediate. The write/select/compress/isolate taxonomy with examples from popular agents; companion notebooks repo langchain-ai/context_engineering [V-fetched README]. [V-indexed] — [Source](https://blog.langchain.com/context-engineering-for-agents/); [Repo](https://github.com/langchain-ai/context_engineering)
5. **"The rise of 'context engineering'"** — LangChain (Harrison Chase) — June 2025 — Beginner. [V-indexed; date from memory] — [Source](https://blog.langchain.com/the-rise-of-context-engineering/)
6. **"How Long Contexts Fail"** (URL slug: how-contexts-fail-and-how-to-fix-them) — Drew Breunig — 2025-06-22 — Intermediate. Four failure modes: context poisoning, distraction, confusion, clash. [V-indexed] — [Source](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html)
7. **"How to Fix Your Context"** — Drew Breunig — 2025-06-26 — Intermediate. Fixes: RAG, tool loadout, context quarantine, pruning, summarization, offloading; cites a quantized Llama 3.1 8B failing with 46 tools but succeeding with 19. [V-indexed] — [Source](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html)
   - Related: "Prompts vs. Context" — Drew Breunig — 2025-06-25 [V-indexed] — [Source](https://www.dbreunig.com/2025/06/25/prompts-vs-context.html); O'Reilly podcast "Generative AI in the Real World: Context Engineering with Drew Breunig" [V-indexed] — [Source](https://www.oreilly.com/radar/podcast/generative-ai-in-the-real-world-context-engineering-with-drew-breunig/)
8. **"Context Rot: How Increasing Input Tokens Impacts LLM Performance"** — Chroma Research (Kelly Hong, Anton Troynikov, Jeff Huber — authors from memory) — July 2025 — Intermediate. Shows models do not process context uniformly; experiments = NIAH extension with semantic (non-lexical) needles and haystack variations, LongMemEval, repeated-words replication; replication toolkit on GitHub. [V-fetched repo README; report page V-indexed] — [Source](https://research.trychroma.com/context-rot); [Repo](https://github.com/chroma-core/context-rot)
9. **"Don't Build Multi-Agents"** — Walden Yan (Cognition) — 2025-06-12 (date from memory) — Intermediate. Two principles (from memory): "Share context, and share full agent traces, not just individual messages" and "Actions carry implicit decisions, and conflicting decisions carry bad results"; prefer single-threaded linear agents, add a compression model for very long tasks. Read alongside Anthropic's multi-agent post for the counterpoint. [V-indexed URL] — [Source](https://cognition.ai/blog/dont-build-multi-agents)
10. **"The New Skill in AI is Not Prompting, It's Context Engineering"** — Philipp Schmid — ~2025-06-30 (from memory) — Beginner. Lists the components of context (instructions/system prompt, user prompt, state/short-term history, long-term memory, retrieved info, tools, structured output). [V-indexed URL] — [Source](https://www.philschmid.de/context-engineering)
11. **"12-Factor Agents — Principles for building reliable LLM applications"** — Dex Horthy (HumanLayer) — 2025 (repo, CC BY-SA content) — Intermediate. Verified factor list includes Factor 2 "Own your prompts," Factor 3 "Own your context window," Factor 5 "Unify execution state and business state," Factor 8 "Own your control flow," Factor 9 "Compact Errors into Context Window," Factor 10 "Small, Focused Agents," Factor 12 "Make your agent a stateless reducer." [V-fetched] — [Source](https://github.com/humanlayer/12-factor-agents)
12. **"Getting AI to Work in Complex Codebases"** (repo: advanced-context-engineering-for-coding-agents, file ace-fca.md) — Dex Horthy (HumanLayer) — Aug 2025 (based on a Y Combinator talk on Aug 20; screenshots dated 2025-08-29) — Advanced. "Frequent intentional compaction": design the whole workflow around context management, "keeping utilization in the 40-60% range"; research -> plan -> implement phases; claims Claude Code handled "300k LOC Rust codebases." Related X post on using commit messages for intentional compaction. [V-fetched] — [Source](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md); [X post](https://x.com/dexhorthy/status/1961490837017088051)

**Anthropic follow-ups that operationalize context engineering (all [V-fetched], dates from page metadata)**
- "How we built our multi-agent research system" — 2025-06-13 — Intermediate. Orchestrator-worker; subagents as context isolation/compression. — [Source](https://www.anthropic.com/engineering/multi-agent-research-system)
- "Writing effective tools for AI agents" — 2025-09-11 — Intermediate. Token-efficient tool responses, namespacing, eval-driven tool design. — [Source](https://www.anthropic.com/engineering/writing-tools-for-agents)
- "Equipping agents for the real world with Agent Skills" — 2025-10-16 — Intermediate. Progressive disclosure via SKILL.md. — [Source](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
- "Code execution with MCP: Building more efficient agents" (page title "Code execution with MCP for efficient agents") — 2025-11-04 — Advanced. Keep intermediate tool data out of context by having the model write code against tools. — [Source](https://www.anthropic.com/engineering/code-execution-with-mcp)
- "Introducing advanced tool use on the Claude Developer Platform" (page title "Introducing advanced tool use for Claude") — 2025-11-24 — Advanced. Tool search / programmatic tool calling / tool-use examples to shrink tool-definition context. — [Source](https://www.anthropic.com/engineering/advanced-tool-use)
- "Effective harnesses for long-running agents" — 2025-11-26 — Advanced. Initializer agent + progress files + git for multi-window work. — [Source](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
- "Harness design for long-running apps" — 2026-03-24 — Advanced. Planner/generator/evaluator; "context anxiety" ("begin wrapping up work prematurely as they approach what they believe is their context limit"); "Context resets—clearing the context window entirely and starting a [new agent]" vs. compaction ("compaction preserves continuity, it doesn't give the agent a clean slate"); Sonnet 4.5's context anxiety made resets "essential." — [Source](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- "Scaling Managed Agents with stable interfaces" — 2026-04-08 — Advanced. "Harnesses encode assumptions that go stale as models improve." — [Source](https://www.anthropic.com/engineering/managed-agents)
- "Demystifying evals for AI agents" — 2026-01-09 — Intermediate. Needed to measure any context-engineering change. — [Source](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)
- "Building effective agents" — Anthropic (authors Erik Schluntz & Barry Zhang, from memory) — 2024-12-19 — Beginner. FOUNDATIONAL: workflows vs. agents, keep it simple. — [Source](https://www.anthropic.com/engineering/building-effective-agents)

**Other 2026 material found (secondary/preprints; [V-indexed] via search results)**
- "Context Engineering in 2026: Why We Stopped Compacting Our Agent's Context" — Louis-François Bouchard — 2026 — Intermediate. Reports that with prompt caching, keeping full context was often cheaper/faster/better recall than summarizing; compact only for a specific constraint. — [Source](https://www.louisbouchard.ai/context-engineering-2026/)
- "Codified Context: Infrastructure for AI Agents in a Complex Codebase" — arXiv:2602.20478 — Feb 2026 — Advanced. — [Source](https://arxiv.org/pdf/2602.20478)
- "Harness Engineering for Agentic AI Coding Tools: An Exploratory Study" — arXiv:2602.14690 — Feb 2026 — Advanced. — [Source](https://arxiv.org/pdf/2602.14690)
- "When History Is Multimodal: Rethinking Context Management for Long-Horizon Agents" — arXiv:2608.29897 — Aug 2026 — Advanced. — [Source](https://arxiv.org/pdf/2608.29897)
- Meirtz/Awesome-Context-Engineering — curated paper list, actively updated into 2026 (contains 2601-2605 arXiv entries) — All levels. [V-fetched] — [Source](https://github.com/Meirtz/Awesome-Context-Engineering)

### Inferences
- The field converged on a shared toolkit within ~3 months (June-Sept 2025). Since then the frontier moved from "what to put in context" to "harness design": when to reset vs. compact, how to hand off state via files/git, and how to keep harness assumptions from going stale as models change (Anthropic Mar-Apr 2026).
- There is live disagreement on compaction: Anthropic's 2026 harness post and Horthy favor resets/intentional compaction; Bouchard's 2026 write-up argues full context + caching often beats summarization. A hackbot should treat this as an empirical question settled by its own evals.
- Cognition vs. Anthropic on multi-agent is reconcilable: isolate context for read-only/parallel exploration (recon, per-endpoint probing), but keep decisions that must be consistent (exploit chain, what's in scope) in one thread.

### Gaps
- Could not fetch the Manus, Cognition, Phil Schmid, Chroma report, LangChain and Drew Breunig pages themselves; content claims for those come from search snippets, a GitHub-hosted secondary write-up (Lance Martin), and prior knowledge. Cognition's two principles and Chroma's author list/model count (believed 18 models) are unverified.
- Tobi Lütke's exact status URL and date not confirmed.
- Phil Schmid's possible "part 2" follow-up could not be checked.

---

## Q3. Key research papers (prompting, long-context degradation, prompt optimization, ACE, memory) with arXiv IDs

### Takeaway
Read the reasoning-prompting classics only for intuition (modern reasoning models internalize CoT/self-consistency); the papers that matter most for agent builders in 2025-2026 are long-context degradation (Lost in the Middle, RULER, NoLiMa, Context Rot), prompt/context optimization (DSPy, GEPA, ACE), memory systems (MemGPT/Letta, Mem0, A-MEM), and the Context Engineering survey.

### Cited Findings
Prompting foundations (FOUNDATIONAL; mostly superseded in practice by built-in reasoning)
- Chain-of-Thought Prompting Elicits Reasoning in LLMs — Wei et al. (Google) — arXiv:2201.11903 — 2022 — Beginner. [UNVERIFIED ID; canonical] — [Source](https://arxiv.org/abs/2201.11903)
- Self-Consistency Improves Chain of Thought Reasoning — Wang et al. — arXiv:2203.11171 — 2022 — Intermediate. [V-indexed] — [Source](https://arxiv.org/abs/2203.11171)
- Tree of Thoughts: Deliberate Problem Solving with LLMs — Yao et al. — arXiv:2305.10601 — 2023 — Intermediate. [V-indexed] — [Source](https://arxiv.org/abs/2305.10601)
- ReAct: Synergizing Reasoning and Acting in Language Models — Yao et al. — arXiv:2210.03629 — 2022 — Beginner (the agent-loop ancestor). [V-indexed] — [Source](https://arxiv.org/abs/2210.03629)
- Reflexion: Language Agents with Verbal Reinforcement Learning — Shinn et al. — arXiv:2303.11366 — 2023 — Intermediate. [V-indexed] — [Source](https://arxiv.org/abs/2303.11366)
- The Prompt Report: A Systematic Survey of Prompting Techniques — Schulhoff et al. — arXiv:2406.06608 — 2024 (revised 2025) — All levels; best single taxonomy of 58+ text prompting techniques. [V-indexed] — [Source](https://arxiv.org/abs/2406.06608)

Long-context degradation
- Lost in the Middle: How Language Models Use Long Contexts — Liu et al. — arXiv:2307.03172 — 2023 (TACL 2024) — Beginner. FOUNDATIONAL: U-shaped position effect. [UNVERIFIED ID; canonical] — [Source](https://arxiv.org/abs/2307.03172)
- RULER: What's the Real Context Size of Your Long-Context Language Models? — Hsieh et al. (NVIDIA) — arXiv:2404.06654 — 2024 — Intermediate. [V-indexed] — [Source](https://arxiv.org/abs/2404.06654)
- NoLiMa: Long-Context Evaluation Beyond Literal Matching — Modarressi et al. — arXiv:2502.05167 — 2025 — Intermediate. [UNVERIFIED ID] — [Source](https://arxiv.org/abs/2502.05167)
- Context Length Alone Hurts LLM Performance Despite Perfect Retrieval — Du et al. — arXiv:2510.05381 — Oct 2025 — Intermediate. [UNVERIFIED ID] — [Source](https://arxiv.org/abs/2510.05381)
- LLMs Get Lost In Multi-Turn Conversation — Laban et al. (Microsoft/Salesforce) — arXiv:2505.06120 — 2025 — Intermediate. [UNVERIFIED ID] — [Source](https://arxiv.org/abs/2505.06120)
- Chroma "Context Rot" technical report (see Q2). [V-fetched repo]

Prompt / context optimization
- DSPy: Compiling Declarative Language Model Calls into Self-Improving Pipelines — Khattab et al. — arXiv:2310.03714 — 2023 — Intermediate. [V-indexed; repo V-fetched] — [Source](https://arxiv.org/abs/2310.03714); [Repo](https://github.com/stanfordnlp/dspy)
- Large Language Models as Optimizers (OPRO) — Yang et al. (Google DeepMind) — arXiv:2309.03409 — 2023 — Intermediate. [UNVERIFIED ID] — [Source](https://arxiv.org/abs/2309.03409)
- TextGrad: Automatic "Differentiation" via Text — Yuksekgonul et al. — arXiv:2406.07496 — 2024; published in Nature (2025) — Advanced. [V-fetched repo] — [Source](https://arxiv.org/abs/2406.07496); [Nature](https://www.nature.com/articles/s41586-025-08661-4)
- GEPA: Reflective Prompt Evolution Can Outperform Reinforcement Learning — Agrawal et al. — arXiv:2507.19457 — July 2025 — Advanced. Repo claims "35x faster than RL" (100-500 evaluations vs. 5,000-25,000+ for GRPO); integrated in DSPy; note a 2026 paper used GEPA-optimized red-team prompts to evade monitoring (arXiv:2602.04930). [V-fetched repo] — [Source](https://arxiv.org/abs/2507.19457); [Repo](https://github.com/gepa-ai/gepa)
- Agentic Context Engineering: Evolving Contexts for Self-Improving Language Models (ACE) — Zhang et al. (Stanford/SambaNova) — arXiv:2510.04618 — Oct 2025 (ICLR per search results) — Advanced. Treats contexts as "evolving playbooks" via generation/reflection/curation; repo reports +10.6% on agent tasks (AppWorld) and +8.6% on finance benchmarks. Directly applicable to a hackbot that accumulates technique playbooks. [V-fetched repo] — [Source](https://arxiv.org/abs/2510.04618); [Repo](https://github.com/ace-agent/ace)
- A Survey of Context Engineering for Large Language Models — Mei et al. — arXiv:2507.13334 — July 2025 — All levels (map of the field). [V-indexed] — [Source](https://arxiv.org/abs/2507.13334)

Memory
- MemGPT: Towards LLMs as Operating Systems — Packer et al. — arXiv:2310.08560 — 2023 — Intermediate. FOUNDATIONAL for paged/tiered memory; productized as Letta ("Letta (f.k.a. MemGPT)"; active code now in letta-ai/letta-code). [V-indexed; repo V-fetched] — [Source](https://arxiv.org/abs/2310.08560); [Repo](https://github.com/letta-ai/letta)
- Generative Agents: Interactive Simulacra of Human Behavior — Park et al. — arXiv:2304.03442 — 2023 — Intermediate (memory stream + reflection + retrieval scoring). [V-indexed] — [Source](https://arxiv.org/abs/2304.03442)
- A-MEM: Agentic Memory for LLM Agents — Xu et al. — arXiv:2502.12110 — 2025 — Advanced. [V-indexed] — [Source](https://arxiv.org/abs/2502.12110)
- Mem0: Building Production-Ready AI Agents with Scalable Long-Term Memory — Chhikara et al. — arXiv:2504.19413 — 2025 — Advanced. [V-indexed] — [Source](https://arxiv.org/abs/2504.19413)

### Inferences
- For a hackbot, ACE + GEPA are the most actionable research: the agent's per-vuln-class playbooks (skills) can be evolved from its own successful/failed trajectories, with an eval set of known-vulnerable targets as the fitness signal.
- Long-context papers justify keeping raw recon/HTTP data out of the window: degradation appears well before advertised context limits, and semantic (non-lexical) retrieval from long context is notably worse (NoLiMa, Chroma).

### Gaps
- arXiv itself was unreachable; IDs marked [UNVERIFIED ID] are high-confidence from prior knowledge but were not cross-checked in a fetched file. Author lists for ACE/Survey are from memory.
- Did not verify the "Inverse Scaling in Test-Time Compute" paper (believed arXiv:2507.14417) relevant to overthinking.

---

## Q4. Memory, retrieval, compaction, summarization, structured note-taking, tool-result clearing, prompt caching — practical techniques and best sources

### Takeaway
The practical toolkit is now productized in vendor APIs: Anthropic offers prompt caching, context editing (automatic clearing of stale tool results), server-side compaction, a client-side file-based memory tool, and "context awareness" (model sees its remaining token budget); OpenAI's cookbook has session-memory, long-term memory notes, and memory+compaction recipes. The design principle across all sources: keep the prefix stable and cacheable, keep raw tool output out of the window (store it, reference it), clear stale results first, summarize with a schema only when needed, and persist state to files/git so a fresh window can resume.

### Cited Findings
- **"Managing context on the Claude Developer Platform"** (context editing + memory tool) — Anthropic — 2025-09-29 — Intermediate. "Context editing automatically clears stale tool calls and results from within the context window when approaching token limits"; the memory tool lets Claude "store and consult information outside the context window through a file-based system" and "operates entirely client-side through tool calls" (developer owns storage). [V-fetched] — [Source](https://claude.com/resources/articles/context-management)
- Anthropic docs (living; [V-fetched]) — Intermediate/Advanced:
  - "Prompt caching" — [Source](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
  - "Context editing" — [Source](https://platform.claude.com/docs/en/build-with-claude/context-editing)
  - "Compaction overview" — [Source](https://platform.claude.com/docs/en/build-with-claude/compaction)
  - "Memory tool" — [Source](https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool)
  - "Context windows" (incl. context awareness: Sonnet 5, Sonnet 4.6, Sonnet 4.5, Haiku 4.5 "track... remaining context window (that is, its 'token budget')") — [Source](https://platform.claude.com/docs/en/build-with-claude/context-windows); claim per [best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
- Anthropic "Effective context engineering": compaction, structured note-taking, sub-agents, tool-result clearing, just-in-time retrieval (see Q2). [V-fetched] — [Source](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- Anthropic "Harness design for long-running apps" (2026-03-24): context resets + handoff artifact vs. compaction; context anxiety. [V-fetched] — [Source](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- OpenAI cookbook (dates/authors from registry, [V-fetched registry]):
  - "Context Engineering - Short-Term Memory Management with Sessions" — emreokcular — 2025-09-09 — Intermediate — [Source](https://cookbook.openai.com/examples/agents_sdk/session_memory)
  - "Context Engineering for Personalization - State Management with Long-Term Memory Notes" — emreokcular — 2026-01-05 — Intermediate — [Source](https://cookbook.openai.com/examples/agents_sdk/context_personalization)
  - "Building Reliable Agents with Memory and Compaction" — PASFIELD-OAI, emreokcular — 2026-05-01 — Advanced — [Source](https://cookbook.openai.com/examples/agents_sdk/building_reliable_agents_memory_compaction)
  - "Prompt Caching 101" — 2024-10-01 and "Prompt Caching 201" — erikakettleson-openai — 2026-02-18 — Intermediate — [Source](https://cookbook.openai.com/examples/prompt_caching_201)
- Manus: KV-cache hit rate as the top metric; stable append-only prefix; filesystem as external memory; restorable compression. [V-indexed] — [Source](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)
- Manus (via Lance Martin): full vs. compact tool-result representations; clear stale results first, then schema-based summarization. [V-fetched] — [Source](https://rlancemartin.github.io/2025/10/15/manus/)
- HumanLayer: frequent intentional compaction at 40-60% utilization; Factor 9 "Compact Errors into Context Window." [V-fetched] — [Source](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md); [Source](https://github.com/humanlayer/12-factor-agents)
- Claude Code memory model: "Each Claude Code session begins with a fresh context"; CLAUDE.md files ("instructions you write to give Claude persistent context"), `.claude/rules/` with path-specific rules, AGENTS.md read "in place of CLAUDE.md," and "auto memory." [V-fetched] — [Source](https://code.claude.com/docs/en/memory)
- Counterpoint: full context with prompt caching was often cheaper/faster/better than summarization in one production tutor. [V-indexed] — [Source](https://www.louisbouchard.ai/context-engineering-2026/)
- "LLM Powered Autonomous Agents" — Lilian Weng — 2023-06-23 — Beginner. FOUNDATIONAL memory/planning/tool taxonomy. [UNVERIFIED] — [Source](https://lilianweng.github.io/posts/2023-06-23-agent/)
- "Agents" — Chip Huyen — 2025-01-07 — Intermediate. [UNVERIFIED] — [Source](https://huyenchip.com/2025/01/07/agents.html)
- "Context Engineering: Sessions & Memory" whitepaper — Google (Kaggle 5-Day AI Agents Intensive) — Nov 2025 — Intermediate. [UNVERIFIED] — [Source](https://www.kaggle.com/whitepaper-context-engineering-sessions-and-memory)

### Inferences
- Order of operations for a hackbot's context budget: (1) stable, cached prefix (system prompt + scope + tool defs); (2) tool-result clearing of stale scanner/HTTP output; (3) offload raw artifacts to disk with path references; (4) schema-based summarization (e.g., `{endpoints_tested, hypotheses, confirmed_findings, dead_ends, next_steps}`) only when (2)-(3) stop helping; (5) context reset with a handoff file for very long engagements.
- Anything that mutates the prefix (dynamic timestamps, re-ordering tools, injecting per-target scope mid-prompt) silently kills cache hit rate (Manus) — put volatile data at the end.

### Gaps
- Could not extract quantitative results for context editing / memory tool (e.g., token-reduction percentages) from the Anthropic announcement page text; not reported here.
- No independent benchmark comparing compaction strategies head-to-head was found.

---

## Q5. Context engineering specific to security agents (scope/RoE, recon compression, keeping HTTP responses out of context, attack-surface graphs, Agent Skills/SKILL.md, CLAUDE.md/AGENTS.md)

### Takeaway
There is little authoritative writing that is specifically about "context engineering for offensive agents"; the best evidence is (a) academic pentest agents that externalize state into task trees / shared knowledge bases with planner-executor splits (PentestGPT, hackingBuddyGPT, CAI), (b) open-source hackbots that ship SKILL.md skills and config-driven rules of engagement (Strix, Shannon, Trail of Bits skills), and (c) general Anthropic/Manus techniques (progressive disclosure, file-backed tool results, sub-agent isolation). 2026 preprints have started measuring context stress and long-horizon failure modes in security agents.

### Cited Findings
**Format/convention standards**
- **Agent Skills** — Anthropic docs "Agent Skills" overview — living — Intermediate. [V-fetched] — [Source](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- **"Equipping agents for the real world with Agent Skills"** — Anthropic — 2025-10-16 — Intermediate. Lance Martin quotes it: "Progressive disclosure is the core design principle that makes Agent Skills flexible and scalable... skills let Claude load information only as needed." [V-fetched] — [Source](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
- **anthropics/skills** repo — Anthropic — Beginner/Intermediate. "Each skill is self-contained in its own folder with a `SKILL.md` file containing the instructions and metadata"; points to the open standard at agentskills.io. [V-fetched] — [Source](https://github.com/anthropics/skills); standard: [agentskills.io](https://agentskills.io) [V-indexed]
- **"Extend Claude with skills"** — Claude Code docs — living — Beginner. [V-fetched] — [Source](https://code.claude.com/docs/en/skills)
- **AGENTS.md** — agentsmd/agents.md — "a simple, open format for guiding coding agents... a README for agents." [V-fetched README] — [Source](https://agents.md); [Repo](https://github.com/agentsmd/agents.md)
- **"How Claude remembers your project"** (CLAUDE.md, .claude/rules, AGENTS.md, auto memory) — Claude Code docs — living — Beginner. [V-fetched] — [Source](https://code.claude.com/docs/en/memory)
- **"Best practices for Claude Code"** — Claude Code docs (the old anthropic.com/engineering/claude-code-best-practices URL now redirects here) — "tell Claude what it can't see, give it a check to run, and enforce the rules that can't have exceptions." [V-fetched] — [Source](https://code.claude.com/docs/en/best-practices)
- Simon Willison, Claude Skills post (title believed to be "Claude Skills are awesome, maybe a bigger deal than MCP"; title from memory), section "skills depend on a coding environment" — 2025-10-16 — Beginner. [V-indexed URL] — [Source](https://simonwillison.net/2025/Oct/16/claude-skills/)

**Security-agent systems and papers**
- **PentestGPT** — Deng et al. — USENIX Security 2024 (arXiv:2308.06782, ID UNVERIFIED) — Intermediate. FOUNDATIONAL: a "Pentesting Task Tree" to avoid context loss (per survey snippet); repo now has an autonomous mode while preserving the classic human-in-the-loop version. [V-fetched repo] — [Source](https://github.com/GreyDGL/PentestGPT); [Paper](https://www.usenix.org/conference/usenixsecurity24/presentation/deng)
- **hackingBuddyGPT** — Andreas Happe (TU Wien / ipa-lab) — ongoing; paper "Getting pwn'd by AI: Penetration Testing with Large Language Models" (arXiv:2308.00121, FSE'23) — Intermediate. AD assumed-breach module "uses a planner/executor design: a persistent strategic planner maintains a task tree and shared knowledge base, delegating each task to a fresh" executor; exposes `llm.context_size` for prompt trimming. [V-fetched] — [Source](https://github.com/ipa-lab/hackingBuddyGPT); [Paper](https://arxiv.org/abs/2308.00121)
- **CAI (Cybersecurity AI)** — Alias Robotics — "CAI: An Open, Bug Bounty-Ready Cybersecurity AI" arXiv:2504.06017 (Apr 2025); CAIBench arXiv:2510.24317 (Oct 2025) — Intermediate. README also claims a blackboard architecture composing scaffolds solved 19/33 Cybench challenges vs. 15/33 for the best single scaffold (arXiv:2605.28334) — vendor claim. [V-fetched README] — [Source](https://github.com/aliasrobotics/cai); [Paper](https://arxiv.org/abs/2504.06017)
- **Strix** — usestrix — open-source "Autonomous AI hackers"; ships nine SKILL.md-compatible skills ("Give Claude Code, Cursor, Codex, or any SKILL.md-compatible agent the ability to run pentests"); CLI supports "diff scope, instruction files, and budgets." [V-fetched README] — [Source](https://github.com/usestrix/strix)
- **Shannon** — KeygraphHQ — open-source autonomous AI pentester for web apps/APIs ("No exploit, no report"); supports "rules of engagement through configuration"; ships `llms.txt` / `llms-full.txt` as a context map for agents. [V-fetched README] — [Source](https://github.com/KeygraphHQ/shannon)
- **Trail of Bits Skills Marketplace** — Trail of Bits — "skills and plugins for AI-assisted security analysis, testing, and development" for Claude Code/Codex. Good real-world SKILL.md examples for security. [V-fetched README] — [Source](https://github.com/trailofbits/skills)
- **claude-code-security-review** GitHub Action — Anthropic — diff-aware scanning, false-positive filtering; explicitly "not hardened against prompt injection attacks and should only be used to review trusted PRs." [V-fetched README] — [Source](https://github.com/anthropics/claude-code-security-review)
- "Pentest Harness Architecture for AI Security" — Strobes (vendor blog) — 2025/2026 — Intermediate. Argues loading all skill instructions into the system prompt "would consume most of the context window," so uses progressive disclosure; opinionated tools that auto-inject auth and log history improved results over generic wrappers. [V-indexed] — [Source](https://strobes.co/blog/ai-harness-offensive-security-llm-pentest-architecture/)
- "Towards Optimal Agentic Architectures for Offensive Security Tasks" — arXiv:2604.18718 — Apr 2026 — Advanced. Includes a context-stress pilot. [V-indexed] — [Source](https://arxiv.org/pdf/2604.18718)
- "Beyond End-to-End Success: Diagnosing Failures in Long-Horizon Security LLM Agents" — arXiv:2608.20563 — Aug 2026 — Advanced. [V-indexed] — [Source](https://arxiv.org/pdf/2608.20563)
- "A Survey on Agentic Security: Applications, Threats and Defenses" — arXiv:2510.06445 — Oct 2025 — Intermediate. [V-indexed] — [Source](https://arxiv.org/pdf/2510.06445)
- "LLM-Based Agents for Software and Systems Security: Approaches, Applications, and Assessment" — arXiv:2608.28490 — Aug 2026 — Intermediate. [V-indexed] — [Source](https://arxiv.org/pdf/2608.28490)
- Anthropic "Building AI for cyber defenders" — 2025-09-29 — Beginner. Sonnet 4.5 trained to emphasize cyber skills (matched/eclipsed Opus 4.1 at finding code vulns); references DARPA AIxCC "cyber reasoning systems." [V-fetched] — [Source](https://www.anthropic.com/research/building-ai-cyber-defenders)
- Anthropic "Disrupting an AI-orchestrated cyber espionage campaign" — 2025-11-13 — Beginner. Real-world case of an agentic offensive harness (threat intel; useful for understanding task decomposition by attackers). [V-fetched] — [Source](https://www.anthropic.com/news/disrupting-AI-espionage)
- Google Project Zero "Project Naptime: Evaluating Offensive Security Capabilities of LLMs" — June 2024 — Intermediate. FOUNDATIONAL agent design principles for vuln research (space for reasoning, interactive environment, specialized tools, verification). [UNVERIFIED] — [Source](https://googleprojectzero.blogspot.com/2024/06/project-naptime.html)

### Inferences (proposed hackbot context design, synthesized from the sources above)
- **System prompt (stable, cached prefix):** identity + program scope + rules of engagement + safety/reversibility policy at the "right altitude" (Anthropic), in XML sections (e.g., `<scope>`, `<out_of_scope>`, `<rate_limits>`, `<prohibited_actions>`, `<reporting_format>`). Use calm declarative language, not "CRITICAL/MUST" (best practices). Enforce scope in code/egress too, not only in the prompt (see Q6).
- **Keep HTTP responses and scanner output out of context:** tools should return compact summaries (status, length, key headers, diff vs. baseline, reflected-param hits) and write full bodies to disk, returning a file path (Manus full/compact pattern; Anthropic code-execution-with-MCP; Strobes "opinionated tools"). Clear stale tool results first (context editing).
- **Attack-surface knowledge graph / blackboard as external memory:** hosts -> endpoints -> params -> auth contexts -> hypotheses -> evidence, persisted as structured JSON/DB and queried just-in-time; this generalizes PentestGPT's task tree, hackingBuddyGPT's planner-maintained task tree + shared knowledge base, and CAI's blackboard.
- **Planner + isolated executors:** a planner owns scope, priorities and exploit-chain decisions; executor sub-agents (per endpoint or vuln class) get fresh contexts and return condensed findings (Manus, Anthropic multi-agent, hackingBuddyGPT). Keep conflicting decisions out of parallel workers (Cognition).
- **Skills = per-vuln-class playbooks** (IDOR, SSRF, XSS, OAuth, upload) loaded by progressive disclosure via SKILL.md; per-program notes (tech stack, auth flow, known dupes) in CLAUDE.md/AGENTS.md-style files; evolve playbooks from trajectories (ACE).
- **Long engagements:** progress files + git-tracked notes so a fresh window can resume (Anthropic harness posts); measure with an eval set of intentionally vulnerable targets (Anthropic evals post).

### Gaps
- Found no first-party long-form write-up from XBOW, Google Big Sleep, or major bug-bounty platforms specifically on context engineering for their hackbots (search budget ran out before XBOW-specific queries could run; XBOW blog host was blocked).
- No X/Twitter threads by @dexhorthy/@dbreunig/@simonw specifically on offensive-agent context were found.
- The security-agent preprints (2604.18718, 2608.20563, 2608.28490) were seen only as search results; their content was not read.

---

## Q6. Prompt-injection risk for the hackbot itself (target pages containing injected instructions)

### Takeaway
A hackbot is the worst case for indirect prompt injection: it deliberately ingests attacker-controlled content, holds secrets (session tokens, credentials, unreported findings), and must make outbound requests — Simon Willison's "lethal trifecta" by design. Model-level robustness has improved (Anthropic reports ~1% attack success for Opus 4.5 in browser use against an adaptive attacker) but no vendor claims immunity, so the defense must be architectural: containment/egress allowlists tied to scope, untrusted-content tagging/classifiers, plan-then-execute or CaMeL-style capability separation, and human approval for irreversible or out-of-scope actions.

### Cited Findings
- **"The lethal trifecta for AI agents: private data, untrusted content, and external communication"** — Simon Willison — 2025-06-16 — Beginner. Core mental model: an agent with all three can be tricked into exfiltrating data. [UNVERIFIED in-session; high confidence] — [Source](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/)
- **CaMeL: "Defeating Prompt Injections by Design"** — Debenedetti et al. (Google DeepMind / ETH) — arXiv:2503.18813 — Mar 2025 — Advanced. Capability-based separation of control flow (trusted planner) from data flow (quarantined LLM); repo warns the interpreter "might not be fully secure." [V-fetched repo] — [Source](https://arxiv.org/abs/2503.18813); [Repo](https://github.com/google-research/camel-prompt-injection); Willison commentary (2025-04-11) [UNVERIFIED] — [Source](https://simonwillison.net/2025/Apr/11/camel/)
- "Design Patterns for Securing LLM Agents against Prompt Injections" — Beurer-Kellner et al. — arXiv:2506.08837 — June 2025 — Advanced. Six patterns (action-selector, plan-then-execute, LLM map-reduce, dual LLM, code-then-execute, context-minimization). [UNVERIFIED ID] — [Source](https://arxiv.org/abs/2506.08837)
- "Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection" — Greshake et al. — arXiv:2302.12173 — 2023 — Intermediate. FOUNDATIONAL. [UNVERIFIED ID] — [Source](https://arxiv.org/abs/2302.12173)
- AgentDojo — Debenedetti et al. — arXiv:2406.13352 — 2024 — Advanced (benchmark for injection attacks/defenses on tool-using agents). [UNVERIFIED ID] — [Source](https://arxiv.org/abs/2406.13352)
- Spotlighting (Microsoft: "Defending Against Indirect Prompt Injection Attacks With Spotlighting") — Hines et al. — arXiv:2403.14720 — 2024 — Intermediate (delimiting/datamarking/encoding untrusted input). [UNVERIFIED ID] — [Source](https://arxiv.org/abs/2403.14720)
- **"Hacking the AI Hackers via Prompt Injection"** — Alias Robotics (CAI team) — arXiv:2508.21669 — Aug 2025 — Intermediate. Directly on-topic: "AI security tools are themselves injectable"; proposes and empirically validates a "four-layer guardrail defense." [V-fetched citation in CAI README] — [Source](https://arxiv.org/abs/2508.21669)
- **"Mitigating the risk of prompt injections in browser use"** (page title "Mitigating prompt injections in browser use") — Anthropic — 2025-11-24 — Intermediate. RL training against injections embedded in simulated web content; classifiers that "scan all untrusted content that enters the model's context window"; expert red-teaming; "A 1% attack success rate—while a significant improvement—still represents meaningful risk. No browser agent is immune to prompt injection." [V-fetched] — [Source](https://www.anthropic.com/research/prompt-injection-defenses)
- **"How we contain Claude across products"** — Anthropic — 2026-05-25 — Advanced. Cap the "blast radius" by containment — "sandboxes, virtual machines, and egress controls" — rather than only supervising behavior; Claude models have "'helpfully' escape[d] a sandbox in order to complete a task"; "Claude Mythos Preview is an example of a model whose blast radius was deemed too high to ship in April 2026." [V-fetched] — [Source](https://www.anthropic.com/engineering/how-we-contain-claude)
- "Making Claude Code safer with sandboxing" — Anthropic — 2025-10-20 — Intermediate (filesystem + network isolation). [V-fetched] — [Source](https://www.anthropic.com/engineering/claude-code-sandboxing)
- "How we built Claude Code auto mode" — Anthropic — 2026-03-25 — Intermediate. Users approve 93% of permission prompts; classifiers reduce approval fatigue. [V-fetched] — [Source](https://www.anthropic.com/engineering/claude-code-auto-mode)
- "Mitigate jailbreaks and prompt injections" — Anthropic docs — living — Beginner. Harmlessness screens with a lightweight model (e.g., Haiku 5.5) and input validation for known injection patterns. [V-fetched] — [Source](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/mitigate-jailbreaks)
- **OWASP GenAI LLM Top 10 2026** — OWASP GenAI Security Project — published 2026-08-04 — Beginner. Current release; the 2025 edition (where LLM01 = Prompt Injection) is now marked "archived release." [V-fetched via OWASP repo README] — [Source](https://genai.owasp.org/resource/owasp-genai-llm-top-10-2026/); 2025 edition: [Source](https://genai.owasp.org/resource/owasp-top-10-for-llm-applications-2025/); LLM01:2025 page [UNVERIFIED] — [Source](https://genai.owasp.org/llmrisk/llm01-prompt-injection/)
- OWASP Top 10 for Agentic Applications (for 2026) — OWASP GenAI Security Project — Dec 2025 — Intermediate. [UNVERIFIED] — [Source](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/)
- "Agents Rule of Two: A Practical Approach to AI Agent Security" — Meta AI — ~2025-10-31 — Beginner. An agent should satisfy at most two of: processing untrusted input, accessing sensitive data, changing state/communicating externally, without human oversight. [UNVERIFIED] — [Source](https://ai.meta.com/blog/practical-ai-agent-security/)
- OpenAI "Understanding prompt injections: a frontier security challenge" — Nov 2025 — Beginner. [UNVERIFIED] — [Source](https://openai.com/index/prompt-injections/)
- Microsoft MSRC "How Microsoft defends against indirect prompt injection attacks" — July 2025 — Intermediate. [UNVERIFIED] — [Source](https://msrc.microsoft.com/blog/2025/07/how-microsoft-defends-against-indirect-prompt-injection-attacks/)

### Inferences
- Map the hackbot onto the trifecta / Rule of Two: untrusted content (target responses) is unavoidable and external communication (HTTP to target) is the job, so the controllable leg is private data and the reach of communication. Concretely: egress allowlist = in-scope hosts only (enforced at the network layer, not the prompt); keep platform API keys, other programs' data and unreported findings out of the executor's context and environment; route report submission through a separate, non-browsing component with human approval.
- Treat every tool result from the target as data: wrap in tagged blocks (e.g., `<untrusted_http_response>`), tell the model instructions inside are never to be followed, and optionally run a cheap classifier/screen on bodies before they enter context (Anthropic docs, spotlighting). Prompt-level defenses alone are insufficient (Anthropic's own 1% ASR caveat).
- A plan-then-execute split (planner never sees raw target content; executors return only schema-constrained findings) is both a context-engineering win and a CaMeL-style injection defense — the same architecture serves both goals.

### Gaps
- Could not verify the OWASP Agentic Top 10 list contents, Meta's Rule of Two post, OpenAI's injection guidance, or MSRC's post in-session.
- Did not find the specific contents of the OWASP GenAI LLM Top 10 2026 (whether prompt injection remains #1).

---

## Q7. Recommended reading sequence (synthesis across Q1-Q6)

### Takeaway
Read in five passes: foundations (1-2 days), context engineering canon (2-3 days), long-horizon harness and memory (2 days), security-specific architecture and injection defense (2 days), then research/optimization papers as reference.

### Cited Findings
Pass 1 — Foundations (Beginner)
1. Anthropic "Building effective agents" (2024-12-19) — [Source](https://www.anthropic.com/engineering/building-effective-agents)
2. Anthropic "Prompt engineering overview" -> "Prompting best practices" -> your model's page — [Source](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
3. Anthropic interactive tutorial (skim; dated) — [Source](https://github.com/anthropics/prompt-eng-interactive-tutorial)
4. OpenAI GPT-5.2 Prompting Guide (2025-12-11) for cross-vendor perspective — [Source](https://cookbook.openai.com/examples/gpt-5/gpt-5-2_prompting_guide)
5. The Prompt Report (arXiv:2406.06608) as a taxonomy reference — [Source](https://arxiv.org/abs/2406.06608)

Pass 2 — Context engineering canon (Intermediate)
6. Karpathy X post (2025-06-25) + Simon Willison "Context engineering" (2025-06-27) — [Source](https://x.com/karpathy/status/1937902205765607626); [Source](https://simonwillison.net/2025/Jun/27/context-engineering/)
7. Drew Breunig "How Long Contexts Fail" then "How to Fix Your Context" — [Source](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html); [Source](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html)
8. Chroma "Context Rot" — [Source](https://research.trychroma.com/context-rot)
9. LangChain "Context Engineering for Agents" (write/select/compress/isolate) — [Source](https://blog.langchain.com/context-engineering-for-agents/)
10. Anthropic "Effective context engineering for AI agents" (2025-09-29) — [Source](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
11. Manus "Lessons from Building Manus" (2025-07-18) + Lance Martin "Context Engineering in Manus" (2025-10-15) — [Source](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus); [Source](https://rlancemartin.github.io/2025/10/15/manus/)
12. Cognition "Don't Build Multi-Agents" vs. Anthropic "How we built our multi-agent research system" — [Source](https://cognition.ai/blog/dont-build-multi-agents); [Source](https://www.anthropic.com/engineering/multi-agent-research-system)
13. HumanLayer 12-Factor Agents + "Getting AI to Work in Complex Codebases" — [Source](https://github.com/humanlayer/12-factor-agents); [Source](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md)

Pass 3 — Tools, memory, long-horizon harnesses (Intermediate/Advanced)
14. Anthropic "Writing effective tools for AI agents" — [Source](https://www.anthropic.com/engineering/writing-tools-for-agents)
15. Anthropic context management (context editing + memory tool) + docs on prompt caching/compaction — [Source](https://claude.com/resources/articles/context-management); [Source](https://platform.claude.com/docs/en/build-with-claude/compaction)
16. Anthropic "Code execution with MCP" and "Advanced tool use" — [Source](https://www.anthropic.com/engineering/code-execution-with-mcp); [Source](https://www.anthropic.com/engineering/advanced-tool-use)
17. Anthropic "Effective harnesses for long-running agents" (2025-11-26) then "Harness design for long-running apps" (2026-03-24) — [Source](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents); [Source](https://www.anthropic.com/engineering/harness-design-long-running-apps)
18. OpenAI "Building Reliable Agents with Memory and Compaction" (2026-05-01) — [Source](https://cookbook.openai.com/examples/agents_sdk/building_reliable_agents_memory_compaction)
19. Anthropic "Demystifying evals for AI agents" (2026-01-09) — [Source](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)

Pass 4 — Security agents and injection defense (Intermediate/Advanced)
20. Agent Skills post + docs + anthropics/skills; Claude Code memory docs (CLAUDE.md/AGENTS.md) — [Source](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills); [Source](https://code.claude.com/docs/en/memory)
21. Study real hackbot repos: hackingBuddyGPT (planner/executor + task tree), PentestGPT, CAI, Strix (SKILL.md), Shannon (RoE config), Trail of Bits skills — [Source](https://github.com/ipa-lab/hackingBuddyGPT); [Source](https://github.com/usestrix/strix); [Source](https://github.com/KeygraphHQ/shannon); [Source](https://github.com/trailofbits/skills)
22. Simon Willison "lethal trifecta" -> CaMeL (arXiv:2503.18813) -> "Hacking the AI Hackers via Prompt Injection" (arXiv:2508.21669) — [Source](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/); [Source](https://arxiv.org/abs/2503.18813); [Source](https://arxiv.org/abs/2508.21669)
23. Anthropic "Mitigating prompt injections in browser use" + "How we contain Claude across products" — [Source](https://www.anthropic.com/research/prompt-injection-defenses); [Source](https://www.anthropic.com/engineering/how-we-contain-claude)
24. OWASP GenAI LLM Top 10 2026 — [Source](https://genai.owasp.org/resource/owasp-genai-llm-top-10-2026/)

Pass 5 — Research/optimization (Advanced, reference)
25. Lost in the Middle (2307.03172), RULER (2404.06654), NoLiMa (2502.05167) — [Source](https://arxiv.org/abs/2307.03172)
26. DSPy (2310.03714) -> GEPA (2507.19457) -> ACE (2510.04618) — [Source](https://arxiv.org/abs/2510.04618)
27. MemGPT (2310.08560), Mem0 (2504.19413), A-MEM (2502.12110) — [Source](https://arxiv.org/abs/2310.08560)
28. Survey of Context Engineering (2507.13334) as a map — [Source](https://arxiv.org/abs/2507.13334)

Superseded / dated items to flag
- Anthropic interactive tutorial (Claude 3 Haiku; prefill-era techniques) — dated but still useful for fundamentals. — [Source](https://github.com/anthropics/prompt-eng-interactive-tutorial)
- The old URL anthropic.com/engineering/claude-code-best-practices now redirects to Claude Code docs "Best practices for Claude Code" (content has been re-homed and rewritten). — [Source](https://code.claude.com/docs/en/best-practices)
- GPT-4.1 Prompting Guide is superseded by GPT-5.x guides for current OpenAI models. — [Source](https://cookbook.openai.com/examples/gpt4-1_prompting_guide)
- OWASP Top 10 for LLM Applications 2025 is now "archived" in favor of the 2026 release. — [Source](https://github.com/OWASP/www-project-top-10-for-large-language-model-applications)
- Manual CoT / self-consistency prompting is largely superseded by built-in extended/adaptive thinking and effort controls. — [Source](https://platform.claude.com/docs/en/build-with-claude/extended-thinking)

### Inferences
- Count: this note lists 80+ distinct resources; ~45 are [V-fetched], ~25 [V-indexed], the remainder [UNVERIFIED] and labeled.
- The minimum viable reading list for a hackbot builder is items 2, 10, 11, 13, 15, 17, 20, 21, 22, 23 — roughly 10 documents.

### Gaps
- Talks/videos were not systematically verified (e.g., Dex Horthy's AI Engineer talk linked from the 12-factor README at youtube.com/watch?v=8kMaTybvDUw; Lance Martin/Peak Ji webinar at youtu.be/6_BcCthVvb8; Karpathy's June 2025 YC talk). They are linked from fetched primary files but their content was not reviewed.
- Web-search budget exhaustion prevented a final sweep for late-2026 (Aug-Oct 2026) context-engineering posts beyond what is listed.
