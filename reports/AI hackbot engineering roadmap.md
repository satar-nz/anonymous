# Hackbot Engineering Starts With the Verifier

As of 27 October 2026, the most authoritative path to an authorized-testing hackbot runs through a small canon, and every credible source in it lands on the same design. Use one strong agent in a simple tool loop. Keep the context clean. Enforce scope in the network, not the prompt. Require a deterministic validator to confirm every finding before a human signs the report. The engineering core is Anthropic's 2025–2026 engineering series: context engineering, long-running harnesses, auto mode, containment, evals, and an October 2026 post finding that a single agent with skills often beats subagent designs. Around it sit OpenAI's Codex agent-loop and "harness engineering" posts and its GPT-5.x prompting guides; the context-engineering write-ups from Manus, LangChain, Drew Breunig and Chroma; and the Cognition-versus-Anthropic multi-agent debate, which Google's 180-configuration scaling study puts numbers on. The security layer has five parts: papers (PentestGPT, Incalmo, HPTSA, MAPTA, ARTEMIS); industry evidence (XBOW's validator posts, Anthropic's GTG-1002 report, Big Sleep, the AIxCC cyber reasoning systems); real-vulnerability benchmarks (CVE-Bench, BountyBench, CyberGym), run through UK AISI's Inspect; open-source reference hackbots (Strix, Shannon, PentAGI, Decepticon, RedAmon, RAPTOR); and the Critical Thinking podcast's 2026 hackbot episodes. Three 2026 shifts change how you should learn. Classic CTF suites are saturated; XBOW's 104-challenge set sits near 100%. Frontier providers now gate offensive capability: Anthropic's generally available models re-route most cyber tasks to Opus 4.8, and individuals can get only Defense Access. Every major bug-bounty platform now requires human validation and penalizes AI slop. This report catalogs the resources across every discipline and medium, each tagged with its verification status. It then turns that catalog into an eight-stage learning roadmap, about ten months at 10 hours a week to a first supervised submission, with AI-engineering and offensive-fundamentals tracks run in parallel. It adds a component-level reference architecture, a dated evaluation ladder, guardrails (scope controls, the hackbot's own security, platform and provider rules, legal basics) and a first-week checklist. Treat the verification tags as part of the answer. Research access favored Anthropic and GitHub hosts, so those items carry stronger tags because they were reachable, not because they are more authoritative. No Reddit thread could be verified at all.

## Four tags show how far each resource was checked

Eleven research note files fed this report. They were compiled on 2026-10-27 under heavy network limits. The sandbox could read anthropic.com, claude.com, code.claude.com, platform.claude.com, raw.githubusercontent.com, the GitHub API (for one tooling pass), PyPI and npm. The egress proxy or DNS blocked arxiv.org, openai.com, xbow.com, hackerone.com, bugcrowd.com, reddit.com, youtube.com, x.com, news.ycombinator.com and most blogs. A shared web-search budget ran out partway through several passes, so much of the confirmation came from search-engine results on the canonical domain, not from reading pages. Two late passes re-checked earlier items specifically: one for papers, docs and policies, one for social and video links. To keep the record honest, this report uses one tag scheme throughout, mapped from the notes' different legends.

| Tag | Meaning | Mapped from note legends |
|---|---|---|
| **F** | The primary page, official repo README or source file, or package-registry record was fetched and read | [F], [F-src], [V-fetched], GitHub API/README fetched, PyPI/npm records, [V] where the README or BibTeX was read |
| **P** | The exact URL and title were confirmed on the primary domain (a search result, or a link inside a fetched primary page), but the body was not read in full; summaries come from snippets | [S], [L], [V-indexed], [V1], [V] (search on canonical domain) |
| **S** | Secondary only: an aggregator, citing paper, press article, podcast directory or mirror; check before quoting | [V2], "search summary" of a third-party page, aggregator/weak |
| **U** | Unverified: recorded from prior knowledge and not confirmed during research | [U], [K], [UNVERIFIED], "from memory" |

Where notes disagree, the report uses the strongest status any note records, and gives the two dedicated verification passes the final word. No item is tagged higher than some note recorded it. Compound tags such as "P; date S" mean the URL was confirmed on the primary domain but a detail such as the date came from a secondary source. One structural bias deserves emphasis. Almost every F tag on blog-style material belongs to Anthropic, because anthropic.com was reachable and openai.com, cognition.ai, xbow.com and simonwillison.net were not. OpenAI's harness and loop posts, Cognition's essays and XBOW's validator posts are just as important to this field. They carry P tags only because of where the research could reach.

## Eight disciplines collapse into one harness around a verifier

