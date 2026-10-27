# Graph Engineering, Agent Swarms, Subagents and Multi-Agent Orchestration for an Autonomous Pentest / Bug-Bounty Hackbot

Research date: 2026-10-27.

How I checked sources (applies to every section). Each item carries one of these tags:
- **[F] Fetched**: I read the page's content this session. Sources: anthropic.com and code.claude.com pages; official GitHub READMEs/docs sources via raw.githubusercontent.com; live PyPI/npm registry metadata, which gives version numbers and release dates as of 2026-10-27. **[F-src]** means I read the page's source file in the official docs repo, not the rendered page, and the URL is built from that file's path.
- **[S] Search-verified**: the URL showed up in live web-search results this session. I saw only snippets, not the full page.
- **[K] Known, unverified this session**: the URL comes from prior knowledge. Its host (arxiv.org, cognition.ai, blog.langchain.com, googleprojectzero.blogspot.com and others) could not be reached from this sandbox, and the shared web-search budget ran out partway through. The URL is probably right, but the report writer should check it again before publishing.
- Tool limits I hit: the egress proxy blocked direct fetches of arxiv.org, docs.langchain.com, learn.microsoft.com, devblogs.microsoft.com, google.github.io, modelcontextprotocol.io, a2a-protocol.org, xbow.com, cognition.ai and YouTube. The search budget ("200 WebSearch calls per turn, shared by every agent") was used up after about 15 searches. For many docs I confirmed the URL from the official source repo or from the URLs the maintainers list in PyPI metadata.

---

## Q1. Graph-based orchestration: what "graph engineering" means, the main graph runtimes, and knowledge graphs / attack graphs

### Takeaway
"Graph engineering" is a 2026 label, not yet settled, for making an agent system's structure explicit: nodes, typed state, edges and transitions, checkpoints, and human gates. It was formalized in an August 2026 survey (arXiv 2608.21156). LangGraph (1.x), Microsoft Agent Framework workflows (1.0 GA, April 2026), Google ADK workflow agents, pydantic-graph, LlamaIndex Workflows and Apache Burr all implement it. For a hackbot, the graph should own the engagement lifecycle (scope → recon → hypotheses → exploit → verify → report), and LLM agents should reason only inside nodes.

