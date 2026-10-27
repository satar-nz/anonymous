# AI Hackbots / AI Pentest Agents and AI-Augmented Offensive Tooling: Ecosystem Catalog (checked 2026-10-27)

> **Method and verification note (read first).** All GitHub star counts, licenses, languages, `created_at`, `pushed_at` (last push to any branch, used as the "last commit" proxy) and `archived` flags were pulled from the GitHub REST search API (via the GitHub MCP tool) on **2026-10-27**. Architecture claims come from each repo's README, fetched the same day. PyPI and Kali GitLab metadata came from their JSON/raw endpoints the same day. **Limits of this session:** the shared WebSearch budget ran out after 4 searches, and the egress proxy refused (HTTP 403) every non-code host: xbow.com, portswigger.net, caido.io, hackerone.com, horizon3.ai, runsybil.com, aisle.com, depthfirst.com, zeropath.com, daniel.haxx.se, thehackernews.com, arxiv.org, techcrunch.com and news.ycombinator.com. So commercial-vendor facts come from (a) the 4 web searches that did run, (b) GitHub-hosted secondary sources (curated lists), and (c) clearly flagged prior knowledge (marked **[UNVERIFIED-THIS-SESSION]**, with no URLs made up). Treat every benchmark number below as self-reported unless it says otherwise.

## 1. Open-source AI pentest agents and frameworks: status, architecture, what to learn

### Takeaway
The open-source field is large, fast-moving and very star-inflated. The projects that are maintained, have substantive architectures and are worth studying are Strix, Shannon, PentAGI, PentestGPT v1, Decepticon, RedAmon, PentestAgent, hackingBuddyGPT, deadend-cli and RAPTOR. **CAI was archived on 2026-08-28** and is now unmaintained, with a commercial successor. Many "XBOW benchmark 97–98%" claims mean little now, because XBOW itself has declared its benchmark saturated (about 100%).

### Cited Findings

#### 1a. Core catalog of open-source agents (metadata checked 2026-10-27)

| # | Project | URL | Maintainer/org | License | Lang | Stars (2026-10-27) | Created | Last push | Status |
|---|---|---|---|---|---|---|---|---|---|
| 1 | Strix | https://github.com/usestrix/strix | usestrix (strix.ai) | Apache-2.0 | Python | ~72.3k (8.1k forks, 505 open issues) | 2025-08-05 | 2026-10-27 | Very active; PyPI `strix-agent` 1.7.0 uploaded 2026-10-05 |
| 2 | Shannon | https://github.com/KeygraphHQ/shannon | KeygraphHQ (keygraph.io) | AGPL-3.0 | TypeScript | ~49.5k | 2025-09-27 | 2026-10-27 | Very active |
| 3 | PentAGI | https://github.com/vxcontrol/pentagi | vxcontrol (pentagi.com) | MIT | Go | ~32.6k | 2025-01-06 | 2026-10-22 | Active |
| 4 | PentestGPT | https://github.com/GreyDGL/PentestGPT | Gelei Deng (GreyDGL; NTU) | MIT | Python | ~16.1k | 2023-02-27 | 2026-07-14 | Maintained (v1.0 "Agentic Upgrade"); PyPI `pentestgpt` stuck at 0.8.0 (2023-06-15), so install from the repo |
| 5 | HexStrike AI (MCP) | https://github.com/0x4m4/hexstrike-ai | 0x4m4 | MIT | Python | ~13.1k | 2025-07-10 | 2026-08-03 | Semi-active (v6.0; "v7.0 coming soon", no date); 125 open issues |
| 6 | CAI (Cybersecurity AI) | https://github.com/aliasrobotics/cai | Alias Robotics | "Other" (mixed MIT plus research-only proprietary) | Python | ~9.8k | 2025-03-31 | 2026-08-22 | **ARCHIVED 2026-08-28**; PyPI `cai-framework` frozen at 0.5.10 (2025-12-19) |
| 7 | Decepticon | https://github.com/BitterSecurity/Decepticon | BitterSecurity (the PurpleAILAB/Decepticon path named in the brief was not found via GitHub search) | Apache-2.0 | Python | ~6.5k | 2025-06-06 | 2026-10-27 | Very active |
| 8 | RAPTOR | https://github.com/gadievron/raptor | Gadi Evron, Daniel Cuthbert, Thomas Dullien (Halvar Flake), Michael Bargury, John Cartwright | MIT per README (GitHub reports NOASSERTION) | Python | ~3.9k | 2025-10-17 | 2026-10-27 | Very active (v3.1.0) |
| 9 | RedAmon | https://github.com/samugit83/redamon | samugit83 | MIT | Python | ~3.2k | 2025-12-29 | 2026-10-27 | Very active |
| 10 | PentestAgent | https://github.com/GH05TCREW/pentestagent | GH05TCREW | MIT | Python | ~3.2k | 2025-05-15 | 2026-10-26 | Active |
| 11 | Pentest Copilot (BugBase) | https://github.com/bugbasesecurity/pentest-copilot | BugBase | MIT | TypeScript | ~1.5k | 2025-03-08 | 2026-08-14 | Maintained (browser-based assistant) |
| 12 | NeuroSploit | https://github.com/JoasASantos/NeuroSploit | Joas A. Santos (CyberSecurityUP/NeuroSploit was not found via GitHub search) | MIT | Rust | ~1.5k | 2025-08-17 | 2026-10-19 | Active (rewritten in Rust) |
| 13 | hackingBuddyGPT | https://github.com/ipa-lab/hackingBuddyGPT | ipa-lab (TU Wien; hackingbuddy.ai) | MIT | Python | ~1.3k | 2023-08-02 | 2026-10-24 | Active research project |
| 14 | Nebula | https://github.com/berylliumsec/nebula | Beryllium Security | BSD-2-Clause | Python | ~1.1k | 2023-09-30 | 2026-10-26 | Active; now described as a "Cybersecurity Harness"; PyPI `nebula-ai` 2.0.0 (2026-07-11) |
| 15 | deadend-cli | https://github.com/straylabs-ai/deadend-cli | Stray Labs (Yassine Bargach, per PyPI) | AGPL-3.0 | Python | ~319 | 2025-07-22 | 2026-08-07 | Releases paused while a "better version for the harness" is built; PyPI 0.1.15 (2026-04-12) |
| 16 | VulnBot | https://github.com/KHenryAegis/VulnBot | KHenryAegis (paper repo) | MIT | Python | ~203 | 2025-01-20 | 2026-10-13 | Low activity, research code |
| 17 | AutoPentest | https://github.com/JuliusHenke/autopentest | Julius Henke | NOASSERTION | Python | ~75 | 2024-06-06 | 2026-09-05 | Small research CLI |
| 18 | HackSynth | https://github.com/aielte-research/HackSynth | ELTE research | AGPL-3.0 | Python | ~319 | 2024-11-27 | 2025-06-24 | **Stale** (no push in 16 months) |
| 19 | ARTEMIS | https://github.com/Stanford-Trinity/ARTEMIS | Stanford Trinity project | Apache-2.0 | Rust | ~548 | 2025-07-07 | 2026-04-14 | Research; forked from OpenAI Codex (`codex-rs`) with a supervisor |
| 20 | Cyber-AutoAgent | https://github.com/westonbrown/Cyber-AutoAgent | westonbrown | MIT | TypeScript | ~550 | 2025-05-26 | 2025-11-29 | **ARCHIVED** |