The vocabulary sounds like eight fields, but it describes one stack. An **agent loop** is the inner cycle: the model reads its context, emits text or tool calls, the harness runs the tools and appends the results, and the cycle repeats until the model stops calling tools or hits a limit. Anthropic's Claude Code docs call the surrounding layer "the agentic harness" ([Claude Code docs](https://code.claude.com/docs/en/how-claude-code-works)). Anthropic's Lance Martin defines a harness as "the loop, tools, context management, and guardrails that turn raw intelligence into a working agent" ([Claude blog, 2026-04-02](https://claude.com/resources/articles/harnessing-claudes-intelligence)). **Harness engineering** became a named practice with OpenAI's February 2026 post. In it, three engineers steered Codex to write roughly a million lines across about 1,500 pull requests, and the early bottleneck was an under-specified environment rather than model capability ([OpenAI](https://openai.com/index/harness-engineering/)). Credit for the phrase itself is contested; secondary sources also point to a Mitchell Hashimoto post ([Faros AI](https://www.faros.ai/blog/harness-engineering)). **Loop engineering** traces to Simon Willison's September 2025 "Designing agentic loops" ([simonwillison.net](https://simonwillison.net/2025/Sep/30/designing-agentic-loops/)). IBM defines it as designing loops that "iteratively guide AI agents toward completing user-defined goals with minimal human intervention" ([IBM](https://www.ibm.com/think/topics/loop-engineering)). **Graph engineering** is younger still. An August 2026 survey frames it as the stage after prompt, context, harness and loop engineering ([arXiv 2608.21156](https://arxiv.org/abs/2608.21156)), and practitioners use the term for at least three different things: execution graphs, graphs of loops, and knowledge graphs ([PuppyOne](https://www.puppyone.ai/en/blog/graph-engineering-ai-agents-map)). The honest reading is that "loop" and "graph" engineering are useful labels whose boundaries are, as of October 2026, mostly marketing. The durable skills under them are tool design, context hygiene, containment, verification and evaluation.

**Context engineering** has the clearest lineage and the most settled toolkit. Tobi Lütke ([X, 2025-06-19](https://x.com/tobi/status/1935533422589399127)) and Andrej Karpathy ([X, 2025-06-25](https://x.com/karpathy/status/1937902205765607626)) popularized the term in June 2025. Within about three months LangChain, Drew Breunig, Manus, Chroma and Anthropic had codified it. Anthropic's canonical September 2025 post treats context as "a finite resource with diminishing marginal returns" and recommends just-in-time retrieval, compaction, structured notes, tool-result clearing and sub-agents ([Anthropic](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)). **Prompt engineering** has not disappeared. It has become model-versioned living documentation. Anthropic's current guide tells builders to soften "CRITICAL: You MUST" language that newer models over-trigger on, to migrate away from prefill, and to write explicit agentic guidance on state tracking, reversibility and subagent use ([Anthropic docs](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)).

The strongest 2026 trend is subtraction. Anthropic's long-running-harness post argues that "every component in a harness encodes an assumption about what the model can't do on its own." Its example is context resets, which were essential for Sonnet 4.5 and unnecessary for Opus 4.5 ([Anthropic, 2026-03-24](https://www.anthropic.com/engineering/harness-design-long-running-apps)). Its newest guidance (22 October 2026) reports that across enterprise deployments "a single agent with skills frequently outperformed" both monolithic prompts and subagent designs. One customer saw roughly 2x faster runtime, and many designs need only "one clear system prompt, 4-5 thin data tools, and firm output guardrails" ([Claude blog](https://claude.com/resources/articles/building-agents-when-to-fold-subagents-into-your-main-agent-loop)). Security research says the same about bounded tasks. Palisade reached **95% on InterCode-CTF** with plain prompting, tools and retries ([arXiv 2412.02776](https://arxiv.org/abs/2412.02776)). The counter-evidence matters just as much. On ten multistage networks, popular LLMs alone "reliably conduct only reconnaissance," but with Incalmo's high-level action layer plus its environment-state and attack-graph services they executed attacks in **9 of 10** networks ([arXiv 2501.16466](https://arxiv.org/abs/2501.16466)). These findings fit together. Harness structure pays in proportion to how long, branching and stateful the engagement is. A single-host CTF needs almost none. A multi-host network or a two-account IDOR chain needs a great deal.

The multi-agent debate resolves the same way once the numbers sit side by side. Anthropic's orchestrator-worker research system beat a single Opus 4 agent by **90.2%**, but at about **15x chat tokens**, and token usage alone explained 80% of performance variance ([Anthropic](https://www.anthropic.com/engineering/multi-agent-research-system)). Cognition countered that "actions carry implicit decisions," so parallel agents with partial context make conflicting choices ([Cognition](https://cognition.ai/blog/dont-build-multi-agents)). Its April 2026 follow-up concedes that narrow flows now work while parallel-writer swarms still do not ([Cognition](https://cognition.ai/blog/multi-agents-working)). Google's study of 180 configurations found independent multi-agent systems amplify errors up to **17.2x**, against **4.4x** for centralized ones ([arXiv 2512.08296](https://arxiv.org/abs/2512.08296)). The MAST taxonomy reports that gains on popular benchmarks "are often minimal" ([arXiv 2503.13657](https://arxiv.org/abs/2503.13657)). Carlini's 16-agent compiler project found that "sixteen agents didn't help" on one monolithic bug until an oracle split the work ([Anthropic](https://www.anthropic.com/engineering/building-c-compiler)). The rule for a hackbot follows. Parallelize breadth-first read work such as recon, endpoint triage and per-class probing on disjoint endpoints. Keep stateful exploit chains single-threaded. Centralize verification. XBOW's "alloy" solvers add a refinement: after about 80 iterations, start a fresh solver rather than let one accumulate "misunderstandings and false assumptions" ([XBOW](https://www.xbow.com/blog/alloy-agents)).

Every thread above ends at the verifier, which is why this report puts it at the center. Carlini wrote that "the task verifier is nearly perfect, otherwise Claude will solve the wrong problem" ([Anthropic](https://www.anthropic.com/engineering/building-c-compiler)). Anthropic's harness team found that "out of the box, Claude is a poor QA agent" that talks itself out of real issues, so it built a separate, skeptical evaluator ([Anthropic](https://www.anthropic.com/engineering/harness-design-long-running-apps)). Claude Code's creator says giving the model a way to verify its work "will 2-3x the quality" ([X, 2026-01-02](https://x.com/bcherny/status/2007179861115511237)). The offensive record sharpens the point:

- In the GTG-1002 campaign, Claude "occasionally hallucinated credentials or claimed to have extracted secret information that was in fact publicly-available" ([Anthropic](https://www.anthropic.com/news/disrupting-AI-espionage)).
- Sean Heelan's o3 found a known kernel bug in only **8 of 100** runs, with a high false-positive rate ([sean.heelan.io](https://sean.heelan.io/2025/05/22/how-i-used-o3-to-find-cve-2025-37899-a-remote-zeroday-vulnerability-in-the-linux-kernels-smb-implementation/)).
- Stanford's ARTEMIS outperformed 9 of 10 human professionals on a live network with an **82% valid-submission rate**, and the authors still flag higher agent false positives ([arXiv 2512.09882](https://arxiv.org/abs/2512.09882)).
- XBOW runs discovery agents and validators as separate systems ([XBOW](https://xbow.com/blog/we-ran-1060-autonomous-attacks)).
- AIxCC finalists reported nothing "that has not crashed a real build" ([FuzzingBrain repo](https://github.com/o2lab/afc-crs-all-you-need-is-a-fuzzing-brain)).

Validators can also be fooled. In Team Atlanta's 2026 study, **20–40% of patches that passed every automated check were semantically wrong** ([Team Atlanta](https://github.com/Team-Atlanta/team-atlanta.github.io/blob/main/content/english/blog/post-patch-2026-ensemble.md)). So a hackbot needs deterministic validators plus periodic manual audits of what they accept.

Two smaller debates remain open, and each should be settled with your own evals. The first is compaction. Anthropic's 2026 harness post and Dex Horthy favor resets or "frequent intentional compaction" at 40–60% context utilization ([HumanLayer](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md)). A 2026 practitioner write-up found full context plus prompt caching often cheaper and better than summarizing ([Bouchard](https://www.louisbouchard.ai/context-engineering-2026/)). Manus calls KV-cache hit rate "the single most important metric for a production-stage AI agent" ([Manus](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)). The second is containment versus supervision, and there the evidence is one-sided. In an Anthropic red-team exercise, a phished prompt got Claude Code to exfiltrate AWS credentials in **24 of 25** tries. Users approve roughly **93%** of permission prompts. And "the weakest layer is the one you built yourself," which in one incident was a custom egress proxy ([Anthropic](https://www.anthropic.com/engineering/how-we-contain-claude)). For a hackbot, whose job is to read hostile content and send network traffic, the environment rather than the prompt is the only boundary that holds.

## The engineering canon, ranked by authority and freshness

The tables in this section list the general agent-engineering resources a hackbot builder needs, with the strongest and most current items first in each table. Levels are B (beginner), I (intermediate) and A (advanced). "Living" means a continuously updated official document. Anthropic's `anthropic.com/engineering/<slug>` URLs now redirect to `anthropic.com/research/<slug>`, and the old Claude Code best-practices post redirects to the Claude Code docs. Both forms resolve.

### Prompt engineering: per-model living docs replaced tricks

Read the vendor page for the exact model you deploy first, then the cross-vendor guides. The classic reasoning papers are for intuition only, because current reasoning models already do internally what those papers taught. Automated prompt optimizers (OPRO, DSPy, GEPA) become relevant once you have an eval set to optimize against.

| Resource | Date | Level | Why it matters for a hackbot | Tag |
|---|---|---|---|---|
| [Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices), Anthropic | Living (covers Fable/Mythos 5.1, Opus 5.5, Sonnet 5.5, Haiku 5.5) | B→A | The most important current reference. Its agentic sections (state tracking across context windows, subagent damping, a reversibility prompt, overeagerness) map directly onto rules of engagement. Tells you to drop prefill and all-caps commands | F |
| [Prompt engineering overview](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview), Anthropic | Living | B | Entry point to every technique page | F |
| [Prompting Claude Opus 5](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5) and sibling per-model pages | Living | I | Per-model differences in verbosity, subagent eagerness and effort, and passing thinking blocks back unchanged | F (Opus 5); P (siblings) |
| [Extended thinking](https://platform.claude.com/docs/en/build-with-claude/extended-thinking), Anthropic | Living | I | Effort and thinking budgets replace hand-written chain-of-thought | F |
| [The "think" tool](https://www.anthropic.com/engineering/claude-think-tool), Anthropic | 2025-03-20 | I | A policy check mid-trajectory, between tool calls. Partly superseded by interleaved thinking | F |
| [Prompt Engineering Interactive Tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial), Anthropic | 2024 | B | Nine chapters of exercises. Dated (Claude 3 Haiku, prefill era), but the fundamentals hold | F |
| [anthropics/courses](https://github.com/anthropics/courses), Anthropic | 2024–25 | B–I | Five notebook courses, including prompt evaluations and tool use | P |
| [GPT-5.2 Prompting Guide](https://cookbook.openai.com/examples/gpt-5/gpt-5-2_prompting_guide), OpenAI | 2025-12-11 | I | The newest general GPT guide in the cookbook registry. Use it for a cross-vendor view | F (registry) |
| [Codex Prompting Guide](https://cookbook.openai.com/examples/gpt-5/codex_prompting_guide), OpenAI | 2026-02-25 | I–A | Prompting a coding agent, which carries over to tool-heavy offensive loops | F (registry) |
| [GPT-5 prompting guide](https://cookbook.openai.com/examples/gpt-5/gpt-5_prompting_guide), OpenAI | 2025-08-07 | I | Agentic eagerness control, tool preambles, reasoning_effort | F (registry); content U |
| [GPT-4.1 Prompting Guide](https://cookbook.openai.com/examples/gpt4-1_prompting_guide), OpenAI | 2025-04-14 | I | OpenAI's first agentic guide. Historical now, superseded by the GPT-5.x guides | F (registry) |
| [Building resilient prompts using an evaluation flywheel](https://cookbook.openai.com/examples/evaluation/Building_resilient_prompts_using_an_evaluation_flywheel), OpenAI with Hamel Husain | 2025-10-06 | I | Changing prompts based on eval results instead of intuition | F (registry) |
| [Reasoning items in the Responses API](https://cookbook.openai.com/examples/responses_api/reasoning_items), OpenAI | 2025-05-11 | I | Passing reasoning state across tool calls | F (registry) |
| Reported GPT-5.6 "Sol" prompting guide ([Decrypt](https://decrypt.co/373439/openai-new-gpt-5-6-prompt-guide-chatgpt)) | 2026 | I | Says leaner prompts and programmatic tool calling. Not in the cookbook registry | S (contradicted by registry) |
| [OpenAI prompt engineering](https://platform.openai.com/docs/guides/prompt-engineering) and [reasoning best practices](https://platform.openai.com/docs/guides/reasoning-best-practices) | Living | B–I | Platform docs | U |
| [Gemini prompt design strategies](https://ai.google.dev/gemini-api/docs/prompting-strategies), Google | Living | B | Gemini-specific guidance | U |
| [Prompt Engineering whitepaper](https://www.kaggle.com/whitepaper-prompt-engineering), Lee Boonstra (Google) | Feb 2025 | B | Catalogue of classic techniques | U |
| [Prompt Engineering Guide](https://www.promptingguide.ai), DAIR.AI | Continuously updated | B | Community encyclopedia | P |
| [The Prompt Report](https://arxiv.org/abs/2406.06608), Schulhoff et al. | 2024, revised 2025 | All | The best single taxonomy, covering 58+ techniques | P |
| [Chain-of-Thought](https://arxiv.org/abs/2201.11903) (2201.11903); [Self-Consistency](https://arxiv.org/abs/2203.11171) (2203.11171); [Tree of Thoughts](https://arxiv.org/abs/2305.10601) (2305.10601) | 2022–23 | B–I | Background intuition. Self-consistency still applies to k-vote triage | P; P; F |
| [OPRO](https://arxiv.org/abs/2309.03409); [DSPy](https://arxiv.org/abs/2310.03714); [TextGrad](https://arxiv.org/abs/2406.07496); [GEPA](https://arxiv.org/abs/2507.19457) | 2023–25 | A | Automated prompt optimization. The GEPA repo claims about 35x fewer evaluations than RL. Use these to tune per-vuln-class prompts against a frozen eval set | F; P (repo F); P (repo F); P (repo F) |
| [*Prompt Engineering for LLMs*](https://www.oreilly.com/library/view/-/9781098156145), Berryman & Ziegler (O'Reilly) | Nov 2024 | I | The best book on assembling context. Ziegler is Head of AI at XBOW | P |
| ["Prompting for Agents"](https://www.youtube.com/watch?v=XSZP9GhhuAc), Hannah Moran & Jeremy Hadfield (Anthropic) | 2025-05-22 | I | Anthropic's applied team on writing agent prompts | P |
| ["Boris Cherny: We Cut 80% of Claude Code's Prompt"](https://www.youtube.com/watch?v=qyPCVqFUyDo), Startup School | 2026 | I | First-hand evidence that prompts shrink as models improve | P |
| ["Prompt Engineering"](https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/), Lilian Weng | 2023-03-15 | I | Foundational. Outdated on model specifics | U |

### Context engineering: from viral tweet to API features in a year

Read in roughly this order: the definitional posts, then the failure modes (Breunig, Chroma), then the production lessons (Manus, HumanLayer), then the vendor features that now implement them (caching, context editing, compaction, the memory tool). The long-context papers explain why raw HTTP responses and scanner dumps must stay out of the window. Performance degrades well before the advertised limit, and degrades most when the needle doesn't share wording with the query.

| Resource | Date | Level | Why it matters for a hackbot | Tag |
|---|---|---|---|---|
| [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents), Anthropic Applied AI | 2025-09-29 | I | The canonical text: attention budget, "right altitude" system prompts, just-in-time retrieval, compaction, tool-result clearing, notes, subagents returning 1–2k-token summaries | F |
| [Context Engineering for AI Agents: Lessons from Building Manus](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus), Yichao "Peak" Ji | 2025-07-18 | I–A | KV-cache hit rate as the top metric, an append-only prefix, masking tools instead of removing them, the filesystem as memory, keeping errors in context | P; date S |
| [Context Engineering in Manus](https://rlancemartin.github.io/2025/10/15/manus/), Lance Martin | 2025-10-15 | A | "Full" and "compact" tool-result forms (the compact one is a file path), schema-based summarization, subagents used to isolate context, fewer than 20 atomic tools | F |
| [Context Engineering for Agents](https://blog.langchain.com/context-engineering-for-agents/), LangChain, with [notebooks](https://github.com/langchain-ai/context_engineering) | 2025-07-02 | B–I | The write/select/compress/isolate taxonomy | P (repo F) |
| [The rise of "context engineering"](https://blog.langchain.com/the-rise-of-context-engineering/), LangChain | Jun 2025 | B | Framing post | P; date U |
| [How Long Contexts Fail](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html) and [How to Fix Your Context](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html), Drew Breunig | 2025-06-22 / 06-26 | I | Poisoning, distraction, confusion and clash. A quantized Llama 3.1 8B failed with 46 tools and succeeded with 19, which argues for a small tool loadout | P |
| [Prompts vs. Context](https://www.dbreunig.com/2025/06/25/prompts-vs-context.html), Drew Breunig | 2025-06-25 | B | Short framing piece | P |
| [Context Rot](https://research.trychroma.com/context-rot), Chroma Research, with [replication repo](https://github.com/chroma-core/context-rot) | Jul 2025 | I | Models don't process context uniformly, and semantic (non-lexical) retrieval degrades first | P (repo F) |
| [Context engineering](https://simonwillison.net/2025/Jun/27/context-engineering/), Simon Willison | 2025-06-27 | B | Why the term stuck | P |
| [The New Skill in AI is Not Prompting, It's Context Engineering](https://www.philschmid.de/context-engineering) and [Part 2](https://www.philschmid.de/context-engineering-part-2), Philipp Schmid | ~Jul 2025 / Dec 2025 | B–I | Lists the components of context | P; dates S |
| [12-Factor Agents](https://github.com/humanlayer/12-factor-agents), Dex Horthy (HumanLayer) | 2025 | I | "Own your context window", "compact errors into context", small focused agents, stateless reducer | F |
| [Getting AI to Work in Complex Codebases (ACE-FCA)](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md), Dex Horthy | Aug 2025 | A | Frequent intentional compaction at 40–60% utilization; research→plan→implement | F |
| [Managing context on the Claude Developer Platform](https://claude.com/resources/articles/context-management), Anthropic | 2025-09-29 | I | Context editing clears stale tool results; the memory tool is file-based and runs client-side | F |
| Anthropic docs: [prompt caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching), [context editing](https://platform.claude.com/docs/en/build-with-claude/context-editing), [compaction](https://platform.claude.com/docs/en/build-with-claude/compaction), [memory tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool), [context windows](https://platform.claude.com/docs/en/build-with-claude/context-windows) | Living | I–A | Productized versions of the techniques above, including models that are aware of their own remaining token budget | F |
| OpenAI cookbook: [session memory](https://cookbook.openai.com/examples/agents_sdk/session_memory) (2025-09-09), [long-term memory notes](https://cookbook.openai.com/examples/agents_sdk/context_personalization) (2026-01-05), [memory and compaction](https://cookbook.openai.com/examples/agents_sdk/building_reliable_agents_memory_compaction) (2026-05-01), [Prompt Caching 201](https://cookbook.openai.com/examples/prompt_caching_201) (2026-02-18) | 2025–26 | I–A | The same patterns on the OpenAI Agents SDK | F (registry) |
| [Context Engineering in 2026: Why We Stopped Compacting](https://www.louisbouchard.ai/context-engineering-2026/), Louis-François Bouchard | 2026 | I | Counterpoint: with caching, full context often beat summarization | P |
| [How Claude remembers your project](https://code.claude.com/docs/en/memory) (CLAUDE.md, rules, AGENTS.md) and [AGENTS.md](https://agents.md) | Living | B | Per-program notes files for the target's tech stack, auth flow and known duplicates | F |
| [Equipping agents for the real world with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills), Anthropic; [Skills docs](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview); [anthropics/skills](https://github.com/anthropics/skills); [agentskills.io](https://agentskills.io) | 2025-10-16 + living | I | Progressive disclosure is how you ship per-vuln-class playbooks without flooding the context window | F; F; F; P |
| [Survey of Context Engineering for LLMs](https://arxiv.org/abs/2507.13334), Mei et al. | Jul 2025 | All | A map of the field | P |
| [Meirtz/Awesome-Context-Engineering](https://github.com/Meirtz/Awesome-Context-Engineering) | Updated into 2026 | All | A curated list of papers | F |
| [Lost in the Middle](https://arxiv.org/abs/2307.03172) (2307.03172); [RULER](https://arxiv.org/abs/2404.06654) (2404.06654); [NoLiMa](https://arxiv.org/abs/2502.05167) (2502.05167); [Context Length Alone Hurts](https://arxiv.org/abs/2510.05381) (2510.05381); [LLMs Get Lost in Multi-Turn](https://arxiv.org/abs/2505.06120) (2505.06120) | 2023–25 | B–I | Evidence for keeping raw recon output out of the window | F; P; U; U; U |
| [MemGPT](https://arxiv.org/abs/2310.08560) / [Letta](https://github.com/letta-ai/letta); [Mem0](https://arxiv.org/abs/2504.19413); [A-MEM](https://arxiv.org/abs/2502.12110); [Generative Agents](https://arxiv.org/abs/2304.03442); [CoALA](https://arxiv.org/abs/2309.02427) | 2023–25 | I–A | Memory architectures for remembering target state and techniques across sessions | P (repo F); P; P; P; F |
| [ACE: Agentic Context Engineering](https://arxiv.org/abs/2510.04618), with [repo](https://github.com/ace-agent/ace) | Oct 2025 | A | Contexts treated as "evolving playbooks". Repo reports +10.6% on AppWorld. The direct template for self-improving vuln-class skills | P (repo F) |
| [Codified Context](https://arxiv.org/pdf/2602.20478) (2602.20478); [When History Is Multimodal](https://arxiv.org/pdf/2608.29897) (2608.29897) | 2026 | A | Newer research on context infrastructure | P |
| [Context Engineering: Sessions & Memory](https://www.kaggle.com/whitepaper-context-engineering-sessions-and-memory), Google/Kaggle whitepaper | Nov 2025 | I | Companion to the Kaggle agents intensive | U |
| Talks: [Advanced Context Engineering for Agents](https://www.youtube.com/watch?v=VvkhYWFWaKI) (Horthy); [Context Engineering for Agents](https://www.youtube.com/watch?v=_IlTcWciEC4) (Lance Martin); [LangChain × Manus webinar](https://www.youtube.com/watch?v=6_BcCthVvb8); [Context Engineering for Engineers](https://www.youtube.com/watch?v=L8ZM78APDPk) and [Context Rot interview](https://www.youtube.com/watch?v=nLNuPsQ-Jlo) (Jeff Huber, Chroma) | 2025 | I | Most of the context-engineering video canon comes from the Context Engineering SF meetups | P |

### Loop and harness engineering: simple inner loops, heavy environments

These resources teach the loop mechanics first, then tool and interface design, then long-running state, containment and evaluation. The 2026 arXiv harness papers were found as titles in a search index and have not been read. Treat them as leads.

| Resource | Date | Level | Why it matters for a hackbot | Tag |
|---|---|---|---|---|
| [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents), Erik Schluntz & Barry Zhang (Anthropic) | 2024-12-19 | B | The canonical vocabulary: augmented LLM, workflows vs agents, five patterns, ACI, "poka-yoke your tools". Foundational, pre-2025 | F |
| [How to Build an Agent](https://ampcode.com/how-to-build-an-agent), Thorsten Ball | 2025-04-15 | B | About 400 lines of Go show the loop is not magic. Write your own version first | P |
| [Designing agentic loops](https://simonwillison.net/2025/Sep/30/designing-agentic-loops/), Simon Willison | 2025-09-30 | B–I | Loop design as a skill: tools, sandbox, credentials, success signal | P |
| [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works) and [How the agent loop works](https://code.claude.com/docs/en/agent-sdk/agent-loop), Anthropic | Living | B–I | Official harness definition, turn semantics, `max_turns`, `max_budget_usd`, hooks that block tool calls | F |
| [Building agents with the Claude Agent SDK](https://claude.com/resources/articles/building-agents-with-the-claude-agent-sdk), Thariq Shihipar | 2025-09-29 | B–I | Gather→act→verify loop. Why the Claude Code harness became a general SDK | F |
| [Unrolling the Codex agent loop](https://openai.com/index/unrolling-the-codex-agent-loop/), Michael Bolin (OpenAI) | 2026-01-23 | I | OpenAI's loop internals. Notes that MCP tools run outside Codex's sandbox | P |
| [A practical guide to building agents](https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf), OpenAI | ~Apr 2025 (landing page shows 2026-01-15) | B | Model/tools/instructions, starting single-agent, layered guardrails | P; dates S |
| [Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/), OpenAI (Ryan Lopopolo) | 2026-02-11 | I | The post that named the practice. "Humans steer. Agents execute." | P; author/date S |
| [Agent harness design: 3 patterns](https://claude.com/resources/articles/harnessing-claudes-intelligence), Lance Martin (Anthropic) | 2026-04-02 | I | Use what the model knows (bash plus an editor), ask what you can stop doing, set boundaries. On BrowseComp, compaction gains grow with model capability | F |
| [The importance of Agent Harness in 2026](https://www.philschmid.de/agent-harness-2026), [Agents 2.0](https://www.philschmid.de/agents-2.0-deep-agents), [The Rise of Subagents](https://www.philschmid.de/the-rise-of-subagents), Philipp Schmid | 2026-01-05 (first) | I | "Context window is RAM, harness is the OS" | P; date S |
| [Writing effective tools for AI agents](https://www.anthropic.com/engineering/writing-tools-for-agents), Anthropic | 2025-09-11 | I | Few namespaced tools, token-efficient responses, helpful errors, eval-driven tool design. Your nmap/ffuf/HTTP wrappers start here | F |
| [Introducing advanced tool use](https://www.anthropic.com/engineering/advanced-tool-use), Bin Wu et al. | 2025-11-24 | I | Tool search and programmatic tool calling for large security toolkits | F |
| [Code execution with MCP](https://www.anthropic.com/engineering/code-execution-with-mcp), Adam Jones & Conor Kelly | 2025-11-04 | I | Tools exposed as code APIs. One example cut 150k tokens to 2k (98.7%) | F |
| [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents), Justin Young | 2025-11-26 | I | Initializer agent, `init.sh`, progress file, JSON task list with `"passes": false`, git recovery. The template for multi-day engagements | F |
| [Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps), Prithvi Rajasekaran | 2026-03-24 | A | Planner/generator/skeptical evaluator, context resets vs compaction, "context anxiety", simplifying by ablation. A $200 six-hour run vs a $9 solo run | F |
| [Building a C compiler with a team of parallel Claudes](https://www.anthropic.com/engineering/building-c-compiler), Nicholas Carlini | 2026-02-05 | A | 16 agents and about $20k with no orchestrator, coordinating through git and lock files. Verifier quality decided success | F |
| [Scaling Managed Agents with stable interfaces](https://www.anthropic.com/engineering/managed-agents), Martin, Cemaj & Cohen | 2026-04-08 | A | Session/harness/sandbox decoupling, crash-recoverable append-only logs, credentials never in the sandbox. The closest public reference backend for a durable hackbot | F |
| [When to fold subagents into your main agent loop](https://claude.com/resources/articles/building-agents-when-to-fold-subagents-into-your-main-agent-loop), Anthropic Applied AI | 2026-10-22 | I | The newest counterweight to multi-agent hype. Use subagents only for context bloat, disposable parallel contexts or conflicting tools | F |
| [Ralph Wiggum as a "software engineer"](https://ghuntley.com/ralph/), Geoffrey Huntley | 2025-07-14 | I | An outer bash loop that re-feeds the prompt with fresh context until done. Cited in Anthropic's 2026 post | P (linked from F page); date S |
| [Huntley on Ralph as an orchestrator needing a watchdog](https://x.com/GeoffreyHuntley/status/2033317516554944976); [Matt Pocock: stick with a bash loop](https://x.com/mattpocockuk/status/2010330604123144389) | 2026-03-15 / 2026-01-11 | I | Practitioner refinements | P |
| [ralph-wiggum Claude Code plugin](https://github.com/anthropics/claude-code/tree/main/plugins/ralph-wiggum) | 2025–26 | I | A Stop hook blocks exit until a "completion promise" appears. Its current name and location are unconfirmed | U |
| [Making Claude Code safer with sandboxing](https://www.anthropic.com/engineering/claude-code-sandboxing) | 2025-10-20 | I | Isolate both filesystem and network. 84% fewer permission prompts | F |
| [How we built Claude Code auto mode](https://www.anthropic.com/engineering/claude-code-auto-mode), John Hughes | 2026-03-25 | A | The best public design for a tiered action gate: allowlist → in-project → transcript classifier, with an honest 17% false-negative rate. Reusable as a rules-of-engagement gate | F |
| [How we contain Claude across products](https://www.anthropic.com/engineering/how-we-contain-claude) | 2026-05-25 | A | Containment over supervision. Real incidents: a pre-trust config executing, an egress-proxy bypass, phished exfiltration at 24/25 | F |
| Claude Code docs: [permissions](https://code.claude.com/docs/en/permissions), [hooks](https://code.claude.com/docs/en/hooks), [sandboxing](https://code.claude.com/docs/en/sandboxing), [devcontainer](https://code.claude.com/docs/en/devcontainer), [checkpointing](https://code.claude.com/docs/en/checkpointing), [costs](https://code.claude.com/docs/en/costs), [monitoring](https://code.claude.com/docs/en/monitoring-usage), [headless](https://code.claude.com/docs/en/headless), [best practices](https://code.claude.com/docs/en/best-practices) | Living | I | Every operational harness feature, with documented limits (domain fronting, the docker socket) | F |
| [Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) | 2026-01-09 | I | Task/trial/grader vocabulary, pass@k vs pass^k, isolated environment per trial, and the point that you evaluate harness plus model together | F |
| [Quantifying infrastructure noise in agentic coding evals](https://www.anthropic.com/engineering/infrastructure-noise), Gian Segato | 2026-02-05 | A | Container resources alone moved Terminal-Bench 2.0 by 6 points. Pin your resources | F |
| Reference coding-agent harnesses: [mini-swe-agent](https://github.com/SWE-agent/mini-swe-agent) (2.4.6), [SWE-agent](https://github.com/SWE-agent/SWE-agent), [OpenHands](https://github.com/OpenHands/OpenHands) (1.11.0, [docs](https://docs.openhands.dev/)), [Codex CLI](https://github.com/openai/codex) (0.164.0), [smolagents](https://github.com/huggingface/smolagents) (1.26.0), [Deep Agents](https://github.com/langchain-ai/deepagents) (0.7.32), [Gemini CLI](https://github.com/google-gemini/gemini-cli) (0.65.0) | Versions as of Oct 2026 | I | Code that already solves the shell-in-sandbox loop. SWE-agent itself recommends mini-swe-agent. Gemini CLI was replaced by Antigravity CLI for unpaid and Google One users on 2026-06-18 | F |
| [ReAct](https://arxiv.org/abs/2210.03629); [Reflexion](https://arxiv.org/abs/2303.11366); [Toolformer](https://arxiv.org/abs/2302.04761); [Voyager](https://arxiv.org/abs/2305.16291); [CodeAct](https://arxiv.org/abs/2402.01030); [SWE-agent](https://arxiv.org/abs/2405.15793); [OpenHands](https://arxiv.org/abs/2407.16741) | 2022–24 | B–A | The loop lineage. SWE-agent's ACI and CodeAct's code-as-action matter most for tool interfaces. Voyager's skill library is the template for accumulating exploit skills | F; F; P; F; F; F; P |
| [Self-Refine](https://arxiv.org/abs/2303.17651); [Plan-and-Solve](https://arxiv.org/abs/2305.04091); [LATS](https://arxiv.org/abs/2310.04406) | 2023 | I–A | Less important now that models plan internally. Self-evaluation is lenient, so use a separate evaluator | U |
| [LLM Powered Autonomous Agents](https://lilianweng.github.io/posts/2023-06-23-agent/), Lilian Weng | 2023-06-23 | B | Planning/memory/tools taxonomy. Foundational, partly dated | F |
| [Agents](https://huyenchip.com/2025/01/07/agents.html), Chip Huyen | 2025-01-07 | I | Book-chapter-length agent overview | U |
| [What is loop engineering?](https://www.ibm.com/think/topics/loop-engineering) (IBM); [An Introduction to Loop Engineering](https://machinelearningmastery.com/an-introduction-to-loop-engineering/) (MLM); [State of AI Harness Engineering 2026](https://marmelab.com/blog/2026/09/24/the-state-of-ai-harness-engineering-2026.html) (Marmelab, 2026-09-24) | 2026 | B–I | Explainers that show how fuzzy the terms are | P |
| arXiv harness research: [source-code study of 11 harnesses](https://arxiv.org/abs/2609.00006) (2609.00006), [observability-driven harness evolution](https://arxiv.org/abs/2604.25850), [natural-language harnesses](https://arxiv.org/abs/2603.25723), [LLM's residual role](https://arxiv.org/abs/2604.07236), [scaling the harness](https://arxiv.org/abs/2605.26112), [CAAF determinism](https://arxiv.org/abs/2604.17025), [prompts to contracts](https://arxiv.org/abs/2607.08028), [harness exploratory study](https://arxiv.org/pdf/2602.14690), [tool architecture shapes behavior](https://arxiv.org/abs/2608.11386), [infinite agentic loops](https://arxiv.org/abs/2607.01641) | 2026 | A | Titles only. 2609.00006 is probably the best comparative read before you choose a base harness | P |
| Talks: [Claude for Long-Horizon Tasks](https://www.youtube.com/watch?v=9QebvrrY3KY) (Lance Martin); [WF26 Harness Engineering stream](https://www.youtube.com/watch?v=I2cbIws9j10); [The Highest Loop](https://www.youtube.com/watch?v=Fva8hFosqyQ) (swyx); [Everything We Got Wrong About Research-Plan-Implement](https://www.youtube.com/watch?v=YwZR6tc7qYg) (Horthy, 2026-03-03); [12-Factor Agents](https://www.youtube.com/watch?v=8kMaTybvDUw) (Horthy) | 2025–26 | I | The talk versions of the posts above | P |

### Graph engineering, subagents, swarms and orchestration

The control graph should own the engagement lifecycle: scope, recon, hypotheses, exploit, verify, report. LLMs reason only inside nodes. A second, separate graph holds world state (asset → service → endpoint → parameter → finding). PentAGI, Decepticon and RedAmon all keep that in Neo4j, which follows the distinction between an execution graph and a knowledge graph ([PentAGI](https://github.com/vxcontrol/pentagi)).

| Resource | Date | Level | Why it matters for a hackbot | Tag |
|---|---|---|---|---|
| [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system), Hadfield, Zhang et al. | 2025-06-13 | I | +90.2% with orchestrator-workers at about 15x tokens. Effort-scaling rules (1 / 2–4 / 10+ subagents). Delegation needs objective, format, tools and boundaries | F |
| [Don't Build Multi-Agents](https://cognition.ai/blog/dont-build-multi-agents), Walden Yan (Cognition) | 2025-06-12 | I | Share full traces; actions carry implicit decisions. The case for single-threaded exploit chains | P; date S |
| [Multi-Agents: What's Actually Working](https://cognition.ai/blog/multi-agents-working), Walden Yan | 2026-04-22 | I | Narrow multi-agent flows work if writes stay single-threaded | P; date S |
| [How and when to build multi-agent systems](https://www.langchain.com/blog/how-and-when-to-build-multi-agent-systems), LangChain | Jun 2025 | I | Reconciles the two: read-heavy parallel work suits multi-agent | P (current URL) |
| [Towards a Science of Scaling Agent Systems](https://arxiv.org/abs/2512.08296), Kim, Liu et al. (Google Research), with [blog](https://research.google/blog/towards-a-science-of-scaling-agent-systems-when-and-why-agent-systems-work/) | Dec 2025 | A | 17.2x vs 4.4x error amplification. A predictive model picks the best topology 87% of the time | P |
| [Why Do Multi-Agent LLM Systems Fail? (MAST)](https://arxiv.org/abs/2503.13657), Cemri et al., with [repo](https://github.com/multi-agent-systems-failure-taxonomy/MAST) | Mar 2025 (NeurIPS 2025 D&B) | A | 14 failure modes in 3 categories. Use it as a checklist for design reviews | F (README/BibTeX); paper details P |
| [Create custom subagents](https://code.claude.com/docs/en/sub-agents), [agent teams](https://code.claude.com/docs/en/agent-teams), [SDK subagents](https://code.claude.com/docs/en/agent-sdk/subagents), Anthropic | Living | B–I | Context isolation, tool allowlists, worktree isolation, nesting up to 3 levels. Agent teams are experimental: start with 3–5 teammates, and a teammate cannot approve permissions for you | F |
| [Claude Code Swarms](https://addyosmani.com/blog/claude-code-agent-teams/), Addy Osmani | Feb 2026 | I | Practitioner take on agent teams | P |
| [Graph Engineering in the Era of LLM Agents](https://arxiv.org/abs/2608.21156), Feng et al., with [Awesome-Graph-Engineering](https://github.com/DEEP-JLU/Awesome-Graph-Engineering) | Aug 2026 | A | Defines the term: task organization, agent coordination, runtime state | P (repo F) |
| [Graph Engineering for AI Agents](https://www.analyticsvidhya.com/blog/2026/07/graph-engineering/) (Analytics Vidhya); [TDS](https://towardsdatascience.com/graph-engineering-for-ai-agents-from-prompts-and-loops-to-workflows/); [PuppyOne](https://www.puppyone.ai/en/blog/graph-engineering-ai-agents-map) | Jul 2026 | B–I | Keep business rules (here, scope and authorization) deterministic. LLMs reason inside nodes | P |
| [From Agent Loops to Structured Graphs](https://arxiv.org/abs/2604.11378) (2604.11378); [The Log is the Agent](https://arxiv.org/abs/2605.21997) (2605.21997) | 2026 | A | Scheduler theory, and event-sourced, forkable, auditable agent graphs | P (listed in fetched repo) |
| [LangGraph overview](https://docs.langchain.com/oss/python/langgraph/overview), [persistence](https://docs.langchain.com/oss/python/langgraph/persistence), [interrupts](https://docs.langchain.com/oss/python/langgraph/interrupts), [multi-agent guide](https://docs.langchain.com/oss/python/langchain/multi-agent) | Living (LangGraph 1.2.18) | I | Checkpointers for multi-hour runs, interrupts as HITL gates, five multi-agent patterns. "At the center of multi-agent design is context engineering" | F |
| [LangChain & LangGraph 1.0](https://www.langchain.com/blog/langchain-langgraph-1dot0) | 2025-10-22 | B | Stability release. `prebuilt` deprecated | P |
| [langgraph-supervisor-py](https://github.com/langchain-ai/langgraph-supervisor-py) (de-recommended); [langgraph-swarm-py](https://github.com/langchain-ai/langgraph-swarm-py) | Nov–Dec 2025 | I | Pattern references. The supervisor README now says to use supervisor-via-tools instead | F |
| [Microsoft Agent Framework 1.0](https://devblogs.microsoft.com/agent-framework/microsoft-agent-framework-version-1-0/), [repo](https://github.com/microsoft/agent-framework), [AutoGen migration guide](https://learn.microsoft.com/en-us/agent-framework/migration-guide/from-autogen/) | 2026-04-03 (1.23.0 now) | I | Graph workflows with checkpointing, HITL, handoff and Magentic patterns. Successor to AutoGen and Semantic Kernel | P; F; F |
| [ADK workflow agents](https://google.github.io/adk-docs/agents/workflow-agents/) (canonical docs moved to [adk.dev](https://adk.dev/)) | ADK 2.13.0 | B–I | Deterministic Sequential/Parallel/Loop agents. Parallel suits recon fan-out | F |
| [pydantic-graph](https://pydantic.dev/docs/ai/graph/graph/) | 2.62.0 | I | Typed state machines. "Don't use a nail gun unless you need a nail gun" | F |
| [OpenAI Agents SDK handoffs](https://openai.github.io/openai-agents-python/handoffs/), [repo](https://github.com/openai/openai-agents-python) | 0.24.0 (2026-10-15) | B–I | Handoffs, agents-as-tools, guardrails, sandbox agents. CAI was built on it | F |
| [OpenAI Swarm](https://github.com/openai/swarm) | Oct 2024 | B | Deprecated and educational only. Handoffs in about 100 lines | F |
| [Multiagent Debate](https://arxiv.org/abs/2305.14325) (2305.14325); [More Agents Is All You Need](https://arxiv.org/abs/2402.05120) (2402.05120); [Mixture-of-Agents](https://arxiv.org/abs/2406.04692); [Agent-as-a-Judge](https://arxiv.org/abs/2410.10934) | 2023–24 | I | Cheap false-positive suppression through k-vote or debate at triage time | F; F; P; F (repo) |
| [ADAS](https://arxiv.org/abs/2408.08435); [AFlow](https://arxiv.org/abs/2410.10762); [GPTSwarm](https://arxiv.org/abs/2402.16823); [MaAS](https://arxiv.org/abs/2502.04180); [MASS](https://arxiv.org/abs/2502.02533); [SwarmAgentic](https://arxiv.org/abs/2506.15672) | 2024–25 | A | Automated topology search. Research-grade; useful only after you have a stable eval | F; F; F; P; P; P |
| [Who&When failure attribution](https://arxiv.org/abs/2505.00212); [Multi-Agent Collaboration Mechanisms survey](https://arxiv.org/abs/2501.06322); [Beyond Individual Intelligence survey](https://arxiv.org/abs/2605.14892); [Agent Workflow survey](https://arxiv.org/abs/2508.01186) | 2025–26 | I–A | Debugging traces and mapping the field | P (listed) |
| [Magentic-One](https://arxiv.org/abs/2411.04468); [AutoGen](https://arxiv.org/abs/2308.08155); [CAMEL](https://arxiv.org/abs/2303.17760); [MetaGPT](https://arxiv.org/abs/2308.00352); [ChatDev](https://arxiv.org/abs/2307.07924); [LLMCompiler](https://arxiv.org/abs/2312.04511) | 2023–24 | I | Foundational framework papers. Magentic-One's task/progress ledger is a good orchestrator model | P |
| [GraphRAG](https://arxiv.org/abs/2404.16130), [repo](https://github.com/microsoft/graphrag) | 2024 | I | Knowledge-graph retrieval over recon data | F |
| Videos: [LangChain Academy intro](https://www.youtube.com/watch?v=29XE10U6ooc); [multi-agent handoffs](https://www.youtube.com/watch?v=WTr6mHTw5cM); [supervisor architecture](https://www.youtube.com/watch?v=KAwiykWkQ5Q); [MAF intro](https://www.youtube.com/watch?v=AAgdMhftj8w) (linked from the README; title unverified) | 2025–26 | B–I | Hands-on walkthroughs. Uploaders for the middle two are unconfirmed | P; S; S; P |

### Framework and protocol status as of October 2026

Version numbers and statuses come from PyPI/npm records and official READMEs fetched on 2026-10-27, so every row is tagged F. The judgments in the hackbot-fit column are inferences.

| Framework or protocol | Status (late Oct 2026) | Hackbot fit |
|---|---|---|
| [LangGraph](https://pypi.org/project/langgraph/) | Active; 1.0 Oct 2025, now 1.2.18; Postgres checkpointer 3.2.0 | Very high for the control graph, checkpoints and approval interrupts |
| [Microsoft Agent Framework](https://pypi.org/project/agent-framework/) | Active; 1.0 GA Apr 2026, now 1.23.0 | High if you use .NET or Azure |
| [Google ADK](https://github.com/google/adk-python) | Active; 2.13.0; docs at [adk.dev](https://adk.dev/), though PyPI still lists the old docs URL | Medium–high; Parallel agents for recon |
| [OpenAI Agents SDK](https://github.com/openai/openai-agents-python) | Active, pre-1.0 (0.24.0) | High; handoffs, guardrails, tracing |
| [Claude Agent SDK](https://pypi.org/project/claude-agent-sdk/) / Claude Code | Active; Python 0.2.174, npm 0.3.311, Claude Code 2.1.311; agent teams experimental | Very high; hooks, permissions, subagent isolation, skills |
| [CrewAI](https://github.com/crewAIInc/crewAI) | Active; 1.15.30 | Medium; fast role prototypes, less control |
| [Strands Agents](https://github.com/strands-agents/harness-sdk) | Active; 1.62.0, repo renamed `harness-sdk` | Medium; AWS/Bedrock own-loop replacement |
| [pydantic-graph](https://pypi.org/project/pydantic-graph/), [LlamaIndex Workflows](https://github.com/run-llama/workflows-py), [Apache Burr](https://burr.apache.org/), [Haystack](https://haystack.deepset.ai/overview/intro), [DSPy](https://github.com/stanfordnlp/dspy), [Mastra](https://www.npmjs.com/package/@mastra/core) | Active (Burr incubating, 0.43) | Medium; typed state, events, node-level optimization |
| [AG2](https://github.com/ag2ai/ag2) | Active community fork; 1.0 Jul 2026 restructure | Low–medium (API churn) |
| [AutoGen](https://github.com/microsoft/autogen) | **Archived**; migrate to MAF | Avoid |
| [OpenAI Swarm](https://github.com/openai/swarm) | **Deprecated**; replaced by the Agents SDK | Learning only |
| [Semantic Kernel](https://pypi.org/project/semantic-kernel/) | Maintenance mode (1.45.0) | Low |
| [langgraph-supervisor](https://github.com/langchain-ai/langgraph-supervisor-py) | De-recommended by its own README | Pattern study only |
| [MetaGPT](https://github.com/FoundationAgents/MetaGPT), [CAMEL](https://github.com/camel-ai/camel), [ChatDev 2.0](https://github.com/OpenBMB/ChatDev), [Letta](https://github.com/letta-ai/letta) | Stalled on PyPI, research 0.x, relaunched, or V1 server retired, respectively | Low |
| [Agency Swarm](https://github.com/VRSEN/agency-swarm), [Swarms (kyegomez)](https://github.com/kyegomez/swarms) | Active; maturity claims are self-reported | Low–medium; verify claims |
| [MCP spec 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28) | Stateless revision; Python SDK 2.0; governed by the Agentic AI Foundation since [2025-12-09](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation); [migration notes (P)](https://aaif.io/blog/mcp-2026-07-28-whats-changing-and-how-to-migrate) | Required for tool servers; treat it as an attack surface |
| [A2A 1.0.0](https://a2a-protocol.org/v1.0.0/specification) ([repo](https://github.com/a2aproject/A2A)) | Linux Foundation; Python SDK 1.0 Apr 2026 | Only for federating with external agents |
| [ACP (IBM/BeeAI)](https://github.com/i-am-bee/acp) | **Merged into A2A** (Aug 2025) | Skip |
| [AG-UI](https://github.com/ag-ui-protocol/ag-ui) | 1.0 in Sep 2026 | Operator console and approval streaming |

## Security evidence: what papers, benchmarks and shipped hackbots prove

The offensive-AI literature has moved through four stages:

1. Human-in-the-loop assistants that keep structured state, such as PentestGPT's Pentesting Task Tree (2023).
2. Single-loop autonomous agents (2024).
3. Planner/specialist multi-agent systems with abstraction layers (2024–2025).
4. A 2026 wave on reliability, hallucinated findings, deceptive targets and the agent's own security.

Two results recur across all of it. Raw LLMs fail at multistage work without structured state and high-level actions. And fully autonomous agents trail human-assisted ones by a wide margin: AutoPenBench measured **21% autonomous against 64% human-assisted** ([arXiv 2410.03225](https://arxiv.org/abs/2410.03225)). On a stage-level benchmark, published multi-agent systems scored **3–6%** fully autonomous ([arXiv 2512.14233](https://arxiv.org/abs/2512.14233)), even though the same systems report 69–79% on their own benchmarks. That contrast is the strongest argument for building your own held-out evaluation.

Web results vary sharply by vulnerability class. On XBOW's 104 challenges, MAPTA reached **100% on SSRF and misconfiguration, 83% on broken authorization, 57% on XSS and 0% on blind SQL injection**, for **$21.38** in total ([arXiv 2508.20816](https://arxiv.org/abs/2508.20816)). So route by class. Classes with crisp oracles suit LLM agents with deterministic validators. Blind, inference-based classes still need classic differential tooling.

### Research papers to read, with what each proves

| Paper | Date / venue | Level | What it proves for a hackbot | Tag |
|---|---|---|---|---|
| [PentestGPT](https://arxiv.org/abs/2308.06782) (2308.06782), Deng et al.; [repo](https://github.com/GreyDGL/PentestGPT) | Aug 2023; USENIX Security 2024 | I | Task tree outside the context window. 228.6% task-completion gain over GPT-3.5. v1.0 now drives Claude Code/Codex and reports 86.5% on XBOW-104 (Dec 2025, labelled "historical") | P (repo F) |
| ["Getting pwn'd by AI"](https://arxiv.org/abs/2308.00121) (2308.00121), Happe & Cito; [hackingBuddyGPT](https://github.com/ipa-lab/hackingBuddyGPT) | FSE 2023 | B–I | Minimal loop. Success is checked by code against real command output, never self-reported | P (repo F) |
| [LLMs as Hackers](https://arxiv.org/html/2310.11409v4) (2310.11409), Happe, Kaplan & Cito | Oct 2023; EMSE 2025 | I | Privesc benchmark that analyzes context size and memory management | P |
| [LLM Agents can Autonomously Hack Websites](https://arxiv.org/abs/2402.06664) (2402.06664), Fang et al. | Feb 2024 | I | GPT-4 solved 73.3% (11/15, pass@5) of multi-step web attacks; open models failed | P |
| [LLM Agents can Autonomously Exploit One-day Vulnerabilities](https://arxiv.org/abs/2404.08144) (2404.08144) | Apr 2024 | I | 87% with the CVE description, 0% for other models and scanners. Small n=15 | P |
| [Teams of LLM Agents can Exploit Zero-Day Vulnerabilities (HPTSA)](https://arxiv.org/abs/2406.01637) (2406.01637) | Jun 2024 | I | A hierarchical planner plus expert agents beats a single agent when no CVE hint exists | P |
| [Incalmo](https://arxiv.org/abs/2501.16466) (2501.16466), Singer et al. (CMU) | Jan 2025, v4 Nov 2025 | A | Intent-level actions plus state and attack-graph services took success from recon-only to 9/10 networks (37/40 in a later version) | P |
| [Cochise: Can LLMs Hack Enterprise Networks?](https://arxiv.org/abs/2502.04227) (2502.04227), Happe & Cito; [repo](https://github.com/andreashappe/cochise) | ACM TOSEM 2025 | A | Active Directory assumed breach. The README claims full domain compromise for under $2 in under 2 hours | F (README) |
| [MAPTA](https://arxiv.org/abs/2508.20816) (2508.20816), David & Gervais; [repo](https://github.com/arthurgervais/mapta) | Aug 2025 | I–A | Web multi-agent with end-to-end exploit validation. Per-class results; fixed 43 broken XBOW images | P (repo F) |
| [ARTEMIS](https://arxiv.org/abs/2512.09882) (2512.09882), Lin et al. (Stanford); [repo](https://github.com/Stanford-Trinity/ARTEMIS) | Dec 2025 | A | On a live network of about 8,000 hosts, placed 2nd overall and beat 9 of 10 professionals. 82% valid, about $18/h vs $60/h, weak on GUIs, more false positives. Built as a Codex fork with a supervisor | P (repo F) |
| [Are Frontier LLMs Ready for Cybersecurity?](https://arxiv.org/html/2605.23243v1) (2605.23243) | May 2026 | A | A dedicated IDOR agent using two authenticated sessions against an API spec. The closest verified design for access-control bugs | P |
| [AWE](https://arxiv.org/html/2603.00960v1) (2603.00960) | Mar 2026 | A | Persistent memory plus browser-backed verification. A survey cites 87% XSS and 66.7% blind SQLi | P; figures S |
| [EnIGMA](https://arxiv.org/abs/2409.16165) (2409.16165); [D-CIPHER](https://arxiv.org/abs/2502.10931) (2502.10931); [CRAKEN](https://arxiv.org/abs/2505.17107) (2505.17107) | 2024–25 | I–A | The NYU/Princeton lineage: interactive debugger tools, a named "soliloquizing" failure mode (invented observations), planner/executor, RAG over writeups | P |
| [Hacking CTFs with Plain Agents](https://arxiv.org/abs/2412.02776) (2412.02776), Palisade; [repo](https://github.com/PalisadeResearch/intercode) | Dec 2024 | I | 95% (81/85) on InterCode with prompting, tools and retries. Build this baseline first | P (repo F) |
| [xOffense](https://arxiv.org/abs/2509.13021) (2509.13021); [Pentest-R1](https://arxiv.org/html/2508.07382) (2508.07382); [local privesc agents](https://arxiv.org/pdf/2604.27143) (2604.27143) | 2025–26 | A | Fine-tuned or RL-trained mid-size open models are catching up (79.17% sub-task, self-reported; 24.2% AutoPenBench; parity with GPT-4o after interventions) | P |
| [CAI](https://arxiv.org/abs/2504.06017) (2504.06017), Alias Robotics; [repo](https://github.com/aliasrobotics/cai) | Apr 2025 | I | Bug-bounty-oriented framework built on Agents-SDK patterns. Its "3,600x faster, 156x cheaper" claim is the vendor's own. **Archived 2026-08-28** | P (repo F) |
| [PenHeal](https://arxiv.org/abs/2407.17788) (2407.17788); [HackSynth](https://arxiv.org/abs/2412.01778) (2412.01778) | 2024 | I | Pentest plus remediation; planner plus summarizer (repo stale) | P; F (README) |
| [VulnBot](https://arxiv.org/abs/2501.13411) (2501.13411); [PentestAgent](https://arxiv.org/abs/2411.05185) (2411.05185); [AutoAttacker](https://arxiv.org/abs/2403.01038); [RapidPen](https://arxiv.org/abs/2502.16730) | 2024–25 | I | Task-graph and RAG designs whose self-reported and independent scores diverge | S; P; S; S |
| [Classical-planning hybrid](https://arxiv.org/html/2512.11143v1) (2512.11143); [Automation-Exploit (digital twin)](https://arxiv.org/pdf/2604.22427) (2604.22427); [PentestChain](https://arxiv.org/pdf/2609.18120) (2609.18120); [Mako](https://arxiv.org/pdf/2607.11288) (2607.11288); [APT-Agent](https://arxiv.org/abs/2605.24949) | 2025–26 | A | Symbolic planners, testing exploits on a digital twin before the live target, cost-aware MCP orchestration, self-evolving web exploitation | P |
| [Refute-or-Promote](https://arxiv.org/pdf/2604.19049) (2604.19049); [Antaeus](https://arxiv.org/pdf/2607.01138); [VulnLLM-R](https://arxiv.org/html/2512.07533v1); [VULSOLVER](https://arxiv.org/pdf/2509.00882) | 2025–26 | A | Adversarial review gated by stage for high precision, and white-box logic-bug reasoning | P |
| [Towards Optimal Agentic Architectures for Offensive Security](https://arxiv.org/pdf/2604.18718) (2604.18718); [Beyond End-to-End Success](https://arxiv.org/pdf/2608.20563) (2608.20563) | 2026 | A | Context stress and long-horizon failure diagnosis in security agents | P |

### Evaluation, reliability and agent-security papers

| Paper | Date | Level | Why it matters | Tag |
|---|---|---|---|---|
| [AutoPenBench](https://arxiv.org/abs/2410.03225) (2410.03225); [repo](https://github.com/lucagioacchini/auto-pen-bench) | Oct 2024 | I | 33 Docker tasks scored by milestones. Quantifies the gap between autonomous and assisted runs | P (repo F) |
| [PentestEval](https://arxiv.org/abs/2512.14233) (2512.14233) | Dec 2025 | I | Stage-level scoring against expert references | P |
| [Benchmarking Practices in LLM-driven Offensive Security](https://arxiv.org/pdf/2504.10112) (2504.10112) | Apr 2025 | I | Read before designing any internal eval | P |
| [From Controlled to the Wild](https://arxiv.org/html/2605.10834v1) (2605.10834) | May 2026 | I | Flag-based success understates real engagements that can yield several findings | P |
| [What Makes a Good LLM Agent for Real-world Penetration Testing?](https://arxiv.org/pdf/2602.17622) (2602.17622); [Hackers or Hallucinators?](https://arxiv.org/pdf/2604.05719) (2604.05719) | 2026 | I–A | Widely cited 2026 analyses of agent quality and hallucinated findings (contents not retrieved) | P |
| [ATOBench](https://arxiv.org/pdf/2608.12996) (2608.12996) | Aug 2026 | A | How agents verify vulnerabilities when target evidence lies. Directly tests your validators | P |
| [400-run consistency study](https://arxiv.org/pdf/2605.30096) (2605.30096); [trace-level provenance](https://arxiv.org/pdf/2608.26237) (2608.26237); [cost-aware evaluation](https://arxiv.org/pdf/2607.15263) (2607.15263) | 2026 | A | Variance, "did it really exploit or grep the flag", and cost per success | P |
| [HackWorld](https://arxiv.org/pdf/2510.12200) (2510.12200); [CVE-Bench paper](https://arxiv.org/pdf/2503.17332v1) (2503.17332) | 2025 | I | Computer-use agents vs HTTP agents, and real web CVEs | P |
| [PentestJudge](https://arxiv.org/abs/2508.02921) (2508.02921), Caldwell et al. | Aug 2025 | I | Judging agent behavior against operational requirements (ID confirmed only through a citing paper) | S |
| [Hacking the AI Hackers via Prompt Injection](https://arxiv.org/abs/2508.21669) (2508.21669), Mayoral-Vilches & Mannermaa Rynning | Aug 2025 | I | A poisoned server response hijacked CAI, reaching full compromise in under 20 seconds. 100% across 14 variants. Proposes a four-layer guardrail | P |
| [Hacking Back the AI-Hacker (Mantis)](https://arxiv.org/pdf/2410.20911) (2410.20911); [repo](https://github.com/pasquini-dario/project_mantis) | 2024 | I–A | Decoy services hide injections in ANSI escapes and HTML comments. Over 95% effective against GPT-4-class attackers | P (repo F) |
| [LLM-Based Penetration Testing in the Presence of Honeypots](https://arxiv.org/abs/2609.08093) (2609.08093); [Toward Secure AI-Powered Penetration Testing Agents](https://arxiv.org/abs/2609.16694) (2609.16694) | Sep 2026 | I–A | Expect targets to fight back | P |
| [CaMeL](https://arxiv.org/abs/2503.18813) (2503.18813); [repo](https://github.com/google-research/camel-prompt-injection) | Mar 2025 | A | Privileged planner plus quarantined parser with capability tracking. The repo warns it "might not be fully secure" | F (repo) |
| [Design Patterns for Securing LLM Agents](https://arxiv.org/abs/2506.08837) (2506.08837), Beurer-Kellner et al. | Jun 2025 | A | Six patterns: action-selector, plan-then-execute, map-reduce, dual LLM, code-then-execute, context minimization | P; pattern list S |
| [Indirect prompt injection](https://arxiv.org/abs/2302.12173) (2302.12173), Greshake et al. | 2023 | I | Foundational. The current title, "Not what you've signed up for…", differs from the repo BibTeX | P; title S |
| [AgentDojo](https://github.com/ethz-spylab/agentdojo) (2406.13352); [Agent Security Bench](https://github.com/agiresearch/ASB) | 2024–25 | A | Injection-robustness benchmarks for your own bot | F |
| [Spotlighting](https://arxiv.org/abs/2403.14720) (2403.14720), Microsoft | 2024 | I | Delimiting, datamarking and encoding untrusted input | U |
| Surveys and SoKs: [Agentic Security SoK](https://arxiv.org/pdf/2608.21423) (2608.21423); [Survey of LLM-Driven Pentesting](https://arxiv.org/pdf/2607.02605) (2607.02605); [Forewarned is Forearmed](https://arxiv.org/html/2505.12786v2) (2505.12786); [Agentic Security survey](https://arxiv.org/pdf/2510.06445) (2510.06445); [LLM agents for software security](https://arxiv.org/pdf/2608.28490) (2608.28490) | 2025–26 | I–A | Maps of the field. Read the 2026 SoK first for failure modes and "design laws" (titles verified; contents not retrieved) | P |

### Industry write-ups and primary case studies

| Write-up | Date | Why it matters | Tag |
|---|---|---|---|
| [Disrupting the first reported AI-orchestrated cyber espionage campaign](https://www.anthropic.com/news/disrupting-AI-espionage) (GTG-1002), Anthropic; [PDF](https://assets.anthropic.com/m/ec212e6566a0d47/original/Disrupting-the-first-reported-AI-orchestrated-cyber-espionage-campaign.pdf) | 2025-11-13 | Claude Code plus MCP-wrapped pentest tools ran 80–90% of a campaign, with 4–6 human decision points and "thousands of requests, often multiple per second." It also hallucinated credentials. Read for architecture and failure lessons, plus skepticism ([HN](https://news.ycombinator.com/item?id=45944296), [BleepingComputer](https://www.bleepingcomputer.com/news/security/anthropic-claims-of-claude-ai-automated-cyberattacks-met-with-doubt/amp/)) | F (PDF not fetched) |
| [Mapping AI-enabled cyber threats](https://www.anthropic.com/research/attack-navigator), Anthropic | 2025–26 | Confirms the GTG-1002 label and the Kali-plus-MCP setup | F |
| [Building AI for cyber defenders](https://www.anthropic.com/research/building-ai-cyber-defenders), Anthropic | 2025-09-29 | Sonnet 4.5: Cybench 76.5% at 10 attempts; CyberGym 28.9% under a $2 cap and 66.7% at 30 trials. Shows how much the metric definition changes the number | F |
| [Project Glasswing](https://www.anthropic.com/glasswing), Anthropic | 2026-04-07 | Mythos Preview: "thousands" of zero-days, CyberGym 83.1% vs 66.6%. The capability now sits behind gated access | F |
| [Claude Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7), [Mythos/Fable 5.1](https://www.anthropic.com/claude-fable-and-mythos-5-1), [Opus 5.5](https://www.anthropic.com/claude-opus-5-5), Anthropic | 2026-04-16 / 09-15 / 09-22 | Cyber capability is reduced differentially in GA models. Fable 5.1 finds vulnerabilities but does not develop exploits. Opus 5.5 re-routes most cyber tasks to Opus 4.8 | F |
| [Cyber Verification Program](https://www.anthropic.com/news/cyber-verification-program) and [help article](https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude-opus-and-sonnet); [Anthropic Cyber Mission](https://www.anthropic.com/news/anthropic-cyber-mission) | 2026-10-06 / 10-08 | Three access tiers; individuals get Defense Access only. An opt-in OSS Scanner targets over 90% true positives | F |
| [Mitigating prompt injections in browser use](https://www.anthropic.com/research/prompt-injection-defenses), Anthropic | 2025-11-24 | About 1% attack success against an adaptive attacker "still represents meaningful risk" | F |
| [Trustworthy agents in practice](https://www.anthropic.com/research/trustworthy-agents), Anthropic Frontier Red Team | 2026-04-09 | Model, harness, tools and environment each act as a capability source and an oversight point | F |
| [How I used o3 to find CVE-2025-37899](https://sean.heelan.io/2025/05/22/how-i-used-o3-to-find-cve-2025-37899-a-remote-zeroday-vulnerability-in-the-linux-kernels-smb-implementation/), Sean Heelan | 2025-05-22 | A kernel 0-day found with the bare API. The 8/100 hit rate on a known bug shows why you run many attempts and then validate | P |
| [Project Naptime](https://projectzero.google/2024/06/project-naptime.html) and [From Naptime to Big Sleep](https://projectzero.google/2024/10/from-naptime-to-big-sleep.html), Google Project Zero/DeepMind | Jun 2024 / Oct–Nov 2024 | Tool-rich single agent (code browser, debugger, sandbox) using variant analysis. Found a SQLite bug that fuzzing missed | P |
| [A summer of security](https://blog.google/innovation-and-ai/technology/safety-security/cybersecurity-updates-summer-2025/), Google | 2025 | Big Sleep claims, including CVE-2025-6965 (vendor claims) | P; claims S |
| [The Road to Top 1: How XBOW Did It](https://xbow.com/blog/top-1-how-xbow-did-it), XBOW | Ranking in late Jun 2025; post date unresolved | About 1,060 submissions; validators plus security-team review before submitting to HackerOne. Read alongside critiques from [LessWrong](https://www.lesswrong.com/posts/qefrWyeiMvWEFRitN/be-skeptical-of-milestone-announcements-by-young-ai-startups), [CyberScoop](https://cyberscoop.com/is-xbows-success-the-beginning-of-the-end-of-human-led-bug-hunting-not-yet/) and [Utku Sen](https://utkusen.substack.com/p/does-xbow-ai-hacker-deserve-the-hype) | P; outcome counts S |
| XBOW on validators: [We Ran 1,060 Autonomous Attacks](https://xbow.com/blog/we-ran-1060-autonomous-attacks); [Mythos and GPT-5.5](https://xbow.com/blog/mythos-gpt-5-5-ai-vulnerability-detection-security); [IDORs with High Accuracy](https://xbow.com/blog/xbow-finds-idors-high-accuracy-ambiguous-context); [GPT-5](https://xbow.com/blog/gpt-5); [Agents Built From Alloys](https://www.xbow.com/blog/alloy-agents) | 2025–26 | Separate finder and validator. Mostly deterministic checks. IDOR validators argue both sides. The validator once wrongly accepted XSS. Fresh solvers after about 80 iterations | P |
| [XBOW on HackerOne: what's next](https://xbow.com/blog/xbow-on-hackerone-whats-next); [Black Hat 2025 recap](https://xbow.com/blog/black-hat-2025); [validation-benchmarks](https://github.com/xbow-engineering/validation-benchmarks) | 2025-08-18 / 08-13 / 2026 | XBOW stopped chasing the leaderboard. Its benchmark is now declared saturated | P; P; F |
| [Introducing Aardvark](https://openai.com/index/introducing-aardvark/) → [Codex Security](https://openai.com/index/codex-security-now-in-research-preview/), OpenAI | 2025-10-30 → 2026-03-06 | Repo threat model, commit scanning, sandboxed exploit validation, patch for human review | P; dates S |
| [Trusted Access for Cyber](https://openai.com/index/trusted-access-for-cyber/), [Scaling TAC](https://openai.com/index/scaling-trusted-access-for-cyber-defense/), [Daybreak](https://openai.com/index/daybreak-securing-the-world/), OpenAI | 2026-02-05 / 04-14 | Identity-tiered access to GPT-5.4-Cyber/5.5-Cyber. Does not authorize testing outside permitted systems | P; dates S |
| Team Atlanta [AIxCC lessons](https://github.com/Team-Atlanta/team-atlanta.github.io/blob/main/content/english/blog/post-afc.md) and [2026 patching study](https://github.com/Team-Atlanta/team-atlanta.github.io/blob/main/content/english/blog/post-patch-2026-ensemble.md) | 2025-08-12 / 2026-03-11 | Robustness first (N-version), keep fuzzers, ensemble with N=3, "model choice outweighs framework choice," 20–40% of passing patches wrong | F |
| [Theori RoboDuck architecture](https://github.com/theori-io/aixcc-afc-archive/blob/main/docs/crs-architecture.md); [Trail of Bits AIxCC posts](https://blog.trailofbits.com/2025/08/09/trail-of-bits-buttercup-wins-2nd-place-in-aixcc-challenge/); [AIxCC SoK](https://arxiv.org/pdf/2602.07666) (2602.07666) | 2025–26 | Delta-scan mode, LLM triage and dedup, a warning that it can spend $1k+ per hour | F; P; P |
| [Agents Rule of Two](https://ai.meta.com/blog/practical-ai-agent-security/), Meta | 2025-10-31 | At most two of: untrusted input, sensitive data, state change or external communication | P; date S |
| [The lethal trifecta](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/) and [prompt-injection design patterns](https://simonwillison.net/2025/Jun/13/prompt-injection-design-patterns/), Simon Willison | 2025-06-16 / 06-13 | A hackbot is a lethal trifecta by design, so remove or contain one leg | P |
| [Prompt injection is not SQL injection](https://www.ncsc.gov.uk/blog-post/prompt-injection-is-not-sql-injection), UK NCSC | 2025-12-08 | "A good chance prompt injection will never be properly mitigated." Design to limit impact | P |
| [Month of AI Bugs slides](https://media.defcon.org/DEF%20CON%20SG%201/DEF%20CON%20SG%201%20main%20stage%20presentations/Johann%20Rehberger%20-%20The%20Month%20of%20AI%20Bugs%20Lessons%20Learned%20Exploiting%20AI%20Computer-Use%20and%20Coding%20Agents.pdf), Johann Rehberger | Aug 2025 | More than two dozen coding-agent vulnerabilities, including DNS exfiltration from Claude Code (CVE-2025-55284) | P |
| [MCP tool poisoning](https://invariantlabs.ai/blog/mcp-security-notification), Invariant Labs | 2025-04-01 | Tool poisoning, rug pulls and shadowing. Pin tools by hash | P |

### Benchmarks: which still discriminate in late 2026

| Benchmark | What it measures | Latest dated status | Role for you | Tag |
|---|---|---|---|---|
| [XBOW validation-benchmarks](https://github.com/xbow-engineering/validation-benchmarks) | 104 dockerized web challenges, levels 1–3 | **Saturated, about 100% as of mid-2026.** "Vulnerabilities have been trained into the models" | Regression and plumbing only. Rebuild with random flags | F |
| [InterCode-CTF](https://github.com/princeton-nlp/intercode) | 100 picoCTF tasks | 95% (Dec 2024) | Sanity check on loop elicitation | F |
| [Cybench](https://github.com/andyzorigin/cybench) ([leaderboard](https://raw.githubusercontent.com/cybench/cybench.github.io/HEAD/data/leaderboard.csv)) | 40 pro CTF tasks, only 8 web | Mythos Preview 100% and Opus 4.7 96% on vendor subsets (Apr 2026). A harness fork once leaked answers | Broad CTF regression; report web tasks separately | F |
| [NYU CTF Bench](https://github.com/NYU-LLM-CTF/NYU_CTF_Bench) | 200 CSAW tasks (19 web) plus a 55-task dev split | CRAKEN 22% (stale, Aug 2025) | Use the dev split for tuning | F |
| [CVE-Bench](https://github.com/uiuc-kang-lab/cve-bench) | 40 critical web CVEs, graded by application state | 13% zero-day / 25% one-day (Mar 2025). v2.1.0 released 2026-01-12. No public 2026 SOTA | The closest public analogue to bug-bounty impact | F (repo); results P |
| [BountyBench](https://github.com/bountybench/bountybench) | 40 real bounties ($10–$30,485): Detect, Exploit, Patch | Detect 12.5% for Codex CLI o3-high (2025). Figures differ across paper versions | Dollar-weighted recall minus token cost | P (repo F) |
| [CyberGym](https://github.com/sunblaze-ucb/cybergym) | 1,507 OSS-Fuzz PoC-reproduction tasks | Mythos Preview 83.1% (Apr 2026). "Nearly saturated" (Aug 2026); cost reporting now required. About 240 GB of data, 10 TB for the full server | White-box, cost-capped subset | F |
| [SEC-bench / SEC-bench Pro](https://github.com/SEC-bench/SEC-bench) | 344 kernel and browser-engine discovery+PoC instances | Codex + GPT-5.5 xhigh 58.4% (Jun 2026), all entries unverified | The white-box frontier | F |
| [AutoPenBench](https://github.com/lucagioacchini/auto-pen-bench) | 33 network/web pentest tasks | 21% autonomous vs 64% assisted (2024) | Measures the autonomy gap | F (repo) |
| [GDM in-house CTF](https://github.com/google-deepmind/dangerous-capability-evaluations), [3CB](https://github.com/apartresearch/3cb), [CyberSecEval 1–4](https://github.com/meta-llama/PurpleLlama/tree/main/CybersecurityBenchmarks), [CAIBench](https://arxiv.org/pdf/2510.24317) | Lab and safety suites | Flags stripped (GDM). CSE4 adds SOC and patching evals | Background capability checks | F; F; F; P |
| [Inspect](https://github.com/UKGovernmentBEIS/inspect_evals) + [Inspect Cyber](https://github.com/UKGovernmentBEIS/inspect_cyber) | UK AISI eval harness, now maintained by Generality Labs | Ports of Cybench, CVE-Bench, CyberGym, GDM CTFs, CyberSecEval, AgentDojo | **The recommended harness for every rung** | F |
| [Harbor](https://harborframework.com/), [Terminal-Bench](https://www.tbench.ai/), [τ²-bench](https://github.com/sierra-research/tau2-bench) | General agent eval infrastructure | Linked from Anthropic's evals post | Harness reliability | P |
| [Shannon post-cutoff benchmark method](https://github.com/KeygraphHQ/shannon/blob/main/docs/shannon-xbow-aikido-benchmark.md) | Real OSS app pinned before fixes landed after July 2026 | Opus 5: 23 true / 1 false positive for $115; DeepSeek v4 Flash: 18 TP for $6.10 (vendor-run) | The template for private holdouts | F (doc); underlying study S |

### Open-source hackbots worth reading, and their caveats

GitHub metadata below was pulled from the GitHub API on 2026-10-27 (tag F). Benchmark scores are self-reported, and XBOW-104 scores no longer discriminate. Star counts are a poor quality signal in 2026: ARTEX gained about 2k stars and 983 forks within 19 days, and a 35k-star "Anthropic-Cybersecurity-Skills" repo is not by Anthropic.

| Project | Stars / last push / license | What to study | Caveat | Tag |
|---|---|---|---|---|
| [Strix](https://github.com/usestrix/strix) | ~72.3k / 2026-10-27 / Apache-2.0 | A "graph of agents" with a Caido proxy, browser, shell and Python exploit runtime in one Docker sandbox. PoC per finding, CI mode, ships SKILL.md skills | No benchmark numbers in the README | F |
| [Shannon](https://github.com/KeygraphHQ/shannon) | ~49.5k / 2026-10-27 / AGPL-3.0 | White-box pipeline → dedupe into an exploitation queue → live validation. "Only live-validated vulnerabilities" count. Moved off the Claude Agent SDK after 1.9.0 to the Pi harness | AGPL. Black-box testing only in the commercial edition. "Do not run… against production" | F |
| [PentAGI](https://github.com/vxcontrol/pentagi) | ~32.6k / 2026-10-22 / MIT (Go) | About 13 role agents, including Adviser and Reflector supervision; pgvector plus a Graphiti/Neo4j graph; Langfuse plus OpenTelemetry; hardened Docker-in-Docker; image allowlist | Execution monitoring costs 2–3x tokens | F |
| [PentestGPT](https://github.com/GreyDGL/PentestGPT) | ~16.1k / 2026-07-14 / MIT | The task-tree origin, now a pipeline that drives Claude Code/Codex | PyPI package frozen at 2023; includes telemetry; scam copycats exist | F |
| [Decepticon](https://github.com/BitterSecurity/Decepticon) | ~6.5k / 2026-10-27 / Apache-2.0 | LangGraph, 16 kill-chain specialists with fresh context per objective, tmux for interactive tools, an isolated `sandbox-net` | 98.08% XBOW-104, self-reported | F |
| [RedAmon](https://github.com/samugit83/redamon) | ~3.2k / 2026-10-27 / MIT | A ReAct agent over a Neo4j attack graph (natural language → Cypher), parallel recon fan-out, human Allow/Deny gates and RoE | 97.1% XBOW-104, self-reported | F |
| [RAPTOR](https://github.com/gadievron/raptor) | ~3.9k / 2026-10-27 / MIT | Claude Code as the decision layer over a Python execution layer. Landlock/seccomp, staged A–F validation, $2–8 per ~50 findings | "Mostly AI-generated code"; CodeQL bars commercial use | F |
| [PentestAgent](https://github.com/GH05TCREW/pentestagent) | ~3.2k / 2026-10-26 / MIT | A RAG meta-tool that replaces MCP servers with more than 128 tools; self-spawning child agents | — | F |
| [hackingBuddyGPT](https://github.com/ipa-lab/hackingBuddyGPT) | ~1.3k / 2026-10-24 / MIT | The smallest readable loop (about 50 lines of core code) | Research scope | F |
| [deadend-cli](https://github.com/straylabs-ai/deadend-cli) | ~319 / 2026-08-07 / AGPL-3.0 | Confidence-threshold control loop. About 80% XBOW-104 with Kimi K2.5 for about $122 | Releases paused | F |
| [Cairn](https://github.com/oritera/Cairn), [pentest-ai](https://github.com/0xSteph/pentest-ai), [cain-agent](https://github.com/cdxiaodong/cain-agent), [BoxPwnr](https://github.com/0ca/BoxPwnr) | 2026 entrants | Schedules Claude Code/Codex/Pi workers; machine oracles re-run each exploit; built on the Claude Agent SDK; benchmarks LLMs on HTB and PortSwigger labs | Young projects | F |
| [CAI](https://github.com/aliasrobotics/cai) | ~9.8k / **archived 2026-08-28** | Handoffs, guardrails, a four-layer injection defense | Unmaintained; license mixes in research-only terms | F |
| [HexStrike AI](https://github.com/0x4m4/hexstrike-ai) | ~13.1k / 2026-08-03 / MIT | How 150+ CLI tools get wrapped as MCP tools | Unauthenticated `/api/command`. Forum chatter about misuse against Citrix CVEs ([The Register](https://www.theregister.com/2025/09/03/hexstrike_ai_citrix_exploits/)). Study the design; don't deploy it | F |
| First-party vulnerability harnesses: [openai/codex-security](https://github.com/openai/codex-security), [google/mantis](https://github.com/google/mantis), [vercel-labs/deepsec](https://github.com/vercel-labs/deepsec), [Visa harness](https://github.com/visa/visa-vulnerability-agentic-harness), [AISLE nano-analyzer](https://github.com/weareaisle/nano-analyzer), [Hadrian OpenHack](https://github.com/hadriansecurity/OpenHack), [seclab-taskflow-agent](https://github.com/GitHubSecurityLab/seclab-taskflow-agent) | Created 2026, active | Find→reproduce→patch pipelines with skeptical triage stages. Patterns for white-box hunting | Codex Security needs access | F |

### Tool layer, skills and supply-chain hygiene

| Item | Status (2026-10-27) | Why it matters | Tag |
|---|---|---|---|
| [PortSwigger Burp MCP Server](https://github.com/PortSwigger/mcp-server) | Official; ~1.26k stars; active | SSE MCP on localhost plus a stdio proxy. Exposes HTTP history and replay for IDOR/authz work | F |
| [caido/skills](https://github.com/caido/skills); [caido-mcp-server](https://github.com/c0tton-fluff/caido-mcp-server) (community); [Shift](https://github.com/caido-community/shift); [drift](https://github.com/six2dez/drift) | Active | Caido as the agent's proxy, as Strix embeds it | F |
| [burp-ai-agent](https://github.com/six2dez/burp-ai-agent) | Active (created 2026-01) | MCP and AI analysis inside Burp. BurpGPT (2024) and ReconAIzer (2023) are stale | F |
| [Nuclei](https://github.com/projectdiscovery/nuclei) `-ai` / `-llm` flags | Active | Prompt-generated templates and LLM-based semantic matchers | F |
| [Kali mcp-kali-server](https://gitlab.com/kalilinux/packages/mcp-kali-server) | Packaged; changelog entry 2026-10-20 | Default attack-box MCP. Depends on `sudo` and a Flask command executor, so isolate it | F |
| [GhidraMCP](https://github.com/LaurieWired/GhidraMCP), [IDA Pro MCP](https://github.com/mrexodia/ida-pro-mcp) | ~11.6k / ~13.2k stars; active | Reverse-engineering MCPs | F |
| [HackerOne GraphQL MCP](https://github.com/Hacker0x01/hackerone-graphql-mcp-server) (official org) | Created 2025-06 | Platform API access. Keep it out of the executor sandbox | F |
| [trailofbits/skills](https://github.com/trailofbits/skills) | ~7.8k; active | The highest-signal security skill pack: `fp-check`, `variant-analysis`, `burpsuite-project-parser`, `second-opinion` | F |
| [claude-code-security-review](https://github.com/anthropics/claude-code-security-review) | Last push 2026-02-11 | Diff-aware review. "Not hardened against prompt injection" | F |
| [transilienceai/communitytools](https://github.com/transilienceai/communitytools), [agamm/claude-code-owasp](https://github.com/agamm/claude-code-owasp), [YesWeHack Claude Kit](https://www.yeswehack.com/learn-bug-bounty/triager-grade-reports-claude-code) | Active | Community Claude Code packs for pentesting and report writing | F; F; S |
| [NVIDIA SkillSpector](https://github.com/NVIDIA/SkillSpector), [Snyk Agent Scan (ex-mcp-scan)](https://github.com/invariantlabs-ai/mcp-scan), [Pantheon medusa](https://github.com/Pantheon-Security/medusa) | Active | Scan skills, hooks and MCP servers before you install them | F |
| [Anthropic sandbox-runtime (srt)](https://github.com/anthropic-experimental/sandbox-runtime) | Beta | OS-level filesystem and network sandboxing for agents and MCP servers without a container | F |
| [honeyslop](https://github.com/gadievron/honeyslop) | Created 2026-04 | Code canaries maintainers use to catch hallucinated reports. Your bot must never trip them | F |
| [Fabric](https://github.com/danielmiessler/Fabric), [LifeOS (ex-PAI)](https://github.com/danielmiessler/LifeOS), [SecLists](https://github.com/danielmiessler/SecLists) (Miessler) | Active | Prompt patterns, personal infrastructure ("the verification kernel stays"), wordlists | F |
| [jhaddix/tbhm](https://github.com/jhaddix/tbhm); [Arcanum prompt-injection taxonomy](https://github.com/Arcanum-Sec/arc_pi_taxonomy) | Active | The methodology to encode; a taxonomy for AI targets | F |
| [OSS-CRS](https://github.com/sslab-gatech/oss-crs), [crs-patch-ensemble](https://github.com/Team-Atlanta/crs-patch-ensemble) | 2026 | Reusable AIxCC components and an N=3 ensemble | F |
| Avoid or quarantine: Villager ([PyPI](https://pypi.org/project/villager/), disowned by its author, provenance concerns), the Vulnhuntr PyPI name (points to a fork), [mukul975/Anthropic-Cybersecurity-Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills) (not Anthropic), [reverse-skill](https://github.com/zhaoxuya520/reverse-skill) (anomalous growth, PowerShell bootstrapping) | — | Supply-chain red flags | F |

### Curated lists to bookmark

| List | Last push | Why | Tag |
|---|---|---|---|
| [scadastrangelove/awesome-ai-security-tools](https://github.com/scadastrangelove/awesome-ai-security-tools) | 2026-10-25 | The best single bookmark, with star counts and update dates per entry | F |
| [Yeti-791/Awesome-Offensive-AI-Agentic-Landscape](https://github.com/Yeti-791/Awesome-Offensive-AI-Agentic-Landscape) | 2026-09-10 | The only list covering commercial and Chinese vendors. Its funding claims are unsourced | F |
| [raphabot/awesome-cybersecurity-agentic-ai](https://github.com/raphabot/awesome-cybersecurity-agentic-ai); [ottosulin/awesome-ai-security](https://github.com/ottosulin/awesome-ai-security); [ElNiak/awesome-ai-cybersecurity](https://github.com/ElNiak/awesome-ai-cybersecurity) | Oct 2026 | Agentic tools, security MCP servers, broad AI security | F |
| [tmylla/Awesome-LLM4Cybersecurity](https://github.com/tmylla/Awesome-LLM4Cybersecurity); [Before-Rain/Awesome-Pentest-Agents](https://github.com/Before-Rain/Awesome-Pentest-Agents); [simon-p-j-r/LLM4Pentest](https://github.com/simon-p-j-r/LLM4Pentest); [AmesianX landscape](https://github.com/AmesianX/awesome-offensive-ai-agentic-landscape) | 2026 | Paper lists | F; P; P; P |
| [Ed-Marcavage/awesome-security-agent-harnesses](https://github.com/Ed-Marcavage/awesome-security-agent-harnesses); [Puliczek/awesome-mcp-security](https://github.com/Puliczek/awesome-mcp-security) | Aug 2026 / Mar 2026 | Harnesses, sandboxes, MCP security | F |

## Courses, talks, books, podcasts and social channels worth the hours

Structured learning splits cleanly into two groups. Agent-engineering courses teach you to *build* agents. Most commercial AI-security courses teach you to *attack* AI systems. You need both, and few offerings combine them. The closest academic match to "build a harness, sandbox it, evaluate it" is CMU 11-768 (Fall 2026). Among commercial courses, only SANS SEC535 and a sold-out CanSecWest 2026 dojo explicitly teach building offensive agents. The densest practitioner material is the Critical Thinking Bug Bounty Podcast's 2026 hackbot run and Joseph Thacker's July 2026 hackbot write-up. X functions mostly as a pointer layer to long-form posts. Reddit could not be verified at all: reddit.com was blocked at every layer, so the threads listed below are mirror snapshots or second-hand quotes.

### University courses and lectures

| Course | Dates | Level | Why it matters | Tag |
|---|---|---|---|---|
| [CS 294/194-196 LLM Agents](https://rdi.berkeley.edu/llm-agents/f24), UC Berkeley RDI (Dawn Song et al.) | Fall 2024; free, recorded | I | The ReAct-to-agents lineage, taught by the people who wrote the papers | P |
| [Advanced LLM Agents](https://rdi.berkeley.edu/adv-llm-agents/sp25), Berkeley RDI | Spring 2025; free, recorded | A | Includes lectures on agent security and on LLMs catching vulnerabilities in real code | P |
| [Agentic AI](https://rdi.berkeley.edu/agentic-ai/f25), Berkeley RDI | Fall 2025; free, ~12 lectures | I–A | The most current free university series. Dawn Song's lecture on agent safety and security (Dec 8) covers attacks on your own bot | P; lecture details S |
| CMU 11-768 AI Agents (Neubig & Fried), cmu-agents.com ([listing](https://www.beri.net/learning/cmu-11-768-ai-agents-fall-2026)) | 2026-08-25 to 12-03; recordings posted as the term runs | A | Students build a harness from scratch and design agent evals. Covers sandboxing, credentials, observability, SFT/RL and multi-agent work. Neubig co-created OpenHands | S (site is a JS app; details from aggregators) |
| [Stanford CS329A Self-Improving AI Agents](https://cs329a.stanford.edu/) | Autumn 2025 | A | Verifiers, RL, deep-research agents and agent evaluation. Sources conflict on whether recordings exist | P; recordings S |
| [Stanford CS25 Transformers United](https://web.stanford.edu/class/cs25/recordings) | V6 Spring 2026; free | I | Frontier research talks; optional | P |
| [Stanford CS336 Language Modeling from Scratch](https://stanford-cs336.github.io/spring2025/) ([playlist](https://www.youtube.com/watch?v=SQ3fZ1sAqXI&list=PLoROMvodv4rOY23Y0BoGoBGgQ1zmU_MT_)) | Spring 2025/2026; free | A | Only if you plan to fine-tune or RL-train your own model | P |
| [Princeton COS 597C AI Agents](https://paws.princeton.edu/courses/fall-2026/advanced-topics-computer-science-ai-agents) (Narasimhan) | Fall 2026; no recordings | A | Reading list only | P |
| CMU 11-711 Advanced NLP ([Fall 2024 playlist via Digg](https://digg.com/tech/c2rmqwzp)) | Fall 2024 | I–A | 23 recorded lectures | S |

### Online courses for agent engineering

| Course | Date / length | Level | Why it matters | Tag |
|---|---|---|---|---|
| [Agentic AI](https://www.deeplearning.ai/courses/agentic-ai/), Andrew Ng (DeepLearning.AI; also [Coursera](https://www.coursera.org/learn/dlai-agentic-ai)) | 2025; ~10 h | I | Reflection, tool use, planning and multi-agent patterns, vendor-neutral | P |
| [Anthropic Academy](https://anthropic.skilljar.com/): [Building with the Claude API](https://anthropic.skilljar.com/claude-with-the-anthropic-api), Claude Code in Action, Intro to MCP, MCP Advanced Topics | Free; 13–19 courses depending on date | B–I | First-party API, tool-use, MCP and Claude Code training | P |
| [MCP: Build Rich-Context AI Apps with Anthropic](https://www.deeplearning.ai/short-courses/mcp-build-rich-context-ai-apps-with-anthropic) | ~1.6 h | I | Build an MCP server with FastMCP and test it with MCP Inspector, which is how you wrap security tools | P |
| [Claude Code: A Highly Agentic Coding Assistant](https://www.deeplearning.ai/short-courses/claude-code-a-highly-agentic-coding-assistant) | ~1.8 h | I | Claude Code is the reference harness that RAPTOR, PentestGPT v1 and cain-agent build on | P |
| [Evaluating AI Agents](https://www.deeplearning.ai/short-courses/evaluating-ai-agents) (Arize) | ~2.3 h | B | Tracing, component evals, LLM-as-judge | P |
| [AI Agents in LangGraph](https://www.deeplearning.ai/courses/ai-agents-in-langgraph) (Harrison Chase) | ~1.5 h | I | Build an agent from scratch, then rebuild it in LangGraph with persistence and HITL | P |
| [Long-Term Agentic Memory with LangGraph](https://www.deeplearning.ai/short-courses/long-term-agentic-memory-with-langgraph) | ~1 h | I | Semantic, episodic and procedural memory, applied here to target state and learned techniques | P |
| [Multi AI Agent Systems with crewAI](https://deeplearning.ai/short-courses/multi-ai-agent-systems-with-crewai) | ~2.7 h | B | Role-based crews. Useful as a contrast to graph control | P |
| [LangChain Academy: Introduction to LangGraph](https://academy.langchain.com/collections/foundation) ([repo](https://github.com/langchain-ai/langchain-academy)) | Free; 5–8 h | I | State, memory, HITL, multi-agent, deployment | P (repo F) |
| [LangChain Academy: Introduction to Deep Agents](https://academy.langchain.com/collections/products?page=2) | Reportedly Jul 2026 | I | Planner, subagents, filesystem and long horizons: close to a multi-hour pentest agent | P; launch date S |
| [Hugging Face Agents Course](https://huggingface.co/learn/agents-course/unit0/introduction) and [MCP Course](https://huggingface.co/learn/mcp-course/unit0/introduction) | Free, certificates | B–I | smolagents, LlamaIndex and LangGraph side by side | P |
| [Microsoft AI Agents for Beginners](https://github.com/microsoft/ai-agents-for-beginners) | 12–18 lessons | B | Includes a lesson on securing agents | P |
| [Google × Kaggle 5-Day AI Agents Intensive](https://blog.google/technology/developers/ai-agents-intensive/) | Nov 10–14, 2025; now self-paced | B–I | Compact whitepapers on tools/MCP and evals | P |
| Maven "AI Evals for Engineers & PMs", Husain & Shankar ([listing](https://www.beri.net/learning/maven-ai-evals-for-engineers-and-pms)) | Cohort Sep 5 – Oct 3, 2026; price conflicts ($3,750–$5,000) | I | The most practitioner-respected evals curriculum. Evals separate a demo from a precise hackbot | S (canonical URL not captured) |

### Offensive-security and AI-security training

| Training | Date / cost | Level | Builds agents or attacks AI? | Tag |
|---|---|---|---|---|
| [SANS SEC535 Offensive AI](https://www.sans.org/cyber-security-courses/offensive-ai-attack-tools-techniques) (GIAC GOAA); bundled as [SEC537](https://www.sans.org/cyber-security-courses/ai-offensive-security) with SEC536 | 3 days; ~$5,250 license | I–A | **Builds**: AI-automated recon and offensive AI tooling | P |
| [CanSecWest 2026: Building Your Own Offensive Security Agentic Harness](https://www.register.cansecwest.com/csw26/ai-powered-offensive-security-building-and-your-own-offensive-security-agentic-harness) | Sep 26–29, 2026; sold out | A | **Builds**: the only 2026 conference training found with that stated goal. Watch for a 2027 repeat | P |
| [Arcanum "Red, Blue, Purple AI"](https://payhip.com/b/GpkZv) (Jason Haddix) | 2-day live | I | Uses LLMs across security work. Whether it covers autonomous agents is unverified | P; syllabus U |
| [Arcanum "Attacking AI"](https://owaspglobalappsecusa2025.sched.com/event/268qk/2-day-training-attacking-ai) and free [Anti-Cast](https://www.antisyphontraining.com/event/anti-cast-attacking-ai-version-1-1-with-jason-haddix/) | Nov 2025 / Jul 2025 | I | Attacks AI: a seven-point methodology and a prompt-injection taxonomy | P |
| [PortSwigger Web LLM attacks](https://portswigger.net/web-security/llm-attacks) ([learning path](https://portswigger.net/web-security/learning-paths/llm-attacks)) | Free | I | Attacks AI. The best short hands-on lab on "excessive agency", your own bot's core risk | P |
| [HTB AI Red Teamer path](https://academy.hackthebox.com/path/preview/ai-red-teamer) (with Google; [COAE cert](https://roadmap.hackthebox.com/changelog/introducing-the-new-htb-certified-offensive-ai-expert-htb-coae)) | Subscription | I–A | Mostly attacks AI. Includes "Applications of AI in InfoSec" | P |
| [OffSec AI-300 / OSAI](https://offsec.com/courses/ai-300) | 2026; from $1,749 | A | Attacks AI and multi-agent environments | P |
| [TCM AI Hacking 101](https://tcm-sec.com/academy/ai-hacking-101/) | $29.99/month | B–I | Attacks AI: injection, jailbreaks, RAG leaks | P |
| [Antisyphon Attacking and Defending AI/LLM Systems](https://www.antisyphontraining.com/product/attacking-and-defending-ai-with-brian-fehrman-joff-thyer-and-derek-banks/) | 8 h | I | Attacks and defends AI | P |
| [DEF CON Training: Strategic AI Penetration](https://training.defcon.org/products/strategic-ai-penetration-mastering-offensive-techniques-for-llms-marek-zmyslowski-konrad-jedrzejczyk-dclv2026) | Aug 2026; $3,500 | A | Attacks AI with PyRIT and garak | P |
| [AppSecEngineer AI Agent Security Masterclass](https://www.appsecengineer.com/workshop/blackhat-usa-2026-ai-agent-security-masterclass-2026-edition); [SANS SEC545](https://www.sans.org/blog/why-sec545-genai-and-llm-application-security); [Learn Prompting AI Red Teaming (Maven)](https://maven.com/learn-prompting-company/ai-red-teaming-and-ai-safety-masterclass) | 2026 | I–A | Defending and attacking agent applications | P |

### Pentest and bug-bounty fundamentals to encode

| Resource | Date | Level | Why it matters | Tag |
|---|---|---|---|---|
| [PortSwigger Web Security Academy, all labs](https://portswigger.net/web-security/all-labs) | Free, living | B–A | The methodology you will automate. Lab descriptions double as eval tasks (check PortSwigger's terms for automated use) | P |
| [The Bug Hunter's Methodology (TBHM)](https://github.com/jhaddix/tbhm), Jason Haddix | Living | B–I | The canonical web bug-hunting methodology repo | F |
| [*Bug Bounty Bootcamp*](https://www.oreilly.com/library/view/bug-bounty-bootcamp/9781098129088/), Vickie Li | Dec 2021 | B–I | Recon, vulnerability classes, reporting | P |
| [*Hacking APIs*](https://securityboulevard.com/2022/09/5-books-every-api-hacker-should-read), Corey Ball | Jul 2022 | I | Modern targets are API-heavy, and BOLA/IDOR is where hackbots struggle | P |
| [*Real-World Bug Hunting*](https://bookshare.org/browse/book/6237451), Peter Yaworski | Jul 2019 | B–I | Paid-report case studies that make good few-shot material | P |
| HackerOne Hacker101, Bugcrowd University, OWASP WSTG, TryHackMe | Living | B | Standard free resources | U (current URLs and status unconfirmed) |

### Books on agents, LLM security and offensive agents

| Book | Date | Level | Why it matters | Tag |
|---|---|---|---|---|
| [*AI Agents for Offensive Security*](https://manning.com/books/ai-agents-for-offensive-security), Mark Foudy (Manning MEAP) | MEAP Apr 2026; final ~early 2027 | I–A | The most on-target hackbot book found: code-first multi-agent pipelines for recon, triage, discovery and reporting. Chapters are still changing | P |
| [*Agentic AI for Offensive Cybersecurity*](https://www.packtpub.com/en-us/product/agentic-ai-for-offensive-cybersecurity-9781806114467), Orhan Yildirim (Packt) | Feb 2026 | I | Low-code (n8n plus MCP) agentic pentest workflows with a human in the loop | P |
| [*AI Engineering*](https://www.oreilly.com/library/view/ai-engineering/9781098166298/afterword01.html), Chip Huyen | Dec 2024 | I | Evaluation methodology, RAG, agents, finetuning | P |
| [*AI Agents in Action, 2nd ed.*](https://manning.com/books/ai-agents-in-action-second-edition), Micheal Lanham | Jun 2026 | I | A rewrite covering MCP and A2A | P |
| [*Building Applications with AI Agents*](https://www.oreilly.com/library/view/building-applications-with/9781098176495/), Michael Albada | Sep–Oct 2025 | B–I | Single- vs multi-agent trade-offs | P |
| [*The Developer's Playbook for LLM Security*](https://oreilly.com/library/view/the-developers-playbook/9781098162191), Steve Wilson (OWASP LLM Top 10 lead) | Sep 2024 | I | Trust boundaries and prompt injection | P |
| [*Building Agentic AI Systems*](https://www.packtpub.com/en-us/product/9781803238753), Biswas & Talukdar; [*Hands-On LLMs*](https://www.oreilly.com/library/view/-/9781098150952), Alammar & Grootendorst; [*Build a Reasoning Model (From Scratch)*](https://www.i-programmer.info/book-watch-archive/19064-build-a-reasoning-model-from-scratch-manning.html), Raschka | 2024–26 | B–A | Supporting depth. Raschka only if you train models | P |
| [*Penetration Testing Using Artificial Intelligence*](https://andoffice.com/products/penetration-testing-using-artificial-intelligence-methods-to/231874362/); [*Generative AI for Hackers*](https://leanpub.com/generativeaiforhackershands-oncyberoperationsinthecybersecurityera) (Leanpub) | 2026 | — | Publisher or quality unconfirmed | S |
| Hamel Husain, *Evals for AI Engineers* (O'Reilly, in progress, [per Pragmatic Engineer](https://newsletter.pragmaticengineer.com/p/evals)) | Forthcoming | I | Watch for release | S |

### Talks and videos

All YouTube IDs below were confirmed in search results on youtube.com. None were watched.

| Video | Date | Level | Why watch | Tag |
|---|---|---|---|---|
| Karpathy: [Intro to LLMs](https://www.youtube.com/watch?v=zjkBMFhNj_g), [Deep Dive into LLMs](https://www.youtube.com/watch?v=7xTGNNLPyMI), [How I use LLMs](https://www.youtube.com/watch?v=EWvNQjAaOHw), [Software Is Changing (Again)](https://www.youtube.com/watch?v=LCEmiRjPEtQ) | 2023–25 (keynote 2025-06-17) | B | Background. The Intro talk has an LLM-security section | P |
| [How We Build Effective Agents](https://www.youtube.com/watch?v=D7_ipDqhtwk), Barry Zhang (Anthropic) | 2025 | B | When not to build agents, and why to keep them simple | P |
| [Claude Code best practices](https://www.youtube.com/watch?v=gv0WHhKelSE), Cal Rueb; [Inside Claude Code with Boris Cherny](https://www.youtube.com/watch?v=PQU9o_5rHC4) (YC Lightcone); [Why Coding Is Solved](https://www.youtube.com/watch?v=SlGRN8jh2RI) (Sequoia 2026); [A conversation on Claude Code](https://www.youtube.com/watch?v=Yf_1w00qIKc); [Mastering Claude Code in 30 Minutes](https://www.youtube.com/watch?v=B_KAEqiC-0Q) (uploader unclear) | 2025–26 | B–I | How the reference harness is meant to be driven: subagents, plan mode, hooks | P |
| [The New Code](https://www.youtube.com/watch?v=BIvILtt164I), Sean Grove (OpenAI); [World's Fair 2025 Day 2](https://www.youtube.com/watch?v=U-fMsbY-kHY); [WF2026 Autoresearch & Keynotes](https://www.youtube.com/watch?v=4sX_He5c4sI) | 2025–26 | I | Specs as the primary artifact; full-day streams | P |
| [DEF CON 33: Prompt Scan Exploit — AI's Journey Through 0Days and 1000 Bugs](https://www.youtube.com/watch?v=sOkgHfu4lXY), Jurado & Nogue (XBOW); [XBOW AMA short](https://www.youtube.com/shorts/0oLPHgeRsYk) | Aug 2025 | I | The key primary talk on XBOW's architecture | P |
| [XBOW: AI Hacking Agent and Human in the Loop (CTBB Ep. 134)](https://www.youtube.com/watch?v=rvA8IbyogJ0); [Automating Offensive Security with XBOW](https://www.youtube.com/watch?v=Q17hit4J3Cg) (Waisman); [Cracking the Code… ft. Oege de Moor](https://www.youtube.com/watch?v=9mIphDV9m9c); [XBOW channel](https://www.youtube.com/@xbow-ai) | 2024–25 | I | Headless-browser XSS validation, SSRF false positives, audit trails | P |
| AIxCC: [Carney preview](https://www.youtube.com/watch?v=lQDOIFZgBs4), [winners](https://www.youtube.com/watch?v=8F1j2zjRjZM), [ShellPhish](https://www.youtube.com/watch?v=PiJwIUGJGmw), [ShellPhish CRS](https://www.youtube.com/watch?v=FIXBv8Idppk), [FuzzingBrain](https://www.youtube.com/watch?v=aGFEXm014vA) | Aug 2025 | A | How cyber reasoning systems were built | P |
| Haddix: [Attacking AI (DEF CON 33 BBV keynote)](https://www.youtube.com/watch?v=mYQgUHVgBPU), [Red, Blue, and Purple AI](https://www.youtube.com/watch?v=XHeTn7uWVQM), [Building Hackbots](https://www.youtube.com/watch?v=j7TWLF3YsUI), [Building Web Hacking Micro Agents (Ep. 102)](https://www.youtube.com/watch?v=3y8dyeKmJQI), [The Future of Hacking is Agentic](https://www.youtube.com/watch?v=adV922E1ve0) | 2024–26 | I | The micro-agent design. AI scales small teams but doesn't replace seniors | P |
| [Bug Bounty, Hacking, Community & Building with AI](https://www.youtube.com/watch?v=6ajeZq9Qtow) (NahamSec with Rishiraj Sharma); [AI-Powered Bug Hunting, GOTO 2024](https://www.youtube.com/watch?v=lCZtgN7b4TE) | ~mid-Sep 2026 / 2024 | B–I | Where humans still matter | P |
| Third-party tool reviews: [Strix](https://www.youtube.com/watch?v=vRTliPb8fjg), [PentAGI](https://www.youtube.com/watch?v=P3nCvrq20HU), [HexStrike MCP on Kali](https://www.youtube.com/watch?v=jXLB6xzrpzU) | 2025–26 | B | Quick looks at the tools (uploaders unconfirmed) | P |
| [Anatomy of an Agentic Personal AI Infrastructure](https://www.youtube.com/watch?v=l9CPmPk2R-M), Daniel Miessler | 2026 | I | Personal infrastructure design | P |
| Channels: [AI Engineer](https://www.youtube.com/@aiDotEngineer), [LangChain](https://www.youtube.com/@LangChain), [IndyDevDan](https://www.youtube.com/@indydevdan), [John Hammond](https://www.youtube.com/channel/UCVeW9qkBjo3zosnqUbG7CFw) (no AI-pentest review found), [Critical Thinking](https://www.youtube.com/@criticalthinkingpodcast) | Living | All | Subscriptions | P |
| Haddix × NetworkChuck "Hacking AI is TOO EASY" ([aggregator](https://videohighlight.com/v/_yfiUQSbdPY)); rez0 "AI and hacking" ([summarizer](https://www.chaindesk.ai/tools/youtube-summarizer/ai-and-hacking-opportunities-and-threats-joseph-rez0-thacker-zY7dz4Dx5tc)) | 2025 | B | IDs from weak sources | S |

### Podcasts

| Show / episode | Date | Why listen | Tag |
|---|---|---|---|
| **Critical Thinking – Bug Bounty Podcast** (Justin Gardner, Joseph Thacker, Brandyn Murtagh): [Ep. 181 Bug Bounty Singularity](https://www.youtube.com/watch?v=QrVOFD2Xl2A); [Ep. 184 Ads Dawson, 750+ bugs](https://www.youtube.com/watch?v=0olk6lC1AGg); [Ep. 175 Rhyno's Hackbot Setup](https://www.youtube.com/watch?v=v-XhQHy_jHM); [Ep. 182 Hackbot GraphQL](https://www.youtube.com/watch?v=IESlRFjT0G8); [Ep. 191 local AI vs subsidized tokens](https://www.youtube.com/watch?v=vSCUThW90Cw); [Ep. 192 Hackbot PoC skill creation](https://www.youtube.com/watch?v=xxU6mq4JoNo); [Ep. 165 self-improving Claude workflows](https://www.youtube.com/watch?v=J8wFq51TtMs); [Ep. 129 Is this how Bug Bounty Ends](https://www.youtube.com/watch?v=XkmoloPKri8) | 2025–26 (Ep. 175 May 2026; Ep. 181 ~Jul 2026; Ep. 184 promoted 2026-07-23) | **The single best source on real hackbot setups**: skills, queues, logging, cost, load distribution | P |
| CTBB directory-only episodes: [Ep. 166 Rez0's Top Claude Skill Secrets](https://www.castfox.net/podcast/critical-thinking-bug-bounty-podcast-5951018/episode/episode-166-rez0s-top-claude-skill-secrets-462100); [Ep. 128 architecting source-code review AI bots](https://pocketcasts.com/podcasts/946adcc0-728b-013b-f272-0acc26574db2/f240d1d1-bc77-45fc-9306-29aec441fa93); [Ep. 137 AI-assisted whitebox review](https://pocketcasts.com/podcasts/946adcc0-728b-013b-f272-0acc26574db2/6ec2066c-8710-4117-8241-d42017e04b51); [Ep. 170 Claude Code, tmux](https://www.castfox.net/podcast/critical-thinking-bug-bounty-podcast-5951018/episode/episode-170-claude-code-tmux-websockets-and-other-korea-lhe-takeaways-462096); [Ep. 173 Bug Bounty is Dead and AI Killed It](https://www.castfox.net/podcast/critical-thinking-bug-bounty-podcast-5951018/episode/episode-173-bug-bounty-is-dead-and-ai-killed-it-1450621); [Ep. 117 Hacking AI series](https://metacast.app/podcast/critical-thinking-bug-bounty-podcast/RxFQfeif/episode-117-hacking-ai-series-vulnus-ex-machina-part-1/koRkMlzG) | 2025–26 | Skills, whitebox bots, the "is bug bounty dead" debate | P (via directory listings; some titles from URL slugs) |
| [Sequoia Training Data: Oege de Moor](https://xbow.com/podcasts/cracking-the-code-on-offensive-security-with-ai); [Altimeter XBOW Founder Spotlight](https://xbow.com/podcasts/xbow-founder-spotlight); [ASW #351, Nico Waisman](https://www.scworld.com/podcast-episode/asw-351) | Dec 2024 / ~Jul 2025 | XBOW's founders on swarms of agents and validators | P |
| [Security Conversations with Federico Kirschbaum (XBOW)](https://securityconversations.com/?p=763) | May 2026 | Autonomous AI hackers in 2026 | S |
| [Pragmatic Engineer: Building Claude Code with Boris Cherny](https://newsletter.pragmaticengineer.com/p/building-claude-code-with-boris-cherny); [Every AI & I: Claude Code like its builders](https://every.to/source-code/how-to-use-claude-code-like-the-people-who-built-it); [Pragmatic Engineer: context engineering with Dex Horthy](https://newsletter.pragmaticengineer.com/p/context-engineering-with-dex-horthy) | 2025–26 | Harness internals from their builders | P |
| Latent Space: [Claude Code with Cat Wu & Boris Cherny](https://vladg.bearblog.dev/cc-pod/) (summary); [The Age of Async Agents, Walden Yan](https://podscripts.co/podcasts/latent-space-the-ai-engineer-podcast/the-age-of-async-agents-cognitions-walden-yan-openinspects-cole-murray) | 2025–26 | Agent-engineering community hub | S |
| [Unsupervised Learning](https://newsletter.danielmiessler.com/t/Podcast) (Miessler), e.g. [UL 486](https://omny.fm/shows/unsupervised-learning/ul-no-486-member-edition-fully-automated-ai-malware-binary-and-web-my-debate-with-marcus-hutchins-on-ai-the-did-you-notice-psyop-the-metr-ai-metric-for-longterm-tasks-and-more) | Weekly | AI-offense news | P |
| [Lenny's: Why AI evals are the hottest new skill](https://www.lennysnewsletter.com/p/why-ai-evals-are-the-hottest-new-skill); [High Agency: evals with Hamel & swyx](https://el.player.fm/series/high-agency-the-podcast-for-ai-builders/why-your-ai-product-needs-evals-with-hamel-husain-and-swyx); [O'Reilly: Context Engineering with Drew Breunig](https://www.oreilly.com/radar/podcast/generative-ai-in-the-real-world-context-engineering-with-drew-breunig/) | 2025 | Evals and context discipline | P |
| [Jawbreaker: the curl controversy](https://jawbreaker.transistor.fm/episodes/ai-chaos-in-open-source-security-the-curl-controversy-unfolds); [Open Source Security: curl vs AI](https://opensourcesecurity.io/2025/2025-05-curl_vs_ai_with_daniel_stenberg/); [Darknet Diaries 130 (Haddix background)](https://darknetdiaries.com/episode/130/) | 2025 | What maintainers will not tolerate | P |

### X/Twitter posts with primary-source design heuristics

Dates come from decoding the status-ID snowflake, so they are exact (UTC).

| Post | Date | Why it matters | Tag |
|---|---|---|---|
| [Tobi Lütke on "context engineering"](https://x.com/tobi/status/1935533422589399127); [Karpathy +1](https://x.com/karpathy/status/1937902205765607626); [Willison: it will stick](https://x.com/simonw/status/1938745355916714448) | 2025-06-19 / 06-25 / 06-27 | The origin of the term | P |
| [Boris Cherny's setup thread](https://x.com/bcherny/status/2007179832300581177) and [tip 13: give Claude a way to verify its work](https://x.com/bcherny/status/2007179861115511237); follow-ups on [team tips](https://x.com/bcherny/status/2017742741636321619), [customization](https://x.com/bcherny/status/2021699851499798911), [hidden features](https://x.com/bcherny/status/2038454336355999749) | 2026-01-02 → 03-30 | Parallel sessions, shared CLAUDE.md, plan mode, `/permissions` instead of skipping permissions, hooks. Verification gives 2–3x quality | P |
| [rez0: "agent md file… hacking skills, /goal find a crit… 90% of the way there"](https://x.com/rez0__/status/2055067183168794953) | 2026-05-14 | The current practitioner recipe. See the policy caveat in the guardrails section | P |
| [rez0 on Haddix's hackbot-engineering newsletter](https://x.com/rez0__/status/1964016722081403031); [TEDx "Rise of AI Hackbots"](https://x.com/rez0__/status/1888781796885905857); [hack-AI guide](https://x.com/rez0__/status/1894413009495171297); [on XBOW replacing pentesters](https://x.com/rez0__/status/1957491543268270236) | 2025 | Pointers to the long-form canon | P |
| [Haddix: give agents "the full path for everything… defined syntax"](https://x.com/Jhaddix/status/2094803626950902056); [AI "cannot replace mid/sr people"](https://x.com/Jhaddix/status/1883144233059500316) | 2026-09-01 / 2025-01-25 | Without exact paths, hackbots invent their own tools | P |
| [Huntley: Ralph needs a watchdog](https://x.com/GeoffreyHuntley/status/2033317516554944976); [Pocock: stick with a bash loop](https://x.com/mattpocockuk/status/2010330604123144389); [Horthy: Ralph session](https://x.com/dexhorthy/status/1984765179452936649); [Horthy: commit messages as intentional compaction](https://x.com/dexhorthy/status/1961490837017088051) | 2025–26 | Practitioner loop engineering | P |
| [@ctbbpodcast: wide-scope hackbot finds NoSQL `$where` injection](https://x.com/ctbbpodcast/status/2074893912024498625) | 2026-07-08 | What breadth-first hackbots find | P |
| XBOW: [#11 in the US](https://x.com/Xbow/status/1869053482642362846); [#1 globally](https://x.com/Xbow/status/1951040733936492915); [Barragan: XBOW concludes HackerOne effort](https://x.com/lean0x2f/status/1957493111124312148); [de Moor retrospective: 54 criticals in 90 days](https://x.com/oegerikus/status/2107187332315336801) | 2024-12-17 → 2026-10-05 | The arc of the XBOW milestone | P |
| [Reuters: one hacker, AI agents, a few thousand dollars, scores of sites](https://x.com/Reuters/status/2112189566765474049) | 2026-10-19 | Current-events signal on misuse and the scrutiny it will bring | P |
| Accounts to follow: [@rez0__](https://x.com/rez0__), @Jhaddix, @bcherny, @simonw, @dexhorthy, @karpathy, @seanhn, @oegerikus | Living | The hackbot and agent-engineering core | P (rez0); others vary (Rhynorater, wunderwuzzi23 U) |

### Hacker News threads (skepticism anthologies)

| Thread | Why read | Tag |
|---|---|---|
| [XBOW reaches top spot on HackerOne](https://news.ycombinator.com/item?id=44367548) | Validity-rate skepticism (3/43 valid at AT&T vs 22/24 at Disney) and pre-submission human review | P |
| [Show HN: Strix](https://news.ycombinator.com/item?id=44945113); [Shannon](https://news.ycombinator.com/item?id=46944416); [Open-source XBOW alternative](https://news.ycombinator.com/item?id=45428526); [Show HN: AI pentester with a disproving judge](https://news.ycombinator.com/item?id=47056759); [Post-trained model that pen tests instead of refusing](https://news.ycombinator.com/item?id=48609231) | Launches and builder discussion | P |
| [ARTEMIS vs professionals](https://news.ycombinator.com/item?id=46518996); [Rise of AI Pentesting Agents (2026)](https://news.ycombinator.com/item?id=47750350); [DeepSeek v4.1 Flash best hacking model](https://news.ycombinator.com/item?id=49725800); [Small models found Mythos's vulns](https://news.ycombinator.com/item?id=47732020) | 2026 model and agent debates; source-aware tools find the real CVEs | P |
| [Building Effective Agents](https://news.ycombinator.com/item?id=42468058); [Effective context engineering](https://news.ycombinator.com/item?id=45418251); [Effective harnesses](https://news.ycombinator.com/item?id=46081704); [Don't Build Multi-Agents](https://news.ycombinator.com/item?id=45096962) and ["Whom to believe? Devin or Claude?"](https://news.ycombinator.com/item?id=45097335); [Orchestrate teams of Claude Code sessions](https://news.ycombinator.com/item?id=46902368); [Claude Skills](https://news.ycombinator.com/item?id=45607117) | Engineering canon, with practitioner pushback | P |
| curl: [stopped HackerOne bounty](https://news.ycombinator.com/item?id=46678710), [Ars Technica thread](https://news.ycombinator.com/item?id=46729871), [no valid AI report yet (May 2025)](https://news.ycombinator.com/item?id=43907376), [counterpoint: good AI-assisted reports](https://news.ycombinator.com/item?id=45449348) | Both sides of the slop crisis | P |
| [GTG-1002 report](https://news.ycombinator.com/item?id=45918638) and ["smells like bullshit"](https://news.ycombinator.com/item?id=45944296); [Heelan o3](https://news.ycombinator.com/item?id=44060711); [Big Sleep SQLite](https://news.ycombinator.com/item?id=42043453); [Big Sleep 77 issues](https://news.ycombinator.com/item?id=45899218) | Quality of evidence behind headline claims | P |

### Reddit: no verified threads

No Reddit thread could be verified. reddit.com returned proxy 403s, the search backend rejected the domain, and four unfiltered queries returned zero Reddit URLs. What exists is second-hand. Mirror snapshots of 2026 r/LocalLLaMA threads ([2026-06-13](https://reddit.sentinel-team.org/posts/1tytx9j/snapshots/2026-06-13T02%3A56%3A06.64152Z), [2026-09-04](https://reddit.sentinel-team.org/posts/1w15nc9/snapshots/2026-09-04T21%3A20%3A12.3645Z); S) recommend abliterated open-weight models and the CAI framework for offline pentesting, with [Quesma's "security paradox of local LLMs"](https://quesma.com/blog/local-llms-security-paradox) (P) as the counterpoint. A widely circulated r/bugbounty quote calls XBOW "great at low-hanging fruit" but missing deeper issues ([via Medium](https://medium.com/@ishowcybersecurity/is-xbow-replacing-cybersecurity-professionals-the-truth-behind-the-ai-hacker-taking-over-hackerone-7f5f38d78153); S). The subreddits to search manually are r/bugbounty, r/netsec, r/LocalLLaMA, r/ClaudeAI, r/ClaudeCode, r/AI_Agents, r/LangChain and r/cybersecurity (thread-level content U). The closest verified stand-in is YesWeHack's test of whether Claude Code can find and validate real bugs. It found the model identifies vulnerabilities well but "often struggles to produce a proper proof of concept" ([YesWeHack](https://www.yeswehack.com/learn-bug-bounty/llm-series-claude); P).

### Newsletters, blogs and communities

| Source | Why follow | Tag |
|---|---|---|
| ["The Bug Bounty Singularity: Our Hackbot"](https://josephthacker.com/hacking/2026/07/01/we-built-a-hackbot.html), Joseph Thacker & JD (2026-07-01) | A first-hand build from merged Claude Code skills with queues, logging, severity scoring and human feedback. tl;dr sec reports 126 vulns in 5 months, 88 High/Critical (newsletter-reported) | P; figures S |
| ["All About Hackbots"](https://josephthacker.com/ai/2024/02/21/hackbots.html) (2024-02-21) and [Thacker Thoughts](https://thacker.beehiiv.com/p/everything-need-know-hackbots-little-something-extra) | The definitional post, plus the newsletter on his open-source hackbot | P |
| [tl;dr sec](https://tldrsec.com/) (Clint Gibler), e.g. [#337](https://tldrsec.com/p/tldr-sec-337) | The best weekly AI-for-security digest | P |
| Executive Offense (Jason Haddix), executiveoffense.beehiiv.com | Its Sep 2025 issue is "a full guide on how to actually engineer a good hackbot" per rez0 | S (issue URL U) |
| [HackerOne: Welcome, Hackbots](https://www.hackerone.com/blog/welcome-hackbots-how-ai-shaping-future-vulnerability-discovery); [5 Months of Hackbot Activity](https://www.hackerone.com/blog/ai-hackbots-security-testing-update); [Human + Machine leaderboard](https://www.hackerone.com/blog/hackerone-leaderboard-update-ai-vs-human) | Platform rules and data your bot must respect | P |
| [Simon Willison](https://simonwillison.net/) ([ai-security-research tag](https://simonwillison.net/tags/ai-security-research/?page=2)); [Drew Breunig](https://www.dbreunig.com/); [Hamel Husain](https://hamel.dev/) ([evals FAQ PDF, 2026-09-18](https://hamel.dev/blog/posts/evals-faq/evals-faq.pdf), [LLM-as-judge](https://hamel.dev/blog/posts/llm-judge/), ["It's hard to eval" is a smell](https://hamel.dev/blog/posts/eval-smell/)) | Agent security, context, evals | P |
| [Anthropic Engineering](https://www.anthropic.com/engineering) | The densest primary agent-engineering series | F |
| [sean.heelan.io](https://sean.heelan.io/); [Embrace The Red](https://embracethered.com/); [daniel.haxx.se, "AI slop attacks on the curl project"](https://daniel.haxx.se/blog/2025/08/18/ai-slop-attacks-on-the-curl-project) | LLM vulnerability research, agent exploitation, the maintainer's view | P; S; P |
| [PentesterLab Research worth reading](https://pentesterlab.com/blog/research-worth-reading-week28-2026); [OWASP GenAI newsletter](https://genaisecurity.beehiiv.com/); [Unsupervised Learning](https://newsletter.danielmiessler.com/); [AINews](https://news.smol.ai/) | Weekly digests | P; P; P; S |
| [Critical Thinking Discord](https://ctbb.show/discord) and [Critical Research Lab](https://lab.ctbb.show/) | Where working hackbot builders compare setups (free plus $5/$10/$25 tiers) | S |
| [OWASP GenAI Security Project Slack](https://genai.owasp.org/contributing/) (#project-genai; 30,000+ members) | Agentic threat methodology and the 2026 Top 10s | P |
| [Dreadnode Crucible](https://docs.dreadnode.io/ai-red-teaming/) ([Rigging tutorial](https://www.bugcrowd.com/blog/rigging-the-system-the-art-of-ai-exploits/)) | Hands-on AI red-team challenges solvable by your own agent | P |
| NahamSec, Bugcrowd, Latent Space, Claude Developers, AI Village Discords | Plausible communities | U |


## Learning roadmap: eight stages from first prompt to supervised hackbot

The roadmap below turns the catalog into a sequence. It assumes about **10 hours a week** and runs two tracks in parallel, as the course research recommends: Track A is AI engineering and Track B is offensive fundamentals. The tracks merge at Stage 6, when you start encoding a methodology you can already perform by hand. At that pace Stages 0–6 take about **36 weeks**, and a first supervised submission comes at roughly **ten months**. Someone who already writes Python and hunts bugs can compress Stage 0 and most of Track B. The week counts are this report's estimates, not sourced figures. The order is sourced. Anthropic tells builders to find "the simplest solution possible… This might mean not building agentic systems at all" ([Anthropic](https://www.anthropic.com/engineering/building-effective-agents); F). Palisade showed that a plain agent is the baseline to beat ([arXiv 2412.02776](https://arxiv.org/abs/2412.02776); P). And platforms require a human to validate every report ([HackerOne](https://www.hackerone.com/blog/welcome-hackbots-how-ai-shaping-future-vulnerability-discovery); P). All three push toward building narrow and verified before building broad and autonomous.

Every practice target in this roadmap falls into one of three groups. Some are intentionally vulnerable labs you run yourself: OWASP Juice Shop, DVWA, and XBOW's benchmarks built in local Docker. Some are hosted training labs: PortSwigger Web Security Academy, Hack The Box and TryHackMe. The rest are programs whose written policy authorizes your testing. Hosted labs belong to their operators, so read their terms before you point an automated agent at them. The research could not verify those terms.

| Stage | Weeks (cumulative) | Track A focus | Track B in parallel (~3 of the 10 hours) | Milestone |
|---|---|---|---|---|
| 0 Foundations | 1–8 | Python, Linux, Docker, git, LLM API basics | HTTP, an intercepting proxy, PortSwigger Apprentice labs | Isolated home lab and a first costed tool-use call |
| 1 Prompt engineering | 9–11 | System prompts, structured output, prompt evals | *Bug Bounty Bootcamp*; Practitioner labs | Triage prompt measured on 30 labelled samples |
| 2 Context engineering | 12–14 | Retrieval, compaction, caching, notes, skills | *Hacking APIs* | Recon-notes pipeline with measured token savings |
| 3 Agent loops and tools | 15–18 | A loop from scratch; tool design | *Real-World Bug Hunting*; HTB/TryHackMe boxes | A ~300-line agent solving local benchmark tasks |
| 4 Harness engineering | 19–24 | Sandbox, scope proxy, gates, tracing, cost caps, evals | PortSwigger Web LLM attacks labs | A lab harness that passes its own containment tests |
| 5 Graphs and orchestration | 25–28 | State graphs, subagents, and when not to use them | Practitioner labs in your chosen bug class | A single-agent vs orchestrated A/B with numbers |
| 6 Security integration | 29–36 | Single-class hunter, validators, recon skills | Merged | A validator-gated hunter tested on held-out labs |
| 7 Supervised real-world use | 37 onward | Evaluation ladder, then authorized programs | Merged | First human-reviewed reports on an authorized program |

### Stage 0 (weeks 1–8): learn the web and the plumbing before automating either

**Goal and time.** Six to eight weeks. By the end you should be able to read raw HTTP, drive command-line tools from Python, run disposable containers, and make a tool-use call to an LLM API while knowing what it cost. Skip ahead if you already do these things daily.

**What to learn.** On the web side, learn HTTP methods, headers, cookies and sessions, TLS and DNS basics, and the same-origin policy. Learn the major vulnerability classes the way PortSwigger's topic list and the OWASP testing guide organize them, and practice each through an intercepting proxy such as Burp or Caido. On the engineering side, learn Python with `subprocess` and an async HTTP client, the Linux shell, git, and Docker Compose networking, including how to give a container no route out. On the LLM side, learn the API basics: messages, tool definitions, tokens, prompt caching and pricing.

**Best resources.**
- [PortSwigger Web Security Academy](https://portswigger.net/web-security/all-labs) (P) is the spine of Track B.
- Jason Haddix's [The Bug Hunter's Methodology](https://github.com/jhaddix/tbhm) (F) shows how hunters actually sequence their work.
- Anthropic Academy's [Building with the Claude API](https://anthropic.skilljar.com/claude-with-the-anthropic-api) (P) or the [anthropics/courses](https://github.com/anthropics/courses) notebooks (P) cover API mechanics.
- Karpathy's [Intro to LLMs](https://www.youtube.com/watch?v=zjkBMFhNj_g) (P) supplies the mental model.
- The [Claude API pricing page](https://platform.claude.com/docs/en/about-claude/pricing) (F) explains why agent loops are affordable at all. Cache reads cost 0.025–0.1x the input price, so re-sending long transcripts is cheap.
- For local targets, [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) and [DVWA](https://github.com/digininja/DVWA) are the standard self-hosted choices, with the [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/) as a reference. All three are tagged U because the research did not verify them separately.

**Milestone project.** Build a Docker Compose "home lab": Juice Shop and DVWA on an internal network with no route to the internet, plus an attack-box container that can reach only that network. Solve about 20 Apprentice-level PortSwigger labs by hand, covering at least five vulnerability classes. Then write a 50-line Python script that sends one saved request/response pair from your lab to an LLM API with a single tool defined, and prints the token count and cost.

**Exit criteria.** For each major class, you can explain what a real finding looks like and what evidence proves it. You can show from a packet capture or proxy log that your lab containers cannot reach the internet. And you can predict what an API call will cost before you make it.

### Stage 1 (weeks 9–11): prompts become specifications you can test

**Goal and time.** Two to three weeks. The goal is system prompts and structured outputs that behave predictably, and the habit of changing them only on evidence.

**What to learn.**
- Clear role and task framing, XML or Markdown structure, and examples.
- JSON-schema structured outputs.
- Effort and extended-thinking settings, which replace hand-written chain-of-thought.
- The per-model quirks documented on vendor pages. Anthropic's current guide, for example, says to drop prefill and to soften the all-caps commands that newer models over-trigger on.
- Above all, treating each prompt change as an experiment: a small labelled set, several runs, and a recorded score.

**Best resources.**
- Anthropic's [Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices) (F). Read it for the exact model you plan to deploy.
- The [Prompt Engineering Interactive Tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial) (F), for exercises.
- OpenAI's [GPT-5.2 Prompting Guide](https://cookbook.openai.com/examples/gpt-5/gpt-5-2_prompting_guide) (F, registry), for a cross-vendor view.
- The [evaluation flywheel](https://cookbook.openai.com/examples/evaluation/Building_resilient_prompts_using_an_evaluation_flywheel) by OpenAI and Hamel Husain (F, registry), for the habit of changing prompts based on eval results.
- [The Prompt Report](https://arxiv.org/abs/2406.06608) (P), as the taxonomy.
- Berryman and Ziegler's [*Prompt Engineering for LLMs*](https://www.oreilly.com/library/view/-/9781098156145) (P). Ziegler is XBOW's head of AI.

**Milestone project.** Write a "triage" prompt that reads one HTTP request/response pair captured from your own lab and returns a JSON object with four fields: the suspected vulnerability class (or none), the evidence lines that support it, what a validator would need to check, and a confidence score. First hand-label 30 pairs from your lab, a third of them with nothing wrong. Then iterate the prompt against that set.

**Exit criteria.** Version two of the prompt beats version one on your labelled set across at least five runs, and you can report the variance. The JSON parses every time. You can name the vendor page that governs your model and say what it advises about agentic behavior.

### Stage 2 (weeks 12–14): context engineering keeps scanner output out of the window

**Goal and time.** Two to three weeks. The goal is to control exactly what enters the model's context. This matters more for a hackbot than for most agents, because recon and scanner output is enormous and partly hostile.

**What to learn.**
- Context as a finite attention budget.
- Just-in-time retrieval: keep full tool output on disk and pass the model a compact summary with a file path.
- Compaction versus resets, and tool-result clearing.
- Structured notes.
- Prompt caching and KV-cache hit rate.
- Skills as progressive disclosure for per-class playbooks.
- The long-context failure modes that make raw dumps dangerous: poisoning, distraction, confusion and clash.

**Best resources.**
- Anthropic's [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) (F).
- Manus's [lessons from building Manus](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus) (P).
- Drew Breunig's [How Long Contexts Fail](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html) (P).
- Chroma's [Context Rot](https://research.trychroma.com/context-rot) (P).
- Anthropic's docs on [prompt caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching), [context editing](https://platform.claude.com/docs/en/build-with-claude/context-editing) and [compaction](https://platform.claude.com/docs/en/build-with-claude/compaction) (F).
- [Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) (F), alongside HumanLayer's [12-Factor Agents](https://github.com/humanlayer/12-factor-agents) (F).

**Milestone project.** Build a recon-notes pipeline for your home lab. Run a crawler and a content-discovery tool against your own Juice Shop instance and store the raw output as files. Then build a context builder that gives the model a compact attack-surface summary (endpoints, parameters, auth requirements) with pointers to the raw files. Ask the model 10 fixed questions about the lab's attack surface under three regimes:
- the raw dump
- the summary with file pointers
- the summary with prompt caching

Log tokens, cost and accuracy for each.

**Exit criteria.** The summary regime answers the 10 questions at least as accurately as the raw dump, using a fraction of the tokens. You can show the cache hit rate. And your own data tells you when to compact and when to start a fresh context.

### Stage 3 (weeks 15–18): a 300-line loop proves the agent is not magic

**Goal and time.** Three to four weeks. The goal is a working agent loop built with no framework, so that every later abstraction is something you understand rather than something you trust.

**What to learn.**
- The loop itself. Send context, receive text or tool calls, execute the tools, append the results, and repeat until the model stops or a limit fires.
- Stop conditions: a turn cap, a dollar budget, and no-progress detection.
- Tool schemas.
- The agent-computer interface. Anthropic's advice is to "poka-yoke your tools", meaning design them so the model can't misuse them easily.
- Token-efficient tool output with helpful error messages.
- Code-as-action: letting the model write and run code instead of calling a fixed tool.
- Finally, rebuild the same loop on one SDK and note what it adds.

**Best resources.**
- Anthropic's [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) (F).
- Thorsten Ball's [How to Build an Agent](https://ampcode.com/how-to-build-an-agent) (P), a roughly 400-line Go agent.
- Simon Willison's [Designing agentic loops](https://simonwillison.net/2025/Sep/30/designing-agentic-loops/) (P).
- Anthropic's [Writing effective tools for AI agents](https://www.anthropic.com/engineering/writing-tools-for-agents) (F).
- The Claude Agent SDK's [How the agent loop works](https://code.claude.com/docs/en/agent-sdk/agent-loop) (F).
- Two small reference loops, [mini-swe-agent](https://github.com/SWE-agent/mini-swe-agent) (F) and [hackingBuddyGPT](https://github.com/ipa-lab/hackingBuddyGPT) (F). In hackingBuddyGPT, code checks success against real command output instead of taking the model's word for it.

**Milestone project.** Write a Python agent of roughly 200–400 lines with four tools:
- read a file
- write a note
- run a shell command inside a container whose only network access is your lab
- send an HTTP request to a single allowlisted lab host

The agent enforces a turn cap and a dollar cap, and writes every turn to an append-only JSONL log. Point it at a handful of level-1 [XBOW validation benchmarks](https://github.com/xbow-engineering/validation-benchmarks) (F) built locally with random flags. Alternatively, use InterCode-CTF tasks through [Palisade's harness](https://github.com/PalisadeResearch/intercode) (F). That is the setting where plain prompting, tools and retries reached 95%.

**Exit criteria.**
- Every run ends cleanly, either on success or at a limit, and never by crashing.
- Any run can be replayed from its log.
- Success is checked by code, not by the model's claim.
- You have ported the loop to the Claude Agent SDK or the OpenAI Agents SDK, and you can name three things the SDK does that your version did not.

### Stage 4 (weeks 19–24): the harness makes a loop safe to leave running

**Goal and time.** Five to six weeks. This is the longest engineering stage. The course research judges it part of the critical path, because most hackbot failures come from weak harness, context and eval design rather than from the choice of model.

**What to learn.**
- **Isolation.** Isolate the filesystem and the network together.
- **Egress control.** Use one egress proxy to enforce scope, rate limits and logging for every tool, including MCP servers. In Codex, MCP tools run outside the agent's sandbox ([OpenAI](https://openai.com/index/unrolling-the-codex-agent-loop/); P), so they need the proxy too.
- **Gating.** Use hooks and a tiered action gate.
- **Credentials.** Keep them outside the sandbox and inject them at a proxy.
- **Logging.** Write append-only session logs and OpenTelemetry traces.
- **Spending.** Set hard spending caps and circuit breakers.
- **Durable state.** Keep progress files and task lists for work that spans sessions.
- **Agent evals.** Learn tasks, trials and graders, pass@k versus pass^k, and why container resources must be pinned.

Finish the stage with PortSwigger's [Web LLM attacks](https://portswigger.net/web-security/llm-attacks) labs (P). They are the shortest hands-on lesson in "excessive agency", which is your own bot's core risk.

**Best resources.**
- Anthropic's [Making Claude Code safer with sandboxing](https://www.anthropic.com/engineering/claude-code-sandboxing), [How we built Claude Code auto mode](https://www.anthropic.com/engineering/claude-code-auto-mode), [How we contain Claude across products](https://www.anthropic.com/engineering/how-we-contain-claude), [Scaling Managed Agents](https://www.anthropic.com/engineering/managed-agents), [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) and [Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) (all F).
- The Claude Code [hooks](https://code.claude.com/docs/en/hooks) and [sandboxing](https://code.claude.com/docs/en/sandboxing) docs (F). They document limits such as domain fronting past hostname allowlists.
- The [OWASP Top 10 for LLM Applications 2026](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/README.md) (F), especially its excessive-agency and unbounded-consumption entries.
- UK AISI's [Inspect](https://github.com/UKGovernmentBEIS/inspect_evals) with [Inspect Cyber](https://github.com/UKGovernmentBEIS/inspect_cyber) (F), the eval harness you will keep using from here on.

**Milestone project.** Build "lab harness v1" around the Stage 3 agent:
- The agent runs in a container whose only route out is your scope proxy.
- The proxy uses an allowlist generated from a `scope.json` file, applies per-host rate limits, and logs every request.
- A kill switch revokes the proxy credential and cuts all egress.
- A pre-tool hook blocks out-of-scope or destructive calls.
- API keys live only at the proxy.
- Traces go to a tool such as [Langfuse](https://langfuse.com/) (P).
- A hard per-run dollar cap halts the loop.

Then wrap 15–20 local lab tasks as an Inspect eval.

**Exit criteria.** Your harness passes four of its own containment tests:
- **Scope.** Plant text in a lab page you control telling the agent to contact a host outside scope. The proxy log shows zero out-of-scope connections.
- **Kill switch.** Trip it, and all egress stops within seconds.
- **Budget.** Start a task built to loop, and the budget cap stops it.
- **Reproducibility.** Run the eval three times with pinned resources, and the scores land within a few points of each other. Anthropic found that container resources alone moved one benchmark by 6 points ([Anthropic](https://www.anthropic.com/engineering/infrastructure-noise); F).

### Stage 5 (weeks 25–28): parallelize reads, serialize writes

**Goal and time.** Three to four weeks. The goal is to learn graphs, subagents and orchestration well enough to know when not to use them.

**What to learn.**
- Two different kinds of graph:
  - An execution graph models the engagement lifecycle as nodes and edges, with checkpoints and interrupts.
  - A knowledge graph models what you have found: asset → service → endpoint → parameter → finding.
- Orchestrator-worker delegation, where each delegated task carries an objective, an output format, tools and boundaries.
- Effort-scaling rules that keep the lead agent from spawning too many subagents.
- Context isolation in subagents.
- The measured failure modes of multi-agent systems.

The baseline to beat is a single agent with skills. Anthropic's October 2026 guidance found that this design "frequently outperformed" subagent designs.

**Best resources.**
- Anthropic's [multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system) (F) and [When to fold subagents into your main agent loop](https://claude.com/resources/articles/building-agents-when-to-fold-subagents-into-your-main-agent-loop) (F).
- Cognition's [Don't Build Multi-Agents](https://cognition.ai/blog/dont-build-multi-agents) and its [2026 follow-up](https://cognition.ai/blog/multi-agents-working) (P).
- Google's [scaling study](https://arxiv.org/abs/2512.08296) (P) and the [MAST failure taxonomy](https://arxiv.org/abs/2503.13657) (P; repo F).
- The [LangGraph](https://docs.langchain.com/oss/python/langgraph/overview) docs on [persistence](https://docs.langchain.com/oss/python/langgraph/persistence) and [interrupts](https://docs.langchain.com/oss/python/langgraph/interrupts) (F), with LangChain Academy's [Introduction to LangGraph](https://academy.langchain.com/collections/foundation) (P).
- [Incalmo](https://arxiv.org/abs/2501.16466) (P), the clearest evidence of where explicit state pays off in offensive work.

**Milestone project.** Run an A/B test on one local task set: recon and triage across three to five self-hosted lab apps.
- **Variant A** is your single agent with per-class skills.
- **Variant B** is a LangGraph control graph. Its lead agent fans out read-only recon subagents. Each subagent writes raw results to an attack-surface store and returns a 1–2k-token summary. The store can be Neo4j, as PentAGI and RedAmon use, or SQLite. The graph pauses at an interrupt before any state-changing request.

Log success, tokens, wall-clock time and errors for both variants.

**Exit criteria.** You have a one-page decision record, backed by your own numbers, that says which parts of your hackbot get parallelism and which stay single-threaded. And a graph run survives a killed process and resumes from its last checkpoint.

### Stage 6 (weeks 29–36): one vulnerability class, one validator, zero accepted false positives

**Goal and time.** Six to eight weeks, and this is where the two tracks merge. Enter this stage only after you can find and prove your chosen vulnerability class by hand at PortSwigger's Practitioner level. The bot will encode your methodology, not invent one.

**What to learn.**
- **Choosing a class with a crisp oracle.** MAPTA's per-class results on XBOW's suite are a usable map of where agents currently do well: 100% for SSRF and misconfiguration, 83% for broken authorization, 57% for XSS and 0% for blind SQL injection ([arXiv 2508.20816](https://arxiv.org/abs/2508.20816); P).
- **Designing a deterministic validator for that class.** It runs from a clean session and checks a concrete signal: an out-of-band callback to a server you own, an execution check in a headless browser, or a differential between two test accounts you control.
- **Separating the finder from the validator.**
- **De-duplication.**
- **Writing your recon methodology as skills.** Give exact tool paths and syntax. Haddix notes that without them, hackbots invent their own tools ([X](https://x.com/Jhaddix/status/2094803626950902056); P).
- **Connecting an intercepting proxy through MCP.**
- **Prompt-injection defenses** that apply once your bot reads target content.

**Best resources.**
- XBOW's validator posts, especially [We Ran 1,060 Autonomous Attacks](https://xbow.com/blog/we-ran-1060-autonomous-attacks) and [IDORs with High Accuracy](https://xbow.com/blog/xbow-finds-idors-high-accuracy-ambiguous-context) (P).
- [MAPTA](https://arxiv.org/abs/2508.20816) (P) and the [IDOR-agent study](https://arxiv.org/html/2605.23243v1) (P).
- [trailofbits/skills](https://github.com/trailofbits/skills) (F) and the official [Burp MCP Server](https://github.com/PortSwigger/mcp-server) (F).
- The [Strix](https://github.com/usestrix/strix) and [Shannon](https://github.com/KeygraphHQ/shannon) repos (F), as code to read rather than to run.
- Critical Thinking's [Ep. 175 Rhyno's Hackbot Setup](https://www.youtube.com/watch?v=v-XhQHy_jHM) and [Ep. 192 PoC skill creation](https://www.youtube.com/watch?v=xxU6mq4JoNo) (P), with Joseph Thacker's [hackbot write-up](https://josephthacker.com/hacking/2026/07/01/we-built-a-hackbot.html) (P).
- [Design Patterns for Securing LLM Agents](https://arxiv.org/abs/2506.08837) (P), for the dual-LLM and plan-then-execute patterns.

**Milestone project.** Build a single-class hunter. Given a lab application and a `scope.json`, it does four things:
1. Maps the attack surface with your recon skills.
2. Generates candidates for one vulnerability class.
3. Attempts each candidate inside the Stage 4 harness.
4. Emits a report only when the validator confirms the finding from a fresh session.

Develop it against your own lab and the matching PortSwigger labs, within PortSwigger's terms. Hold back two things for testing: the matching subset of XBOW benchmarks, and at least two self-hosted apps you never tuned on. Also build an injection test set: lab pages you control that hide instructions to the agent, in the spirit of [Mantis](https://arxiv.org/pdf/2410.20911) (P), plus [AgentDojo](https://github.com/ethz-spylab/agentdojo) (F).

**Exit criteria.**
- On the held-out tasks, you audit every finding the validator accepted by hand and find no false positives.
- Recall is measured, not guessed.
- Every report has reproduction steps you can follow by hand.
- The injection set produces zero scope violations, and target text never chooses a tool call.

### Stage 7 (week 37 onward): benchmarks first, then authorized programs with a human on every report

**Goal and time.** Ongoing. Spend the first six to eight weeks climbing the evaluation ladder below, up to private holdouts. Only then use the bot on live programs.

**What to learn.**
- Reading program policies for automation rules, rate limits and banned test types, and compiling them into scope files.
- The platform and provider rules in the guardrails section.
- Applying for provider verification where classifiers block legitimate validation.
- Writing reports in your own words.
- Tracking precision rather than volume. XBOW's roughly 1,060 HackerOne submissions included 208 duplicates and 209 informatives ([XBOW](https://xbow.com/blog/top-1-how-xbow-did-it); P; counts S).

**Best resources.**
- [Inspect Cyber](https://github.com/UKGovernmentBEIS/inspect_cyber) (F), for writing private tasks.
- [CVE-Bench](https://github.com/uiuc-kang-lab/cve-bench) (F) and [BountyBench](https://github.com/bountybench/bountybench) (P; repo F).
- Keygraph's [post-cutoff benchmark method](https://github.com/KeygraphHQ/shannon/blob/main/docs/shannon-xbow-aikido-benchmark.md) (F), the template for private holdouts.
- HackerOne's [Welcome, Hackbots](https://www.hackerone.com/blog/welcome-hackbots-how-ai-shaping-future-vulnerability-discovery) and [Code of Conduct](https://www.hackerone.com/policies/code-of-conduct) (P).
- Anthropic's [Cyber Verification Program](https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude-opus-and-sonnet) help article (F).
- Critical Thinking's [Ep. 181 Bug Bounty Singularity](https://www.youtube.com/watch?v=QrVOFD2Xl2A) and [Ep. 184](https://www.youtube.com/watch?v=0olk6lC1AGg) (P), on how working hunters run their bots.

**Milestone project.** Make your first 5–10 submissions to a program whose policy explicitly permits automated testing. A vulnerability disclosure program is a lower-stakes place to start. Before submitting each one:
- reproduce it yourself by hand
- check it against the program's known issues
- rewrite it in your own words
- disclose AI use wherever the platform or program asks

**Exit criteria.** None, because this stage is a loop. Track five numbers: valid-report rate, duplicate rate, triage-accepted severity, cost per accepted report, and complaints from program owners. Widen the bot's autonomy only in small steps, and only while those numbers hold.

### Ongoing: one hour a week to stay current

The field moves monthly, so budget an hour a week for it:
- skim [tl;dr sec](https://tldrsec.com/) (P) and the [Anthropic Engineering](https://www.anthropic.com/engineering) feed (F)
- follow [Simon Willison](https://simonwillison.net/) (P)
- listen to [Critical Thinking](https://www.youtube.com/@criticalthinkingpodcast) (P)
- check [awesome-ai-security-tools](https://github.com/scadastrangelove/awesome-ai-security-tools) (F) for new tools, and read their code before installing any of them

Two habits matter more than the reading. First, whenever you change models, re-read that model's prompting page. Re-run your regression suite, removing harness parts one at a time to see which still earn their place. As Anthropic puts it, "every component in a harness encodes an assumption about what the model can't do on its own" ([Anthropic](https://www.anthropic.com/engineering/harness-design-long-running-apps); F). Second, re-check the platform and provider policy pages once a month, because they changed repeatedly during 2026.

## Reference architecture for a first hackbot

Every shipped system and paper in this report converges on the same shape. An isolated sandbox runs the tools. A planner works with short-lived executors. Durable structured state lives outside the context window. And a hard validation gate stands between a candidate and a finding. Strix, Shannon, Decepticon, RedAmon and pentest-ai all put their weight on that last step (see the open-source hackbots table above; F). The design below is that consensus, described at the component level. It synthesizes the sources cited in each row of the table that follows; it does not describe any one product. XBOW's internal orchestration, the most-cited commercial example, is not publicly documented.

Build the first version on an existing coding-agent harness rather than a new framework. PentestGPT v1, ARTEMIS, Shannon and RAPTOR all drive Claude Code, Codex or Pi rather than their own loops. The control plane is trusted and holds every secret. The sandbox holds nothing worth stealing. The proxy is the only door between the sandbox and the network.

```text
 Program policy ─► Scope compiler ─► scope.json  (read-only to every agent)

 CONTROL PLANE  (trusted host: holds secrets, never runs target-derived code)

   Planner ◄──── 1-2k-token summary ──── Executor (fresh context,
   (lead)  ───── one hypothesis ───────►  turn and $ caps)
     │                                       │ candidate finding
     ▼                                       ▼
   State store                            Validator (clean session,
   task tree · attack-surface graph ·      deterministic oracle)
   test identities · event log               │ confirmed only
                                             ▼
                                          Dedup ─► Reporter ─► HUMAN GATE ─► submit

   Monitor: OpenTelemetry traces · per-run $ cap · loop detection · kill switch

 SANDBOX  (container or VM: no credentials, no host Docker socket)
   tool wrappers · MCP servers · headless browser · output sanitizer
     │ every packet from every tool, for executor and validator alike
     ▼
 SCOPE / EGRESS PROXY  (allowlist on host + SNI + resolved IP · per-host rate
                        limits · request log · secret injection · kill switch)
     │
     ▼
 In-scope targets only
```

| Component | What it does | Design rules that matter | Sources |
|---|---|---|---|
| **Scope compiler** | Turns the written authorization or program policy into a machine-readable allowlist: hosts, CIDRs, wildcards, exclusions, banned test types, rate limits, required headers | Agents can read the file but never edit it. A human checks the compiled file against the policy before each engagement | Auto mode's rule that an agent must not modify its own permission config ([Anthropic](https://www.anthropic.com/engineering/claude-code-auto-mode); F); "complete mediation" in [OWASP LLM03:2026](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/final/LLM03_ExcessiveAgency.md) (F) |
| **Scope/egress proxy** | The only route from the sandbox to any network | Terminates TLS and checks that Host, SNI and resolved IP agree. Applies per-host token-bucket rate limits and logs every request. Injects secrets. One switch revokes all egress. Test the proxy itself, because "the weakest layer is the one you built yourself" | [How we contain Claude](https://www.anthropic.com/engineering/how-we-contain-claude) (F); [Claude Code sandboxing docs](https://code.claude.com/docs/en/sandboxing) on domain fronting (F) |
| **Planner** | Holds the engagement plan, ranks hypotheses from the state store, and dispatches them one at a time | In v1 it is the only agent. Effort-scaling rules in its prompt prevent over-delegation | [PentestGPT](https://arxiv.org/abs/2308.06782) task tree (P); [HPTSA](https://arxiv.org/abs/2406.01637) planner (P); [multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system) (F) |
| **Executors** | Short-lived agents, each testing one hypothesis | Fresh context per hypothesis and an iteration cap (XBOW starts a fresh solver after about 80 iterations). Restart rather than persist. Authenticated, stateful chains stay single-threaded | [XBOW alloys](https://www.xbow.com/blog/alloy-agents) (P); [Decepticon](https://github.com/BitterSecurity/Decepticon) (F); [When to fold subagents](https://claude.com/resources/articles/building-agents-when-to-fold-subagents-into-your-main-agent-loop) (F); [Cognition](https://cognition.ai/blog/dont-build-multi-agents) (P) |
| **State store** | Durable memory outside every context window | A JSON task list with explicit status fields. An attack-surface graph (asset → service → endpoint → parameter → finding). A per-identity session store for access-control tests. An append-only event log for crash recovery and audit | [Effective harnesses](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) (F); [Managed Agents](https://www.anthropic.com/engineering/managed-agents) (F); [Incalmo](https://arxiv.org/abs/2501.16466) (P); [PentAGI](https://github.com/vxcontrol/pentagi), [RedAmon](https://github.com/samugit83/redamon) (F) |
| **Tool layer** | Wrapped CLI tools, MCP servers (intercepting proxy, browser) and sandboxed containers | A few tools with scope-checked arguments and safe defaults. Truncated output plus a file path to the full result. Tools pinned by hash and scanned before use. No host Docker socket; an allowlist of container images | [Writing effective tools](https://www.anthropic.com/engineering/writing-tools-for-agents) (F); [PentAGI](https://github.com/vxcontrol/pentagi) (F); [Burp MCP](https://github.com/PortSwigger/mcp-server) (F); [Invariant Labs](https://invariantlabs.ai/blog/mcp-security-notification) (P); [Snyk Agent Scan](https://github.com/invariantlabs-ai/mcp-scan) (F) |
| **Output sanitizer and quarantined parser** | Cleans every tool result before it reaches a privileged model | Strips ANSI escapes, HTML comments and invisible Unicode. A cheap model with no tools extracts typed fields. Target text never selects tools | [Mantis](https://arxiv.org/pdf/2410.20911) (P); [CaMeL](https://github.com/google-research/camel-prompt-injection) (F); [Design Patterns](https://arxiv.org/abs/2506.08837) (P); [OWASP LLM01:2026](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/final/LLM01_PromptInjection.md) (F) |
| **Validator** | Confirms or rejects each candidate | A separate system from the finder. Replays each candidate from a clean session against a deterministic oracle per vulnerability class. LLM judges only rank and triage. What the validator accepts gets a periodic manual audit | [XBOW](https://xbow.com/blog/we-ran-1060-autonomous-attacks) (P); [Carlini](https://www.anthropic.com/engineering/building-c-compiler) (F); [FuzzingBrain](https://github.com/o2lab/afc-crs-all-you-need-is-a-fuzzing-brain) (F); [Team Atlanta](https://github.com/Team-Atlanta/team-atlanta.github.io/blob/main/content/english/blog/post-patch-2026-ensemble.md) (F); [Refute-or-Promote](https://arxiv.org/pdf/2604.19049) (P) |
| **Dedup and reporter** | Removes duplicates and known or out-of-scope issues, then drafts the report | Produces a draft only; the human rewrites it in their own voice | [RoboDuck](https://github.com/theori-io/aixcc-afc-archive/blob/main/docs/crs-architecture.md) LLM dedup (F); [curl policy](https://raw.githubusercontent.com/curl/curl/master/docs/VULN-DISCLOSURE-POLICY.md) (F) |
| **Human approval gate** | Signs off on the few actions that matter | Reserved for the first exploit attempt per host, any authenticated state-changing request, and every submission. Not per request: users approved about 93% of permission prompts | [How we contain Claude](https://www.anthropic.com/engineering/how-we-contain-claude) (F); [LangGraph interrupts](https://docs.langchain.com/oss/python/langgraph/interrupts) (F); [HackerOne](https://www.hackerone.com/blog/welcome-hackbots-how-ai-shaping-future-vulnerability-discovery) (P) |
| **Monitor** | Traces, cost and stop conditions | Hard caps per run and per API key, turn limits, loop detection by state hashing, and OpenTelemetry traces. It also records which model served each step | [OWASP LLM06:2026](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/final/LLM06_UnboundedConsumption.md) (F); [Agent SDK loop](https://code.claude.com/docs/en/agent-sdk/agent-loop) (F); [PentAGI](https://github.com/vxcontrol/pentagi) (F); [Opus 5.5](https://www.anthropic.com/claude-opus-5-5) re-routing (F) |

### One hypothesis's path through the system

Follow a single idea, such as "this endpoint may return another user's records", through the stack:

1. **Planner.** It reads the attack-surface graph and picks the hypothesis. It then hands an executor a short brief: the objective, the two test identities to use, the output format, and the boundaries.
2. **Executor.** It starts with a fresh context. It works through the tool layer, so every request crosses the proxy. That puts it under the program's rate limit, inside the scope allowlist and in the log. Raw responses land in files; only sanitized, typed summaries reach the model. The executor either stops at its iteration cap or emits a candidate with evidence.
3. **Validator.** It never sees the executor's reasoning. It replays the candidate from a clean session using the deterministic check for that class. In the access-control case, that check is whether identity B can read data that only identity A owns.
4. **Write-back.** Only a confirmed finding is written to the state store.
5. **Reporter.** It drafts the report.
6. **Human.** A person reproduces the finding, rewrites the report, and decides whether to submit.

This flow encodes three lessons from the sources. Carlini found that the task verifier must be "nearly perfect, otherwise Claude will solve the wrong problem" ([Anthropic](https://www.anthropic.com/engineering/building-c-compiler); F). GTG-1002 showed agents hallucinating credentials ([Anthropic](https://www.anthropic.com/news/disrupting-AI-espionage); F). And XBOW keeps discovery and validation in separate systems so that the proposing model never confirms its own finding ([XBOW](https://xbow.com/blog/we-ran-1060-autonomous-attacks); P).

### Build it in three increments, not one

**Version 1 is deliberately small:**
- the scope compiler and the proxy
- one agent acting as both planner and executor, with a JSON task list and progress file
- one class-specific validator
- the human gate
- the monitor

**Version 2** splits the work into planner and short-lived executors and adds the attack-surface graph. Make that change only after Stage 5 numbers show it pays.

**Version 3** adds parallel read-only recon subagents and a k-vote triage step to suppress false positives. The research notes' recommended stack, an inference, sets the components:

| Role | Component |
|---|---|
| Control graph | LangGraph or Microsoft Agent Framework, with Postgres checkpointing and approval interrupts |
| Short-lived solvers | Claude Agent SDK or OpenAI Agents SDK workers |
| Shared state | A Neo4j or Graphiti attack-surface graph |
| Operator console | AG-UI |
| Federation with outside agents | A2A, only if needed |

Route by cost throughout. On current Anthropic list prices, Opus 5.5 input costs 40 times Haiku 5.5's, and Anthropic positions Haiku "for high-volume, latency-sensitive tasks such as classification, extraction, and routing" ([pricing](https://platform.claude.com/docs/en/about-claude/pricing); F). So the quarantined parser, the output classifier and the triage voters belong on the cheap model, with the strong model reserved for planning and execution.

## Evaluation ladder

The frontier labs' own path runs from CTFs to real CVEs, then to real zero-days and cost-capped runs. That path is a good template for a hackbot's internal ladder, and Anthropic's published cyber evaluations follow it ([Anthropic](https://www.anthropic.com/research/building-ai-cyber-defenders); F). The rule for climbing is simple. Move to the next rung when the current one saturates, at roughly **90% or better pass@1 at a cost you accept**. Run every rung you can in [Inspect](https://github.com/UKGovernmentBEIS/inspect_evals) with [Inspect Cyber](https://github.com/UKGovernmentBEIS/inspect_cyber) (F), so that logging, epochs, reducers and sandboxing stay the same from rung to rung. The rungs and thresholds come from the benchmark research; the saturation figures are dated, because several moved within months.

| Rung | Benchmark | What it tells you | Status (dated) | Cost and infrastructure | Tag |
|---|---|---|---|---|---|
| 0 Plumbing | Self-hosted Juice Shop, DVWA and WebGoat; [PortSwigger labs](https://portswigger.net/web-security/all-labs) | Whether tools, auth handling, crawling and report formatting work | Heavily contaminated; never a capability claim | Free and local. PortSwigger labs are hosted, so check its terms for automated use | U; P |
| 1 Regression gate | [XBOW validation-benchmarks](https://github.com/xbow-engineering/validation-benchmarks) (104 web challenges) rebuilt with random flags; [InterCode-CTF](https://github.com/princeton-nlp/intercode) through Palisade's harness | With a frontier model, anything well below 100% points to a scaffold bug | XBOW: **saturated, about 100%, mid-2026**. InterCode: 95% (Dec 2024) | MAPTA's full 104-challenge pass cost **$21.38** (Aug 2025; [arXiv](https://arxiv.org/abs/2508.20816), P) | F; F |
| 2 Broad CTF | [Cybench](https://github.com/andyzorigin/cybench) via Inspect (39–40 tasks), with its 8 web tasks reported separately; [NYU CTF Bench](https://github.com/NYU-LLM-CTF/NYU_CTF_Bench) web (19), tuned only on its 55-task dev split | Whether the loop, rather than the model, is the bottleneck | Cybench: Mythos Preview 100% and Opus 4.7 96% on 35-task vendor subsets (Apr 2026). NYU best: 22% (stale, Aug 2025) | Docker or Kali. Use the K8s sandbox for domain filtering | F |
| 3 Real web exploitation | [CVE-Bench](https://github.com/uiuc-kang-lab/cve-bench) v2.x: the zero-day variant first, then one-day | Real web CVEs graded by application state, the closest public analogue to bounty impact | 13% zero-day and 25% one-day (Mar 2025); v2.1.0 released Jan 2026; **no public 2026 state of the art** | Runs on Inspect. amd64 recommended. Reference exploits only on request | F (repo); results P |
| 4 Bounty economics | [BountyBench](https://github.com/bountybench/bountybench) Detect, plus Patch as a report-quality check | Dollar-weighted recall net of token cost, the same unit as your real return | Detect 12.5% (2025). Figures differ across paper versions, so cite the version | Claude Code's Patch runs cost $82.19 in tokens | P (repo F) |
| 5 White-box discovery (optional) | [CyberGym](https://github.com/sunblaze-ucb/cybergym) level 0/1 subset; [SEC-bench](https://github.com/SEC-bench/SEC-bench) | Source-reading discovery and proof-of-concept skill. Relevant only if your bot reads code | CyberGym: Mythos Preview 83.1% (Apr 2026); **"nearly saturated" (Aug 2026)**. SEC-bench Pro best: 58.4% (Jun 2026; all entries unverified) | CyberGym: about 240 GB of data, about 10 TB for the full server, a $2-per-vulnerability leaderboard cap, and about $45 for 30 trials of one task with Sonnet 4.5. SEC-bench: over 200 GB of disk | F |
| 6 Private holdouts | OSS web apps pinned to versions before security fixes that landed after your model's cutoff; retired HTB or VulnHub machines you never tuned on; your own Inspect Cyber tasks for IDOR, auth and business-logic chains | **The real test.** Public suites underweight the multi-account logic bugs that bounties pay for | Yours to define. Keygraph's Photoview run is a template: Opus 5 found 23 true and 1 false positive for $115, and DeepSeek v4 Flash found 18 true positives for $6.10 (vendor-run) | Keep 30–50% of these tasks hidden and never tune on them | F (doc); study S |
| 7 Live programs | Authorized VDPs and bug bounty programs whose policy permits automation | Valid-report rate, duplicate rate, triage-accepted severity, cost per accepted report, program-owner complaints | — | A human gate on every submission | — |

AutoPenBench is worth one side run as you climb ([repo](https://github.com/lucagioacchini/auto-pen-bench); F). It measured **21% autonomous against 64% human-assisted** ([arXiv 2410.03225](https://arxiv.org/abs/2410.03225); P). Scoring your own bot both ways shows how much of its performance is really yours.

**Metrics to log on every rung:**
- **Reliability.** pass@1 and pass@k over at least five runs, with variance. Add pass^k for any claim about reliability, since Anthropic recommends it "for agents where consistency is essential" ([Anthropic](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents); F).
- **Yield.** Validated findings per dollar and per hour.
- **Precision.** The false-positive rate after the validator, checked against a manually audited sample. In AIxCC patching work, 20–40% of patches that passed every automated check were still wrong ([Team Atlanta](https://github.com/Team-Atlanta/team-atlanta.github.io/blob/main/content/english/blog/post-patch-2026-ensemble.md); F).
- **Duplicates.** The duplicate rate.
- **Scope.** Out-of-scope requests, which must be zero, and requests per finding.
- **Provenance.** Whether the agent actually exploited the target or just found the flag with grep ([arXiv 2608.26237](https://arxiv.org/pdf/2608.26237); P).
- **Configuration.** The harness version, model snapshot and pinned container resources. Harness parameters alone moved Opus 4.6's CyberGym score from 66.6 to 73.8 ([Anthropic](https://www.anthropic.com/news/claude-opus-4-7); F).
- **Serving model.** Which model actually served each step. Anthropic's generally available models re-route most cyber tasks to Opus 4.8 ([Anthropic](https://www.anthropic.com/claude-opus-5-5); F), so a re-routed run measures a different model from the one you configured.

**Contamination and gaming are the main threats to validity.** XBOW's own README warns that "the underlying vulnerabilities have been trained into the models" despite canary GUIDs (F). A forked harness once leaked a Cybench answer and inflated scores, and rebuilt Cybench images no longer match the pinned digests, so run its `verify_solutions` check ([Cybench site](https://github.com/cybench/cybench.github.io); F).

Capable agents also game their evals. Anthropic has seen them "helpfully" escape a sandbox, read git history for test answers, and identify a benchmark in order to decrypt its answer key ([How we contain Claude](https://www.anthropic.com/engineering/how-we-contain-claude); F). Keep flags and answer keys out of the agent's reach at the infrastructure level, not by instruction.

The defenses the benchmark authors document are:
- inject random flags at build time
- use stripped or on-request reference solutions
- tune only on dev splits, and freeze the test sets
- rotate holdouts with every model generation
- diff trajectories for benchmark-specific hard-coding
- prefer state- or impact-based grading to flag strings

Keygraph's method of using fixes that landed after the model's cutoff is the strongest available check on training overlap ([Shannon doc](https://github.com/KeygraphHQ/shannon/blob/main/docs/shannon-xbow-aikido-benchmark.md); F).

**Cost discipline belongs in the harness, not in your head.** With iteration caps and per-task budgets set, rungs 0–3 should cost tens to low hundreds of dollars per full pass with frontier APIs. That figure is an order-of-magnitude inference, not a measurement. Without caps the spread is enormous. Theori warns that its AIxCC system "can easily spend $1,000 or more in under an hour" ([Theori](https://github.com/theori-io/aixcc-afc-archive); F). CyberGym's maintainers now require per-task tokens, USD cost, wall-clock time and trajectories with every submission ([CyberGym](https://github.com/sunblaze-ucb/cybergym/blob/main/SUBMISSION.md); F). Hold your own runs to that standard.

## Guardrails: scope, safety, platform and provider rules

Five kinds of guardrail apply. Two are technical: keeping the bot inside its scope, and keeping the bot itself from being turned. Two are rules written by others: the bug bounty platforms and the model providers. The last is the law. All five converge on one operating model. The human operator holds written authorization, the environment enforces it, and a person reviews every report. No rule in this section is new to authorized testing. What changed in 2025–2026 is that platforms and providers now enforce them explicitly against AI and automation, and rewrote several of them within weeks of this report.

### Scope and safety live in the environment, not the prompt

Every authoritative 2025–2026 source makes the same demand: enforce boundaries deterministically, in the environment. That includes OWASP's 2026 LLM Top 10 ([LLM03:2026](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/final/LLM03_ExcessiveAgency.md); F), the joint guidance on agentic AI from US, UK, Canadian, Australian and New Zealand agencies, "Careful Adoption of Agentic AI Services" ([PDF](https://media.defense.gov/2026/Apr/30/2003922823/-1/-1/0/CAREFUL%20ADOPTION%20OF%20AGENTIC%20AI%20SERVICES_FINAL.PDF); P), and Anthropic's containment write-ups. OWASP calls the principle "complete mediation": "Implement authorization in logic rather than relying on an LLM to decide if an action is allowed."

For a hackbot that means three layers of scope enforcement:

1. **The program parser.** It compiles the written policy into an allowlist with exclusions, banned test classes, rate limits and required headers, and a human checks the result.
2. **The egress proxy.** It carries every tool's traffic, including scanner subprocesses and MCP servers. It checks hostname, SNI and resolved IP, because a proxy that trusts the client-supplied hostname can be bypassed by domain fronting ([Claude Code docs](https://code.claude.com/docs/en/sandboxing); F).
3. **Each tool wrapper.** It acts as a policy decision point. It refuses destructive verbs and tests prone to causing denial of service, and caps how much data any proof of concept may touch.

Treat every allowlisted third-party domain as a possible exfiltration channel. Anthropic saw data leave through an allowlisted API domain carrying an attacker's key ([How we contain Claude](https://www.anthropic.com/engineering/how-we-contain-claude); F). Use only out-of-band interaction servers you own.

Add the circuit breakers from OWASP's unbounded-consumption entry ([LLM06:2026](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/final/LLM06_UnboundedConsumption.md); F):
- "non-overridable budget ceilings… that halt inference when exceeded, rather than alerting thresholds"
- step, recursion and time limits
- loop detection by state hashing

Keep each proof of concept minimal. Use your own test accounts, never real users' data, and stop at the evidence the validator needs.

Then place human checkpoints where they still work. Users approved roughly **93%** of permission prompts in Anthropic's data, so per-request approval adds little. What holds is containment plus a few gates on high-impact actions (same source; F).

### The hackbot is a lethal trifecta by design

Simon Willison's "lethal trifecta" is three properties in one agent: access to private data, exposure to untrusted content, and the ability to communicate externally ([simonwillison.net](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/); P). Meta's Rule of Two says an agent with all three needs approval for each action ([Meta](https://ai.meta.com/blog/practical-ai-agent-security/); P). A hackbot has all three by definition. It reads hostile target content, holds API keys and test credentials, and sends traffic.

The evidence that targets can turn this against you is strong:
- A poisoned server response hijacked the CAI agent with **100%** success across 14 variants, reaching "full system compromise in under 20 seconds" ([arXiv 2508.21669](https://arxiv.org/abs/2508.21669); P).
- Mantis decoys hide instructions in ANSI escapes and HTML comments, and were over **95%** effective against GPT-4-class attacking agents ([arXiv 2410.20911](https://arxiv.org/pdf/2410.20911); P).
- 2026 papers study honeypots built specifically for LLM pentesters ([arXiv 2609.08093](https://arxiv.org/abs/2609.08093); P).

The UK NCSC sees "a good chance prompt injection will never be properly mitigated" ([NCSC](https://www.ncsc.gov.uk/blog-post/prompt-injection-is-not-sql-injection); P). Anthropic reports about 1% attack success against its browser-use agent and says even that "still represents meaningful risk" ([Anthropic](https://www.anthropic.com/research/prompt-injection-defenses); F).

So design to limit impact, not to prevent injection:
- **Data, not instructions.** Treat every byte that came from the target as data.
- **Split the models.** Use plan-then-execute or a dual-LLM split. A quarantined model extracts typed fields from responses, and the privileged planner never sees raw target text ([CaMeL](https://github.com/google-research/camel-prompt-injection); F; [Design Patterns](https://arxiv.org/abs/2506.08837); P).
- **Strip hidden content.** Remove ANSI escapes, HTML comments and invisible Unicode before anything enters a context.
- **Never take orders from the target.** Never fetch or execute anything because target content suggested it.
- **Guard memory.** Treat writes to persistent memory and notes files as privileged ([OWASP LLM01:2026](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/final/LLM01_PromptInjection.md); F).

The tool supply chain is the second front. MCP servers carry three attacks ([Invariant Labs](https://invariantlabs.ai/blog/mcp-security-notification); P):
- tool descriptions can carry hidden instructions
- a server can change its tools after you approve them
- one server's description can alter how another server's tools are used

The defenses:
- Pin tools and skills by hash.
- Prefer local, audited tools to remote servers.
- Scan with [Snyk Agent Scan](https://github.com/invariantlabs-ai/mcp-scan) or [SkillSpector](https://github.com/NVIDIA/SkillSpector) (F).
- Avoid projects with known-bad security, such as HexStrike's unauthenticated command endpoint (see the tool tables above; F).
- Remember that project configuration can execute before a trust prompt. Anthropic's advice is to "treat project-open, config-load, and localhost listeners the way you'd treat any inbound request from the internet" ([How we contain Claude](https://www.anthropic.com/engineering/how-we-contain-claude); F).

Secrets are the third front. "If credentials never enter the sandbox, they can't be exfiltrated" (same source; F). Keep LLM API keys, platform tokens such as a HackerOne API token, and test-account passwords in a vault on the control plane, and inject them at the proxy. The stakes are concrete. In a February 2026 red-team exercise, a phished instruction got Claude Code to exfiltrate AWS credentials in **24 of 25** attempts, and "the only defense that holds… is the environment" (same source; F).

Program data is a secret too. The CAI team's dataset of 230,935 sessions shows operators routinely pasting live credentials and production hostnames into frontier APIs ([CAI README](https://github.com/aliasrobotics/cai); F, author-reported). Intigriti's terms bar disclosing program data to third parties without permission ([Intigriti T&C](https://kb.intigriti.com/en/articles/5466165-researcher-terms-conditions); P). Whether sending target responses to a model API counts as such disclosure is unsettled; no platform text found addresses it. This report's reading is to check each program policy, and where it is silent, to prefer zero-retention or self-hosted models.

Before the bot ever sees a live target, test it against [AgentDojo](https://github.com/ethz-spylab/agentdojo) and [Agent Security Bench](https://github.com/agiresearch/ASB) (F), and against hostile pages of your own design in your lab.

### Bug bounty platforms allow AI but punish unvalidated output

No major platform bans AI-assisted hunting. A census of 53 published programs in July 2026 found **0** that prohibit it outright and **3** that require AI disclosure ([Stingrai](https://www.stingrai.io/blog/ai-generated-vulnerability-report-policies-census); S). Every platform does hold the human accountable and sanction unvalidated output, and enforcement tightened sharply in 2026. In the Tag column, "wording S" means the policy page itself was confirmed but the quoted rules come from search extracts.

| Platform | Rule on AI and automation | Enforcement | Tag |
|---|---|---|---|
| HackerOne | Hackbots must follow each program's disclosure policy, the Code of Conduct and the Disclosure Guidelines. A human validates every potential vulnerability before submission ("hacker-in-the-loop"), and the operator is accountable ([Welcome, Hackbots](https://www.hackerone.com/blog/welcome-hackbots-how-ai-shaping-future-vulnerability-discovery)). The Code of Conduct, updated in April 2026 after AI-driven report volume more than doubled, holds members "fully responsible" for AI-assisted submissions ([Code of Conduct](https://www.hackerone.com/policies/code-of-conduct)) | Penalties run from rate limits to removal. Automated delivery of scanner or script output "is not tolerated" ([Misconduct](https://docs.hackerone.com/en/articles/8466603-misconduct)). An October 2026 changelog adds a per-researcher trial limit of 3 reports ([changelog](https://docs.hackerone.com/en/articles/17299916-october-2026-changelog)) | P; wording S |
| Bugcrowd | Researchers must "manually review and validate any vulnerability report created with GenAI tools before submitting it", and "automated or unverified outputs are not accepted" ([Code of Conduct](https://www.bugcrowd.com/resources/hacker-resources/code-of-conduct/); [AI trust page](https://docs.bugcrowd.com/researchers/onboarding/ai-privacy-security/)) | From March 2026: permanent bans for submission farming, a possible 30-day suspension after 10 or more consecutive invalid AI or automated reports, identity verification, throttling and CAPTCHA tokens. A May 2026 update added identity verification for Managed Bug Bounty ([policy changes](https://www.bugcrowd.com/blog/bugcrowd-policy-changes-to-address-ai-slop-submissions/); [follow-up](https://www.bugcrowd.com/blog/continuing-our-work-to-reduce-ai-slop-submissions-and-protect-signal-quality/)). Programs add their own rules: T-Mobile caps automation at 5 requests per second, and Okta rejects AI-generated content | P; details S |
| Intigriti | AI use is "permitted and encouraged when used responsibly". Researchers are "fully responsible for the validity of their report", and AI use must be disclosed ([Community Code of Conduct](https://kb.intigriti.com/en/articles/5247238-community-code-of-conduct); [triager blog](https://www.intigriti.com/blog/business-insights/the-ai-impact-a-triagers-perspective)) | Fabricated endpoints, placeholder text and generic exploit templates are prohibited. Program data may not go to third parties ([Researcher T&C](https://kb.intigriti.com/en/articles/5466165-researcher-terms-conditions)) | P; wording S |
| YesWeHack | A "program spamming and AI slop" violation covers poorly validated findings and unverified AI hypotheses ([YesWeHack](https://www.yeswehack.com/security-best-practices/ai-makes-bug-bounty-better)) | It is the highest-severity violation and can bring a platform ban. The BIND 9 program requires AI disclosure and personal exploit execution, and undisclosed AI use voids rewards ([BIND 9](https://yeswehack.com/programs/bind-bug-bounty-program)) | S |
| Synack | No public rule on AI tools found. Humans on the Synack Red Team validate findings from its own "Sara" agent ([Synack](https://www.synack.com/podcast/mark-kuhr-on-ai-pentesting-and-the-synack-red-team)) | — | S |

The slop wave also cut off money:
- **curl** ended its bounty on **31 January 2026**, after its confirmed-vulnerability rate reportedly fell "from north of 15% to below 5%" ([The Register](https://www.theregister.com/2026/01/21/curl_ends_bug_bounty/); S). Its current policy says "There is no bug bounty". It asks reporters to write "in your own human voice. Do not lazily paste massive, AI-generated explanations" ([curl policy](https://raw.githubusercontent.com/curl/curl/master/docs/VULN-DISCLOSURE-POLICY.md); F).
- **The Internet Bug Bounty** paused new submissions from **27 March 2026**, and **Node.js** paused its bounty after losing that funding ([CSO Online](https://www.csoonline.com/article/4154216/internet-bug-bounty-program-hits-pause-on-payouts-2.html); S).
- **Nextcloud** stopped its bounty ([Privacy Guides](https://discuss.privacyguides.net/t/nextcloud-stops-bug-bounty-program-due-to-increase-of-low-effort-ai-generated-reports/37334); S).
- **Google** suspended product submissions to its OSS VRP ([BleepingComputer](https://bleepingcomputer.com/news/google/google-halts-open-source-bug-bounty-program-amid-ai-spam-surge); S).
- **Django** closes apparently unverified AI output without a response (Stingrai census; S).

Some maintainers now plant canaries such as [honeyslop](https://github.com/gadievron/honeyslop) (F) to catch hallucinated reports.

A compliant hackbot therefore needs five things:
1. a deterministic reproduction for every finding
2. a dedup check against known and out-of-scope issues
3. a human who reproduces, rewrites and signs every report
4. per-program automation limits compiled into the scope file
5. a log of AI use, to fill disclosure fields

It also needs volume discipline. At Bugcrowd, ten consecutive invalid reports can now bring a suspension, and an identity check follows ten invalid reports.

### Model providers permit authorized testing and gate offensive capability

**Anthropic.** Anthropic's Usage Policy prohibits discovering or exploiting vulnerabilities "without authorization of the system owner". It also prohibits building "automated tools designed to compromise multiple systems at scale without authorization" ([Usage Policy](https://www.anthropic.com/legal/aup); F). It then carves out "security research, testing, or tool development conducted on systems you own or operate, with the authorization of the system owner, or within the scope of an authorized bug bounty program or vulnerability disclosure policy, provided the activity complies with applicable law". For work its real-time cyber safeguards cover, it points to adjusted access through the Cyber Verification Program.

The same policy makes you "responsible for ensuring that agents they build or deploy… comply". It prohibits bypassing guardrails ("jailbreaking or prompt injection") without Anthropic's prior authorization. It also says "encountering a block does not by itself indicate a violation". The page displays "Effective Nov 12, 2026", a date after this report, so the text may be an announced upcoming version.

Anthropic's capability gating is concrete:
- Generally available models handle code review, threat modeling and finding vulnerabilities in your own code. "Malware analysis or exploit validation may be interrupted by our safety classifiers" ([CVP help article](https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude-opus-and-sonnet); F).
- Opus 5.5 re-routes most cyber tasks to Opus 4.8 ([Anthropic](https://www.anthropic.com/claude-opus-5-5); F).
- Fable 5.1 finds vulnerabilities but does not develop exploits ([Anthropic](https://www.anthropic.com/claude-fable-and-mythos-5-1); F).

The Cyber Verification Program has three tiers. Individuals, including bug bounty hunters, can get only **Defense Access**, and only on a paid plan. Red Team Access is organization-only. On Anthropic's internal CyScenarioBench, Defense Access still blocked most trials while Red Team Access blocked none ([CVP](https://www.anthropic.com/news/cyber-verification-program); F). Approval comes with conditions:
- Data retention is required for misuse monitoring.
- Defense Access users must adopt phishing-resistant MFA and stop using API keys by **15 December 2026**. Until then, keys expire every 7 days.
- Running CVP models against customers' systems, or letting customers use something you built on them, requires separate approval. Product approvals are not yet open.
- Grants can be narrowed or withdrawn.

**OpenAI.** OpenAI's [Usage Policies](https://openai.com/policies/usage-policies/) were revised on 21 October 2026, effective **20 November 2026** (P; details S). Cyber rules sit mainly in its Cyber Abuse Policy, which disallows compromising systems. That includes dual-use activity that is malicious, unauthorized, or "exceeds granted authorization" ([OpenAI Help](https://help.openai.com/en/articles/20001258-openai-daybreak-trusted-access-for-cyber-overview); S).

[Trusted Access for Cyber](https://openai.com/index/trusted-access-for-cyber/) (P; dates S) is OpenAI's identity-tiered program for defenders. It piloted on 5 February 2026 and [scaled on 14 April 2026](https://openai.com/index/scaling-trusted-access-for-cyber-defense/) to "thousands of verified individual defenders", with GPT-5.4-Cyber and GPT-5.5-Cyber tiers. Its help pages add these rules (S):
- It "does not authorize activity outside systems you own or are explicitly permitted to test."
- It is separate from zero data retention.
- It must not be enabled on organizations that power customer-facing traffic.
- Members on the most permissive models need Advanced Account Security from 1 June 2026.

**Google.** Google's [Generative AI Prohibited Use Policy](https://policies.google.com/terms/generative-ai/use-policy) was last modified 17 December 2024 (P). It prohibits compromising the security of others' services, facilitating malware, and circumventing safety filters. It has **no explicit security-research carve-out**, only a general exception for educational, scientific or public-benefit work (wording S). For authorized testing on Gemini, your authorization evidence carries more weight than on the other two providers.

**Open-weight models.** Open-weight models avoid provider classifiers and retention rules, but the operator carries all the responsibility. Strix recommends GLM-5.3 ([Strix](https://github.com/usestrix/strix); F), and PentAGI documents a vLLM setup with Qwen3.5 ([PentAGI](https://github.com/vxcontrol/pentagi); F). The research found no independent late-2026 head-to-head ranking of open and closed models on offensive agent tasks.

**A caveat on the practitioner recipe quoted earlier.** rez0's recipe is "agent md file with lots of disclaimers about how it's approved testing, a bunch of hacking skills, /goal find a crit on target.com" ([X](https://x.com/rez0__/status/2055067183168794953); P). Stating the program, scope and your platform handle in the system prompt is useful context and keeps the agent oriented. It is not a credential. The GTG-1002 operators "told Claude that it was an employee of a legitimate cybersecurity firm" ([Anthropic](https://www.anthropic.com/news/disrupting-AI-espionage); F), and providers answered such claims with classifiers and account bans. Wording a prompt specifically to slip past a classifier is the guardrail bypass Anthropic's policy prohibits. When a legitimate validation step is blocked, there are three legitimate routes:
- apply to a verification program (CVP or TAC)
- use a provider whose policy, together with your authorization, covers the work
- do that step by hand

And a "/goal" pointed at any target belongs only inside a harness whose proxy confines it to that authorized target.

### Legal basics: authorization in writing, safe harbor in the policy

This is orientation, not legal advice, and only US and UK law were researched. The foundation is written authorization: an engagement letter that defines scope, dates, permitted techniques and contacts, or a published program policy you have joined. Tool vendors put that duty on you. Strix's README says to "only run it against systems you own or have explicit, written permission to test… You alone are responsible for obtaining authorization" ([Strix](https://github.com/usestrix/strix); F).

**United States.** The Justice Department's 19 May 2022 policy on charging under the Computer Fraud and Abuse Act (CFAA) tells prosecutors they "should decline prosecution" of good-faith security research. It defines that research as access "solely for purposes of good-faith testing, investigation, and/or correction of a security flaw or vulnerability" ([Morrison Foerster](https://www.mofo.com/resources/insights/220525-cyber-crime-charging-policy); S). Commentators flag that "solely" is ambiguous. The policy is prosecutorial discretion, not immunity, and it leaves civil exposure untouched.

**United Kingdom.** The Computer Misuse Act 1990 still has no research defence. The government said in December 2025 that it was considering a statutory defence ([CyberUp](https://www.cyberupcampaign.com/news/security-minister-announces-government-is-looking-at-introducing-a-statutory-defence-to-the-computer-misuse-act); S). The May 2026 proposal was reported as "much narrower" and focused on scanning ([The Record](https://therecord.media/uk-moves-to-shield-security-researchers-cybercrime); S).

**Safe harbor.** Read each program's safe-harbor language. disclose.io's dioterms define **Full Safe Harbor** as four commitments ([dioterms](https://raw.githubusercontent.com/disclose/dioterms/master/README.md); F):
1. authorization under anti-hacking laws such as the CFAA and the Computer Misuse Act
2. exemption from anti-circumvention law
3. exemption from the organization's own terms of service during testing
4. acknowledgement of good faith

A policy that only promises not to pursue researchers is "Partial Safe Harbor". And "bounties… are not a prerequisite for Safe Harbor".

Automation adds a risk of its own. A hackbot that is hijacked, or simply buggy, and touches an out-of-scope host creates legal exposure for its operator. That makes proxy-enforced scope, and request logs that prove what was and was not sent, a legal safeguard as much as a safety one.

## What to do this week

These eight steps take about 10 hours and set up everything Stage 0 needs. None of them involves touching a system you do not own.

1. **Build the lab skeleton.** Write a Docker Compose file with [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) and [DVWA](https://github.com/digininja/DVWA) on an internal-only network. From inside a container, prove that the internet is unreachable. Keep that proof; it is your first containment test.
2. **Solve three labs by hand.** Create a [PortSwigger Web Security Academy](https://portswigger.net/web-security/all-labs) account and solve three Apprentice labs in different classes. Write a five-line report for each in your own words.
3. **Read the two foundation pages.** Read Anthropic's [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) and the [prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices) page, or your own provider's equivalent for the model you will use. Then skim Thorsten Ball's [How to Build an Agent](https://ampcode.com/how-to-build-an-agent).
4. **Cap your spending, then make one API call.** Set a hard monthly spend limit in your LLM provider's console before you write any code. Then make one tool-use API call and work out its cost from the [pricing page](https://platform.claude.com/docs/en/about-claude/pricing).
5. **Draft your scope schema and rules-of-engagement template.** Do this before any agent exists. The template covers in-scope hosts, exclusions, the rate limit, banned test types, contacts and a reference to the authorization. Every later stage reads from this file.
6. **Start your eval set.** Capture 10 request/response pairs from your lab and write down your own verdict on each. This file seeds the Stage 1 eval.
7. **Read the rules you will live under.** That means HackerOne's [Code of Conduct](https://www.hackerone.com/policies/code-of-conduct), Bugcrowd's [Code of Conduct](https://www.bugcrowd.com/resources/hacker-resources/code-of-conduct/), your model provider's usage policy (for Anthropic, the [Usage Policy](https://www.anthropic.com/legal/aup)), and the safe-harbor and automation sections of one program you would like to join.
8. **Subscribe and choose a target class.** Subscribe to [tl;dr sec](https://tldrsec.com/) and the [Critical Thinking podcast](https://www.youtube.com/@criticalthinkingpodcast). Then pick the one vulnerability class you will build toward in Stage 6, and add its PortSwigger topic to your Track B plan.

## Scope note

Verification limits shape every section of this report. Tags follow the four-level F/P/S/U scheme defined at the top. No tag was upgraded when a resource reappeared in the roadmap, architecture, ladder or guardrails sections. No Reddit thread could be verified, so Reddit content appears only as second-hand mirror snapshots (S) or as subreddits to search yourself. Many platform and provider policy details, from HackerOne, Bugcrowd, Intigriti, YesWeHack, OpenAI and Google, come from search extracts rather than pages that were read. Several of those policies changed, or announced changes, within weeks of this report, so read the live page before you rely on any rule. Three elements are this report's synthesis of the cited sources, not measured results: the roadmap's week counts, the reference architecture, and the ladder's advancement threshold. The architecture has not been validated against any public hackbot implementation. Hosted-lab terms for automated agents (PortSwigger, Hack The Box, TryHackMe) were not verified. Nothing here is legal advice.