### Cited Findings
**What "graph engineering" means and who uses the term**
- The survey "Graph Engineering in the Era of LLM Agents: From Individual Intelligence to System Intelligence" (arXiv 2608.21156, Aug 2026) presents graph engineering as the paradigm that follows prompt, context, harness and loop engineering. It "constructs explicit, dynamic, evolving graph structures representing tasks, agents, and system states." [S] — [paperswithcode.co listing](https://paperswithcode.co/paper/2608.21156); [arXiv PDF](https://arxiv.org/pdf/2608.21156)
- The official companion repo gives the citation as Feng, Yuyuan; Xiang, Zhishang; Yang, Chaobin; Ma, Qichao; Chen, Zerui; … et al., 2026. It frames a progression from Model Intelligence through Individual Intelligence to System Intelligence, where System Intelligence "organizes tasks, agents, runtime state, and system evolution through explicit graph structures, with ontology engineering providing a shared semantic layer." [F] — [DEEP-JLU/Awesome-Graph-Engineering](https://github.com/DEEP-JLU/Awesome-Graph-Engineering). Aggregators disagreed about the authors; the repo's BibTeX settles it.
- A third-party summary describes the survey's framework as three modules: Task Organization, Agent Coordination and Runtime State Management. [S] — [papers.cool](https://papers.cool/arxiv/2608.21156)
- Practitioner usage, all 2026 blogs: Analytics Vidhya (July 2026) defines graph engineering as designing "nodes, dependencies, state transitions, execution routes, validation gates, recovery paths, and control boundaries." Agents reason within nodes while the graph restricts which transitions are allowed, and known business rules should stay deterministic. [S] — [Analytics Vidhya, "Graph Engineering for AI Agents: Beyond the Single-Agent Loop"](https://www.analyticsvidhya.com/blog/2026/07/graph-engineering/)
- A PuppyOne explainer says the term covers three competing meanings: execution/orchestration graphs, graphs of interacting loops, and graph-structured knowledge/memory. [S] — [PuppyOne](https://www.puppyone.ai/en/blog/graph-engineering-ai-agents-map). Other uses: [Towards Data Science, "Graph Engineering for AI Agents: From Prompts and Loops to Workflows"](https://towardsdatascience.com/graph-engineering-for-ai-agents-from-prompts-and-loops-to-workflows/) [S]; [Adnan Masood, Medium](https://medium.com/@adnanmasood/graph-engineering-for-ai-agents-the-practitioners-guide-to-designing-multi-agent-systems-as-f9a4559aa693) [S].
- Related 2026 theory: "From Agent Loops to Structured Graphs: A Scheduler-Theoretic Framework for LLM Agent Execution" (arXiv 2604.11378) and "The Log is the Agent: Event-Sourced Reactive Graphs for Auditable, Forkable Agentic Systems" (arXiv 2605.21997). [F, IDs as listed in the curated repo] — [Awesome-Graph-Engineering](https://github.com/DEEP-JLU/Awesome-Graph-Engineering)

**LangGraph (LangChain)**
- LangGraph 1.0 and LangChain 1.0 were announced on Oct 22, 2025, as a stability release: core graph APIs unchanged, `langgraph.prebuilt` deprecated in favor of `langchain.agents`, Python 3.9 dropped. [S] — [LangChain blog](https://www.langchain.com/blog/langchain-langgraph-1dot0). PyPI shows `langgraph==1.0.0` uploaded 2025-10-17, a few days before the blog post. As of 2026-10-26 the latest is 1.2.18, and `@langchain/langgraph` (JS) is at 1.5.3. [F] — [PyPI langgraph](https://pypi.org/project/langgraph/)
- The official docs home is docs.langchain.com/oss/python/langgraph/overview (PyPI "Homepage"). [F] — [LangGraph overview](https://docs.langchain.com/oss/python/langgraph/overview). The persistence/checkpointing and interrupts (human-in-the-loop) pages exist in the official docs source. [F-src] — [Persistence](https://docs.langchain.com/oss/python/langgraph/persistence), [Interrupts](https://docs.langchain.com/oss/python/langgraph/interrupts). The Postgres checkpointer `langgraph-checkpoint-postgres` is at 3.2.0 (2026-10-14). [F] — [PyPI](https://pypi.org/project/langgraph-checkpoint-postgres/)
- LangChain's multi-agent guide lists five patterns. **Subagents**: a main agent calls subagents as tools. **Handoffs**: tool calls update state that switches the active agent. **Skills**: context loaded on demand. **Router**: classify, then fan out. **Custom workflow**: bespoke LangGraph flows "mixing deterministic logic and agentic behavior." It also says "not every complex task requires this approach—a single agent with the right (sometimes dynamic) tools and prompt can often achieve similar results" and that "at the center of multi-agent design is context engineering." [F-src] — [LangChain docs: Multi-agent](https://docs.langchain.com/oss/python/langchain/multi-agent)
- Status change: the `langgraph-supervisor` README now says "We now recommend using the supervisor pattern directly via tools rather than this library for most use cases," because the tool-calling approach gives more control over context engineering. [F] — [langgraph-supervisor-py](https://github.com/langchain-ai/langgraph-supervisor-py). Its last release is 0.0.31 (2025-11-19). `langgraph-swarm` (agents "dynamically hand off control to one another," and the system remembers the last active agent) is at 0.1.0 (2025-12-04). [F] — [langgraph-swarm-py](https://github.com/langchain-ai/langgraph-swarm-py), [PyPI](https://pypi.org/project/langgraph-swarm/)
- The LangChain Academy repo describes Modules 0–6: setup, then "Modules 1 - 5 focus on building in LangGraph, progressively adding more advanced themes," and Module 6 covers deployment. [F] — [langchain-ai/langchain-academy](https://github.com/langchain-ai/langchain-academy). Course page: [Introduction to LangGraph](https://academy.langchain.com/courses/intro-to-langgraph) [K].

**Microsoft Agent Framework (MAF), successor to Semantic Kernel + AutoGen**
- MAF first appeared on PyPI on 2025-10-01 (public preview), shipped `1.0.0` on 2026-04-02, and is at 1.23.0 as of 2026-10-22. [F] — [PyPI agent-framework](https://pypi.org/project/agent-framework/). The 1.0 announcement post is dated April 3, 2026 and the release candidate came Feb 19, 2026. [S] — [Microsoft Agent Framework Version 1.0 (devblogs)](https://devblogs.microsoft.com/agent-framework/microsoft-agent-framework-version-1-0/), [RC post](https://devblogs.microsoft.com/foundry/microsoft-agent-framework-reaches-release-candidate/)
- Workflows are a graph-based engine: a directed graph of executors connected by edges that route typed messages, with streaming, shared state, checkpointing and human-in-the-loop pauses. Orchestration patterns are sequential, concurrent, handoff, group chat and Magentic-One. Secondary sources say some orchestration and declarative packages reached 1.0 only in July 2026, and that the 1.0 post said "A2A 1.0 support coming soon." [S] — [agenticwire summary](https://www.agenticwire.news/article/microsoft-agent-framework-1-0-workflows-mcp); [Learn overview](https://learn.microsoft.com/agent-framework/overview/agent-framework-overview)
- The README describes "production-grade AI agents and multi-agent workflows in .NET and Python" and also mentions Go. It links a 30-minute intro video. [F] — [microsoft/agent-framework](https://github.com/microsoft/agent-framework); [intro video](https://www.youtube.com/watch?v=AAgdMhftj8w), linked from the README; its exact YouTube title is unverified.

**Google ADK workflow agents**
- ADK workflow agents (Sequential, Parallel, Loop) control execution order "without consulting an AI model for assistance with the orchestration," which "results in deterministic and predictable execution patterns." [F-src] — [ADK docs: Workflow agents](https://google.github.io/adk-docs/agents/workflow-agents/)
- Version history: `google-adk` 1.0.0 on 2025-05-20, 2.0.0 on 2026-05-19, latest 2.13.0 (2026-10-23). [F] — [PyPI google-adk](https://pypi.org/project/google-adk/)

**Other graph/flow runtimes**
- pydantic-graph: "Graph and state machine library," 1.0.0 on 2025-09-05, 2.0.0 on 2026-06-23, latest 2.62.0 (2026-10-27). The docs warn: "Don't use a nail gun unless you need a nail gun… graphs are a nail gun." [F] — [pydantic-graph docs](https://pydantic.dev/docs/ai/graph/graph/), [PyPI](https://pypi.org/project/pydantic-graph/)
- LlamaIndex Workflows (`llama-index-workflows`): "event-driven, async-first, step-based," 1.0.0 on 2025-06-25, 2.0.0 on 2025-08-29, latest 2.26.1. [F] — [run-llama/workflows-py](https://github.com/run-llama/workflows-py)
- Apache Burr moved to the Apache incubator. The old `burr` package says "This package has moved to apache-burr." `apache-burr` is at 0.43.0 (2026-08-25) and has not reached 1.0. [F] — [Apache Burr](https://burr.apache.org/), [apache/burr](https://github.com/apache/burr)
- Haystack (`haystack-ai`) reached 3.0.0 on 2026-07-20 (latest 3.4.1) and offers "pipelines or agents." [F] — [Haystack docs](https://haystack.deepset.ai/overview/intro)
- DSPy reached 3.0.0 on 2025-08-12 (latest 3.4.1). It optimizes programs and prompts rather than serving as a graph runtime. [F] — [stanfordnlp/dspy](https://github.com/stanfordnlp/dspy)

**Knowledge graphs inside agents (GraphRAG, attack-surface graphs)**
- GraphRAG paper: "From Local to Global: A Graph RAG Approach to Query-Focused Summarization" (arXiv 2404.16130). The `graphrag` package is at 3.3.0 (2026-10-08). [F] — [microsoft/graphrag](https://github.com/microsoft/graphrag)
- PentAGI uses an optional Graphiti-powered Neo4j knowledge graph that "tracks hosts, services and findings," plus PostgreSQL/pgvector memory of "successful approaches" for later flows. [F] — [vxcontrol/pentagi](https://github.com/vxcontrol/pentagi)
- Graph-agent surveys: "Graphs Meet AI Agents: Taxonomy, Progress, and Future Opportunities" (arXiv 2506.18019) and "Graph-Based Agent Memory: Taxonomy, Techniques, and Applications" (arXiv 2602.05665). [F, as listed] — [Awesome-Graph-Engineering](https://github.com/DEEP-JLU/Awesome-Graph-Engineering)

### Inferences
- A hackbot has two graphs. The **control graph** (LangGraph/MAF workflow) encodes the methodology and safety gates. The **world-model graph** (Neo4j/Graphiti-style asset → service → endpoint → parameter → finding) is the shared, durable state that short-lived agents read and write. Treating these separately follows the PuppyOne "execution vs knowledge graph" distinction and PentAGI's design.
- Checkpointing (LangGraph checkpointers, MAF workflow checkpoints) is essential for multi-hour engagements. Anthropic's production lesson that "errors compound" and that systems should "resume from checkpoints rather than restarting" points the same way (see Q3).
- Pydantic's "nail gun" warning and Analytics Vidhya's "keep business rules deterministic" both say the same thing: encode scope checks, rate limits and authorization gates as code edges, not as LLM decisions.

### Gaps
- I could not open the arXiv abstract page for 2608.21156 to confirm the full author list or the exact three-module wording. These come from the companion repo's BibTeX and from aggregators.
- I could not confirm the exact LangChain Academy course URL, how long the course runs, or whether it was updated for LangGraph 1.x (academy.langchain.com was unreachable).
- I could not confirm the exact list of MAF workflow features that were stable at 1.0, because learn.microsoft.com and devblogs were unreachable. That information is secondary-sourced.
- I found no peer-reviewed paper that builds an LLM pentest agent around a formal attack graph. VulnBot's "Penetration Task Graph" and PentestGPT's "Pentesting Task Tree" are the closest I know of [K].

---

## Q2. Swarms and multi-agent frameworks: status and maturity in late 2026

### Takeaway
The field has consolidated. OpenAI Swarm is officially replaced by the OpenAI Agents SDK. Microsoft AutoGen is now archived. Semantic Kernel is in maintenance behind Microsoft Agent Framework 1.x. IBM's ACP merged into A2A. Letta retired its V1 server in favor of "Letta Code." ChatDev became "ChatDev 2.0 (DevAll)." The actively released, production-grade options are LangGraph 1.x, MAF 1.x, Google ADK 2.x, CrewAI 1.x, Strands 1.x, Pydantic AI 2.x and AG2 1.x. The OpenAI Agents SDK and Claude Agent SDK are very active but still pre-1.0 by version number.

### Cited Findings
- **OpenAI Swarm: deprecated.** The README is titled "Swarm (experimental, educational)" and says "Swarm is now replaced by the OpenAI Agents SDK, which is a production-ready evolution of Swarm… We recommend migrating to the Agents SDK for all production use cases." [F] — [openai/swarm](https://github.com/openai/swarm)
- **OpenAI Agents SDK.** First released 2025-03-04, latest `openai-agents` 0.24.0 (2026-10-15), JS `@openai/agents` 0.21.0. Primitives: agents, sandbox agents ("preconfigured to work with a container… over long time horizons"), agents-as-tools and handoffs, guardrails, human-in-the-loop, sessions and tracing. [F] — [openai-agents-python](https://github.com/openai/openai-agents-python); [Handoffs docs](https://openai.github.io/openai-agents-python/handoffs/)
- **AutoGen: archived.** The README now says "⚠️ Project archived and no longer maintained… New users should start with Microsoft Agent Framework," and links a migration guide. [F] — [microsoft/autogen](https://github.com/microsoft/autogen); [AutoGen → MAF migration guide](https://learn.microsoft.com/en-us/agent-framework/migration-guide/from-autogen/). The last `autogen-agentchat` release was 0.7.5 on 2025-09-30. [F] — [PyPI](https://pypi.org/project/autogen-agentchat/)
- **Semantic Kernel** still publishes releases (1.45.0, 2026-10-06), but secondary sources describe it as in maintenance with critical and security fixes after MAF's release. [F] — [PyPI semantic-kernel](https://pypi.org/project/semantic-kernel/); [S] — [devblogs "Semantic Kernel and Microsoft Agent Framework"](https://devblogs.microsoft.com/semantic-kernel/semantic-kernel-and-microsoft-agent-framework/)
- **AG2 (community fork of AutoGen).** 1.0.0 on 2026-07-27, latest 1.2.1. "As of AG2 v1.0, the protocol-driven framework is the top-level package, imported as `ag2`." The classic `autogen` namespace (ConversableAgent, GroupChat) moved to `ag2ai/ag2-classic`. It is maintained by volunteers, with Chi Wang and Qingyun Wu as administrators. [F] — [ag2ai/ag2](https://github.com/ag2ai/ag2), [docs](https://docs.ag2.ai)
- **CrewAI.** 1.0.0 on 2025-10-20, latest 1.15.30 (2026-10-24). It offers "autonomous agent collaboration through Crews and precise, event-driven control through Flows." [F] — [crewAIInc/crewAI](https://github.com/crewAIInc/crewAI), [docs](https://docs.crewai.com)
- **Google ADK.** 2.x (latest 2.13.0). Multi-agent systems are built from LLM agents plus Sequential, Parallel and Loop workflow agents. [F] — [google/adk-python](https://github.com/google/adk-python)
- **Strands Agents (AWS).** 1.0.0 on 2025-07-15, latest 1.62.0. The TypeScript SDK reached 1.0.0 on 2026-04-30. The repo is now `strands-agents/harness-sdk`. "Choose Strands when you would otherwise write your own agent loop"; "MCP, streaming, multi-agent patterns, and structured output are all built in." [F] — [strands-agents/harness-sdk](https://github.com/strands-agents/harness-sdk), [strandsagents.com](https://strandsagents.com)
- **Claude Agent SDK / Claude Code.** `claude-agent-sdk` (Python) first released 2025-09-28, latest 0.2.174. npm `@anthropic-ai/claude-agent-sdk` 0.3.311 and `@anthropic-ai/claude-code` 2.1.311 (2026-10-26). [F] — [PyPI](https://pypi.org/project/claude-agent-sdk/); Agent SDK subagents docs [F] — [Subagents in the SDK](https://code.claude.com/docs/en/agent-sdk/subagents)
- **Claude Code agent teams** are "experimental and disabled by default" (`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`). They have a team lead, teammates (separate Claude Code instances), a shared task list with dependencies and file-locked claiming, and a mailbox. They are "no nested teams" and "one team per session." [F] — [Orchestrate teams of Claude Code sessions](https://code.claude.com/docs/en/agent-teams). A secondary source says they shipped in early Feb 2026 alongside Opus 4.6. [S] — [Addy Osmani, "Claude Code Swarms"](https://addyosmani.com/blog/claude-code-agent-teams/)
- **CAMEL.** `camel-ai` 0.2.90 (2026-03-22), still 0.x. Paper arXiv 2303.17760 (NeurIPS 2023). [F] — [camel-ai/camel](https://github.com/camel-ai/camel)
- **MetaGPT.** The last PyPI release (`metagpt` 0.8.2) was 2025-03-09, which suggests the PyPI package has stalled. The repo has moved to FoundationAgents/MetaGPT. Paper arXiv 2308.00352 (ICLR 2024). [F] — [FoundationAgents/MetaGPT](https://github.com/FoundationAgents/MetaGPT)
- **ChatDev** became "ChatDev 2.0 - DevAll… A Zero-Code Multi-Agent Platform," released Jan 7, 2026. Classic v1.x moved to another branch. A related paper, "Multi-Agent Collaboration via Evolving Orchestration" (arXiv 2505.19591, NeurIPS 2025, the "puppeteer" branch), studies learned orchestrators. [F] — [OpenBMB/ChatDev](https://github.com/OpenBMB/ChatDev)
- **Agency Swarm** "leverages and extends the OpenAI Agents SDK." 1.0.0 on 2025-06-06, latest 1.11.0 (2026-08-03). [F] — [VRSEN/agency-swarm](https://github.com/VRSEN/agency-swarm)
- **Swarms (kyegomez)** bills itself as "The Enterprise-Grade Production-Ready Multi-Agent Orchestration Framework." It offers SequentialWorkflow, GraphWorkflow (optional rustworkx backend) and other workflows, at version 16.0.3 (2026-10-25). This is self-description, not independent evidence. [F] — [kyegomez/swarms](https://github.com/kyegomez/swarms)
- **Letta.** The README says the `archive` branch "contains the retired Letta V1 API server… active projects should use… letta-code." The `letta` PyPI package is now "Letta Code: stateful agents in your terminal" (0.34.12). [F] — [letta-ai/letta](https://github.com/letta-ai/letta), [docs](https://docs.letta.com/letta-code)
- **Mastra** (TypeScript): `@mastra/core` 1.0.0 on 2026-01-20, latest 1.79.0. [F] — [npm @mastra/core](https://www.npmjs.com/package/@mastra/core)

### Inferences
- For a Python hackbot, the low-risk choices are LangGraph 1.x (largest ecosystem, mature checkpointers and HITL), MAF 1.x (if you need .NET or the Azure ecosystem) and Google ADK 2.x. For a "Claude as the harness" design, the Claude Agent SDK gives you the subagent, hooks, permission and worktree-isolation machinery that Claude Code itself uses.
- Avoid starting new work on Swarm, AutoGen, the classic `autogen` namespace, MetaGPT's PyPI package, Letta V1 server or `langgraph-supervisor`. Each is deprecated, archived, stalled or explicitly de-recommended (citations above).
- Several "swarm" frameworks (Swarms, Agency Swarm) are thin layers over other SDKs or marketing-heavy. Their maturity claims are self-reported.

### Gaps
- GitHub star counts and contributor counts were not retrievable (the GitHub API was blocked for this session), so popularity is not quantified.
- I did not confirm exact retirement dates for AutoGen's archival or Letta V1.

---

## Q3. Subagents and orchestrator-worker patterns: the Anthropic, Cognition and LangChain debate, and best practices

### Takeaway
Orchestrator-worker with context-isolated subagents wins on breadth-first, parallelizable "read" work. Anthropic measured +90.2% on internal research evals at about 15× chat tokens. Cognition's 2025 warning still holds for parallel *writers*: context fragmentation produces conflicting implicit decisions. Its April 2026 follow-up says that only narrow, specific multi-agent flows work. The consensus best practice: a lead keeps the plan, workers get explicit objectives, boundaries and output formats, workers return condensed summaries (1–2k tokens), and verifiers are strict.

### Cited Findings
- **Anthropic, "How we built our multi-agent research system"** (Jun 13, 2025; Jeremy Hadfield, Barry Zhang, Kenneth Lien, Florian Scholz, Jeremy Fox, Daniel Ford). [F] — [Anthropic](https://www.anthropic.com/research/multi-agent-research-system)
  - An Opus 4 lead with Sonnet 4 subagents "outperformed single-agent Claude Opus 4 by 90.2%" on an internal research eval. [F]
  - On BrowseComp, three factors explained 95% of performance variance, and token usage alone explained 80%. The other two were the number of tool calls and model choice. [F]
  - Agents use about 4× the tokens of chat, and multi-agent systems about 15×. [F]
  - The lead spins up 3–5 subagents in parallel, and subagents use 3+ tools in parallel, cutting research time "by up to 90%" for complex queries. [F]
  - Effort-scaling rules: simple fact-finding uses 1 agent and 3–10 tool calls; comparisons use 2–4 subagents with 10–15 calls each; complex research uses 10+ subagents. An early failure mode was "spawning 50 subagents for simple queries." [F]
  - Delegation needs "an objective, an output format, guidance on the tools and sources to use, and clear task boundaries." Otherwise subagents duplicate work or leave gaps. [F]
  - A tool-testing agent that rewrote tool descriptions cut task completion time by 40%. [F]
  - The lead saves its plan to memory because context beyond 200k tokens is truncated. A CitationAgent attributes claims. Production uses checkpoints and resume, full tracing, and "rainbow deployments." Synchronous subagent execution is a bottleneck. [F]
  - Not a fit for: domains that need shared context or have many inter-agent dependencies, "most coding tasks," and tasks too low-value to justify the cost. Real-time coordination is also immature. [F]
- **Anthropic, "Effective context engineering for AI agents"** (Sep 29, 2025): subagents "might explore extensively, using tens of thousands of tokens or more, but return only a condensed, distilled summary of its work (often 1,000-2,000 tokens)." [F] — [Anthropic](https://www.anthropic.com/research/effective-context-engineering-for-ai-agents)
- **Cognition, "Don't Build Multi-Agents"** (Walden Yan, June 2025). Its principles are to share full context and agent traces, and that "actions carry implicit decisions," so parallel subagents with partial context make conflicting choices. It recommends single-threaded linear agents plus context compression. [K for URL] — [Cognition](https://cognition.ai/blog/dont-build-multi-agents); date corroborated [S] — [yespress profile](https://yespress.io/walden-yan)
- **Cognition, "Multi-Agents: What's Actually Working"** (Walden Yan, Apr 22, 2026): "we've begun to deploy multi-agent systems that actually work in practice." The original observations "still hold today" for parallel-writer swarms, and "most of the sexy ideas in that space still don't see meaningful adoption." Cognition "implemented a few specific flows for this in Devin." [S] — [Cognition](https://cognition.ai/blog/multi-agents-working)
- **LangChain, "How and when to build multi-agent systems"** (Harrison Chase, June 2025) reconciles the two positions: context engineering is the crux, and read-heavy, parallelizable tasks such as research suit multi-agent better than write-heavy ones. [K for URL and details] — [LangChain blog](https://blog.langchain.com/how-and-when-to-build-multi-agent-systems/). The current LangChain docs say "at the center of multi-agent design is context engineering." [F-src] — [LangChain multi-agent docs](https://docs.langchain.com/oss/python/langchain/multi-agent)
- **Claude Code subagents docs.** Each subagent "runs in its own context window with a custom system prompt, specific tool access, and independent permissions." Frontmatter includes `tools`, `disallowedTools`, `model`, `permissionMode`, `maxTurns`, `skills`, `mcpServers`, `hooks`, `memory`, `isolation: worktree` and `background`. Subagents can nest "up to three layers below the main conversation" (`CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH`). Best practices: "use subagents for high-volume output… return only the summary," "restate critical rules in the delegation prompt" (non-fork subagents don't see history), and prefer the main conversation for tightly linked phases. [F] — [Create custom subagents](https://code.claude.com/docs/en/sub-agents)
- **Claude Code agent teams docs.**
  - Comparison: subagents mean "focused tasks where only the result matters," with lower token cost, while teams mean "complex work requiring discussion," with higher cost because "each teammate is a separate Claude instance." [F]
  - "Start with 3-5 teammates"; "Three focused teammates often outperform five scattered ones"; "5-6 tasks per teammate." [F]
  - "Two teammates editing the same file leads to overwrites." [F]
  - Quality gates via `TeammateIdle`, `TaskCreated` and `TaskCompleted` hooks. [F]
  - A teammate "can't approve a permission prompt or supply consent on your behalf." [F]
  - Source: [Agent teams](https://code.claude.com/docs/en/agent-teams)
- **Anthropic, "Building a C compiler with a team of parallel Claudes"** (Nicholas Carlini, Feb 5, 2026).
  - Scale: 16 parallel agents, about 2,000 Claude Code sessions over about 2 weeks, 2B input / 140M output tokens, just under $20,000, a ~100k-line Rust compiler that builds Linux 6.9. [F]
  - No orchestrator agent: coordination was git plus lock files in `current_tasks/`. [F]
  - "Nearly perfect" verifiers were required. When all agents hit the same kernel bug, "sixteen agents didn't help" until GCC was used as an oracle to split the work. [F]
  - Source: [Anthropic](https://www.anthropic.com/research/building-c-compiler)
- **Anthropic, "Effective harnesses for long-running agents"** (Nov 26, 2025) covers harness design for agents that work across many context windows. [F title/date] — [Anthropic](https://www.anthropic.com/research/effective-harnesses-for-long-running-agents)

### Inferences
- Map this onto a hackbot. Recon and enumeration (subdomains, ports, JS endpoint extraction, parameter discovery, per-endpoint hypothesis triage) is breadth-first "read" work and suits parallel subagents. Exploit chaining against one target state (auth session, multi-step IDOR → privilege escalation) is stateful "write" work and should stay single-threaded or run in a strictly sequenced graph.
- The C-compiler post and the XBOW solver design (Q6) point to the same recipe. Use many short-lived workers with fresh context. Coordinate through a durable shared store (git, task list or attack-surface DB), not through chat. Invest most in the verifier: in a hackbot that is a deterministic PoC validator or oracle.
- The agent-teams rule that relayed approvals are untrusted suits scope authorization. A worker must never be able to "relay" permission to hit an out-of-scope host.

### Gaps
- I couldn't read the full text of either Cognition post or the LangChain post. Exact quotes beyond the snippets above are unverified.
- Secondary blogs claim agent teams use "approximately 7x the tokens of a single session in plan mode," attributed to the Claude Code costs page. I fetched that page; it contains an "Agent team token costs" section saying usage "scales with the number of active teammates." My extraction did not capture a 7× figure, so treat it as unverified. — [Claude Code costs](https://code.claude.com/docs/en/costs)

---

## Q4. Protocols: MCP, A2A, ACP, AG-UI and where they sit in a hackbot

### Takeaway
MCP (agent ↔ tools) and A2A (agent ↔ agent) are now both foundation-governed and at stable versions. The MCP spec reached its stateless 2026-07-28 revision and A2A reached spec 1.0.0. ACP is dead: it merged into A2A. AG-UI (agent ↔ UI) hit 1.0 in Sept 2026. A hackbot needs MCP for tool servers and should treat MCP as an attack surface. It needs A2A only if it federates with external or vendor agents, and AG-UI only for an operator console.

### Cited Findings
- **MCP introduced** Nov 25, 2024. [F] — [Anthropic, "Introducing the Model Context Protocol"](https://www.anthropic.com/news/model-context-protocol). **Donated to the Agentic AI Foundation** on Dec 9, 2025. [F] — [Anthropic, "Donating MCP to the Agentic AI Foundation"](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation)
- **MCP spec 2026-07-28** exists in the official spec repo. Its "Key Details" list "Stateless, self-contained requests" and "Per-request capability negotiation" (JSON-RPC 2.0; hosts, clients and servers). [F-src] — [MCP Specification 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28)
- Secondary sources say this revision replaces the initialize/initialized handshake and the `Mcp-Session-Id` header, moves tasks into an official extension (`io.modelcontextprotocol/tasks`), formalizes extensions, and follows 2025-11-25. [S] — [Vercel changelog](https://vercel.com/changelog/vercel-mcp-now-supports-the-2026-07-28-mcp-specification); [AAIF blog](https://aaif.io/blog/mcp-2026-07-28-whats-changing-and-how-to-migrate)
- The Python `mcp` SDK 2.0.0 was released 2026-07-28 (latest 2.3.0). The TS `@modelcontextprotocol/sdk` is at 1.32.2. [F] — [PyPI mcp](https://pypi.org/project/mcp/)
- **MCP efficiency:** "Code execution with MCP" (Nov 4, 2025) cuts token usage "from 150,000 tokens to 2,000 tokens—a time and cost saving of 98.7%" by letting agents load tool definitions on demand as code. [F] — [Anthropic](https://www.anthropic.com/research/code-execution-with-mcp)
- **MCP and agent security research** (IDs as listed in the curated survey repo [F] — [Awesome-Graph-Engineering](https://github.com/DEEP-JLU/Awesome-Graph-Engineering)):
  - "MCP Security Bench (MSB): Benchmarking Attacks Against Model Context Protocol in LLM Agents" (arXiv 2510.15994)
  - "Beyond the Protocol: Unveiling Attack Vectors in the Model Context Protocol (MCP) Ecosystem" (IEEE TSE 2026, doi 10.1109/TSE.2026.3694876)
  - "CaMeL: Defeating Prompt Injections by Design" (arXiv 2503.18813)
  - "AgentDojo" (arXiv 2406.13352)
  - "FlowSteer: Prompt-Only Workflow Steering Exposes Planning-Time Vulnerabilities in Multi-Agent LLM Systems" (arXiv 2605.11514)
- **Specific to hackbots:** "Hacking the AI Hackers via Prompt Injection" (arXiv 2508.21669, by the CAI team) documents prompt-injection classes against AI pentest agents. [F] — [aliasrobotics/cai README](https://github.com/aliasrobotics/cai)
- **A2A** "is an open source project under the Linux Foundation, contributed by Google." It uses JSON-RPC 2.0 over HTTP(S) and discovers agents via "Agent Cards." It aims to "preserve opacity," meaning agents collaborate without sharing internal memory or tools. [F] — [a2aproject/A2A](https://github.com/a2aproject/A2A). The spec's "Latest Released Version" is 1.0.0. [F-src] — [A2A spec v1.0.0](https://a2a-protocol.org/v1.0.0/specification). `a2a-sdk` (Python) 1.0.0 on 2026-04-20 and `@a2a-js/sdk` 1.0.0 on 2026-07-22. [F]
- **ACP (IBM/BeeAI): merged.** The README says "ACP is now part of A2A under the Linux Foundation!" and links a migration guide. [F] — [i-am-bee/acp](https://github.com/i-am-bee/acp). The merger date is given as late August 2025. [S] — [IBM Research ACP page](https://research.ibm.com/projects/agent-communication-protocol). Don't confuse it with Zed's "Agent Client Protocol," which also abbreviates to ACP. [S]
- **AG-UI** is "an open, lightweight, event-based protocol that standardizes how AI agents connect to user-facing applications." `ag-ui-protocol` 1.0.0 and `@ag-ui/core` 1.0.0 were both released 2026-09-16/17. [F] — [ag-ui-protocol/ag-ui](https://github.com/ag-ui-protocol/ag-ui)

### Inferences
- Hackbot layering:
  - Wrap tools (nmap, httpx, ffuf, nuclei, a Burp/Caido API, a browser) as **MCP servers**, and expose them to workers progressively, code-execution style, so tool definitions don't flood context.
  - Use **A2A** only at trust boundaries, for example a third-party recon service or a separate "report-QA" agent run by another team.
  - Stream operator approvals for HITL interrupts over **AG-UI**.
  - **Skip ACP.**
- Target content (HTTP responses, JS files, error pages) is attacker-controlled input to the agent, and the CAI prompt-injection paper shows AI hackers can be hijacked by it. Tool outputs from targets should pass through a sanitizing/parsing node before reaching any planner with high-privilege tools. This is a CaMeL-style split between privileged and quarantined processing.

### Gaps
- I could not open the official MCP security-best-practices page for 2026-07-28. The guessed path returned 404, and the page may live elsewhere in the docs tree.
- I could not confirm whether MAF 1.x has shipped A2A 1.0 support since April 2026.

---

## Q5. Research papers (with arXiv IDs)

### Takeaway
The literature splits four ways. Some papers show that multi-agent can help (debate, ensembling, orchestrator-worker). Others explain why it fails (MAST: 14 failure modes; Google: independent agents amplify errors 17.2×). Others automate topology and workflow design (ADAS, AFlow, GPTSwarm, MaAS, MASS, G-Designer). Several 2025–2026 surveys tie these together.

### Cited Findings
Failure and scaling evidence:
- **"Why Do Multi-Agent LLM Systems Fail?"** Cemri, Pan, Yang et al. (UC Berkeley). arXiv **2503.13657** (v3 26 Oct 2025), NeurIPS 2025 Datasets & Benchmarks. MAST has 14 failure modes in 3 categories (system design, inter-agent misalignment, task verification), built from 150+ traces with κ=0.88. MAST-Data has 1,642 annotated traces across 7 frameworks. The authors note that "performance gains on popular benchmarks are often minimal." [S] — [arXiv](https://arxiv.org/abs/2503.13657); code [F] — [MAST repo](https://github.com/multi-agent-systems-failure-taxonomy/MAST)
- **"Towards a Science of Scaling Agent Systems"** Yubin Kim, Xin Liu et al. (Google Research and others). arXiv **2512.08296** (Dec 9, 2025; v2 Dec 17, 2025). Findings:
  - 180 configurations, 5 architectures (single-agent; independent, centralized, decentralized and hybrid multi-agent) and 4 benchmarks (Finance-Agent, BrowseComp-Plus, PlanCraft, Workbench).
  - Independent multi-agent systems amplify errors up to **17.2×**; centralized ones contain this to **4.4×**.
  - The predictive model picks the best coordination strategy for **87%** of unseen configurations.
  - v2 validates on GPT-5.2.
  - [S] — [arXiv](https://arxiv.org/abs/2512.08296); [Google Research blog](https://research.google/blog/towards-a-science-of-scaling-agent-systems-when-and-why-agent-systems-work/)
- **"Which Agent Causes Task Failures and When? On Automated Failure Attribution of LLM Multi-Agent Systems"** (Who&When), arXiv **2505.00212**. [F, as listed] — [Awesome-Graph-Engineering](https://github.com/DEEP-JLU/Awesome-Graph-Engineering)

Collaboration mechanisms, debate and ensembling:
- **"Multi-Agent Collaboration Mechanisms: A Survey of LLMs"**, arXiv **2501.06322** (2025). [F, as listed]
- **"Beyond Self-Talk: A Communication-Centric Survey of LLM-Based Multi-Agent Systems"**, arXiv **2502.14321** (Frontiers of CS 2026). [F, as listed]
- **"Beyond Individual Intelligence: Surveying Collaboration, Failure Attribution, and Self-Evolution in LLM-based Multi-Agent Systems"**, arXiv **2605.14892** (2026). [F, as listed]
- **"A Survey on Agent Workflow—Status and Future"**, arXiv **2508.01186**. [F, as listed]
- **"Mixture-of-Agents Enhances Large Language Model Capabilities"**, arXiv **2406.04692** (ICLR 2025). [F, as listed]
- **"Scaling Large Language Model-based Multi-Agent Collaboration"** (MacNet), arXiv **2406.07155** (ICLR 2025). [F, as listed]
- **"Dynamic Role Assignment for Multi-Agent Debate"**, arXiv **2601.17152** (2026). [F, as listed]
- **"More Agents Is All You Need"** (sampling-and-voting ensembles; Li et al., Tencent). The official code repo is MoreAgentsIsAllYouNeed/AgentForest. [F] — [repo](https://github.com/MoreAgentsIsAllYouNeed/AgentForest). arXiv **2402.05120**. [K]
- **"Improving Factuality and Reasoning in Language Models through Multiagent Debate"** (Du, Li, Torralba, Tenenbaum, Mordatch), arXiv **2305.14325** (2023, foundational). [K]
- **Agent-as-a-Judge: "Agent-as-a-Judge: Evaluate Agents with Agents"** (Zhuge et al., Meta/KAUST), arXiv **2410.10934**. [F, ID from official repo] — [metauto-ai/agent-as-a-judge](https://github.com/metauto-ai/agent-as-a-judge)

Automated design of agent systems and topologies:
- **ADAS, "Automated Design of Agentic Systems"** (Hu, Lu, Clune), arXiv **2408.08435** (ICLR 2025). [F] — [ShengranHu/ADAS](https://github.com/ShengranHu/ADAS)
- **AFlow, "AFlow: Automating Agentic Workflow Generation"**, arXiv **2410.10762** (ICLR 2025). [F] — [FoundationAgents/AFlow](https://github.com/FoundationAgents/AFlow)
- **GPTSwarm, "Language Agents as Optimizable Graphs"**, arXiv **2402.16823** (ICML 2024). [F] — [metauto-ai/GPTSwarm](https://github.com/metauto-ai/GPTSwarm)
- Further topology papers [F, as listed in the curated repo]:
  - **MaAS, "Multi-agent Architecture Search via Agentic Supernet"**, arXiv **2502.04180** (ICML 2026 per the list)
  - **MASS, "Multi-Agent Design: Optimizing Agents with Better Prompts and Topologies"**, arXiv **2502.02533** (ICLR 2026 per the list)
  - **G-Designer**, arXiv **2410.11782** (ICML 2025)
  - **AgentPrune, "Cut the Crap"**, arXiv **2410.02506** (ICLR 2025)
  - **SwarmAgentic** ("Towards fully automated agentic system generation via swarm intelligence"), arXiv **2506.15672** (EMNLP 2025)
  - **WebSwarm: Recursive Multi-Agent Orchestration for Deep-and-Wide Web Search**, arXiv **2607.08662** (2026)
  - **Swarm Skills**, arXiv **2605.10052** (2026)

Foundational framework papers [F, as listed in the curated repo or official READMEs]:
- AutoGen, arXiv **2308.08155**
- CAMEL, arXiv **2303.17760** (NeurIPS 2023)
- MetaGPT, arXiv **2308.00352** (ICLR 2024)
- ChatDev, arXiv **2307.07924** (ACL 2024)
- Magentic-One, arXiv **2411.04468**
- LLMCompiler (parallel function calling), arXiv **2312.04511** (ICML 2024)
- StateFlow, arXiv **2403.11322**
- Agent Workflow Memory, arXiv **2409.07429**

### Inferences
- The MAST "task verification" category and Google's centralized-vs-independent result both argue for a hackbot with a **central coordinator plus explicit verification nodes**, not a peer-to-peer swarm.
- Debate and voting ("More Agents", debate) are cheap ways to cut false positives at triage time, such as having k independent judges decide "is this a real vuln?". The orchestrator-worker results argue for using parallelism at discovery time.
- ADAS, AFlow and MaAS-style automated search is research-grade. It is useful later for tuning the hackbot's graph on a benchmark such as the XBOW validation suite (see Q6), but not for v1.

### Gaps
- I couldn't fetch the arXiv pages for 2402.05120, 2305.14325, 2406.01637 and 2501.13411 to confirm IDs and dates (they are marked [K]).
- I found no 2026 meta-analysis that pools multi-agent vs single-agent results across security tasks specifically.

---

## Q6. Multi-agent designs in offensive security: the orchestration view

### Takeaway
Shipped offensive systems converge on a few roles: a planner/coordinator, many short-lived task-specific solvers/executors with fresh context and iteration budgets, a strict verifier/oracle that confirms PoCs, and a reporter. Durable state lives in a task tree or knowledge graph. Peer-to-peer swarms are rare. Parallelism is used for discovery, and verification is centralized.

### Cited Findings
- **PentestGPT** (Deng et al., USENIX Security 2024; arXiv **2308.06782**). [F] — [GreyDGL/PentestGPT](https://github.com/GreyDGL/PentestGPT), [USENIX page](https://www.usenix.org/conference/usenixsecurity24/presentation/deng)
  - The original design had Reasoning, Generation and Parsing modules plus a "Pentesting Task Tree." [K]
  - The v1.0 "Agentic Upgrade" uses a multi-stage pipeline ("recon → exploit → walkthrough for CTF; asset discovery → vulnerability identification → report for pentests"), "feeding each stage's findings into the next." It "drives Claude Code or Codex," and reports an **86.5% success rate (90/104)** on an XBOW validation suite. [F]
- **CAI (Alias Robotics), "CAI: An Open, Bug Bounty-Ready Cybersecurity AI"** (arXiv **2504.06017**, Apr 2025). It is built on OpenAI Swarm and Agents SDK patterns. Its multi-agent docs contrast "orchestrating via LLM," where agents use tools and handoffs to delegate to sub-agents or call agents-as-tools, with "orchestrating via code" (structured outputs, deterministic chaining, guardrails, LLM-as-judge). [F] — [aliasrobotics/cai](https://github.com/aliasrobotics/cai); [CAI multi_agent.md](https://github.com/aliasrobotics/cai/blob/main/docs/multi_agent.md). `cai-framework` 0.5.10 was released 2025-12-19. [F]
- **PentAGI** (vxcontrol): "self-hosted multi-agent system for autonomous penetration testing; a team of specialized AI agents plans the work, runs 20+ professional tools… inside an isolated Docker sandbox." It is a "delegation system with specialized AI agents for research, development, and infrastructure tasks," with optional execution monitoring and a Neo4j/Graphiti knowledge graph. [F] — [vxcontrol/pentagi](https://github.com/vxcontrol/pentagi)
- **XBOW, "Agents Built From Alloys."** A solver is "an agentic loop set to work for a number of iterations" (budget about 80). Past that point, "it becomes more efficient to start a new solver agent unburdened by the misunderstandings and false assumptions accumulated over time." XBOW is model-provider agnostic. [S] — [XBOW blog](https://www.xbow.com/blog/alloy-agents). In the "alloy" idea, models alternate within one solver loop. [K for the mechanism detail]. I found **no primary XBOW source** describing a coordinator dispatching solvers. A competitor page claims "thousands of short-lived agents," which is third-party marketing. [S] — [strix.ai/vs/xbow](https://strix.ai/vs/xbow)
- **Strix**: "autonomous AI penetration testing agents… validate them through actual proofs-of-concept," with "multi-agent orchestration — teams of AI pentesters that collaborate and scale." It ships SKILL.md skills for Claude Code and Codex. [F] — [usestrix/strix](https://github.com/usestrix/strix)
- **Trail of Bits Buttercup** (AIxCC CRS) has an Orchestrator ("coordinates the overall task process"), a Seed Generator, a Patcher, and AI/ML-assisted fuzzing, plus a built-in LLM budget setting. [F] — [trailofbits/buttercup](https://github.com/trailofbits/buttercup)
- **Team Atlanta Atlantis** (AIxCC Final Competition CRS): repo verified, but its README gives little architectural detail. [F] — [Team-Atlanta/aixcc-afc-atlantis](https://github.com/Team-Atlanta/aixcc-afc-atlantis). **Theori RoboDuck** finals archive: a per-agent `MODEL_MAP`, an Agent Log Viewer, and a warning that the finals "afforded a large budget for LLMs." [F] — [theori-io/aixcc-afc-archive](https://github.com/theori-io/aixcc-afc-archive). Final placements (Team Atlanta 1st, Trail of Bits 2nd, Theori 3rd) are [K].
- **HPTSA, "Teams of LLM Agents can Exploit Zero-Day Vulnerabilities"** (Fang, Bindu, Gupta, Zhan, Kang; arXiv **2406.01637**, June 2024). A hierarchical planner explores and dispatches a team manager, which launches task-specific expert agents (e.g., SQLi, XSS). [K] — [arXiv](https://arxiv.org/abs/2406.01637)
- **VulnBot, "VulnBot: Autonomous Penetration Testing for a Multi-Agent Collaborative Framework"** (arXiv **2501.13411**, Jan 2025). It has role-specialized phases (recon, scanning, exploitation) and a penetration task graph. [K] — [arXiv](https://arxiv.org/abs/2501.13411). The guessed GitHub path returned 404, so the repo location is unknown.
- **Google Big Sleep**: Project Zero / DeepMind's evolution of "Naptime." The post "From Naptime to Big Sleep: Using Large Language Models To Catch Vulnerabilities In Real-World Code" (Nov 2024) describes a single-agent-with-specialized-tools design (code browser, debugger, sandbox) rather than a swarm. [K] — [Project Zero blog](https://googleprojectzero.blogspot.com/2024/10/from-naptime-to-big-sleep.html)
- 2026 research example: "Mako: A Self-Evolving Agentic Operating System (SE-AOS) for Autonomous Web Exploitation" (arXiv 2607.11288). [S, title/URL only] — [arXiv PDF](https://arxiv.org/pdf/2607.11288)

### Inferences
Hackbot reference orchestration, synthesized from the sources above:
1. **Scope/ROE gate (deterministic node):** parse the program policy. Every tool call is checked by a hook or guardrail, in the style of Claude Code `PreToolUse` hooks or OpenAI guardrails.
2. **Recon fan-out (parallel, read-only subagents):** each worker returns a compact summary of 1–2k tokens and writes raw data to the attack-surface graph (PentAGI and Graphiti style).
3. **Hypothesis planner (lead):** ranks graph nodes, such as endpoints or parameters, by bug-class likelihood, as HPTSA's planner does.
4. **Short-lived specialist solvers:** for example IDOR, SSRF or XSS. Each has a fresh context, an iteration cap (XBOW ~80) and a restart-rather-than-persist policy.
5. **Verifier/oracle (deterministic first, LLM-judge second):** replays the PoC and checks for a canary or OOB callback. This follows the C-compiler lesson that verifiers must be "nearly perfect," and the MAST verification-failure category.
6. **Triage ensemble:** k-vote judges to suppress false positives.
7. **Reporter:** writes the report, with HITL approval via an interrupt before submission.

Parallel writers should not share one target session. Treat authenticated, stateful exploitation as single-threaded.

### Gaps
- XBOW's coordinator and discovery architecture is not documented in any primary source I could reach. Big Sleep's current (2025–2026) architecture is also unverified.
- The details of Team Atlanta's and Theori's agent topologies need their blog or paper write-ups, which I could not retrieve.

---

## Q7. When NOT to use multi-agent: the evidence

### Takeaway
Avoid multi-agent when subtasks share state or have tight dependencies, when work is sequential or involves edits to the same artifact, when task value is low relative to 4–15× token costs, or when you can't verify results automatically. Independent swarms amplify errors (17.2×), and most measured gains on standard benchmarks are small (MAST). Start single-agent with good tools and context engineering, and add parallel subagents only for independent, read-heavy fan-out.

### Cited Findings
- Anthropic says multi-agent is a poor fit for "domains that require all agents to share the same context or involve many dependencies between agents," for "most coding tasks," and where task value doesn't justify ~15× chat tokens. [F] — [Anthropic multi-agent research](https://www.anthropic.com/research/multi-agent-research-system)
- Claude Code docs: "For sequential tasks, same-file edits, or work with many dependencies, a single session or subagents are more effective." Token costs "scale linearly" with teammates, and there are "diminishing returns." [F] — [Agent teams](https://code.claude.com/docs/en/agent-teams)
- Google/MIT et al.: independent multi-agent systems amplify errors up to 17.2× (centralized: 4.4×), and "more agents" often plateaus or hurts. [S] — [arXiv 2512.08296](https://arxiv.org/abs/2512.08296); [Google Research blog](https://research.google/blog/towards-a-science-of-scaling-agent-systems-when-and-why-agent-systems-work/)
- MAST: "performance gains on popular benchmarks are often minimal," and failures cluster in system design, inter-agent misalignment and task verification. [S] — [arXiv 2503.13657](https://arxiv.org/abs/2503.13657)
- Cognition (2025): context fragmentation, and "actions carry implicit decisions" that conflict across parallel agents. Cognition (2026): this still holds for parallel-writer swarms. [K]/[S] — [Don't Build Multi-Agents](https://cognition.ai/blog/dont-build-multi-agents); [Multi-Agents: What's Actually Working](https://cognition.ai/blog/multi-agents-working)
- LangChain docs: "not every complex task requires this approach—a single agent with the right (sometimes dynamic) tools and prompt can often achieve similar results." [F-src] — [LangChain multi-agent](https://docs.langchain.com/oss/python/langchain/multi-agent)
- Pydantic: "Don't use a nail gun unless you need a nail gun." [F] — [pydantic-graph docs](https://pydantic.dev/docs/ai/graph/graph/)
- Anthropic C compiler: on one monolithic task (the Linux kernel), "sixteen agents didn't help." Agents "overwrote each other's fixes" until an oracle split the work. [F] — [Building a C compiler](https://www.anthropic.com/research/building-c-compiler)
- XBOW: long-running single solvers accumulate "misunderstandings and false assumptions." The fix is a fresh solver, not more concurrent agents. [S] — [XBOW](https://www.xbow.com/blog/alloy-agents)

### Inferences
- Rule of thumb for the hackbot: use parallelism for independent breadth work (assets, endpoints, bug classes on disjoint endpoints), voting for triage, and a single thread for depth (one exploit chain or one authenticated state machine). If you cannot write an automatic verifier for a subtask, do not parallelize it, because errors amplify without verification.
- Budget guardrails are mandatory. Buttercup ships an LLM budget setting, and Theori warns that its finals budget was large. Effort-scaling rules in the lead prompt (Anthropic's 1 / 2–4 / 10+ subagents) prevent "50 subagents for a simple query."

### Gaps
- I found no published cost-per-valid-finding figures comparing single-agent and multi-agent pentest bots.

---

## Q8. Curated, verified resource list (in recommended learning order) and framework comparison

### Takeaway
More than 70 resources are listed below in learning order: foundations → the debate → graph runtimes → subagents and teams → protocols → research → offensive-security orchestration. Each has a verification tag. Prefer [F] and [S] items, and check [K] items again before publishing.

### Cited Findings
Format: **Title**: author/org, date, level. Why it matters. [tag] — link

**Stage 0: Agent-design foundations**
1. **Building effective AI agents**: Anthropic (Erik Schluntz and Barry Zhang, byline per prior knowledge), Dec 19, 2024, beginner (foundational, pre-2025). The canonical workflow-vs-agent taxonomy: prompt chaining, routing, parallelization, orchestrator-workers, evaluator-optimizer. [F] — [link](https://www.anthropic.com/research/building-effective-agents)
2. **Effective context engineering for AI agents**: Anthropic, Sep 29, 2025, intermediate. Explains why subagents return 1–2k-token distilled summaries, and covers compaction and memory. [F] — [link](https://www.anthropic.com/research/effective-context-engineering-for-ai-agents)
3. **Writing effective tools for AI agents**: Anthropic, Sep 11, 2025, intermediate. Tool design matters most for hackbot tool wrappers. [F] — [link](https://www.anthropic.com/research/writing-tools-for-agents)
4. **Demystifying evals for AI agents**: Anthropic, Jan 9, 2026, intermediate. Eval design for agent pipelines. [F] — [link](https://www.anthropic.com/research/demystifying-evals-for-ai-agents)

**Stage 1: The multi-agent debate (read in this order)**
5. **How we built our multi-agent research system**: Anthropic (Hadfield, Zhang, Lien, Scholz, Fox, Ford), Jun 13, 2025, intermediate. Orchestrator-worker numbers: +90.2%, 15× tokens, 80% variance from tokens. [F] — [link](https://www.anthropic.com/research/multi-agent-research-system)
6. **Don't Build Multi-Agents**: Cognition (Walden Yan), June 2025, intermediate. The context-sharing counterargument. [K] — [link](https://cognition.ai/blog/dont-build-multi-agents)
7. **How and when to build multi-agent systems**: LangChain (Harrison Chase), June 2025, intermediate. Reconciles the two sides through context engineering and the read-vs-write distinction. [K] — [link](https://blog.langchain.com/how-and-when-to-build-multi-agent-systems/)
8. **Multi-Agents: What's Actually Working**: Cognition (Walden Yan), Apr 22, 2026, intermediate. A 2026 revision: narrow flows work, but parallel-writer swarms still don't. [S] — [link](https://cognition.ai/blog/multi-agents-working)
9. **Towards a Science of Scaling Agent Systems**: Kim, Liu et al. (Google Research et al.), arXiv 2512.08296, Dec 2025, advanced. Quantitative scaling principles: 17.2× vs 4.4× error amplification, 87% strategy prediction. [S] — [arXiv](https://arxiv.org/abs/2512.08296); companion blog [S] — [Google Research](https://research.google/blog/towards-a-science-of-scaling-agent-systems-when-and-why-agent-systems-work/)
10. **Why Do Multi-Agent LLM Systems Fail?**: Cemri et al. (UC Berkeley), arXiv 2503.13657, NeurIPS 2025 D&B, advanced. The MAST taxonomy is a checklist for hackbot design reviews. [S] — [arXiv](https://arxiv.org/abs/2503.13657); [code](https://github.com/multi-agent-systems-failure-taxonomy/MAST) [F]

**Stage 2: Graph orchestration hands-on (LangGraph track)**
11. **Introduction to LangGraph (LangChain Academy)**: LangChain, course (current version undated), beginner–intermediate. Modules 1–5 cover graphs, state, memory, HITL and multi-agent; Module 6 covers deployment. [K course page; F repo] — [course](https://academy.langchain.com/courses/intro-to-langgraph); [repo](https://github.com/langchain-ai/langchain-academy)
12. **LangGraph overview (docs)**: LangChain, living docs, beginner. The official entry point for the 1.x docs. [F] — [link](https://docs.langchain.com/oss/python/langgraph/overview)
13. **LangChain and LangGraph 1.0 announcement**: LangChain, Oct 22, 2025, beginner. Release notes: stability focus, `prebuilt` deprecated. [S] — [link](https://www.langchain.com/blog/langchain-langgraph-1dot0)
14. **LangGraph Persistence**: LangChain docs, intermediate. Checkpointers and threads, needed for resumable multi-hour engagements. [F-src] — [link](https://docs.langchain.com/oss/python/langgraph/persistence)
15. **LangGraph Interrupts**: LangChain docs, intermediate. Human-in-the-loop approval gates before active exploitation or report submission. [F-src] — [link](https://docs.langchain.com/oss/python/langgraph/interrupts)
16. **LangChain Multi-agent guide**: LangChain docs, intermediate. The subagents, handoffs, skills, router and custom-workflow patterns, plus a selection matrix. [F-src] — [link](https://docs.langchain.com/oss/python/langchain/multi-agent)
17. **langgraph-supervisor-py**: LangChain, 0.0.31 (Nov 2025), intermediate. **De-recommended**: the README says to use supervisor-via-tools instead, but it is useful for studying the pattern. [F] — [link](https://github.com/langchain-ai/langgraph-supervisor-py)
18. **langgraph-swarm-py**: LangChain, 0.1.0 (Dec 2025), intermediate. A reference implementation of handoff-based swarms. [F] — [link](https://github.com/langchain-ai/langgraph-swarm-py)

**Stage 3: Alternative graph and workflow runtimes**
19. **Microsoft Agent Framework Version 1.0**: Microsoft, Apr 3, 2026, intermediate. GA of the SK and AutoGen successor, with graph workflows, checkpointing, HITL and orchestration patterns. [S] — [link](https://devblogs.microsoft.com/agent-framework/microsoft-agent-framework-version-1-0/)
20. **Microsoft Agent Framework overview (Learn docs)**: Microsoft, living docs, intermediate. [S] — [link](https://learn.microsoft.com/agent-framework/overview/agent-framework-overview)
21. **microsoft/agent-framework (repo and README)**: Microsoft, 1.23.0 (Oct 2026), intermediate. Python, .NET and Go. Links a 30-min intro video. [F] — [repo](https://github.com/microsoft/agent-framework); [video](https://www.youtube.com/watch?v=AAgdMhftj8w)
22. **AutoGen → Microsoft Agent Framework migration guide**: Microsoft, intermediate. Needed because AutoGen is archived. [F, as linked from the AutoGen README] — [link](https://learn.microsoft.com/en-us/agent-framework/migration-guide/from-autogen/)
23. **ADK Workflow Agents (Sequential, Parallel, Loop)**: Google, living docs, beginner–intermediate. Deterministic orchestration primitives. [F-src] — [link](https://google.github.io/adk-docs/agents/workflow-agents/)
24. **pydantic-graph docs**: Pydantic, v2.62 (Oct 2026), intermediate. Typed state machines, with an honest "nail gun" warning. [F] — [link](https://pydantic.dev/docs/ai/graph/graph/)
25. **LlamaIndex Workflows (workflows-py)**: LlamaIndex, 2.26.1 (Oct 2026), intermediate. Event-driven, step-based orchestration. [F] — [link](https://github.com/run-llama/workflows-py)
26. **Apache Burr**: Apache (incubating), 0.43.0 (Aug 2026), intermediate. A state-machine framework with tracking UI, moved from DAGWorks. [F] — [link](https://burr.apache.org/)
27. **Haystack**: deepset, 3.4.1 (Oct 2026), intermediate. Pipelines plus agents. More RAG-oriented, but useful for report and knowledge pipelines. [F] — [link](https://haystack.deepset.ai/overview/intro)
28. **DSPy**: Stanford NLP, 3.4.1 (Oct 2026), advanced. Prompt and program optimization for individual nodes, such as triage classifiers. [F] — [link](https://github.com/stanfordnlp/dspy)

**Stage 4: Subagents, handoffs and agent teams**
29. **Create custom subagents (Claude Code docs)**: Anthropic, living docs, beginner–intermediate. Context isolation, tool allowlists, worktree isolation, nesting depth and memory. [F] — [link](https://code.claude.com/docs/en/sub-agents)
30. **Orchestrate teams of Claude Code sessions (agent teams)**: Anthropic, living docs (experimental), intermediate. Lead, teammates, task list and mailbox, with sizing guidance and permission and trust rules. [F] — [link](https://code.claude.com/docs/en/agent-teams)
31. **Subagents in the Agent SDK**: Anthropic, living docs, intermediate. Programmatic subagents for building your own hackbot harness. [F] — [link](https://code.claude.com/docs/en/agent-sdk/subagents)
32. **Building agents with the Claude Agent SDK**: Anthropic/Claude, Sep 29, 2025, beginner. The agent loop (gather context → act → verify) and the SDK rationale. [F] — [link](https://claude.com/resources/articles/building-agents-with-the-claude-agent-sdk)
33. **Building a C compiler with a team of parallel Claudes**: Anthropic (Nicholas Carlini), Feb 5, 2026, advanced. Real 16-agent economics, orchestrator-less git locking, and the need for oracles and verifiers. [F] — [link](https://www.anthropic.com/research/building-c-compiler)
34. **Effective harnesses for long-running agents**: Anthropic, Nov 26, 2025, advanced. Harnesses for multi-context-window work. [F] — [link](https://www.anthropic.com/research/effective-harnesses-for-long-running-agents)
35. **OpenAI Agents SDK: Handoffs**: OpenAI, living docs, beginner–intermediate. Handoffs versus agents-as-tools, plus guardrails and HITL. [F, as linked] — [link](https://openai.github.io/openai-agents-python/handoffs/); [repo](https://github.com/openai/openai-agents-python)
36. **OpenAI Swarm (DEPRECATED, educational)**: OpenAI, Oct 2024, beginner. Historical. Shows handoffs in about 100 lines. Replaced by the Agents SDK. [F] — [link](https://github.com/openai/swarm)
37. **CrewAI docs (Crews and Flows)**: CrewAI, 1.15.x (Oct 2026), beginner. Role-based crews plus event-driven flows. [F] — [link](https://docs.crewai.com)
38. **AG2 docs**: AG2 community, 1.2.1 (Oct 2026), intermediate. The community continuation of AutoGen. v1.0 restructured the APIs. [F] — [link](https://docs.ag2.ai)
39. **Strands Agents**: AWS, 1.62.0 (Oct 2026), beginner–intermediate. A model-driven loop with built-in multi-agent patterns and MCP. [F] — [link](https://strandsagents.com)

**Stage 5: Protocols**
40. **Introducing the Model Context Protocol**: Anthropic, Nov 25, 2024, beginner (foundational). [F] — [link](https://www.anthropic.com/news/model-context-protocol)
41. **MCP Specification 2026-07-28**: MCP project (Agentic AI Foundation), Jul 28, 2026, intermediate–advanced. The current stateless revision. [F-src] — [link](https://modelcontextprotocol.io/specification/2026-07-28)
42. **MCP 2026-07-28: what's changing and how to migrate**: AAIF blog, 2026, intermediate. [S] — [link](https://aaif.io/blog/mcp-2026-07-28-whats-changing-and-how-to-migrate)
43. **Donating MCP to the Agentic AI Foundation**: Anthropic, Dec 9, 2025, beginner. The governance change. [F] — [link](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation)
44. **Code execution with MCP: Building more efficient agents**: Anthropic, Nov 4, 2025, intermediate. Progressive tool loading, 150k → 2k tokens (98.7%). [F] — [link](https://www.anthropic.com/research/code-execution-with-mcp)
45. **A2A Protocol Specification v1.0.0**: A2A Project (Linux Foundation), 2026, intermediate. Agent Cards, tasks, JSON-RPC. [F-src] — [link](https://a2a-protocol.org/v1.0.0/specification); [repo](https://github.com/a2aproject/A2A) [F]
46. **ACP (i-am-bee/acp), merged into A2A**: IBM/BeeAI, merged Aug 2025, beginner. Read only for history and the migration guide. [F] — [link](https://github.com/i-am-bee/acp)
47. **AG-UI: The Agent-User Interaction Protocol**: CopilotKit / AG-UI community (org attribution from prior knowledge), 1.0 Sept 2026, intermediate. Streams agent events to an operator console. [F] — [link](https://github.com/ag-ui-protocol/ag-ui)
48. **MCP Security Bench (MSB)**: arXiv 2510.15994, 2025, advanced. Attacks against MCP agents. [F, as listed] — [link](https://arxiv.org/abs/2510.15994)
49. **CaMeL: Defeating Prompt Injections by Design**: arXiv 2503.18813, 2025, advanced. A privileged/quarantined split relevant to hostile target content. [F, as listed] — [link](https://arxiv.org/abs/2503.18813)
50. **Hacking the AI Hackers via Prompt Injection**: CAI team (Alias Robotics), arXiv 2508.21669, 2025, advanced. Direct threat model for hackbots. [F, as listed] — [link](https://arxiv.org/abs/2508.21669)

**Stage 6: Research, from surveys to automated design**
51. **Multi-Agent Collaboration Mechanisms: A Survey of LLMs**: arXiv 2501.06322, 2025, intermediate. [F, as listed] — [link](https://arxiv.org/abs/2501.06322)
52. **Beyond Individual Intelligence: Surveying Collaboration, Failure Attribution, and Self-Evolution in LLM-based MAS**: arXiv 2605.14892, 2026, advanced. [F, as listed] — [link](https://arxiv.org/abs/2605.14892)
53. **Graph Engineering in the Era of LLM Agents**: Feng et al., arXiv 2608.21156, Aug 21, 2026, advanced. Defines the term. [F repo / S paper] — [arXiv](https://arxiv.org/abs/2608.21156); [resource list](https://github.com/DEEP-JLU/Awesome-Graph-Engineering)
54. **A Survey on Agent Workflow—Status and Future**: arXiv 2508.01186, 2025, intermediate. [F, as listed] — [link](https://arxiv.org/abs/2508.01186)
55. **Which Agent Causes Task Failures and When? (Who&When)**: arXiv 2505.00212, 2025, advanced. Failure attribution, useful for debugging hackbot traces. [F, as listed] — [link](https://arxiv.org/abs/2505.00212)
56. **ADAS: Automated Design of Agentic Systems**: Hu, Lu, Clune, arXiv 2408.08435, ICLR 2025, advanced. [F] — [link](https://arxiv.org/abs/2408.08435)
57. **AFlow: Automating Agentic Workflow Generation**: arXiv 2410.10762, ICLR 2025, advanced. [F] — [link](https://arxiv.org/abs/2410.10762)
58. **GPTSwarm: Language Agents as Optimizable Graphs**: arXiv 2402.16823, ICML 2024, advanced (foundational). [F] — [link](https://arxiv.org/abs/2402.16823)
59. **MaAS: Multi-agent Architecture Search via Agentic Supernet**: arXiv 2502.04180, advanced. [F, as listed] — [link](https://arxiv.org/abs/2502.04180)
60. **Multi-Agent Design: Optimizing Agents with Better Prompts and Topologies (MASS)**: arXiv 2502.02533, advanced. [F, as listed] — [link](https://arxiv.org/abs/2502.02533)
61. **SwarmAgentic: fully automated agentic system generation via swarm intelligence**: arXiv 2506.15672, EMNLP 2025, advanced. The swarm-intelligence (PSO-style) approach to system design. [F, as listed] — [link](https://arxiv.org/abs/2506.15672)
62. **Agent-as-a-Judge: Evaluate Agents with Agents**: Zhuge et al., arXiv 2410.10934, 2024, intermediate. Using agents to judge trajectories, which applies to judging hackbot runs. [F] — [link](https://arxiv.org/abs/2410.10934)
63. **More Agents Is All You Need**: Li et al., arXiv 2402.05120 (TMLR 2024), intermediate (foundational). Sampling and voting scale with the number of agents. [F repo / K ID] — [arXiv](https://arxiv.org/abs/2402.05120); [code](https://github.com/MoreAgentsIsAllYouNeed/AgentForest)
64. **Improving Factuality and Reasoning in Language Models through Multiagent Debate**: Du et al., arXiv 2305.14325, 2023, intermediate (foundational). [K] — [link](https://arxiv.org/abs/2305.14325)
65. **Magentic-One: A generalist multi-agent system for solving complex tasks**: Microsoft Research, arXiv 2411.04468, 2024, intermediate. An Orchestrator with a task/progress ledger; it became a MAF orchestration pattern. [F, as listed] — [link](https://arxiv.org/abs/2411.04468)
66. **GraphRAG: From Local to Global: A Graph RAG Approach to Query-Focused Summarization**: Microsoft, arXiv 2404.16130, 2024, intermediate. [F] — [paper](https://arxiv.org/abs/2404.16130); [repo](https://github.com/microsoft/graphrag)

**Stage 7: Offensive-security orchestration (design patterns only)**
67. **PentestGPT**: Deng et al., USENIX Security 2024 / arXiv 2308.06782, plus v1.0 agentic pipeline, intermediate. Module and task-tree design, now a staged pipeline at 86.5% on the XBOW suite. [F] — [repo](https://github.com/GreyDGL/PentestGPT)
68. **CAI: An Open, Bug Bounty-Ready Cybersecurity AI**: Alias Robotics, arXiv 2504.06017, Apr 2025, intermediate. Handoffs, agents-as-tools, and LLM-vs-code orchestration. [F] — [repo](https://github.com/aliasrobotics/cai); [paper](https://arxiv.org/abs/2504.06017)
69. **PentAGI**: vxcontrol, ongoing, intermediate. A self-hosted multi-agent pentest system with a Neo4j/Graphiti knowledge graph. [F] — [link](https://github.com/vxcontrol/pentagi)
70. **Agents Built From Alloys**: XBOW, 2025 (exact date unverified), intermediate. The solver-loop budget and fresh-solver restarts. [S] — [link](https://www.xbow.com/blog/alloy-agents)
71. **Strix**: usestrix, ongoing, beginner–intermediate. Multi-agent pentest teams with PoC validation, distributed as skills for Claude Code and Codex. [F] — [link](https://github.com/usestrix/strix)
72. **Buttercup CRS**: Trail of Bits, AIxCC 2025, advanced. Orchestrator, seed generator and patcher, plus LLM budget controls. [F] — [link](https://github.com/trailofbits/buttercup)
73. **Atlantis CRS**: Team Atlanta, AIxCC Finals 2025, advanced. [F] — [link](https://github.com/Team-Atlanta/aixcc-afc-atlantis)
74. **Theori AIxCC AFC archive (RoboDuck)**: Theori, 2025, advanced. Per-agent model map and agent log viewer. [F] — [link](https://github.com/theori-io/aixcc-afc-archive)
75. **Teams of LLM Agents can Exploit Zero-Day Vulnerabilities (HPTSA)**: Fang, Bindu, Gupta, Zhan, Kang, arXiv 2406.01637, 2024, intermediate. Hierarchical planner → manager → expert agents. [K] — [link](https://arxiv.org/abs/2406.01637)
76. **VulnBot: Autonomous Penetration Testing for a Multi-Agent Collaborative Framework**: arXiv 2501.13411, 2025, intermediate. [K] — [link](https://arxiv.org/abs/2501.13411)
77. **From Naptime to Big Sleep**: Google Project Zero / DeepMind, Nov 2024, intermediate. [K] — [link](https://googleprojectzero.blogspot.com/2024/10/from-naptime-to-big-sleep.html)

### Inferences

**Framework comparison table.** Status and version columns come from PyPI/npm data fetched 2026-10-27 [F]. The "Learning curve" and "Hackbot relevance" columns are my judgment.

| Framework | Status (late Oct 2026) | Language(s) | Orchestration model | Best for | Learning curve | Hackbot relevance |
|---|---|---|---|---|---|---|
| **LangGraph** | Active. 1.0 Oct 2025, now 1.2.18. JS 1.5.3 | Python, JS/TS | Explicit state graph, checkpointers, interrupts, subgraphs | Durable, auditable, HITL-gated workflows | Medium–High | **Very high**: control graph, checkpoints, approval gates |
| **Microsoft Agent Framework** | Active. 1.0 GA Apr 2026, now 1.23.0 | Python, .NET (Go mentioned) | Graph workflows (executors and edges) plus sequential, concurrent, handoff, group-chat and Magentic patterns | Enterprise and Azure shops, successor to SK and AutoGen | Medium–High | High if .NET or Azure. Magentic-style ledger orchestrator |
| **Google ADK** | Active. 2.13.0 (2.0 May 2026) | Python (plus others) | LLM agents plus deterministic Sequential, Parallel and Loop agents | Gemini and GCP, simple deterministic pipelines | Low–Medium | Medium–High: Parallel agent for recon fan-out |
| **OpenAI Agents SDK** | Active, pre-1.0 (0.24.0) | Python, TS | Handoffs, agents-as-tools, guardrails, sandbox agents | Lightweight multi-agent with tracing | Low | High (CAI and Agency Swarm build on it) |
| **Claude Agent SDK / Claude Code** | Active, pre-1.0 SDK (0.2.x py / 0.3.x npm). Agent teams experimental | Python, TS (CLI) | Subagents (isolated context, ≤3 nesting levels), hooks, permissions, worktrees, agent teams | Agent harnesses that need strong tool, permission and hook control | Low–Medium | **Very high**: subagent isolation, PreToolUse scope hooks, skills |
| **CrewAI** | Active. 1.0 Oct 2025, now 1.15.30 | Python | Role-based Crews plus event-driven Flows | Fast prototyping of role teams | Low | Medium: quick prototypes. Less fine control |
| **AG2** | Active (community). 1.0 Jul 2026 restructure | Python | Protocol-driven agents. Classic GroupChat moved to ag2-classic | AutoGen-style conversations | Medium | Low–Medium (API churn) |
| **AutoGen** | **Archived** (no longer maintained) | Python, .NET | Conversation and group chat | Historical and research only | n/a | Avoid for new work |
| **Semantic Kernel** | Maintenance (still releases, 1.45.0) | .NET, Python, Java | Plugins and planners | Existing SK estates | Medium | Low (migrate to MAF) |
| **Pydantic AI + pydantic-graph** | Active. 2.62.0 | Python | Typed agents plus a typed FSM graph | Type-safe pipelines | Medium | Medium–High: typed finding schemas |
| **LlamaIndex Workflows** | Active. 2.26.1 | Python (TS exists) | Event-driven steps | RAG-heavy flows | Medium | Medium |
| **Apache Burr** | Incubating, 0.43 | Python | State machine plus tracking UI | Inspectable state machines | Low–Medium | Medium |
| **Haystack** | Active. 3.4.1 | Python | Pipelines plus agents | RAG and search pipelines | Medium | Low–Medium (reporting and knowledge) |
| **DSPy** | Active. 3.4.1 | Python | Optimizable LM programs | Prompt and program optimization | High | Medium (node-level tuning) |
| **Strands Agents** | Active. 1.62.0, TS 1.x | Python, TS | Model-driven loop plus built-in multi-agent patterns | AWS and Bedrock, "own-loop" replacement | Low | Medium |
| **CAMEL** | 0.2.x research | Python | Role-playing societies | Research and simulation | Medium | Low |
| **MetaGPT** | PyPI stalled (0.8.2, Mar 2025) | Python | SOP-based role pipelines | Research | Medium | Low |
| **ChatDev 2.0 (DevAll)** | Relaunched Jan 2026 | Python and web | Zero-code multi-agent platform | Experiments | Low | Low |
| **Agency Swarm** | Active. 1.11.0 | Python | Built on the OpenAI Agents SDK, communication flows | Agency-style org charts | Low | Low–Medium |
| **Swarms (kyegomez)** | Active. 16.x (self-described "production-ready") | Python | Many prebuilt workflow types, GraphWorkflow | Quick experiments | Low | Low (verify claims) |
| **Letta** | V1 server retired, pivoted to Letta Code | Python, TS | Stateful memory agents | Long-term memory research | Medium | Low–Medium (memory ideas) |
| **OpenAI Swarm** | **Deprecated** (replaced by the Agents SDK) | Python | Handoffs | Learning only | Very low | Do not use |
| **Mastra** | Active. 1.79.0 | TS | Workflows plus agents | TS shops | Low–Medium | Medium (if TS) |

**Recommended hackbot stack (inference):** LangGraph or MAF for the control graph, with Postgres checkpointing and HITL interrupts. Claude Agent SDK or OpenAI Agents SDK workers for short-lived specialist solvers. MCP tool servers, loaded progressively and treated as hostile-input boundaries. A Neo4j or Graphiti attack-surface graph as shared state. A deterministic PoC verifier plus k-judge triage. AG-UI for the operator console. A2A only for external federation.

### Gaps
- YouTube talks (the LangChain channel and the AI Engineer channel) could not be verified, because YouTube was unreachable and the search budget was exhausted. The only video I can cite is the MAF intro video linked from the official README, and its exact title is unconfirmed. The report writer should add verified talks, for example Anthropic's or LangChain's AI Engineer World's Fair 2025/2026 sessions, only after checking them.
- The LangChain Academy page, the Cognition 2025 post, the LangChain 2025 blog, HPTSA, VulnBot, the multi-agent debate paper, the "More Agents" arXiv ID and Big Sleep are all [K]. Check them again before final publication.
- GitHub stars, contributors and commit cadence could not be collected (GitHub API access was not enabled for this session).