Sources for metadata: each repo URL above (GitHub API, 2026-10-27); PyPI JSON for `strix-agent`, `pentestgpt`, `cai-framework`, `nebula-ai` and `deadend-cli` — [PyPI strix-agent](https://pypi.org/project/strix-agent/), [PyPI pentestgpt](https://pypi.org/project/pentestgpt/), [PyPI cai-framework](https://pypi.org/project/cai-framework/), [PyPI nebula-ai](https://pypi.org/project/nebula-ai/), [PyPI deadend-cli](https://pypi.org/project/deadend-cli/).

#### 1b. Newer 2025–2026 entrants found via GitHub search (metadata 2026-10-27)

| Project | URL | Stars | License | Created → last push | One-line (from repo) |
|---|---|---|---|---|---|
| CyberStrike | https://github.com/CyberStrikeus/CyberStrike | ~3.3k | AGPL-3.0 | 2026-02-14 → 2026-10-20 | "Open-source AI-powered offensive security harness" (its topic list is identical to Strix's) |
| T3MP3ST | https://github.com/elder-plinius/T3MP3ST | ~6.6k | AGPL-3.0 | 2026-07-02 → 2026-09-08 | "autonomous red teaming platform; multi-agent offensive-security meta-harness" (by elder-plinius / "Pliny") |
| Cairn | https://github.com/oritera/Cairn | ~4.6k | AGPL-3.0 | 2026-04-19 → 2026-09-07 | "general-purpose AI state-space search engine, validated first on autonomous penetration testing"; schedules Claude Code / Codex / Pi workers |
| ARTEX | https://github.com/trhhubsd/ARTEX | ~2.0k (983 forks) | AGPL-3.0 | **2026-10-08** → 2026-10-19 | Go + Next.js multi-agent pentest system; claims a win in Baidu's agent+ attack-defense challenge |
| pentest-ai | https://github.com/0xSteph/pentest-ai | ~1.8k | MIT | 2026-04-04 → 2026-10-09 | "proves every finding. Machine oracles re-run each exploit" |
| LuaN1aoAgent | https://github.com/SanMuzZzZz/LuaN1aoAgent | ~1.4k | AGPL-3.0 | 2025-12-04 → 2026-09-15 | Graph-based "plan-execute-reflect" autonomous pentest agent (Guangzhou University per the Yeti list; claims XBOW >90%) |
| PentesterFlow agent | https://github.com/PentesterFlow/agent | ~1.4k | Apache-2.0 | 2026-05-31 → 2026-08-31 | Terminal agentic offensive security, human-in-the-loop, Burp integration |
| cain-agent | https://github.com/cdxiaodong/cain-agent | ~944 | Apache-2.0 | 2026-03-18 → 2026-10-14 | Pentest agent "Built on Claude Agent SDK", with a cloud module (AWS/Azure/GCP plus Chinese clouds) |
| Tsec-Hackathon | https://github.com/Yeti-791/Tsec-Hackathon | ~856 | — | 2026-03-03 | Showcase of the Tencent Cloud intelligent-penetration hackathon's top agents |
| AIDA | https://github.com/Vasco0x4/AIDA | ~506 | AGPL-3.0 | 2026-01-31 → 2026-10-21 | "Turn any LLM into an autonomous pentester"; Docker/Exegol-based, MCP |
| mantishack | https://github.com/deonmenezes/mantishack | ~505 | — | 2026-04-05 | Rust agent harness for bug bounty (109 open issues) |
| BoxPwnr | https://github.com/0ca/BoxPwnr | ~467 | AGPL-3.0 | 2025-01-26 → 2026-07-24 | Benchmarks LLMs/agent strategies on HTB, THM, PortSwigger Labs, Cybench, picoCTF |
| pentest-harness | https://github.com/S1N6H/pentest-harness | ~438 | — | 2026-08-26 | Self-hosted, bring-your-own-model pentest harness |
| cyber-harness | https://github.com/chainreactors/cyber-harness | ~348 | — | 2026-05-07 | "pi-like agent for cyber security — single binary" (Go) |
| DorkAgent | https://github.com/yee-yore/DorkAgent | ~329 | — | 2025-02-26 | LLM agent for Google dorking |
| MAPTA | https://github.com/arthurgervais/mapta | ~112 | MIT | 2025-08-28 (single push) | Code for the MAPTA paper (arXiv 2508.20816); reports XBOW 76.9% per the Yeti list |

Sources: the repo URLs above; the GitHub topic search [topic:ai-pentesting](https://github.com/topics/ai-pentesting); and [Yeti-791/Awesome-Offensive-AI-Agentic-Landscape](https://github.com/Yeti-791/Awesome-Offensive-AI-Agentic-Landscape) for LuaN1ao and MAPTA claims.

#### 1c. Architecture details from the READMEs (fetched 2026-10-27)

- **Strix**: multi-agent "Graph of Agents" ("Specialized AI agents for recon, exploitation, and post-exploitation" that "share discoveries, chain vulnerabilities"). Tools: a Caido-based HTTP interception proxy, an automated browser (XSS/CSRF/clickjacking/auth bypass), an interactive shell, and a Python "Custom Exploit Runtime". It runs in a Docker sandbox ("First run automatically pulls the sandbox Docker image"). Validation is by PoC ("Real exploit validation - working PoCs, not false positives"). CI mode: `strix -n -t ./ --scan-mode quick` exits non-zero on findings. Multi-provider LLMs (OpenAI, Anthropic, Google/Vertex, Bedrock, Azure, OpenRouter, DeepSeek, Moonshot, local via `LLM_API_BASE`). LiteLLM, Nuclei and Playwright are credited in the acknowledgements. The README shows no benchmark numbers. — [usestrix/strix](https://github.com/usestrix/strix)
- **Shannon**: a white-box pipeline. Recon and vulnerability analysis plus agentic code analysis, then reconciliation and dedupe into an "exploitation queue", then exploitation agents against the live app, validation, and reporting. "Only live-validated vulnerabilities become Shannon pentest findings or count toward CI/CD severity gates." Specialized agents cover Injection, XSS, SSRF, AuthN and AuthZ. Each scan runs in an ephemeral Docker container, with source mounted read-only. Framework migration: "1.9.0 is the final release built on the Claude Agent SDK". It now builds on the Pi harness and credits Pi, Playwright CLI and Mantis. LLMs: Anthropic, OpenAI, xAI and Bedrock built in, plus Ollama/vLLM/LM Studio/OpenRouter/LiteLLM through a custom base URL. "A full run can take roughly 1 to 1.5 hours." "Do not run Shannon against production systems." Black-box testing is only in the commercial Pro/Enterprise editions. — [KeygraphHQ/shannon](https://github.com/KeygraphHQ/shannon)
- **PentAGI**: Go backend (GraphQL) with a React/TS frontend. Agent roles named include Orchestrator/Primary Agent, Pentester, Coder, Installer, Searcher, Enricher, Memorist, Generator, Reporter, Adviser (a mentor that monitors execution), Reflector (steps in when the model fails to emit tool calls), Planner and Refiner. Memory is PostgreSQL + pgvector ("Results and successful approaches are kept in PostgreSQL with pgvector"), with an optional Graphiti knowledge graph on Neo4j and chain summarization. It claims "20+ professional tools" (nmap, Metasploit, sqlmap) in an isolated Docker sandbox, an isolated scraper browser, and search via Tavily/Firecrawl/Perplexity/DuckDuckGo/Google/Sploitus/SearXNG. Observability: Langfuse, and OpenTelemetry into VictoriaMetrics/Jaeger/Loki/Grafana. LLMs: OpenAI, Anthropic, Gemini, Bedrock, Ollama, DeepSeek, GLM, Kimi, Qwen, MiniMax, Mistral, xAI, OpenRouter, DeepInfra and vLLM. Self-reported "2x improvement in result quality" from execution monitoring (tested on Qwen3.5-27B), at 2–3x the tokens. It explicitly says it is "not a CALDERA-style Breach and Attack Simulation". — [PentAGI README](https://raw.githubusercontent.com/vxcontrol/pentagi/main/README.md)
- **PentestGPT v1.0**: now "Drives Claude Code or Codex to run tools and reason without human intervention". CTF mode runs recon → exploit → walkthrough; pentest mode runs asset discovery → vuln ID → report. Legacy mode (`pentestgpt-legacy`) keeps the USENIX Security 2024 design: three LLM sessions (reasoning, generation, parsing) plus a "Pentesting Task Tree", with a multi-provider model registry. Claim: "86.5% success rate (90/104 benchmarks) on an XBOW validation-suite experiment in December 2025… a historical research result". It includes telemetry, with an opt-out. — [GreyDGL/PentestGPT](https://github.com/GreyDGL/PentestGPT)
- **Decepticon**: a red-team kill-chain agent. "16 specialist agents organized by kill chain phase, with a fresh context window per objective"; an orchestrator "owns OPPLAN"; LangGraph for orchestration and persistent state; LiteLLM gateway with tiered fallback across about 10 providers, including Ollama. Commands run in persistent tmux sessions, so interactive tools like `msfconsole` work. BloodHound CE, Sliver C2 and Ghidra start on demand. A Neo4j knowledge graph stores findings. "All commands run inside a Kali Linux sandbox on a dedicated operational network (`sandbox-net`)", separate from the management net. It can be exposed as an MCP server (`decepticon mcp serve`). Claim: 102/104 (98.08%) on the XBOW validation benchmarks. — [BitterSecurity/Decepticon](https://github.com/BitterSecurity/Decepticon)
- **RedAmon**: "A LangGraph-based autonomous agent that reasons about the graph, selects security tools via MCP". It uses the ReAct pattern. A Neo4j attack-surface graph is the primary state, queried with NL→Cypher. Recon is a parallel fan-out/fan-in pipeline (Subfinder, Amass, Naabu, Masscan, httpx, Nmap, Katana, FFuf, Nuclei). "All MCP tools run inside a Kali Linux sandbox container". Human "Tool Confirmation" Allow/Deny gates and Rules of Engagement are built in. "CypherFix" agents open GitHub fix PRs. MCP works both ways (48 tools behind 17 permissions). Claim: "101 / 104 (97.1%) of the XBOW web-security benchmark fully black-box." — [RedAmon README](https://raw.githubusercontent.com/samugit83/redamon/master/README.md)
- **PentestAgent**: Python TUI with modes `/assist`, `/agent`, `/crew` ("Orchestrator spawns specialized workers") and `/interact`. It uses a LiteLLM wrapper. As an MCP client and server, its `spawn_mcp_agent` starts isolated child agents. A **RAG tool optimizer** replaces any MCP server exposing more than 128 tools with "a single" retrieval meta-tool. It has RAG knowledge sources, persistent `loot/notes.json`, a "Shadow Graph" of notes, a Docker or Kali sandbox, and playbooks (e.g., `thp3_web`). — [PentestAgent README](https://raw.githubusercontent.com/GH05TCREW/pentestagent/main/README.md)
- **deadend-cli**: a supervisor-subagent design (Requester/HTTP, Shell, Python subagents) with two phases (recon → exploit). "Confidence-based decision making (fail <20%, expand 20-60%, refine 60-80%, validate >80%)". Built on LiteLLM + Instructor (structured outputs) + pgvector, with a Pyodide/WASM Python sandbox and a Docker shell. Claim: "~80% on the full XBOW validation benchmark with Kimi K2.5" at about $122 total API cost, black-box, evaluated January 2026 (the repo description says 81%). OpenAI GPT-5.x is "Under investigation" because of tool-execution issues via LiteLLM. — [deadend-cli README](https://raw.githubusercontent.com/straylabs-ai/deadend-cli/main/README.md)
- **CAI (archived)**: agents are "derived from `openai/openai-agents-python`" (MIT), with OpenAI Swarm credited as inspiration. It supports "300+ [models] via LiteLLM", MCP, Phoenix tracing, handoffs, HITL and a "four-layer" prompt-injection guardrail (arXiv 2508.21669). Self-reported claims: 3,600× faster and 156× cheaper than humans (arXiv 2504.06017), #1 Neurogrid CTF (41/45 flags), and Dragos OT CTF rank #1 in hours 7–8. The README says "`CAI` is no longer under active development… No further releases, bug fixes, security patches or support". The successor is the commercial "Cybersecurity Superintelligence (CSI)". The license mixes MIT with "proprietary additions licensed for research purposes only", and `CAI_LICENSE_OFF=1` bypasses the startup license check. — [aliasrobotics/cai](https://github.com/aliasrobotics/cai)
- **HexStrike AI**: two parts. `hexstrike_server.py` is an HTTP API on :8888, with endpoints including `/api/command` ("Execute arbitrary commands with caching"). `hexstrike_mcp.py` is the MCP bridge. It claims "150+" tools (network 25+, web 40+, cloud 20+, binary 25+, CTF 20+, OSINT 20+) and 12 named "agents" (IntelligentDecisionEngine, BugBountyWorkflowManager, CTFWorkflowManager, CVEIntelligenceManager, AIExploitGenerator…). The README itself warns "Consider implementing authentication for production deployments" and "AI agents can execute arbitrary security tools". — [0x4m4/hexstrike-ai](https://github.com/0x4m4/hexstrike-ai)
- **RAPTOR**: a Claude Code-based framework with a two-layer design. A Python execution layer (`raptor.py`, `core/`, `engine/`) runs tools, and a Claude Code decision layer uses `CLAUDE.md`, `.claude/commands/`, `.claude/skills/`, `.claude/agents/` and `tiers/` personas. Integrations: Semgrep, Coccinelle, CodeQL (optional), AFL++/libFuzzer, Ghidra/radare2/Frida, Z3, rr, ffuf, nuclei. Commands include `/exploit`, `/patch`, `/oss-forensics` and `/sca`, with staged validation (Stages A–F). Subprocesses are sandboxed with namespaces, Landlock and seccomp. It costs "$2-8" for about 50 findings (default $10 cap). The authors admit "RAPTOR is mostly AI-generated code". — [RAPTOR README](https://raw.githubusercontent.com/gadievron/raptor/main/README.md)
- **ARTEMIS**: "This project uses OpenAI Codex as a base, forked from commit c221eab", with a `supervisor` that spawns Codex instances. The README carries no comparative results. — [Stanford-Trinity/ARTEMIS](https://github.com/Stanford-Trinity/ARTEMIS)
- **hackingBuddyGPT**: tagline "Helping Ethical Hackers use LLMs in 50 Lines of Code or less"; a "Minimal (~50 LOC) research framework for LLM-driven Linux priv-esc and web pentesting (FSE'23)". — [ipa-lab/hackingBuddyGPT](https://github.com/ipa-lab/hackingBuddyGPT); [scadastrangelove list](https://github.com/scadastrangelove/awesome-ai-security-tools)
- **Nebula**: "AI pentesting CLI assistant with local-LLM support (Llama-3.1, Mistral, DeepSeek)" (per a curated list); the repo is now titled "Cybersecurity Harness". — [berylliumsec/nebula](https://github.com/berylliumsec/nebula); [scadastrangelove list](https://github.com/scadastrangelove/awesome-ai-security-tools)
- **VulnBot**: "Autonomous Penetration Testing for A Multi-Agent Collaborative Framework", with RAG support. — [KHenryAegis/VulnBot](https://github.com/KHenryAegis/VulnBot)

#### 1d. Comparison table: top open-source agents

| Agent | Stars (10/27/26) | Last push | Topology | Orchestration framework | LLMs | Execution / sandbox | State / memory | Validation approach | Self-reported score | Best for studying |
|---|---|---|---|---|---|---|---|---|---|---|
| Strix | 72.3k | 2026-10-27 | Multi-agent "graph of agents" | Custom (LiteLLM credited) | Multi-provider + local | Docker sandbox; Caido proxy, browser, shell, Python runtime | Shared discoveries between agents | Working PoC per finding; CI exit code | none in README | Web-app hackbot design, proxy/browser-in-sandbox, CI integration |
| Shannon | 49.5k | 2026-10-27 | Phased pipeline with specialist vuln-class agents | Claude Agent SDK (≤1.9.0) → Pi harness | Anthropic/OpenAI/xAI/Bedrock + custom base URL | Ephemeral Docker per scan; read-only source | Exploitation queue (deduped candidates) | "Only live-validated vulnerabilities" count | none in README | White-box→live-exploit pipeline; validation gate; SDK migration |
| PentAGI | 32.6k | 2026-10-22 | Orchestrator plus ~13 role agents (adviser, reflector, planner…) | Custom Go | ~15 providers + vLLM/Ollama | Docker sandbox, scraper browser | pgvector + Graphiti/Neo4j; chain summarization | Adviser/reflector supervision | "2x quality" (internal) | Production system design: roles, memory, observability |
| PentestGPT v1 | 16.1k | 2026-07-14 | Pipeline driving external coding-agent CLIs; legacy 3-session task tree | Claude Code / Codex CLIs; legacy custom | Claude/Codex; legacy multi-provider | Docker image bundling CLIs | Pentesting Task Tree (legacy) | — | 86.5% XBOW (Dec 2025) | How the task-tree idea evolved into "drive a coding agent" |
| Decepticon | 6.5k | 2026-10-27 | Orchestrator + 16 kill-chain specialists | LangGraph + LiteLLM | ~10 providers incl. Ollama | Kali sandbox on isolated `sandbox-net`; tmux sessions | Neo4j KG; OPPLAN | — | 98.08% XBOW | LangGraph multi-agent, fresh context per objective, interactive-tool handling |
| RedAmon | 3.2k | 2026-10-27 | Single ReAct agent over graph + recon pipeline + fix agents | LangGraph | 5 providers / "400+ models" | Kali sandbox, ephemeral per job | Neo4j attack graph (NL→Cypher) | Human Allow/Deny gates, RoE | 97.1% XBOW | Graph-as-state, approval gates, MCP both ways, auto-fix PRs |
| PentestAgent | 3.2k | 2026-10-26 | Assist / agent / crew (orchestrator + workers) | Custom + LiteLLM | Any LiteLLM model | Docker/Kali | Notes + "Shadow Graph" + RAG | — | — | MCP tool-explosion fix (RAG over >128 tools), self-spawning sub-agents |
| CAI (archived) | 9.8k | 2026-08-22 | Agents + handoffs, HITL | Fork of OpenAI Agents SDK | 300+ via LiteLLM | Varies | — | Guardrails (4-layer PI defense) | CTF wins (vendor) | Agents-SDK patterns, guardrails. **Unmaintained; research-only license** |
| HexStrike AI | 13.1k | 2026-08-03 | MCP server + HTTP API; "12 agents" helpers | Custom Python | Client-side (Claude, Copilot, Cursor…) | Runs on the host; no auth by default | Cache/telemetry | — | — | Wrapping many CLI tools as MCP tools (and what not to do security-wise) |
| deadend-cli | 319 | 2026-08-07 | Supervisor + 3 subagents | Custom + LiteLLM + Instructor | Kimi K2.5, Claude, GLM-5… | Pyodide/WASM + Docker | pgvector | Confidence thresholds | ~80% XBOW, ~$122 | Cheap open-weight-model agent; confidence-driven control loop |
| RAPTOR | 3.9k | 2026-10-27 | Claude Code + sub-agents/skills + Python exec layer | Claude Code (other providers for analysis layer) | Claude; OpenAI/Gemini/Mistral/Ollama | Namespaces/Landlock/seccomp | Projects tracking findings | Staged A–F exploitability pipeline | — | Turning a coding agent into a security agent; code→exploit→patch |
| hackingBuddyGPT | 1.3k | 2026-10-24 | Minimal single-agent loops | Custom | Multiple | SSH/local targets | Minimal | — | — | Smallest readable core loop; research baseline |

Sources: rows draw on the repo pages and READMEs cited in 1c.

### Inferences
- The architectures are converging on a pattern: **(1) an isolated sandbox (Docker/Kali) → (2) a planner/orchestrator plus specialist agents with fresh contexts → (3) persistent structured state (graph DB or vector store) → (4) a hard validation gate (PoC/oracle/live exploit) before anything counts as a finding.** Strix, Shannon, Decepticon, RedAmon and pentest-ai all emphasize step 4.
- There is a visible shift from bespoke orchestration toward **driving general coding-agent harnesses** (Claude Code, Codex, Pi). PentestGPT v1, ARTEMIS (Codex fork), Shannon (Claude Agent SDK → Pi), RAPTOR, cain-agent and Cairn all do this. A builder can now prototype on a coding-agent harness instead of writing a ReAct loop from scratch.
- Star counts are poor proxies for quality in 2026. ARTEX gained ~2k stars and 983 forks within 19 days of creation. CyberStrike copied Strix's topic list verbatim. Treat stars as signal for "visible", not "good".

### Gaps
- No "Zen" AI pentest repo could be identified on GitHub. The name may be wrong, or the project may be private or commercial.
- No reliable source was found for Strix's or Shannon's own XBOW benchmark numbers; their current READMEs show none. (A Shannon "96%"-type claim may exist in older docs; not verified.)
- Release/tag dates were not retrievable (the GitHub releases API was blocked for this session); `pushed_at` is used as the maintenance signal.
- NeuroSploit's current Rust architecture and Pentest Copilot's internals were not examined.
- No independent (third-party) replication of any agent's benchmark claim was found.

## 2. MCP servers, proxy integrations (Burp/Caido), reverse-engineering MCPs and AI-in-scanner features

### Takeaway
The "tool layer" for hackbots is maturing. PortSwigger ships an official Burp MCP server. Caido has official AI skills plus community MCPs and AI plugins. Kali packages an MCP server, Nuclei has native `-ai` and `-llm` features, and GhidraMCP and IDA Pro MCP dominate RE. Several early AI-Burp extensions (BurpGPT, ReconAIzer) are now stale.

### Cited Findings

**MCP servers for offensive tooling**

| Item | URL | Stars | License | Created → last push | Notes |
|---|---|---|---|---|---|
| Burp MCP Server (official PortSwigger) | https://github.com/PortSwigger/mcp-server | ~1.26k | GPL-3.0 | 2025-04-02 → 2026-10-27 | Kotlin Burp extension. An SSE MCP server on `http://127.0.0.1:9876`, plus a bundled stdio proxy jar for clients like Claude Desktop. A toggle controls whether tools "can edit Burp configuration files". It has a Claude Desktop auto-installer; the README documents a build-from-source path. — [README](https://raw.githubusercontent.com/PortSwigger/mcp-server/main/README.md) |
| Kali `mcp-kali-server` (official Kali package) | https://gitlab.com/kalilinux/packages/mcp-kali-server | — | Expat (MIT) | packaging repo created 2025-08-07; last activity 2026-10-20 | Packaged by Kali (Sophie Brun). Upstream is **Wh0am123/MCP-Kali-Server**. It depends on python3-flask, python3-mcp and `sudo`, and recommends nmap, gobuster, dirb, nikto, sqlmap, metasploit-framework, hydra, john, wpscan and enum4linux. Changelog 2026-10-20: "Fix CommandExecutor: accept string and list". — [debian/control](https://gitlab.com/kalilinux/packages/mcp-kali-server/-/raw/kali/master/debian/control), [debian/changelog](https://gitlab.com/kalilinux/packages/mcp-kali-server/-/raw/kali/master/debian/changelog) |
| MCP-Kali-Server (upstream) | https://github.com/Wh0am123/MCP-Kali-Server | ~862 | MIT | 2025-04-07 → 2026-03-17 | "MCP configuration to connect AI agent to a Linux machine" |
| HexStrike AI | https://github.com/0x4m4/hexstrike-ai | ~13.1k | MIT | 2025-07-10 → 2026-08-03 | 150+ tools; see section 1 |
| MetasploitMCP | https://github.com/GH05TCREW/MetasploitMCP | ~749 | Apache-2.0 | 2025-04-13 → 2026-02-05 | "MCP Server for Metasploit"; slowing (no push for ~9 months) |
| pentest-ai | https://github.com/0xSteph/pentest-ai | ~1.8k | MIT | 2026-04-04 → 2026-10-09 | "Offensive-security MCP server with 200+ wrapped tools" — [scadastrangelove list](https://github.com/scadastrangelove/awesome-ai-security-tools) |
| BloodHound-MCP-AI | https://github.com/MorDavid/BloodHound-MCP-AI | ~383 | none | 2025-04-04 → 2025-06-02 | **Stale**; connects BloodHound to an LLM via MCP |
| awsome_kali_MCPServers | https://github.com/ccq1/awsome_kali_MCPServers | ~107 | — | 2025-03-31 | Set of Kali-tailored MCP servers |
| HackerOne GraphQL MCP (official HackerOne org) | https://github.com/Hacker0x01/hackerone-graphql-mcp-server | ~19 | — | created 2025-06-12 | "MCP server for the HackerOne GraphQL API" |
| h1-brain | https://github.com/PatrikFehrenbach/h1-brain | ~355 | — | created 2026-03-10 | Community MCP connecting AI assistants to HackerOne |

**Burp Suite AI integrations**
- **Burp AI (PortSwigger, commercial, built into Burp Suite Professional)**: **[UNVERIFIED-THIS-SESSION]** Prior knowledge says it launched in 2025 with AI features such as "Explore Issue", an Explainer, AI-enabled extensions via the Montoya API, broken-access-control false-positive reduction and login-sequence repair, metered by "AI credits". portswigger.net was unreachable, so none of this could be confirmed.
- **burp-ai-agent** (six2dez): Kotlin extension that adds "built-in MCP tooling, AI-assisted analysis, privacy controls, passive and active scanning". About 1.56k stars, MIT, created 2026-01-27, last push 2026-10-21. — [six2dez/burp-ai-agent](https://github.com/six2dez/burp-ai-agent)
- **BurpGPT** (aress31): GPT passive-scan extension, about 2.4k stars, Apache-2.0. Last push **2024-06-09** (stale for about 2.4 years). — [aress31/burpgpt](https://github.com/aress31/burpgpt)
- **ReconAIzer** (hisxo): OpenAI-in-Burp recon helper, about 905 stars, no license. Last push **2023-05-03** (abandoned). — [hisxo/ReconAIzer](https://github.com/hisxo/ReconAIzer)
- Trail of Bits ships a `burpsuite-project-parser` skill that "Searches and extracts data from Burp Suite project files". — [trailofbits/skills README](https://raw.githubusercontent.com/trailofbits/skills/main/README.md)

**Caido AI integrations**
- **caido/skills** (official Caido org): "Caido AI Skills". About 285 stars, MIT, created 2026-03-03, last push 2026-10-27. — [caido/skills](https://github.com/caido/skills)
- **Shift** (caido-community): "Seamless AI Integration into Caido". About 58 stars, MIT, created 2024-12-09, last push 2026-10-27. — [caido-community/shift](https://github.com/caido-community/shift)
- **caido-mcp-server** (c0tton-fluff, community): Go MCP server that "Enables AI assistants like Claude Code to browse, analyse, and interact with HTTP traffic". About 136 stars, MIT, last push 2026-10-21. — [c0tton-fluff/caido-mcp-server](https://github.com/c0tton-fluff/caido-mcp-server)
- **drift** (six2dez): pipes a local Claude Code/Gemini/Codex/Copilot CLI "through 18 MCP tools" for Caido. About 42 stars. — [six2dez/drift](https://github.com/six2dez/drift)
- **Ebka-Caido-AI** (Slonser, about 79 stars) and **Chatio** (caido-community, about 39 stars) are AI assistant plugins. — [Slonser/Ebka-Caido-AI](https://github.com/Slonser/Ebka-Caido-AI), [caido-community/Chatio](https://github.com/caido-community/Chatio)
- Strix embeds Caido as its HTTP interception proxy inside the sandbox. — [usestrix/strix](https://github.com/usestrix/strix)
- No first-party "Caido MCP" repo under the `caido` org was found; the MCP servers are community-built.

**Nuclei (ProjectDiscovery) AI features** (verified from the current README)
- `-ai, -prompt string    generate and run template using ai prompt`
- `-llm    enable llm matchers and extractors (semantic matching)`, with `-llm-provider` (openai, ollama, llamacpp, vllm, lmstudio, groq, openrouter, together, orcarouter), `-llm-max-calls` (default 5000) and `-llm-cache`
- A cloud "AI powered Nuclei Templates Editor" is linked
- Nuclei has ~32.4k stars and is MIT-licensed; last push 2026-10-27. — [projectdiscovery/nuclei README](https://raw.githubusercontent.com/projectdiscovery/nuclei/dev/README.md)

**Reverse-engineering MCPs**
- **GhidraMCP** (LaurieWired): "MCP Server for Ghidra". About 11.6k stars, Apache-2.0, Java, created 2025-03-23, last push 2026-10-10, 93 open issues. — [LaurieWired/GhidraMCP](https://github.com/LaurieWired/GhidraMCP)
- **IDA Pro MCP** (mrexodia, the x64dbg author): "AI-powered reverse engineering assistant that bridges IDA Pro with language models through MCP". About 13.2k stars, MIT, created 2025-03-25, last push 2026-09-26. — [mrexodia/ida-pro-mcp](https://github.com/mrexodia/ida-pro-mcp)
- **binaryninja-mcp** (MCPPhalanx): about 51 stars; last push 2025-05-13 (stale). — [MCPPhalanx/binaryninja-mcp](https://github.com/MCPPhalanx/binaryninja-mcp)
- Decepticon and RAPTOR both wire Ghidra (and RAPTOR radare2/Frida) into their agents. — [Decepticon](https://github.com/BitterSecurity/Decepticon), [RAPTOR](https://raw.githubusercontent.com/gadievron/raptor/main/README.md)

**AI code-vulnerability hunters (adjacent, but study-worthy)**
- **Vulnhuntr** (Protect AI): "Zero shot vulnerability discovery using LLMs". About 2.8k stars, AGPL-3.0. Last push **2025-02-06** (stale for about 20 months). The PyPI name `vulnhuntr` (v1.2.2, 2026-05-05) points to a **third-party fork, diaz3618/vulnhuntr**, not Protect AI. — [protectai/vulnhuntr](https://github.com/protectai/vulnhuntr), [PyPI vulnhuntr](https://pypi.org/project/vulnhuntr/)
- **OpenAI Codex Security**: "Codex Security CLI and TypeScript SDK for finding, validating, and fixing security vulnerabilities". About 11.2k stars, Apache-2.0, created 2026-07-13, active. Scans require Codex Security access. — [openai/codex-security](https://github.com/openai/codex-security); [scadastrangelove list](https://github.com/scadastrangelove/awesome-ai-security-tools)
- **Google Mantis**: "toolkit for AI coding agents to autonomously find, reproduce, and patch vulnerabilities". About 2.6k stars, Apache-2.0, created 2026-06-15, active. — [google/mantis](https://github.com/google/mantis)
- **Vercel deepsec**: "security harness for finding vulnerabilities in your codebase powered by coding agents". About 8.3k stars, Apache-2.0. — [vercel-labs/deepsec](https://github.com/vercel-labs/deepsec)
- **Visa Vulnerability Agentic Harness**: about 3.0k stars, created 2026-06-05, active. — [visa/visa-vulnerability-agentic-harness](https://github.com/visa/visa-vulnerability-agentic-harness)
- **AISLE nano-analyzer**: "Minimal three-stage LLM pipeline (context → scan → skeptical triage) for zero-day discovery in C/C++". About 369 stars. — [weareaisle/nano-analyzer](https://github.com/weareaisle/nano-analyzer); [scadastrangelove list](https://github.com/scadastrangelove/awesome-ai-security-tools)
- **Hadrian OpenHack**: "file-based workspace for source-guided whitebox security review" that orchestrates agents through recon → hunt → validate → evidence → report. About 748 stars, MIT. — [hadriansecurity/OpenHack](https://github.com/hadriansecurity/OpenHack)
- **GitHub Security Lab seclab-taskflow-agent**: YAML-driven taskflows for triaging CodeQL/SAST alerts and filtering false positives. About 282 stars, active. — [GitHubSecurityLab/seclab-taskflow-agent](https://github.com/GitHubSecurityLab/seclab-taskflow-agent)
- **Google Big Sleep** (Project Zero + DeepMind) is **closed source**; no public repo exists. **[UNVERIFIED-THIS-SESSION]** Prior knowledge: Google reported Big Sleep finding a SQLite bug in late 2024 and further OSS vulnerabilities in 2025.

### Inferences
- For a web hackbot, the realistic integration points are Burp's official MCP (SSE on localhost) or Caido, either embedded the way Strix does it or through community MCP servers. Both expose raw HTTP history and replay, which agents need for IDOR/authz work.
- Packaging in Kali (with a `sudo` dependency and a Flask command executor) means "AI MCP command runners" are becoming default attack-box infrastructure. Their safety properties (authentication, allow-lists) lag well behind.

### Gaps
- Could not verify Burp AI feature names, launch dates or pricing (portswigger.net blocked); also could not confirm whether the Burp MCP server is in the BApp Store.
- Could not verify any official Caido MCP or Caido AI launch announcement (caido.io blocked).
- ProjectDiscovery's "Neo" or other agentic products were not checked.

## 3. Claude Code / Codex-based hacking setups (skills, plugins, GitHub Actions, PAI/Fabric, Haddix)

### Takeaway
These are real and actively maintained: Trail of Bits' `trailofbits/skills` marketplace, Anthropic's `claude-code-security-review` GitHub Action (its last push was 2026-02-11, so it is slowing), RAPTOR, Transilience communitytools, Daniel Miessler's Fabric and LifeOS (PAI was renamed to LifeOS), and Jason Haddix's TBHM/Arcanum taxonomy. The skills space also has very-high-star repos with misleading names (e.g., "Anthropic-Cybersecurity-Skills", which is not by Anthropic). Vet them before installing.

### Cited Findings
- **trailofbits/skills**: "Trail of Bits skills for security research, vulnerability detection, and audit workflows". About 7.8k stars, CC-BY-SA-4.0, created 2026-01-14, last push 2026-10-27. Install with `/plugin marketplace add trailofbits/skills`; Codex can also consume Claude plugin marketplaces (`codex plugin marketplace add trailofbits/skills`). Skills include `fp-check` ("Verifies whether security findings are false positives"), `variant-analysis`, `differential-review`, `static-analysis` (CodeQL/Semgrep/SARIF), `semgrep-rule-creator`, `insecure-defaults`, `sharp-edges`, `supply-chain-risk-auditor`, `agentic-actions-auditor`, `burpsuite-project-parser`, `c-review`, `rust-review`, `patching-bugs`, `post-patch-validation`, `firebase-apk-scanner`, `yara-authoring`, `constant-time-analysis`, `zeroize-audit` and `second-opinion` (gets reviews from Codex or Antigravity). — [trailofbits/skills](https://github.com/trailofbits/skills), [README](https://raw.githubusercontent.com/trailofbits/skills/main/README.md)
- **anthropics/claude-code-security-review**: "An AI-powered security review GitHub Action using Claude to analyze code changes for security vulnerabilities." About 6.6k stars, MIT, created 2025-08-04, **last push 2026-02-11** (about 8.5 months; slowing or maintenance-mode). — [anthropics/claude-code-security-review](https://github.com/anthropics/claude-code-security-review)
- **RAPTOR**: "turns Claude Code into a general-purpose AI offensive/defensive security agent" via CLAUDE.md, rules, sub-agents and skills. About 3.9k stars, very active. A community "looping" skill derivative exists: dinosn/raptor-loop-hunt (about 270 stars). — [gadievron/raptor](https://github.com/gadievron/raptor), [dinosn/raptor-loop-hunt](https://github.com/dinosn/raptor-loop-hunt)
- **transilienceai/communitytools**: "Open-source Claude Code skills, agents, and slash commands for AI-powered penetration testing, bug bounty hunting, and security research". About 595 stars, MIT, last push 2026-07-29. — [transilienceai/communitytools](https://github.com/transilienceai/communitytools)
- **Other Claude Code security skill packs** (stars as of 2026-10-27): Masriyan/Claude-Code-CyberSecurity-Skill (about 522; 22 skills), agamm/claude-code-owasp (about 415; OWASP Top 10:2025/ASVS 5.0), AgentSecOps/SecOpsAgentKit (about 228), Stickman230/claude-pentest (about 130), and MyuriKanao/src-hunter-skill (about 645; **archived**; Chinese SRC/bug bounty playbooks). — [Masriyan](https://github.com/Masriyan/Claude-Code-CyberSecurity-Skill), [agamm](https://github.com/agamm/claude-code-owasp), [AgentSecOps](https://github.com/AgentSecOps/SecOpsAgentKit), [Stickman230](https://github.com/Stickman230/claude-pentest), [MyuriKanao](https://github.com/MyuriKanao/src-hunter-skill)
- **mukul975/Anthropic-Cybersecurity-Skills**: "817 structured cybersecurity skills for AI agents", mapped to ATT&CK/NIST/ATLAS/D3FEND. About **35.1k stars** in 8 months, Apache-2.0, last push 2026-08-31. It is **a personal repo, not an Anthropic project**, despite the name. — [mukul975/Anthropic-Cybersecurity-Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills)
- **zhaoxuya520/reverse-skill**: RE/pentest "Skill Router Pack" for Claude Code/Kiro/Cursor/Cline. About **43.5k stars** since 2026-05-13, written in PowerShell, with "on-demand toolchain bootstrapping". — [zhaoxuya520/reverse-skill](https://github.com/zhaoxuya520/reverse-skill)
- **Coding-agent-native offensive harnesses**: cain-agent (Claude Agent SDK), Cairn (schedules Claude Code/Codex/Pi workers), PentestGPT v1 (drives Claude Code/Codex), Shannon (Claude Agent SDK through 1.9.0), ARTEMIS (Codex fork), and humaidhahm/opencode-pentester (opencode). — repos cited in section 1; [humaidhahm/opencode-pentester](https://github.com/humaidhahm/opencode-pentester)
- **Daniel Miessler**: **Fabric** ("modular system for solving specific problems using a crowdsourced set of AI prompts") has about 44.5k stars, MIT, Go, last push 2026-10-26. **PAI → LifeOS**: the v6.0.0 notes say "the project was PAI (Personal AI Infrastructure); this is the same system, renamed". About 19.6k stars, last push 2026-09-04. Runs best on Claude Code ("Daniel builds and runs it on Claude Code"). He also maintains SecLists (about 74.4k stars; a wordlist source used by many agents). — [danielmiessler/Fabric](https://github.com/danielmiessler/Fabric), [danielmiessler/LifeOS](https://github.com/danielmiessler/LifeOS), [danielmiessler/SecLists](https://github.com/danielmiessler/SecLists)
- **Jason Haddix**: public repos include **tbhm** ("The Bug Hunters Methodology"; about 4.4k stars), **SubreconGPT** (about 121 stars; created 2023-05) and devops-attack-surface (2025-10). His company's **Arcanum-Sec/arc_pi_taxonomy** ("The Arcanum Prompt Injection Taxonomy"; about 789 stars; last push 2026-06-29) is the main public AI artifact. — [jhaddix/tbhm](https://github.com/jhaddix/tbhm), [jhaddix/SubreconGPT](https://github.com/jhaddix/SubreconGPT), [Arcanum-Sec/arc_pi_taxonomy](https://github.com/Arcanum-Sec/arc_pi_taxonomy)
- **Defensive tooling for the skills supply chain**: **NVIDIA/SkillSpector** is a "Security scanner for AI agent skills… prompt injection, data exfiltration, and supply-chain risks in Claude Code, Codex, and MCP skills". About 22.7k stars, created 2026-03-21. **Pantheon-Security/medusa** advertises "Claude Code compromise detection — vet .claude/ hooks, permissions & skills before you clone" (about 1.0k stars). — [NVIDIA/SkillSpector](https://github.com/NVIDIA/SkillSpector), [Pantheon-Security/medusa](https://github.com/Pantheon-Security/medusa)

### Inferences
- The highest-signal, lowest-risk Claude Code security setup to study is **Trail of Bits skills plus RAPTOR**. Both come from known security practitioners, are open about their limits, and are actively maintained. Treat third-party skill packs with tens of thousands of stars and fewer than 12 months of history as untrusted until reviewed. Skills and hooks execute code in your environment.

### Gaps
- Could not confirm whether Anthropic's GitHub Action is superseded by a newer first-party product (anthropic.com not checked). **[UNVERIFIED-THIS-SESSION]** Prior knowledge suggests Anthropic released "Claude Code Security" in early 2026 and announced "Claude Mythos" / "Project Glasswing" in 2026; the Yeti-791 list also names "Claude Mythos" and "Project Glasswing" — [Yeti-791 list](https://github.com/Yeti-791/Awesome-Offensive-AI-Agentic-Landscape).
- Jason Haddix's paid or course prompts and any "Arcanum" agent tooling beyond the PI taxonomy were not found publicly.
- No canonical "claude-code for bug bounty" workflow repo from a major hunter was identified, beyond the community packs listed.

## 4. Awesome lists to bookmark (existence and maintenance verified)

### Takeaway
The best maintained lists for this topic are **scadastrangelove/awesome-ai-security-tools** (refreshed 2026-10-26), **raphabot/awesome-cybersecurity-agentic-ai**, **ottosulin/awesome-ai-security**, **ElNiak/awesome-ai-cybersecurity**, **tmylla/Awesome-LLM4Cybersecurity** (papers) and **Yeti-791/Awesome-Offensive-AI-Agentic-Landscape** (includes commercial and China vendors). Tenable's list is archived. The exact name "awesome-hacking-with-ai" was not found.

### Cited Findings

| List | URL | Stars | License | Last push | Notes |
|---|---|---|---|---|---|
| awesome-ai-security-tools | https://github.com/scadastrangelove/awesome-ai-security-tools | ~1.69k | NOASSERTION | 2026-10-25 | Created 2026-05-27; "Most recently refreshed entry: 2026-10-26"; per-entry star counts/dates; sections cover pentest agents, MCP, RE, AI SAST, triage |
| awesome-cybersecurity-agentic-ai | https://github.com/raphabot/awesome-cybersecurity-agentic-ai | ~607 | MIT | 2026-10-27 | "Agentic-AI cybersecurity tools and security MCP servers" (per scadastrangelove) |
| awesome-ai-security | https://github.com/ottosulin/awesome-ai-security | ~1.58k | MIT | 2026-10-25 | Broad AI security (offense and defense) |
| awesome-ai-cybersecurity | https://github.com/ElNiak/awesome-ai-cybersecurity | ~162 | CC0-1.0 | 2026-10-27 | Actively updated, smaller |
| Awesome-LLM4Cybersecurity | https://github.com/tmylla/Awesome-LLM4Cybersecurity | ~1.78k | none | 2026-08-20 | "600+ papers on LLMs for cybersecurity" (per scadastrangelove) |
| Awesome-Offensive-AI-Agentic-Landscape | https://github.com/Yeti-791/Awesome-Offensive-AI-Agentic-Landscape | ~402 | MIT | 2026-09-10 | Open-source, papers, benchmarks, plus "Top 20" international and China commercial vendors with funding claims |
| awesome-llm-security | https://github.com/corca-ai/awesome-llm-security | ~1.72k | none | 2025-08-20 | Securing LLMs; slowing (14 months) |
| awesome-mcp-security | https://github.com/Puliczek/awesome-mcp-security | ~743 | none | 2026-03-03 | MCP security; moderately stale (about 8 months) |
| awesome-llm-cybersecurity-tools (Tenable) | https://github.com/tenable/awesome-llm-cybersecurity-tools | ~483 | none | 2024-04-10 | **ARCHIVED** |
| awesome-ai-pentesting | https://github.com/skyvanguard/awesome-ai-pentesting | ~96 | — | 2026 (created 2026-02-17) | Small, specific to AI pentest + MCP |
| awesome-security-agent-harnesses | https://github.com/Ed-Marcavage/awesome-security-agent-harnesses | ~35 | — | created 2026-08-03 | Harnesses, sandboxes, security MCPs, benchmarks |
| caido-community/awesome | https://github.com/caido-community/awesome | ~51 | — | — | Caido plugin ecosystem (includes AI plugins) |

Sources: the URLs above (GitHub API, 2026-10-27); [scadastrangelove README](https://raw.githubusercontent.com/scadastrangelove/awesome-ai-security-tools/main/README.md).

### Inferences
- For a builder, the scadastrangelove list is the most useful single bookmark. It annotates each entry with stars and update date, which matches this field's need for freshness checks. The Yeti list is the only one found that covers commercial and Chinese vendors, but its funding claims are unsourced aggregations.

### Gaps
- "awesome-hacking-with-ai" under that exact name was not located via GitHub search.

## 5. Commercial AI hackbots and AI-augmented offensive products

### Takeaway
The commercial tier (XBOW, RunSybil, Horizon3.ai NodeZero, Pentera, Hadrian, Terra, Ethiack, Synack Sara, HackerOne Hai, Burp AI, Caido, plus AI-SAST/vuln-discovery firms such as AISLE, Depthfirst and ZeroPath) is heavily funded. Public architecture detail is thin, and most milestones could not be verified from primary sources this session because vendor sites were egress-blocked. What follows is labeled by evidence quality.

### Cited Findings
**Verified via search results or GitHub this session**
- **XBOW funding (conflicting secondary data).** multiples.vc lists a **$35M Series C in May 2026**, about **$272M** cumulative over five rounds, a **$1B valuation**, a **$20M seed in July 2024**, and investors including Sequoia, Altimeter, Nat Friedman, Northzone, DNX and Samsung Ventures. The search tool itself noted internal inconsistencies on that page (two Series C entries; a $1B valuation attached to the June 2025 Series B). — [multiples.vc XBOW](https://multiples.vc/private-comps/xbow). **This conflicts with** the Yeti-791 list, which says "2026-03 … $120M Series C (DFJ Growth/Northzone led)… valuation >$1B" — [Yeti-791 list](https://github.com/Yeti-791/Awesome-Offensive-AI-Agentic-Landscape). Neither is a primary source.
- **XBOW benchmark retired.** XBOW's own validation-benchmarks repo (104 Docker-Compose CTF challenges, levels 1–3, Apache-2.0, about 740 stars) is now marked outdated: "Industry-standard performance against this benchmark set is now approximately 100%" and they "are no longer useful for discriminating between models or attack frameworks." — [xbow-engineering/validation-benchmarks](https://github.com/xbow-engineering/validation-benchmarks)
- **HackerOne.** Its official GitHub org publishes `hackerone-graphql-mcp-server` (created 2025-06-12). Its `hai-on-hackerone` repo (created 2024-06-05) is **archived**. — [Hacker0x01/hackerone-graphql-mcp-server](https://github.com/Hacker0x01/hackerone-graphql-mcp-server), [Hacker0x01/hai-on-hackerone](https://github.com/Hacker0x01/hai-on-hackerone). CAI's README claims HackerOne engineers were inspired by CAI's Retester agent for their deduplication agent (vendor claim). — [aliasrobotics/cai](https://github.com/aliasrobotics/cai)
- **Alias Robotics.** CAI is succeeded by commercial "Cybersecurity Superintelligence (CSI)"; the professional CAI v1.1.5 was "for paid customers". — [aliasrobotics/cai](https://github.com/aliasrobotics/cai)
- **Keygraph (Shannon)** sells Pro/Enterprise editions with black-box testing; the OSS edition is white-box only. — [KeygraphHQ/shannon](https://github.com/KeygraphHQ/shannon)
- **Strix** offers a cloud product with GitLab/Bitbucket/Slack/Jira/Linear integrations. — [usestrix/strix](https://github.com/usestrix/strix)
- **Hadrian** publishes open tools: `OpenHack` (agentic white-box review) and `subwiz` (a nanoGPT model that predicts subdomains, weights on Hugging Face). — [hadriansecurity/OpenHack](https://github.com/hadriansecurity/OpenHack), [scadastrangelove list](https://github.com/scadastrangelove/awesome-ai-security-tools)
- **AISLE** publishes `nano-analyzer` (context → scan → skeptical triage). — [weareaisle/nano-analyzer](https://github.com/weareaisle/nano-analyzer)
- **Prompt Security** publishes `ps-fuzz` (system-prompt hardening fuzzer; about 719 stars) and `clawsec`. — [prompt-security/ps-fuzz](https://github.com/prompt-security/ps-fuzz); [scadastrangelove list](https://github.com/scadastrangelove/awesome-ai-security-tools)

**From the Yeti-791 curated list (secondary; unsourced; quoted as listed)** — [Yeti-791/Awesome-Offensive-AI-Agentic-Landscape](https://github.com/Yeti-791/Awesome-Offensive-AI-Agentic-Landscape)
- Pentera (Pentera AVP): "ARR >$100M (2026-01)".
- Horizon3.ai NodeZero: cumulative funding "$183.5M", valuation "$660–750M (NEA-led $100M Series D)"; "240k+ autonomous pentests"; customers include the NSA and four Fortune-10 companies.
- RunSybil (Sybil): "2026-03 $40M (Khosla Ventures led)"; founder described as "OpenAI's first security employee".
- Terra Security: "2025-09 $30M Series A (Felicis)", cumulative "$38M+"; "Agentic AI + Human-in-the-Loop".
- Hadrian: "multiple Growth rounds (Notion Capital etc.)", no amounts.
- Mindgard: "cumulative $11.9M", Lancaster University spin-out (AI/LLM red teaming, not app pentest).
- Aikido AI Pentest: "2026-01 $60M Series B (DST Global)".
- SPLX: "2025-11 acquired by Zscaler".
- Others: Corridor ($25M Series A, 2026-03), Veria Labs ($3.2M seed, 2026-02), Dreadnode ($14M cumulative), Hex Security (YC W26), Harmony Intelligence ($3M seed, 2025-03), Theori Xint (AIxCC 2025 third place with RoboDuck), MindFort ($3M+ seed, 2026-04), Hacktron ($3.5M cumulative).
- China: NSFOCUS AI-PTS (2025-08), JD Cloud AIPTS (2026-06), 360 "Pozhenzi" agent ("132 critical-infrastructure units, 312 valid vulns"), Alibaba Cloud Agentic BAS (invite beta 2026-07-16), Chaitin, Qi An Xin, DBAPPSecurity, Xmirror.

**[UNVERIFIED-THIS-SESSION] Prior knowledge (cutoff June 2026), not re-checked because vendor and news sites were blocked; no URLs given on purpose:**
- XBOW (founded by Oege de Moor, previously of Semmle/CodeQL and GitHub Copilot) became #1 on HackerOne's US leaderboard around June 2025 and blogged about it ("the road to Top 1"). It raised a $75M Series B led by Altimeter in mid-2025. Its architecture, as described publicly: a coordinator spawning many short-lived "solver" agents, plus non-LLM deterministic "validators" that confirm exploits (e.g., a headless browser confirming XSS) to suppress false positives.
- Google Big Sleep found real SQLite vulnerabilities (2024–2025) and is not public.
- Prompt Security was acquired by SentinelOne in 2025. Horizon3.ai's Series D led by NEA (mid-2025) is consistent with the Yeti list.
- HackerOne markets "Hai" as its AI copilot and agentic system. Synack markets "Sara" as agentic AI on its platform.
- Burp AI shipped in Burp Suite Professional in 2025.
- ZeroPath (YC) and similar AI-SAST tools were credited by an independent researcher with finding many real curl bugs in 2025.

### Inferences
- Public architectural disclosure is best from XBOW (blog posts on validators), Alias Robotics (arXiv papers) and Keygraph/Strix (open-source cores). A builder learns the most by studying the open cores (Shannon, Strix) of companies that also sell commercial editions.
- The funding numbers in community lists conflict (XBOW Series C: $35M per multiples.vc vs $120M per Yeti list). Any figure used in a final report needs a primary press release.

### Gaps
- No primary-source verification this session for XBOW, RunSybil, Horizon3.ai, Hadrian, Ethiack, Pentera, Terra, Mindgard, AISLE, Depthfirst, ZeroPath, Prompt Security, Escape, Ostorlab, Synack Sara, HackerOne Hai/agentic PTaaS, Bugcrowd AI, Burp AI or Caido funding. Reason: WebSearch budget exhausted, and the egress proxy returned 403 for those domains.
- Ethiack, Escape, Ostorlab, Depthfirst, Bugcrowd and Synack had **no** usable secondary data in the GitHub-hosted lists that were checked.
- No public architecture write-ups were retrieved for RunSybil, Terra or Ethiack.

## 6. Real-world effectiveness, false positives, limitations, and community critiques

### Takeaway
The strongest documented critique is from maintainers. **curl ended its bug bounty at the end of January 2026**, writing that bounties incentivize people to "make up 'problems'", and its policy says AI tools "frequently generate inaccurate or fabricated results". On the benchmark side, the **XBOW validation suite is saturated (~100%)**, so the many 97–98% open-source claims no longer discriminate. Validation gates (PoC/oracle) are the field's main answer to false positives.

### Cited Findings
- **curl ends its bug bounty.** Commit on 2026-01-26: "BUG-BOUNTY.md: we stop the bug-bounty end of Jan 2026". The 2026-03-10 rephrase reads "# No curl bug bounty… A bug bounty gives people too strong incentives to find and make up 'problems' in bad faith that cause overload and abuse." — [curl BUG-BOUNTY.md history](https://github.com/curl/curl/commits/master/docs/BUG-BOUNTY.md), [BUG-BOUNTY.md](https://raw.githubusercontent.com/curl/curl/master/docs/BUG-BOUNTY.md)
- **curl AI policy.** "If you asked an AI tool to find problems in curl, you **must** make sure to reveal this fact in your report… AI-based tools frequently generate inaccurate or fabricated results… it is *rarely* a good idea to copy and paste an AI generated report." The disclosure policy adds: "Do not lazily paste massive, AI-generated explanations". — [curl CONTRIBUTE.md](https://raw.githubusercontent.com/curl/curl/master/docs/CONTRIBUTE.md), [curl VULN-DISCLOSURE-POLICY.md](https://raw.githubusercontent.com/curl/curl/master/docs/VULN-DISCLOSURE-POLICY.md)
- **Tooling against "slop".** gadievron/honeyslop: "Code canaries to quickly triage hallucinated ('slop') vulnerability reports". About 101 stars, created 2026-04-21. — [gadievron/honeyslop](https://github.com/gadievron/honeyslop)
- **Benchmark saturation.** XBOW: performance is "now approximately 100%" and the benchmarks "are no longer useful for discriminating between models or attack frameworks". — [xbow-engineering/validation-benchmarks](https://github.com/xbow-engineering/validation-benchmarks). The Yeti list likewise notes it was "brushed to ~100% saturation by mainstream models". — [Yeti-791 list](https://github.com/Yeti-791/Awesome-Offensive-AI-Agentic-Landscape)
- **Spread of self-reported XBOW scores (none independently replicated):** Decepticon 98.08%, RedAmon 97.1%, PentestGPT 86.5% (Dec 2025), Red-MIRROR 86% (arXiv 2603.27127), Cyber-AutoAgent 85%, deadend-cli ~80% (Kimi K2.5, ~$122), MAPTA 76.9%. — [Decepticon](https://github.com/BitterSecurity/Decepticon), [RedAmon](https://raw.githubusercontent.com/samugit83/redamon/master/README.md), [PentestGPT](https://github.com/GreyDGL/PentestGPT), [deadend-cli](https://raw.githubusercontent.com/straylabs-ai/deadend-cli/main/README.md), [Yeti-791 list](https://github.com/Yeti-791/Awesome-Offensive-AI-Agentic-Landscape)
- **The authors' own stated limitations:**
  - PentestGPT calls its number "a historical research result, not a current pentestgpt-agent regression guarantee." — [PentestGPT](https://github.com/GreyDGL/PentestGPT)
  - Shannon: "Do not run Shannon against production systems"; its agents can "create users, submit forms, mutate application state." — [Shannon README](https://raw.githubusercontent.com/KeygraphHQ/shannon/main/README.md)
  - RAPTOR: "not polished software… mostly AI-generated code"; local-model reliability "tracks model scale and quantization." — [RAPTOR](https://raw.githubusercontent.com/gadievron/raptor/main/README.md)
  - deadend-cli: GPT-5.x tool execution "Under investigation"; DeepSeek "functional but limited." — [deadend-cli](https://raw.githubusercontent.com/straylabs-ai/deadend-cli/main/README.md)
  - PentAGI: execution monitoring costs "2-3x higher token use and execution time". — [PentAGI](https://raw.githubusercontent.com/vxcontrol/pentagi/main/README.md)
- **Vendor-claim caution (CAI).** CAI's "3,600× faster… 156× lower cost" and CTF wins are self-reported by Alias Robotics. — [aliasrobotics/cai](https://github.com/aliasrobotics/cai)
- **HexStrike: claimed vs proven misuse.** Check Point reported underground-forum chatter about using HexStrike AI against Citrix NetScaler CVE-2025-7775/-7776/-8424, with actors claiming to cut exploitation "from days to under 10 minutes". Check Point also said it lacked proof "at this time" that the tool actually orchestrated attacks. — [The Register, 2025-09-03](https://www.theregister.com/2025/09/03/hexstrike_ai_citrix_exploits/), [The Hacker News](https://thehackernews.com/2025/09/threat-actors-weaponize-hexstrike-ai-to.html), [BleepingComputer](https://www.bleepingcomputer.com/news/security/hackers-use-new-hexstrike-ai-tool-to-rapidly-exploit-n-day-flaws/) (via search results; pages not opened)
- **Field response.** Several specialized false-positive tools now exist: Trail of Bits `fp-check`, GitHub Security Lab's taskflow agent for "filtering false positives", and AISLE's "skeptical triage" stage. — [trailofbits/skills](https://raw.githubusercontent.com/trailofbits/skills/main/README.md), [seclab-taskflow-agent](https://github.com/GitHubSecurityLab/seclab-taskflow-agent), [nano-analyzer](https://github.com/weareaisle/nano-analyzer)

### Inferences
- For a builder, the 2026 bar is "zero-false-positive by construction": a deterministic oracle or replayable PoC per finding (Strix PoCs, Shannon live validation, pentest-ai "proof capsule", XBOW-style validators), plus human-written reports. Unvalidated LLM findings submitted to bounty programs are now actively rejected or penalized by maintainers.
- New evaluation should move to non-saturated suites (CVE-Bench, CyberGym, BountyBench, Cybench, AutoPenBench, SEC-bench; listed in the Yeti list) rather than the XBOW 104.

### Gaps
- Could not retrieve Daniel Stenberg's blog posts ("death by a thousand slops", etc.), HackerOne/Bugcrowd policy posts on AI reports, the Doyensec study comparing XBOW and Aikido (referenced by Shannon), or Hacker News threads (all hosts blocked).
- No quantitative false-positive rates for any commercial product were found from a primary source.

## 7. Security and ethical concerns: provenance, supply chain, unvetted agents

### Takeaway
Villager has dubious provenance and has gone quiet on PyPI. HexStrike-style MCP servers expose unauthenticated arbitrary command execution by design. Kali now packages an MCP command runner that depends on `sudo`. Package names can point to third-party forks (vulnhuntr). Skill packs with misleading names and anomalous star growth are common. Run all of these only in isolated, authorized lab environments.

### Cited Findings
- **Villager (Cyberspike).** Straiker (researchers Dan Regalado and Amanda Rousseau; report on 2025-09-11) tied Villager to Chinese developer **Cyberspike**. It combines Kali tooling with DeepSeek models, supports MCP, and was released on PyPI on 2025-07-23, with about 10–11k downloads in roughly two months (reported counts vary). Straiker found AsyncRAT and Mimikatz plugins in Cyberspike's product and warned of "AI-powered Persistent Threats"; coverage compared it to Cobalt Strike. — [The Hacker News](https://thehackernews.com/2025/09/ai-powered-villager-pen-testing-tool.html), [The Register](https://www.theregister.com/2025/09/11/cobalt_strikes_ai_successor_downloaded/), [Infosecurity Magazine](https://www.infosecurity-magazine.com/news/chinese-ai-villager-pen-testing/), [CSO Online](https://www.csoonline.com/article/4057785/cobaltstrikes-ai-native-successor-villager-makes-hacking-too-easy.html) (via search results)
- **Villager on PyPI today.** The `villager` package's last version is **0.2.1rc1, uploaded 2025-09-15**, after 18 `0.1.devNN` builds. Author: "stupidfish001 <shovel@hscsec.cn>". The summary now reads "This was an experimental technology project". — [PyPI villager](https://pypi.org/project/villager/)
- **Third-party Villager bundles exist.** For example, Yenn503/villager-hexstrike-AI (about 88 stars) combines Villager with HexStrike; handle these with the same caution. — [Yenn503/villager-hexstrike-AI](https://github.com/Yenn503/villager-hexstrike-AI)
- **HexStrike design risk.** `/api/command` lets clients "Execute arbitrary commands with caching"; the README advises "Consider implementing authentication for production deployments" and to "Run in isolated environments". — [0x4m4/hexstrike-ai](https://github.com/0x4m4/hexstrike-ai)
- **Kali mcp-kali-server.** Depends on `sudo` and `python3-flask`, and its upstream is a personal GitHub repo (Wh0am123). Recent Kali patches touch the `CommandExecutor`. — [Kali debian/control](https://gitlab.com/kalilinux/packages/mcp-kali-server/-/raw/kali/master/debian/control), [changelog](https://gitlab.com/kalilinux/packages/mcp-kali-server/-/raw/kali/master/debian/changelog)
- **Package-name and fork confusion.** PyPI `vulnhuntr` links to `diaz3618/vulnhuntr`, not `protectai/vulnhuntr`. — [PyPI vulnhuntr](https://pypi.org/project/vulnhuntr/). PyPI `pentestgpt` is a 2023 build that does not reflect the v1.0 repo. — [PyPI pentestgpt](https://pypi.org/project/pentestgpt/)
- **Licensing traps.** CAI combines MIT with "proprietary additions licensed for research purposes only" and has a license-check bypass env var. — [aliasrobotics/cai](https://github.com/aliasrobotics/cai). Several leading agents are **AGPL-3.0** (Shannon, deadend-cli, Cairn, T3MP3ST, CyberStrike, AIDA, LuaN1ao, ARTEX, HackSynth, Vulnhuntr), which matters if you fork into a SaaS. — repo pages cited above. RAPTOR warns "CodeQL… does not permit commercial use." — [RAPTOR](https://raw.githubusercontent.com/gadievron/raptor/main/README.md)
- **Telemetry.** PentestGPT includes telemetry, with an opt-out flag. — [PentestGPT](https://github.com/GreyDGL/PentestGPT)
- **Isolation patterns worth copying:**
  - Decepticon separates `sandbox-net` from `decepticon-net`. — [Decepticon](https://github.com/BitterSecurity/Decepticon)
  - RedAmon runs "All MCP tools… inside a Kali Linux sandbox container" with Allow/Deny gates. — [RedAmon](https://raw.githubusercontent.com/samugit83/redamon/master/README.md)
  - RAPTOR uses Landlock/seccomp. — [RAPTOR](https://raw.githubusercontent.com/gadievron/raptor/main/README.md)
  - PentAGI recommends a two-node production setup isolating workers. — [PentAGI](https://raw.githubusercontent.com/vxcontrol/pentagi/main/README.md)
- **Star-inflation and naming red flags:**
  - ARTEX: 983 forks and about 2k stars within 19 days.
  - "Anthropic-Cybersecurity-Skills": about 35k stars, not an Anthropic repo.
  - reverse-skill: about 43.5k stars in 5 months, PowerShell "toolchain bootstrapping".
  - Repo pages: [ARTEX](https://github.com/trhhubsd/ARTEX), [Anthropic-Cybersecurity-Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills), [reverse-skill](https://github.com/zhaoxuya520/reverse-skill)
- **Skill/MCP supply-chain scanners now exist:** NVIDIA/SkillSpector and Pantheon-Security/medusa. — [SkillSpector](https://github.com/NVIDIA/SkillSpector), [medusa](https://github.com/Pantheon-Security/medusa)

### Inferences
- Minimum hygiene for running any of these tools:
  - Use a dedicated VM or VPC with an egress allow-list scoped to the authorized targets.
  - Give MCP servers no host-level `sudo`; authenticate any HTTP or SSE MCP endpoint.
  - Pin commits rather than `pip install <name>`, and read the code first. Villager's own author effectively disowned it, and PyPI names can map to forks.
  - Scan skills/hooks before cloning repos that ship `.claude/` configs.
  - Treat prompt-injection from target content (HTTP responses, repo files) as an attack on the agent itself.

### Gaps
- No Villager GitHub source repo by Cyberspike was located (only PyPI and third-party bundles), so its code could not be inspected.
- No known CVEs against HexStrike, Kali MCP or the Burp MCP server were searched (search budget exhausted).

## 8. Synthesis: the best 5–8 repos to read as reference architectures

### Takeaway
Read, in this order: **PentAGI** (full system design), **Strix** (web hackbot with in-sandbox proxy, browser and PoC validation), **Shannon** (validated white-box→exploit pipeline), **Decepticon** (LangGraph multi-agent kill chain), **RedAmon** (graph-as-state with human approval gates), **RAPTOR** (turning Claude Code into a security agent), **PentestGPT** (task-tree origin → coding-agent driver), and **hackingBuddyGPT** (minimal core loop). Read **CAI** as an archived pattern library only.

### Cited Findings
1. **PentAGI**: the most complete open "production" architecture. Role-specialized agents (Adviser/Reflector supervision), pgvector + Graphiti memory with chain summarization, Langfuse + OpenTelemetry/Grafana observability, Docker sandbox, scraper browser and many search backends. MIT and Go. — [vxcontrol/pentagi](https://github.com/vxcontrol/pentagi)
2. **Strix**: multi-agent graph with Caido proxy, browser, shell and a Python exploit runtime inside one Docker sandbox. PoC-per-finding and a CI-friendly headless mode. Apache-2.0 makes forking easy. — [usestrix/strix](https://github.com/usestrix/strix)
3. **Shannon**: clean phase design (recon/analysis → dedupe → exploitation queue → live validation → report) with a hard rule that "Only live-validated vulnerabilities" count. It also documents a migration off the Claude Agent SDK. AGPL. — [KeygraphHQ/shannon](https://github.com/KeygraphHQ/shannon)
4. **Decepticon**: LangGraph orchestration with 16 kill-chain specialists and "fresh context window per objective". Tmux handles interactive tools. Network-segmented Kali sandbox, Neo4j KG, and it can be exposed as an MCP server. Apache-2.0. — [BitterSecurity/Decepticon](https://github.com/BitterSecurity/Decepticon)
5. **RedAmon**: graph-centric state (Neo4j queried with NL→Cypher), fan-out recon pipeline, human Allow/Deny tool gates and RoE, MCP in both directions, and auto-fix PRs. MIT. — [samugit83/redamon](https://github.com/samugit83/redamon)
6. **RAPTOR**: the reference for "coding-agent-as-hackbot". CLAUDE.md, skills, sub-agents and slash commands over a Python execution layer, staged exploitability validation, exploit and patch generation, and Landlock/seccomp sandboxing. — [gadievron/raptor](https://github.com/gadievron/raptor)
7. **PentestGPT**: two generations in one repo. The legacy reasoning/generation/parsing sessions with a Pentesting Task Tree (USENIX Security 2024), and v1's pipeline that drives Claude Code/Codex. — [GreyDGL/PentestGPT](https://github.com/GreyDGL/PentestGPT)
8. **hackingBuddyGPT**: the smallest readable agent loop for priv-esc and web, from an academic lab. Good for building intuition before reading the larger systems. — [ipa-lab/hackingBuddyGPT](https://github.com/ipa-lab/hackingBuddyGPT)
- **Targeted reads (one idea each):**
  - PentestAgent: a RAG meta-tool when MCP servers expose more than 128 tools; self-spawning MCP child agents. — [GH05TCREW/pentestagent](https://github.com/GH05TCREW/pentestagent)
  - deadend-cli: confidence-threshold control loop; cheap open-weight model results. — [straylabs-ai/deadend-cli](https://github.com/straylabs-ai/deadend-cli)
  - Burp MCP server: how an official proxy exposes tools over SSE plus a stdio proxy. — [PortSwigger/mcp-server](https://github.com/PortSwigger/mcp-server)
  - trailofbits/skills `fp-check` and `variant-analysis`: false-positive triage and variant hunting as skills. — [trailofbits/skills](https://github.com/trailofbits/skills)
  - CAI: an OpenAI Agents SDK fork with handoffs and guardrails; archived and research-licensed. — [aliasrobotics/cai](https://github.com/aliasrobotics/cai)
  - Google Mantis and OpenAI Codex Security: first-party find→reproduce→patch harnesses. — [google/mantis](https://github.com/google/mantis), [openai/codex-security](https://github.com/openai/codex-security)

### Inferences
- A builder's fork choice depends on license and target:
  - Apache/MIT and web-focused: Strix (Apache-2.0) or PentAGI (MIT).
  - Red-team or infrastructure: Decepticon (Apache-2.0) or RedAmon (MIT).
  - Shannon is the best design to *learn* from, but AGPL constrains SaaS forks.
- Avoid basing new work on HexStrike (security posture), CAI (archived, research-only license), Villager (provenance), or stale projects (BurpGPT, ReconAIzer, HackSynth, Vulnhuntr, BloodHound-MCP-AI, Cyber-AutoAgent).

### Gaps
- None of these repos' code quality, test coverage or real-target performance was independently evaluated. The rankings rest on READMEs, licenses and maintenance signals, not code review or replication.
