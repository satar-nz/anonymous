# Benchmarks, Evaluations, CTF-Solving Agents, and AI Vulnerability-Discovery Systems (for building/evaluating a web & bug-bounty hackbot)

> Research date: 2026-10-27. How things were checked: GitHub READMEs and leaderboard data files were pulled directly from raw.githubusercontent.com, along with anthropic.com pages. The rest comes from web-search result summaries. The shared web-search budget ran out partway through. The fetch proxy also blocked arxiv.org, xbow.com, openai.com, darpa.mil, *.github.io sites, sean.heelan.io, blog.trailofbits.com and projectzero.google. Facts from those domains rely on search-result summaries and are marked "(search summary)". Every SOTA number carries a date. Most leaderboards here are self-reported and often run on different task subsets, so treat cross-model comparisons with care.

## 1. Offensive-security benchmarks & eval suites: what exists, what they measure, current SOTA

### Takeaway
Classic Jeopardy-CTF benchmarks are saturated by frontier models as of mid/late 2026: InterCode-CTF (95% in Dec 2024), XBOW's 104 web challenges (~100% "industry standard" per XBOW, mid-2026) and Cybench (Claude Mythos Preview 100% on a 35-task subset, Apr 2026). CyberGym's maintainers also called it "nearly saturated" (Aug 2026) and now require cost reporting. The benchmarks that still discriminate are real-vulnerability, cost-aware and end-to-end: SEC-bench Pro (best 58.4%, Jun 2026), CVE-Bench v2.x (real web CVEs), BountyBench (Detect is still hard, ~12.5% in 2025), plus 2026 work on verification under deceptive evidence and on trace provenance. For a web/bug-bounty hackbot, the relevant core is the XBOW suite (regression), the Cybench/NYU web subsets, CVE-Bench, BountyBench, and fresh private holdouts.

### Cited Findings

**A. Jeopardy-CTF style (largely saturated)**

- **Cybench**: Stanford (Andy Zhang et al.). Paper arXiv 2408.08926. Repo `andyzorigin/cybench`, site cybench.github.io. It has 40 professional-level CTF tasks from 4 competitions, with subtasks for graded evaluation. Run with `./run_task.sh --task_dir ... --unguided_mode --model ...` or `run_benchmark.py`, then `grade_benchmark.py`. Needs Python 3.9–3.10 and Docker/Kali. — [Cybench repo](https://github.com/andyzorigin/cybench)
  - Category mix: Crypto 16, Web 8, Rev 6, Forensics 4, Misc 4, Pwn 2. The web share is small: only 8 tasks. — [Cybench site source](https://github.com/cybench/cybench.github.io)
  - **Official leaderboard data (fetched 2026-10-27):** Claude Mythos Preview 100% (35-problem subset; Mythos Preview system card, Apr 2026); Claude Opus 4.7 96% (35); Claude Opus 4.6 93% (37); Claude Opus 4.5 82% (39); Muse Spark 65.4% (40); Claude Sonnet 4.5 60% (39); Grok 4 43%; Claude Opus 4.1 42%; Grok 4.1 Thinking 39%; o3-mini 22.5%; GPT-4o 12.5% (original paper). Footnotes say the vendor numbers come from vendor system cards on subsets of 35–39 problems, as average pass@1 or estimated single-trial success. They are not uniform runs. — [Cybench leaderboard.csv](https://raw.githubusercontent.com/cybench/cybench.github.io/HEAD/data/leaderboard.csv); [Cybench site](https://github.com/cybench/cybench.github.io)
  - Harness-leak warning from the site: o3-mini/o1-mini HAL scores were inflated "because HAL likely ran on a fork of the Inspect framework that leaked the answer to a task", and were adjusted down by 2.5%. — [Cybench site source](https://github.com/cybench/cybench.github.io)
  - Metric sensitivity: Anthropic reported Sonnet 4.5 at **76.5% with 10 attempts** (37 problems, Sep 29 2025), vs Sonnet 3.7 at 35.9%. The leaderboard lists Sonnet 4.5 at 60% (pass@1 average, 39 problems). — [Anthropic "Building AI for cyber defenders"](https://www.anthropic.com/research/building-ai-cyber-defenders); [leaderboard.csv](https://raw.githubusercontent.com/cybench/cybench.github.io/HEAD/data/leaderboard.csv)
  - Secondary report: the Opus 4.7 system card says Cybench is "near-saturated" (96% pass@1) and that CTF-style tests "may no longer tell the full story". — [the-decoder](https://the-decoder.com/anthropics-claude-opus-4-7-makes-a-big-leap-in-coding-while-deliberately-scaling-back-cyber-capabilities/)
  - Uptake: US AISI and UK AISI used Cybench in joint pre-deployment tests of Claude 3.5 Sonnet and OpenAI o1. It appears in Anthropic system cards (3.7 Sonnet through Opus 4.7), the Amazon Nova Premier card and xAI's RMF. — [Cybench site source](https://github.com/cybench/cybench.github.io)
  - Inspect port: `inspect eval inspect_evals/cybench`, with **39** tasks. The K8s sandbox is the default for domain filtering. The Docker sandbox needs `CYBENCH_ACKNOWLEDGE_RISKS=1`. Images are pinned by sha256 digest, and the README warns that the Dockerfiles "no longer reproduce the images actually in use". — [inspect_evals/cybench README](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/cybench)
- **NYU CTF Bench**: NYU Tandon (Shao, Jancheska, Udeshi, Dolan-Gavitt et al.), NeurIPS 2024 Datasets & Benchmarks. 200 test challenges from CSAW Quals/Finals in 6 categories (crypto 52, forensics 15, misc 24, pwn 39, rev 51, web 19), plus a **55-challenge development split** meant for building agents without biasing the test set. Install with `pip install nyuctf` (it auto-clones the dockerized challenges). — [NYU_CTF_Bench repo](https://github.com/NYU-LLM-CTF/NYU_CTF_Bench); [leaderboard.json](https://raw.githubusercontent.com/NYU-LLM-CTF/nyu-llm-ctf.github.io/HEAD/data/leaderboard.json)
  - Leaderboard (stale, last entries 2025-08-12): best is **CRAKEN (Self-RAG + Graph-RAG) with claude-3.5-sonnet-20241022, 44/200 (22%) pass@1**. Next: CRAKEN Self-RAG 42; D-CIPHER (claude-3.5-sonnet) 38; D-CIPHER (claude-3.7) 35; EnIGMA (claude-3.5-sonnet) 27. Web solves are tiny (≤2 of 19). No 2026 frontier models are listed. — [leaderboard.json](https://raw.githubusercontent.com/NYU-LLM-CTF/nyu-llm-ctf.github.io/HEAD/data/leaderboard.json)
  - Agents repo: D-CIPHER, a multi-agent planner/executor/auto-prompter (arXiv 2502.10931), and the NYU baseline. The site also announces a "CSAW'25 LLM CTF Attack Challenge". — [nyuctf_agents](https://github.com/NYU-LLM-CTF/nyuctf_agents); [NYU site source](https://raw.githubusercontent.com/NYU-LLM-CTF/nyu-llm-ctf.github.io/HEAD/index.html)
- **InterCode-CTF**: Princeton NLP. 100 picoCTF tasks in Docker. — [InterCode repo](https://github.com/princeton-nlp/intercode); [search summary of arXiv 2306.14898](https://arxiv.org/pdf/2306.14898)
  - **Saturated:** Palisade Research's "Hacking CTFs with Plain Agents" (arXiv 2412.02776, Dec 2024) hit **95%**, up from prior results of 29% and 72%. "Standard tools and basic prompting techniques achieved superior results compared to more complex frameworks." — [PalisadeResearch/intercode](https://github.com/PalisadeResearch/intercode); [arXiv 2412.02776](https://arxiv.org/pdf/2412.02776)
  - Some later papers exclude 10 erroneous InterCode tasks, so scores may be reported on 90 or 100 tasks. — (search summary) [arXiv 2505.18384](https://arxiv.org/pdf/2505.18384)
- **XBOW validation benchmarks**: XBOW, Apache-2.0. **104** Jeopardy-style web challenges, each a `docker-compose.yml` plus `benchmark-config.json` (name, description, level 1–3, win_condition, tags, canaries). The flag is injected at build time with `make build FLAG=...`. They were built by contractors to "mirror the variety seen in routine pentesting and bug bounty work". — [xbow-engineering/validation-benchmarks](https://github.com/xbow-engineering/validation-benchmarks)
  - **Status (README, 2026):** "These benchmarks are now outdated… As of mid-2026, these benchmarks are saturated, [industry-standard performance] approximately 100%, and it is no longer useful for discriminating between" models or frameworks. It is kept for historical purposes. The README also warns that "the underlying vulnerabilities have been trained into the models" despite canary GUIDs. — [validation-benchmarks README](https://github.com/xbow-engineering/validation-benchmarks)
  - Historical scores: PentestGPT **86.5% (90/104)**, Dec 2025. — [PentestGPT README](https://github.com/GreyDGL/PentestGPT)
- **GDM in-house CTF (Dangerous Capabilities)**: Google DeepMind (Phuong et al., arXiv 2403.13793). 13 CTF challenges released with flags and solutions stripped "to prevent dataset contamination". You must insert random flags and build your own agent loop. — [dangerous-capability-evaluations](https://github.com/google-deepmind/dangerous-capability-evaluations)
  - Inspect port `gdm_in_house_ctf`: ReAct agent, 50 messages, **10 epochs with an at_least_1 reducer** (the paper's pass@10). Wordlist hints for the db_3, pw_reuse, spray and ssh challenges can be disabled. — [inspect_evals/gdm_in_house_ctf](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/gdm_in_house_ctf)
- **GDM "A Framework for Evaluating Emerging Cyberattack Capabilities of AI"** (Rodriguez, Popa et al., arXiv 2503.11917, Mar–Apr 2025). A cyberattack-chain / MITRE ATT&CK structure with **50 new challenges** across attack phases. It finds models show "significant potential in under-researched phases like reconnaissance, evasion, and persistence". — (search summary) [arXiv 2503.11917](https://arxiv.org/html/2503.11917v3)
- **3CB, Catastrophic Cyber Capabilities Benchmark**: Apart Research (arXiv 2410.09114). Original challenges built for legibility, coverage and generalization. "Elicitations" are first-class config entries in TOML task files. Challenges are designed around ~20 steps. Run with `poetry run python3 run_from_config.py --config_path ./task_configs/...`. — [apartresearch/3cb](https://github.com/apartresearch/3cb)
- **CAIBench**: Alias Robotics (arXiv 2510.24317, Oct 2025). A meta-benchmark with 5 categories: Jeopardy CTFs (includes Cybench), Attack-and-Defense CTFs, Cyber Range, KnowledgeBench and privacy. It also includes robotics/OT challenges. — (search summary) [arXiv 2510.24317](https://arxiv.org/pdf/2510.24317); [CAI repo](https://github.com/aliasrobotics/cai)
  - Alias: a "blackboard" architecture over 5 scaffolds solves **19/33 Cybench (57.6%)** vs 15/33 for the best single scaffold (arXiv 2605.28334, May 2026). Their A&D CTF study reports **54.3% defensive patching vs 28.3% offensive initial access** (arXiv 2510.17521). — [CAI README](https://github.com/aliasrobotics/cai)

**B. Real-vulnerability / real-app benchmarks (still discriminating)**

- **CVE-Bench**: UIUC Kang Lab (Yuxuan Zhu, Daniel Kang et al.), ICML 2025 spotlight. Paper arXiv 2503.17332. **40 critical (CVSS ≥ 9.0) web-app CVEs** from NVD, mostly CMSs such as WordPress plus AI/ML apps. **Zero-day and one-day variants.** Graded on **application state**, not flags, across 8 objectives: DoS, file access, RCE (execute `/tmp/pwn`), DB modification, DB access (table `secret`), unauthorized admin login, privilege escalation, outbound request (SSRF-like). — [cve-bench repo](https://github.com/uiuc-kang-lab/cve-bench); [ICML proceedings](https://proceedings.mlr.press/v267/zhu25i.html)
  - Original results (Mar 2025): the best agent framework reached **13% zero-day and 25% one-day**. — (search summary) [arXiv 2503.17332](https://arxiv.org/abs/2503.17332v2)
  - Versions: **v2.0.0 (2025-10-30)** fixes guided by the Agentic Benchmark Checklist (arXiv 2507.02825); **v2.1.0 (2026-01-12)** replaced "arbitrary file upload" with RCE as a criterion; the runner shows 2.2.0. Runs on **Inspect** (`./run eval -T challenges=CVE-2023-37999 -T variants=one_day`); amd64 recommended. Reference exploits are "available on request" to limit contamination. — [cve-bench README](https://github.com/uiuc-kang-lab/cve-bench)
  - Also in inspect_evals as `cve_bench`. — [inspect_evals tree](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals)
- **BountyBench**: Stanford CRFM (Zhang et al., arXiv 2505.15216, NeurIPS 2025). **40 real bug bounties** with awards from **$10 to $30,485**, in three tasks: **Detect, Exploit, Patch**. Dollar impact = bounty total net of token cost. Exploit gets no dollar value. — (search summary) [arXiv 2505.15216](https://arxiv.org/html/2505.15216v3); [NeurIPS poster](https://neurips.cc/virtual/2025/poster/121456); [CRFM blog](https://crfm.stanford.edu/2025/05/21/bountybench.html)
  - Results (2025, given up to 3 attempts): **OpenAI Codex CLI o3-high 12.5% Detect ($3,720), 90% Patch ($14,152)**. CRFM table: **Claude Code 87.5% Patch, $13,862 bounty, $82.19 token cost**. Earlier version: Claude Code and Codex CLI 5% Detect. **Numbers differ across arXiv v1/v3/NeurIPS, so cite the version.** — (search summary) [arXiv 2505.15216](https://arxiv.org/pdf/2505.15216)
  - Repo: `bountybench/bountybench` (Python 3.11, Docker, `bountytasks` submodule). Tasks include Lunary, gunicorn, mlflow and setuptools, with **invariant tests** so patches don't break functionality. — [bountybench](https://github.com/bountybench/bountybench); [bountytasks](https://github.com/bountybench/bountytasks)
- **CyberGym**: UC Berkeley RDI / sunblaze-ucb (Wang, Shi et al., arXiv 2506.02548, listed at ICLR 2026). **1,507 instances from 188 OSS-Fuzz/ARVO projects.** The main task is PoC generation to reproduce a vulnerability, at levels 0–3 (level0/1 hardest, least info). Metrics: "Reproduced" and "New Vuln" (PoC also crashes the fixed version). — [inspect_evals/cybergym](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/cybergym); [cybergym repo](https://github.com/sunblaze-ucb/cybergym)
  - Size and cost to host: benchmark data ~240 GB; full server data **~10 TB**; binary-only ~130 GB; a 10-task subset is provided. Warning: "Do not expose any part of CyberGym to the public internet." — [cybergym README](https://github.com/sunblaze-ucb/cybergym)
  - **Saturation (2026-08-04):** "The most capable models under unconstrained resources have nearly saturated the benchmark." Submissions must now report per-task tokens (incl. cache), USD cost, wall-clock and LLM requests, plus trajectories for ≥10 tasks. — [CyberGym SUBMISSION.md](https://github.com/sunblaze-ucb/cybergym/blob/main/SUBMISSION.md)
  - Scores: Sonnet 4.5 **28.9% under the leaderboard's $2-per-vuln cap**, **66.7% with 30 trials** (~$45 per task for 30 tries), and found new vulns in 5% of targets at 1 trial and >33% at 30 trials (Sep 2025). — [Anthropic](https://www.anthropic.com/research/building-ai-cyber-defenders)
  - **Claude Mythos Preview 83.1% vs Opus 4.6 66.6%** (Apr 7 2026). — [Anthropic Glasswing](https://www.anthropic.com/glasswing). Opus 4.6 was later revised to **73.8** under updated harness parameters. — [Anthropic Opus 4.7 post](https://www.anthropic.com/news/claude-opus-4-7)
  - Vendor leaderboard claims, Aug–Oct 2026, **unverified**: Trend Micro AESIR 97%, Sangfor 93.2% (DeepSeek-V4-Flash), Fudan 91.2%. — [AI Understanding](https://aiunderstanding.org/news/trend-micro-says-its-multi-model-system-led-cybergym-s-exploit-benchmark); [CryptoBriefing](https://cryptobriefing.com/cybergym-ai-vulnerability-detection-results/). The Inspect implementation's reference run: GPT-4.1 11.6% reproduced, 3.1% new vuln (level1, full 1,507, May 2026). — [inspect_evals/cybergym](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/cybergym)
- **SEC-bench / SEC-bench Pro**: UIUC + Purdue (Hwiwon Lee, Ziqi Zhang, Hanxiao Lu, Lingming Zhang). Automated benchmark generation from OSV/CVE records using a multi-agent pipeline. Docker instances. Tasks: PoC generation (`poc-repo`, `poc-desc`, `poc-san`) and patching. Supports SWE-agent, OpenHands, Aider and smolagents. Needs >200 GB disk. — [SEC-bench repo](https://github.com/SEC-bench/SEC-bench)
  - **SEC-bench Pro (2026)**: kernel/browser-engine vulnerability discovery plus PoC from source files. Snapshot 260617 (2026-06-17): **103 V8 + 104 SpiderMonkey + 137 Linux = 344 instances**. Overall: **Codex + GPT-5.5 (xhigh) 58.4%**, Codex + GPT-5.4 39.0%, Claude Code + Opus 4.6 (max) 30.8%, OpenCode + GLM-5 3.8%, Kimi K2.5 2.3%, MiniMax M2.5 0.6%. On the Linux track GPT-5.5 scored 77.4%. All entries are marked `verified: false`, and no Claude 5.x or Mythos entries appear. — [SEC-bench site data/results.json](https://github.com/SEC-bench/sec-bench.github.io)
- **AutoPenBench**: Gioacchini et al. (arXiv 2410.03225). A Docker pentest range with a Kali workstation (192.168.0.5) and vulnerable machines in "in-vitro" categories (access_control, web_security, network_security, cryptography) and "real-world" CVE tasks. The example agent uses Instructor structured outputs. — [auto-pen-bench](https://github.com/lucagioacchini/auto-pen-bench)
- **CyberSecEval 1–4 (Meta PurpleLlama)**: MIT-licensed evals. CSE4 adds **CyberSOCEval** (malware analysis + threat intel, with CrowdStrike) and **AutoPatchBench**. — [PurpleLlama CybersecurityBenchmarks](https://github.com/meta-llama/PurpleLlama/tree/main/CybersecurityBenchmarks)
  - The Inspect port (`cyberseceval_2/3/4`) exposes cyse4_mitre (1000), mitre_frr (750), instruct/autocomplete insecure-code (1916 each), multilingual prompt injection (1004), multi-turn phishing (100), malware analysis (609) and threat intel (588). It **omits `cyse4_autonomous_uplift`** because it "simulates command feedback instead of executing against a grounded target environment", and omits autopatching. — [inspect_evals/cyberseceval_4](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/cyberseceval_4)
  - AutoPatchBench (Apr 29 2025): 136 C/C++ ARVO vulns (Lite: 113), verified with fuzzing and differential testing. — (search summary) [Meta Engineering](https://engineering.fb.com/2025/04/29/ai-research/autopatchbench-benchmark-ai-powered-security-fixes/). CyberSOCEval (Sep 2025). — [Meta AI](https://ai.meta.com/research/publications/cybersoceval-benchmarking-llms-capabilities-for-malware-analysis-and-threat-intelligence-reasoning/)

**C. 2026 arXiv benchmarks / eval-methodology papers (titles found via search listing; contents mostly not read)**
- **ATOBench**: "Tracing How Autonomous Penetration-Testing Agents Verify Vulnerabilities When Target Evidence Lies" (arXiv 2608.12996). Directly relevant to false-positive control. — [arXiv 2608.12996](https://arxiv.org/pdf/2608.12996)
- "How Do LLM Agents Actually Get the Flag? Trace-Level Provenance for Agentic Offensive Security Evaluation" (arXiv 2608.26237). — [arXiv](https://arxiv.org/pdf/2608.26237)
- "Beyond End-to-End Success: Diagnosing Failures in Long-Horizon Security LLM Agents" (arXiv 2608.20563). — [arXiv](https://arxiv.org/pdf/2608.20563)
- "Beyond Success Rate: Cost-Aware Evaluation of Offensive and Defensive Security Agents" (arXiv 2607.15263). — [arXiv](https://arxiv.org/pdf/2607.15263)
- "How Reliable Are AI Attackers Against a Fixed Vulnerable Target? A 400-Run Empirical Study of LLM Penetration Testing Consistency" (arXiv 2605.30096). — [arXiv](https://arxiv.org/pdf/2605.30096)
- **AWE**: adaptive multi-agent web pentesting with persistent memory and browser-backed verification (arXiv 2603.00960). A survey cites 87% XSS and 66.7% blind SQLi. — [arXiv 2603.00960](https://arxiv.org/html/2603.00960v1); survey [arXiv 2607.02605](https://arxiv.org/html/2607.02605v1)
- Survey (arXiv 2607.02605): CVE-based benchmarks become more common in 2025–26, and agents exploited <15% of real vulns even with CVE descriptions (2025-era). It also mentions **ExploitGym (898 instances: user-space, V8, kernel)** and **CyberGym-E2E** (details not verified). — (search summary) [arXiv 2607.02605](https://arxiv.org/pdf/2607.02605)

**D. Real-app comparison studies (vendor-run; treat as claims)**
- **Doyensec "Comparing AI Application Security Testing Platforms"** (Aikido-sponsored). Aikido vs XBOW on Fider 0.33.0 and Photoview 2.4.0, with source and credentials provided and each finding validated by hand. On Photoview: Aikido 32 TPs, XBOW 7 TPs, $4,000 each (as relayed by Keygraph). — [Doyensec PDF](https://doyensec.com/resources/ComparingAIApplicationSecurityTestingPlatforms_Doyensec.pdf) via [Shannon benchmark doc](https://github.com/KeygraphHQ/shannon/blob/main/docs/shannon-xbow-aikido-benchmark.md)
- Keygraph's follow-up with **Shannon v3** (open source, AGPL-3.0) on the same Photoview: Claude Opus 5 found 23 TP / 1 FP for $115 in 2.4h; Grok 4.6 10 TP for $35.07; DeepSeek v4 Flash 18 TP for $6.10. All models found the pre-auth SQLi (CVSS 9.8). Opus caught 6/7 later-patched vulns, the other two models 3/7. The ground truth used **security fixes landed after the scanned version (July 2026)** "to reduce the chance the models saw the fixes in training". — [Shannon benchmark doc](https://github.com/KeygraphHQ/shannon/blob/main/docs/shannon-xbow-aikido-benchmark.md)

### Inferences
- For a web hackbot in late 2026, Jeopardy CTF suites (InterCode, XBOW-104, Cybench) work as **regression/smoke tests**, not capability frontiers. A frontier-model hackbot should score ~95–100%; anything less points to a scaffold bug.
- Cybench has only 8 web tasks and NYU CTF Bench 19 web tasks, so both say little about web/bug-bounty skill specifically. CVE-Bench (web CVEs, state-based grading) and BountyBench (real bounties with dollar values, Detect being the bug-bounty-relevant task) are the closest public analogues to bug bounty work.
- Leaderboards mix vendor-reported subsets, pass@k definitions and harness versions (e.g. Opus 4.6 CyberGym 66.6 → 73.8 with a harness change). Run your own harness with fixed settings rather than comparing your number to a vendor's.
- No 2026 public leaderboard shows Claude 5.x or Mythos 5.1 on CyberGym, Cybench or SEC-bench Pro (Anthropic's Sep 2026 posts publish no cyber benchmark numbers). Current SOTA on these is therefore unknown beyond the Apr–Jun 2026 figures.

### Gaps
- Could not verify (search budget exhausted / domains blocked): **SecCodePLT** (Virtue AI/UIUC, 2024), **CyberBench**, **PentestEval**, "**CTFBench**" (no repo found at `Aegis-Intelligence/CTFBench`; the name may be wrong), the Hugging Face "FrontierAICybersecurity/Cybersecurity_leaderboard" space (seen only as a search hit), the BountyBench leaderboard site, and CyberGym's official leaderboard page (cybergym.io blocked).
- AutoPenBench task counts and autonomous vs assisted success rates were not verified this session.
- Current (Oct 2026) CVE-Bench v2.x SOTA is unknown. No public leaderboard was found.
- ExploitGym and CyberGym-E2E details are known only via one survey snippet.

## 2. Frontier-lab cyber evaluations (Anthropic, OpenAI, Google DeepMind, UK AISI, US CAISI) and Inspect as a builder tool

### Takeaway
Frontier labs now treat CTF benchmarks as saturated. Their headline evidence has moved to real-vulnerability benchmarks (CyberGym), real-world zero-day discovery (Anthropic's Glasswing/Mythos) and internal ranges. Both Anthropic and OpenAI classify their 2026 models as high cyber capability and gate offensive use. For a solo bug-bounty builder this matters a lot: as of Sep–Oct 2026, Anthropic's generally available models re-route pentest and exploit tasks to an older model (Opus 4.8) unless you hold Cyber Verification Program access, and Red Team Access excludes individuals. The most reusable public tooling is the UK AISI **Inspect** stack: `inspect_ai`, `inspect_evals` (with Cybench, CVE-Bench, CyberGym, GDM CTFs and CyberSecEval ported) and the `inspect_cyber` extension.

### Cited Findings
**Anthropic**
- Sep 29 2025, "Building AI for cyber defenders": "We believe we are now at an inflection point for AI's impact on cybersecurity". The work targeted defensive skills (vuln discovery, patching) and avoided exploitation/malware enhancement. It ran repeated trials to reflect motivated actors. HackerOne reported a 44% reduction in vulnerability intake time and 25% higher accuracy. — [Anthropic](https://www.anthropic.com/research/building-ai-cyber-defenders)
- Feb 5 2026, Claude Opus 4.6: "finds real vulnerabilities in codebases better than any other model". It added six new cybersecurity probes, and may add real-time intervention. — [Anthropic Opus 4.6](https://www.anthropic.com/news/claude-opus-4-6). Secondary report: Opus 4.6 found 500+ vulnerabilities in open source. — [SC World](https://scworld.com/news/anthropic-latest-claude-model-finds-more-than-500-vulnerabilities)
- **Apr 7 2026, Project Glasswing / Claude Mythos Preview (restricted).** "Thousands" of zero-days across every major OS and browser, nearly all found and many exploited autonomously. Examples: a 27-year-old OpenBSD remote crash, a 16-year-old FFmpeg flaw in a line fuzzers executed 5M times, and chained Linux kernel LPEs. Unpatched findings were published as cryptographic hashes. Benchmarks: CyberGym 83.1% vs 66.6% (Opus 4.6); Terminal-Bench 2.0 82.0%; SWE-bench Verified 93.9%. 12 launch partners plus >40 organizations, $100M in credits, $4M in OSS donations, post-preview pricing $25/$125 per M tokens. Technical write-ups: red.anthropic.com/2026/mythos-preview, /firefox/, /exploit/. — [Anthropic Glasswing](https://www.anthropic.com/glasswing)
- Apr 16 2026, Opus 4.7: cyber capability below Mythos Preview. Anthropic "experimented with efforts to differentially reduce these capabilities". Automatic blocking of prohibited/high-risk cyber requests, plus a Cyber Verification Program for pentesters and red-teamers. XBOW reported 98.5% on a visual-acuity benchmark vs 54.5% for Opus 4.6. — [Anthropic Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7)
- Sep 15 2026, **Claude Mythos 5.1 / Fable 5.1** (same model, different safeguards). Mythos 5.1 is restricted to trusted-access US orgs and "demonstrates the strongest cyber capabilities of any model we've released". Fable 5.1 (GA) can find vulnerabilities but not develop exploits. Pentesting, exploit generation and binary vuln scanning are redirected to Opus models. Claude Security now runs on Mythos 5.1. No cyber benchmark scores are published. — [Anthropic Mythos/Fable 5.1](https://www.anthropic.com/claude-fable-and-mythos-5-1)
- Sep 22 2026, **Claude Opus 5.5**: "comparable to Claude Mythos 5.1 in biology and cybersecurity". "Most cybersecurity tasks are re-routed to Opus 4.8", and in testing "cybersecurity tasks were completed by Claude Opus 4.8". — [Anthropic Opus 5.5](https://www.anthropic.com/claude-opus-5-5)
- Oct 6 2026, **Cyber Verification Program** tiers:
  - **Defense Access**: includes "individual researchers with a track record of reported vulnerabilities".
  - **Red Team Access**: authorized pentest/red-team orgs. "Individual researchers are not eligible."
  - **Specialized Access**.
  
  Covers Opus 5.5, Sonnet 5.5 and Mythos 5.1. On the internal **CyScenarioBench**, Defense Access still blocked most trials while Red Team Access had no blocks. Apply at portal.anthropic.com/programs/cvp. — [Anthropic CVP](https://www.anthropic.com/news/cyber-verification-program)
- Oct 8 2026, **Anthropic Cyber Mission**: CVP absorbed Glasswing. A free opt-in **OSS Scanner** with expected true-positive rate >90% (reports not human-reviewed), a Critical Infrastructure Defense Program, and the 0xDAF fund. — [Anthropic Cyber Mission](https://www.anthropic.com/news/anthropic-cyber-mission)
- External evaluators named Oct 15 2026: METR (employee-like access), SecureBio, Accenture/Faculty. UK AISI and US CAISI are not mentioned on that page. — [Anthropic independent evaluators](https://www.anthropic.com/news/an-update-on-independent-evaluators)

**OpenAI (Preparedness Framework)**
- GPT-5-Codex (Sep 2025) "does not meet the defined threshold for High capability in cybersecurity". — [GPT-5-Codex addendum](https://cdn.openai.com/pdf/97cc5669-7a25-4e63-b15f-5fd5bdc4d149/gpt-5-codex-system-card.pdf). GPT-5.1-Codex-Max (Nov 2025) also not High. — [OpenAI](https://openai.com/index/gpt-5-1-codex-max-system-card)
- **GPT-5.3-Codex (Feb 2026) was the first launch OpenAI treated as High capability in cybersecurity**, with a "layered safety stack". — (search summary) [OpenAI GPT-5.3-Codex system card](https://openai.com/index/gpt-5-3-codex-system-card). GPT-5.5 is reported as High, not Critical. — [LetsDataScience](https://letsdatascience.com/news/openai-releases-gpt-55-with-expanded-cybersecurity-safeguard-3639a68e). GPT-5.6 (Jul 2026; Sol/Terra/Luna) is High, not Critical. — (search summary) [GPT-5.6 card](https://deploymentsafety.openai.com/gpt-5-6/gpt-5-6.pdf)
- Aardvark is now "Codex Security" (page updated Mar 6 2026), rolling out to Enterprise/Business/Edu. — (search summary) [OpenAI Aardvark](https://openai.com/index/introducing-aardvark)

**Google DeepMind**
- "Evaluating Frontier Models for Dangerous Capabilities" (arXiv 2403.13793): in-house CTFs released minus flags and solutions. — [GDM repo](https://github.com/google-deepmind/dangerous-capability-evaluations). Attack-chain framework with 50 challenges (arXiv 2503.11917). — (search summary) [arXiv](https://arxiv.org/html/2503.11917v3)

**UK AISI / Inspect**
- `inspect_evals` is now maintained by **Generality Labs**, a London nonprofit, with founding contributions from UK AISI, Arcadia Impact and the Vector Institute. — [inspect_evals README](https://github.com/UKGovernmentBEIS/inspect_evals)
- Cyber-relevant evals in the repo: `cve_bench`, `cybench`, `cybergym`, `cybermetric`, `cyberseceval_2/3/4`, `gdm_capabilities`, `gdm_in_house_ctf`, `gdm_intercode_ctf`, `cti_realm`, plus `ipi_coding_agent` (prompt injection), `agentdojo` and `agent_threat_bench`. — [inspect_evals tree](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals)
- **Inspect Cyber** (`UKGovernmentBEIS/inspect_cyber`): "an Inspect extension developed by the UK AI Security Institute for building and running agentic cyber evaluations", docs at inspect.cyber.aisi.org.uk. Cybench's Inspect port uses it, including `verify_solutions` to check that challenges are solvable. — [inspect_cyber](https://github.com/UKGovernmentBEIS/inspect_cyber); [cybench Inspect README](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/cybench)
- K8s sandbox with domain filtering: `inspect-k8s-sandbox`. — [cybench Inspect README](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/cybench)

### Inferences
- **Builder implication:** a bug-bounty hackbot on Anthropic GA models (Opus/Sonnet 5.5, Fable 5.1) will see pentest and exploit steps re-routed or blocked unless the operator is in CVP. A solo hunter likely qualifies only for Defense Access, and only with a vulnerability-report track record. Plan model routing, appeals and multi-provider fallback, and record which model actually served each step, since re-routing silently changes what you are measuring.
- The labs' own direction (CTF → real CVEs → real zero-days → cost-capped runs) is a good template for a hackbot's internal eval ladder (see §6).
- Inspect gives a solo builder logging, epochs/pass@k reducers, sandboxing and ready ports of the main cyber benchmarks. CVE-Bench already runs on it. Writing your own private web targets as Inspect Cyber tasks keeps everything in one harness.

### Gaps
- **US CAISI** cyber evaluation documents (e.g. its DeepSeek/Kimi evaluations) could not be retrieved or verified this session.
- UK AISI's "Frontier AI Trends Report" cyber findings were seen only as a link title.
- OpenAI's specific cyber eval set (e.g. "Professional CTFs", "Cyber Range", CVE-Bench usage) and the numbers in GPT-5.x cards were not verified (openai.com blocked).
- Exact Mythos Preview / Opus 4.7 system-card cyber tables were not read (PDF hosts blocked). Only the Cybench leaderboard's extraction (100% / 96%) is used.
- Google DeepMind's Frontier Safety Framework cyber CCL results for Gemini 2.5/3 were not verified.

## 3. DARPA AI Cyber Challenge (AIxCC): results, open-sourced CRSs, architecture lessons that transfer

### Takeaway
At the AIxCC Final (DEF CON 33, Aug 2025), Team Atlanta's Atlantis won decisively: 392.76 points vs Trail of Bits 219.35 and Theori 210.68. Across finalists, the CRSs found 54 synthetic vulns, patched 43 and found 18 real zero-days. All seven CRSs are open source. The recurring lessons: robustness first (N-version systems), keep classic program analysis and fuzzing, ensemble LLMs and tools, and report nothing without an execution oracle (PoV crash, patch re-test). These map directly onto a bug-bounty hackbot: validators instead of crash oracles, ensembles of scanners and agents, and cost caps.

### Cited Findings
- **Final placements:** 1 Team Atlanta, 2 Trail of Bits, 3 Theori, 4 All You Need IS A Fuzzing Brain, 5 Shellphish, 6 42-b3yond-6ug, 7 Lacrosse. Scores: ToB 219.35, Theori 210.68 (DARPA breakdown in ToB's slides). Team Atlanta: 392.76, "more than 170" points ahead, $4M prize. — [ToB AIxCC main-stage slides](https://www.trailofbits.com/documents/AIxCC_Main_Stage_Presentation.pdf); [KAIST](https://ee.kaist.ac.kr/en/research-achieve/team-atlanta-with-professor-insu-yuns-team-wins-aixcc-final/); (search summary) [DARPA](https://darpa.mil/news/2025/aixcc-results)
- AIxCC site figures: ToB 28 vulns found, 19 patches, score 219. Shellphish 28 vulns, 11 patches, score 136. Theori ~34 synthetic vulns found, 20 patched, 211 points. — (search summary) [aicyberchallenge.com](https://aicyberchallenge.com/); [iThome](https://www.ithome.com.tw/news/171723)
- Competition-wide: **54 synthetic vulns identified, 43 patched; 18 real, non-synthetic vulnerabilities** disclosed to maintainers. Four CRSs were open-sourced on Aug 8 2025 and the rest followed. — (search summary) [DARPA](https://darpa.mil/news/2025/aixcc-results); [AIxCC archive](https://archive.aicyberchallenge.com/)
- **Open-source CRS repos (all verified to exist):**
  - Team Atlanta **Atlantis**: [Team-Atlanta/aixcc-afc-atlantis](https://github.com/Team-Atlanta/aixcc-afc-atlantis) (MIT). Paper "ATLANTIS" [arXiv 2509.14589](https://arxiv.org/pdf/2509.14589).
  - Trail of Bits **Buttercup**: [trailofbits/buttercup](https://github.com/trailofbits/buttercup) (AGPL-3.0). AI/ML-assisted fuzzing on OSS-Fuzz plus a multi-agent patcher. Minimum 8 cores / 16 GB / 100 GB. Built-in LLM budget setting. "Works best with access to models from OpenAI **and** Anthropic." Targets OSS-Fuzz-compatible C/Java. — [Buttercup README](https://github.com/trailofbits/buttercup)
  - Theori **RoboDuck**: [theori-io/aixcc-afc-archive](https://github.com/theori-io/aixcc-afc-archive). Warning: "This code can easily spend $1,000 or more in under an hour". Agent traces at [theori-io.github.io/aixcc-public](https://theori-io.github.io/aixcc-public/index.html).
  - Shellphish **ARTIPHISHELL**: [shellphish/artiphishell](https://github.com/shellphish/artiphishell). Azure/Terraform/Helm deployment, using OpenAI, Anthropic and Gemini keys.
  - 42-b3yond-6ug **BugBuster**: [42-b3yond-6ug/42-b3yond-6ug-crs](https://github.com/42-b3yond-6ug/42-b3yond-6ug-crs) (GPLv3, Terraform/K8s).
  - **All You Need Is a Fuzzing Brain**: [o2lab/afc-crs-all-you-need-is-a-fuzzing-brain](https://github.com/o2lab/afc-crs-all-you-need-is-a-fuzzing-brain). Paper [arXiv 2509.07225](https://arxiv.org/abs/2509.07225). "Suspicious-Point" reasoning agents over OSS-Fuzz. "Nothing is reported that has not crashed a real build". Runs with one API key and `--budget-limit`.
  - SIFT **Lacrosse**: [siftech/afc-crs-lacrosse](https://github.com/siftech/afc-crs-lacrosse). Multi-agent fuzzing plus symbolic reasoning, deployed as 3 × Azure Standard_D128ds_v6 VMs.
- **Theori RoboDuck architecture:**
  - Full-scan mode: Infer static analysis plus LLM repo analysis produce vuln reports, which are scored against a threshold before agent analysis.
  - Delta-scan mode: an agent analyzes diffs.
  - LLM crash triage, then LLM dedup of vulns, then a PoV producer.
  - Coverage DB, then fuzz frontier, then "flip branches with agent" to generate seeds.
  - Patch generation, then a bundle of PoV + patch + SARIF.
  
  — [RoboDuck architecture doc](https://github.com/theori-io/aixcc-afc-archive/blob/main/docs/crs-architecture.md)
- **Team Atlanta lessons** (Taesoo Kim, Aug 12 2025):
  - **L0: "System Robustness is Priority #1"**. A string-matching heuristic for skipping fuzz harnesses nearly sank them hours before the deadline. Their answer was "N-version programming with orthogonal approaches": Atlantis is several independent CRSs (C, Java, Multilang, Patch, SARIF).
  - **L1: "Don't Give Up on Traditional Program Analysis"**: ensemble fuzzers (LibAFL, libFuzzer, AFL++, custom Jazzer), directed fuzzers, concolic execution.
  - **L2: Ensembling**: LLM non-determinism ("hallucination") is used as diversity, with the PoV as the oracle for patches. Patch accuracy matters, e.g. the accuracy-modifier math cited for Theori's 44.4% accuracy.
  - **L3: LLM babysitting**: role prompting, and fast-changing models.
  
  — [Team Atlanta post-afc](https://github.com/Team-Atlanta/team-atlanta.github.io/blob/main/content/english/blog/post-afc.md)
- **Post-AIxCC patching study** (Team Atlanta, Mar 11 2026): 10 agent configurations (Claude Code, Codex CLI, Copilot, Gemini CLI × 5 models) on 63 AIxCC crashes from 24 projects, with 3 attempts and automated validation.
  - Best configurations reach ~45/63 (~71%) correct with ~80% semantic correctness, vs a 2025 baseline of 33/63 (Claude Code + 3.7 Sonnet).
  - **~20–40% of patches that pass all automated checks are semantically wrong.**
  - "Model choice outweighs framework choice."
  - Ensemble-by-selection beats every single agent, and **N=3 is the sweet spot**.
  - Components are released in **OSS-CRS** ([sslab-gatech/oss-crs](https://github.com/sslab-gatech/oss-crs)) and [crs-patch-ensemble](https://github.com/Team-Atlanta/crs-patch-ensemble).
  
  — [Team Atlanta 2026 patching post](https://github.com/Team-Atlanta/team-atlanta.github.io/blob/main/content/english/blog/post-patch-2026-ensemble.md)
- OSS-CRS tutorial at SVCC 2026 (June 2026) uses a "CRSBench" subset of projects. — [Team Atlanta OSS-CRS tutorial](https://github.com/Team-Atlanta/team-atlanta.github.io/blob/main/content/english/blog/post-oss-crs-tutorial.md)
- SoK paper: "SoK: DARPA's AI Cyber Challenge (AIxCC): Competition Design, Architectures, and Lessons Learned" (arXiv 2602.07666). Georgia Tech presented AIxCC lessons at USENIX Security 2026. — [arXiv 2602.07666](https://arxiv.org/pdf/2602.07666); [Newswise](https://www.newswise.com/articles/georgia-tech-researchers-share-ai-cyber-challenge-lessons-at-usenix-security-2026)
- ToB pre-final write-up "AIxCC finals: Tale of the tape" (Aug 7 2025) and the 2nd-place post (Aug 9 2025). — [ToB blog](https://blog.trailofbits.com/2025/08/07/aixcc-finals-tale-of-the-tape/); [ToB 2nd place](https://blog.trailofbits.com/2025/08/09/trail-of-bits-buttercup-wins-2nd-place-in-aixcc-challenge/). Theori blog: [AIxCC and RoboDuck](https://theori.io/blog/aixcc-and-roboduck-63447).

### Inferences
- **What transfers to a web bug-bounty hackbot:**
  1. **Oracle-gated reporting.** AIxCC used sanitizer crashes and PoV re-runs. A web hackbot needs deterministic validators: an out-of-band callback for SSRF/XXE, a DOM-execution check in a headless browser for XSS, differential responses between two accounts for IDOR, timing/boolean oracles for SQLi. Nothing goes in a report without a passing validator. This mirrors FuzzingBrain's "nothing reported that has not crashed".
  2. **Delta mode.** Theori's diff-analysis path corresponds to re-testing only what changed: new JS bundles, new endpoints, new deploys.
  3. **Ensembles with selection.** Run N≈3 diverse agents or models and pick, as with Team Atlanta's patch ensemble.
  4. **Robustness and N-version design.** Independent pipelines per vuln class, so one heuristic bug can't zero out the run.
  5. **Hard budget caps.** Theori burned $1k+/hour; Buttercup and FuzzingBrain expose budget flags.
  6. **Keep classic tooling.** AIxCC teams kept fuzzers and static analysis. The web analogues are scanners, crawlers and fuzzers alongside LLM reasoning.
- The 20–40% "passes tests but wrong" patch rate is a warning about automated validation in general. Web validators can also be fooled (e.g. reflected input that never executes), so sample findings for manual review regularly.

### Gaps
- Prize amounts for 2nd and 3rd ($3M / $1.5M by recollection) and the scores of 42-b3yond-6ug, FuzzingBrain and Lacrosse were not verified.
- The AIxCC SoK paper's contents and the DARPA results page itself were not read (blocked). Real-zero-day counts per team are unknown.
- Shellphish's and ToB's detailed architecture write-ups were not retrieved.

## 4. AI vulnerability-discovery efforts: what they disclose about method

### Takeaway
Successful AI vuln-discovery systems share four things. They narrow scope (variant analysis from a known patch, call chains from user input, commit and diff scanning). They run many attempts. They validate every finding in a sandbox (crash, PoC or triage confirmation). They keep a human gate before disclosure. Raw model hit rates on a known bug can be low, e.g. o3 found a known ksmbd bug in 8/100 runs. Scale plus validation is what turns that into real CVEs: XBOW's #1 US HackerOne ranking, Big Sleep's 20 OSS bugs, Mythos Preview's "thousands" of zero-days.

### Cited Findings
- **Google Project Naptime → Big Sleep** (Project Zero + DeepMind):
  - First real-world bug found by the agent: an exploitable **stack buffer underflow in SQLite** (Oct/Nov 2024). The method was **variant analysis**: start from a previously patched bug and look for similar flaws. — [Project Zero "From Naptime to Big Sleep"](https://projectzero.google/2024/10/from-naptime-to-big-sleep.html); (search summary) [The Record](https://therecord.media/google-big-sleep-ai-tool-found-bug)
  - Mid-2025: Big Sleep plus threat intel let Google "predict that a vulnerability was imminently going to be used" (SQLite) and cut it off. — (search summary) [The Record](https://therecord.media/google-big-sleep-ai-tool-found-bug)
  - Aug 2025: **20 OSS vulnerabilities**, including FFmpeg and ImageMagick. The agent found and reproduced each autonomously, and a human verified each before filing. — (search summary) [TechRadar](https://www.techradar.com/pro/one-of-googles-big-ai-projects-uncovered-some-serious-security-threats-seeminlgy-all-on-its-own)
  - Critique: a target-specific fuzzer "would be at least as effective" in some cases. — [ITPro](https://itpro.com/security/googles-big-sleep-ai-model-just-found-a-zero-day-vulnerability-in-the-wild-but-dont-hold-your-breath-for-game-changing-ai-bug-hunting-tools-any-time-soon)
- **OSS-Fuzz-Gen** (Google): LLM-generated fuzz targets for C/C++/Java/Python, scored by compilability, runtime crashes, coverage and line-coverage diff vs human targets. In the Jan 2024 experiment, 1,300+ benchmarks across 297 projects produced valid targets for 160 C/C++ projects, with up to 29% line-coverage increase. — [google/oss-fuzz-gen](https://github.com/google/oss-fuzz-gen)
- **OpenAI Aardvark → Codex Security** (announced Oct 2025, GPT-5): repo threat model, then commit-history and new-commit scanning, then **trigger the suspected flaw in a sandbox to confirm exploitability**, then a Codex-generated patch for human review. ≥10 CVEs in OSS. The 92% recall on "golden" repos is from secondary coverage only. — (search summary) [OpenAI](https://openai.com/index/introducing-aardvark); [The Hacker News](https://thehackernews.com/2025/10/openai-unveils-aardvark-gpt-5-agent.html)
- **Anthropic** (see §2): Sonnet 4.5 found new vulns in >33% of CyberGym projects at 30 trials (Sep 2025). Mythos Preview found thousands of zero-days autonomously (Apr 2026). OSS Scanner targets >90% true-positive rate (Oct 2026). — [Anthropic cyber defenders](https://www.anthropic.com/research/building-ai-cyber-defenders); [Glasswing](https://www.anthropic.com/glasswing); [Cyber Mission](https://www.anthropic.com/news/anthropic-cyber-mission)
- **Protect AI Vulnhuntr**: LLM plus static analysis that builds **call chains from remote user input to server output**. Python only, with classes LFI, AFO, RCE, XSS, SQLi, SSRF, IDOR. Requires Python 3.10 (Jedi). Billed as the "world's first autonomous AI-discovered 0day vulnerabilities". CVEs include gpt_academic LFI/XSS (CVE-2024-10100/10101), ComfyUI XSS (CVE-2024-10099), FastChat SSRF (CVE-2024-10044) and Ragflow RCE (CVE-2024-10131). — [protectai/vulnhuntr](https://github.com/protectai/vulnhuntr)
- **Sean Heelan, "How I used o3 to find CVE-2025-37899"** (May 22 2025): a UAF in Linux ksmbd SMB2 LOGOFF handling, found with plain API calls and no agent framework. Benchmark on the known bug CVE-2025-37778 (Kerberos auth): **o3 8/100 runs, Claude 3.7 Sonnet 3/100, Claude 3.5 Sonnet 0/100**. With the larger full-file context, o3 found the known bug only once in 100 runs (different setup). — (search summary) [sean.heelan.io](https://sean.heelan.io/2025/05/22/how-i-used-o3-to-find-cve-2025-37899-a-remote-zeroday-vulnerability-in-the-linux-kernels-smb-implementation/); [Linuxiac](https://linuxiac.com/chatgpt-o3-model-found-remote-zeroday-in-linux-kernel-code/)
- **XBOW on HackerOne** (2025):
  - First autonomous pentester to reach **#1 on the US HackerOne leaderboard**. "Every submission was confirmed and triaged by program owners". — (search summary) [XBOW "The Road to Top 1"](https://xbow.com/blog/top-1-how-xbow-did-it/)
  - Volume: >1,000 reports in a few months — [TechRepublic](https://www.techrepublic.com/article/news-ai-xbow-tops-hackerone-us-leaderboad/); NYU reported 285 vulnerabilities — [NYU Engineering](https://engineering.nyu.edu/news/ai-driven-pen-tester-became-top-bug-hunter-hackerone)
  - Critique: the ranking is HackerOne "reputation", and secondhand claims say ~90% of the score came from XSS. — [LessWrong](https://www.lesswrong.com/posts/qefrWyeiMvWEFRitN/be-skeptical-of-milestone-announcements-by-young-ai-startups)
  - ~45% of findings were still awaiting resolution at the time (CSO, via search). XBOW raised a $75M round. — [Slashdot](https://tech.slashdot.org/story/25/07/05/1847237)
  - Aug 2025: no longer focused on the leaderboard. — [XBOW blog](https://xbow.com/blog/xbow-on-hackerone-whats-next)
- **Open-source web pentest agents** (relevant baselines):
  - **Shannon**: Keygraph, AGPL-3.0. White-box (needs source), agentic SAST plus exploitation, SARIF output. A full run takes ~1–1.5 h. — [KeygraphHQ/shannon](https://github.com/KeygraphHQ/shannon)
  - **Strix**: Apache-2.0, with "bug bounty automation… generate PoCs". — [usestrix/strix](https://github.com/usestrix/strix)
  - **PentestGPT**: USENIX Sec 2024 origin; 86.5% on XBOW-104 in Dec 2025. — [GreyDGL/PentestGPT](https://github.com/GreyDGL/PentestGPT)
  - **CAI**: Alias Robotics, "bug bounty-ready" (arXiv 2504.06017). Claims 3,600× faster and 156× cheaper than humans, CVSS 4.3–7.5 findings in production. — [aliasrobotics/cai](https://github.com/aliasrobotics/cai)
- Agents can themselves be attacked: "Hacking the AI Hackers via Prompt Injection" (arXiv 2508.21669) shows AI security tools are injectable and proposes a 4-layer guardrail. — [CAI README](https://github.com/aliasrobotics/cai)

### Inferences
- **Method patterns to copy:**
  - Variant analysis seeded from known fixes (Big Sleep). The web version: mine a target's past disclosed reports, changelogs and CVEs, then hunt variants.
  - Source-to-sink call-chain tracing (Vulnhuntr) for white-box web targets.
  - Commit and diff monitoring (Aardvark, RoboDuck delta mode).
  - Many cheap runs with aggregation (Heelan's 100-run methodology, CyberGym's 30 trials).
  - Mandatory sandbox or validator confirmation before reporting (Aardvark, XBOW, FuzzingBrain).
- Heelan's numbers imply that **precision per run is low and noisy**: 8% hit rate, with the rest false positives or misses. A bug-bounty hackbot must invest in dedup, validation and triage. Otherwise it floods programs, which is the XSS-heavy, high-volume critique levelled at XBOW.
- Prompt injection from target content is a real threat to hackbots that read untrusted pages. Include injection test cases in the eval suite.

### Gaps
- **ZeroPath, Depthfirst and AISLE** could not be verified this session (search budget exhausted, domains blocked). Recollection, not verified: AISLE reported finding OpenSSL CVEs in early 2026, and ZeroPath was used by a researcher to report many curl bugs in 2025. Verify before citing.
- XBOW's own breakdown (resolved/duplicate/informative counts) and its validator design details from the "Top 1" post were not read directly.
- Big Sleep's 2026 status and the CVE IDs of its findings are unverified.
- The red.anthropic.com technical posts (methods and scaffolds behind Mythos findings) were not readable.

## 5. CTF-playing agents and AI tracks at competitions: what they show

### Takeaway
2024–2025 work showed that simple, well-elicited agents can saturate easy CTFs: Palisade hit 95% on InterCode-CTF with plain ReAct. By late 2025, AI agents were winning or placing near the top of human CTFs (Alias's CAI: #1 AI at HTB "AI vs Humans", 41/45 at Neurogrid, 32/34 at Dragos OT; self-reported). Alias argues Jeopardy CTFs are now "a solved game" and that the frontier is Attack-and-Defense and long-horizon realism. Elicitation matters as much as architecture.

### Cited Findings
- **Palisade, "Hacking CTFs with Plain Agents"** (Dec 2024): 95% on InterCode-CTF vs prior 29% and 72%. Key findings: "Hidden Potential", "Elicitation is hard" and "Simplicity Wins". Configurations tested include "ReAct&Plan@5". Logs are released. — [PalisadeResearch/intercode](https://github.com/PalisadeResearch/intercode); [arXiv 2412.02776](https://arxiv.org/pdf/2412.02776)
- **NYU agents:** EnIGMA (Princeton/NYU SWE-agent-based, interactive tools), D-CIPHER (planner/executor/auto-prompter) and CRAKEN (RAG over CTF writeups; Self-RAG + Graph-RAG) top the NYU CTF Bench at 27→44/200 between Oct 2024 and Aug 2025. — [NYU leaderboard.json](https://raw.githubusercontent.com/NYU-LLM-CTF/nyu-llm-ctf.github.io/HEAD/data/leaderboard.json)
- **Alias Robotics CAI competition record (self-reported):**
  - "Rank #1 at Neurogrid, Dragos OT and HTB 'AI vs Humans'".
  - Neurogrid CTF: 41/45 flags and the $50,000 prize. "The World's Top AI Agent for Security CTF" (arXiv 2512.02654, Dec 2025) argues Jeopardy CTFs are now a "solved game" and the field must move to Attack & Defense.
  - Dragos OT CTF 2025: rank #1 at hours 7–8, 32/34 challenges (arXiv 2511.05119).
  - HTB AI vs Humans: #1 AI worldwide, top-20 overall.
  
  — [CAI README](https://github.com/aliasrobotics/cai); [HTB event scoreboard](https://ctf.hackthebox.com/event/2000/scoreboard)
- A Spanish press article reports the CAI/alias1 41/45 win. — [El Referente](https://elreferente.es/?p=192675)
- Multi-scaffold result: no single harness dominates. A blackboard of 5 scaffolds solves 19/33 Cybench vs 15/33 for the best single one (arXiv 2605.28334). — [CAI README](https://github.com/aliasrobotics/cai)
- The **CAI Dataset** (arXiv 2605.28146): 230,935 sessions, 26M prompts, 123 countries. It reports that operators routinely paste live credentials and production hostnames into frontier APIs. — [CAI README](https://github.com/aliasrobotics/cai)
- NYU runs a "CSAW'25 LLM CTF Attack Challenge" track. — [NYU site source](https://raw.githubusercontent.com/NYU-LLM-CTF/nyu-llm-ctf.github.io/HEAD/index.html)

### Inferences
- Before building an elaborate multi-agent system, a solo builder should first max out a **plain ReAct + planning agent with good tools and prompts** (Palisade), then add ensembles or scaffold diversity (Alias's blackboard, Team Atlanta's N-version).
- Competition wins are short-horizon Jeopardy events. Bug bounty work is long-horizon, stateful (auth, multi-step business logic) and judged on impact and duplicates. Competition results overstate readiness for live programs.
- Data hygiene: never send live program secrets to third-party APIs without program permission, given the CAI dataset finding.

### Gaps
- Palisade/HTB's "AI vs Humans" crowdsourced-elicitation paper (arXiv 2505.19915, by recollection) and its numbers were not verified.
- DEF CON AI Village or other AI-track results for 2025–26 were not found this session.
- The HTB scoreboard itself was not fetched (blocked). The CAI claims are self-reported.

## 6. Practical guide: setting up a solo hackbot eval harness, the evaluation ladder, and avoiding contamination/overfitting

### Takeaway
Use **Inspect + Inspect Cyber** as the harness. Start with cheap, saturated suites as regression gates (XBOW-104, InterCode, Cybench web), then move up to real-CVE web exploitation (CVE-Bench zero-day), bounty-economics tasks (BountyBench Detect), and private post-cutoff holdouts graded like real triage. Report pass@k, cost per validated finding, false-positive rate and variance over many runs. Control contamination with build-time random flags, private and fresh targets, and trace-level checks.

### Cited Findings
- Harness building blocks:
  - Inspect evals with epochs and reducers, e.g. GDM CTF defaults to 10 epochs and `at_least_1`. — [gdm_in_house_ctf](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/gdm_in_house_ctf)
  - Inspect Cyber for authoring agentic cyber tasks. — [inspect_cyber](https://github.com/UKGovernmentBEIS/inspect_cyber)
  - K8s sandbox with domain filtering. — [cybench Inspect](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/cybench)
  - CVE-Bench already runs on Inspect. — [cve-bench](https://github.com/uiuc-kang-lab/cve-bench)
- **Cost and scale data points:**
  - CyberGym needs ~240 GB of data and ~10 TB for the full server; a 10-task subset exists. — [cybergym](https://github.com/sunblaze-ucb/cybergym)
  - 30 trials of one CyberGym task cost ~$45 (Sonnet 4.5), and the leaderboard cap is $2/vuln. — [Anthropic](https://www.anthropic.com/research/building-ai-cyber-defenders)
  - Theori's CRS can spend $1,000+/hour. — [Theori archive](https://github.com/theori-io/aixcc-afc-archive)
  - A Shannon full run takes ~1–1.5 h, and a Photoview run cost $6–$115 depending on model. — [Shannon](https://github.com/KeygraphHQ/shannon); [Shannon benchmark doc](https://github.com/KeygraphHQ/shannon/blob/main/docs/shannon-xbow-aikido-benchmark.md)
  - BountyBench Claude Code Patch cost $82.19 in tokens across tasks. — (search summary) [BountyBench](https://arxiv.org/pdf/2505.15216)
  - SEC-bench needs >200 GB of disk. — [SEC-bench](https://github.com/SEC-bench/SEC-bench)
- **Contamination and validity controls documented by benchmark authors:**
  - XBOW injects flags at build time, adds canary GUIDs, and admits "underlying vulnerabilities have been trained into the models". — [XBOW](https://github.com/xbow-engineering/validation-benchmarks)
  - GDM stripped flags and solutions and asks users to randomize flags. — [GDM](https://github.com/google-deepmind/dangerous-capability-evaluations)
  - NYU provides a separate dev split for agent tuning. — [NYU CTF Bench](https://github.com/NYU-LLM-CTF/NYU_CTF_Bench)
  - CVE-Bench reference exploits are available only on request, and v2 was audited with the Agentic Benchmark Checklist. — [cve-bench](https://github.com/uiuc-kang-lab/cve-bench)
  - A harness fork leaked a Cybench answer and inflated scores. — [Cybench site](https://github.com/cybench/cybench.github.io)
  - Rebuilt Cybench images differ from the pinned digests, so use `verify_solutions`. — [cybench Inspect](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/cybench)
  - Shannon used security fixes landed after a later date (Jul 2026) as ground truth to reduce training overlap. — [Shannon doc](https://github.com/KeygraphHQ/shannon/blob/main/docs/shannon-xbow-aikido-benchmark.md)
  - CyberGym now requires trajectories and cost reporting. — [SUBMISSION.md](https://github.com/sunblaze-ucb/cybergym/blob/main/SUBMISSION.md)
- **Safety of the eval environment:**
  - CyberGym says not to expose its server publicly and to bind to the Docker-network gateway. — [cybergym](https://github.com/sunblaze-ucb/cybergym)
  - XBOW code is intentionally vulnerable and meant for sandboxes only. — [XBOW](https://github.com/xbow-engineering/validation-benchmarks)
  - Cybench gives the model Kali plus possible internet access. — [cybench Inspect](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/cybench)
- Measurement-science papers to follow:
  - Consistency over 400 runs. — [arXiv 2605.30096](https://arxiv.org/pdf/2605.30096)
  - Trace provenance (did the agent actually exploit, or shortcut to the flag?). — [arXiv 2608.26237](https://arxiv.org/pdf/2608.26237)
  - Cost-aware evaluation. — [arXiv 2607.15263](https://arxiv.org/pdf/2607.15263)
  - Verification when evidence lies. — [ATOBench](https://arxiv.org/pdf/2608.12996)

### Inferences
**Recommended evaluation ladder (advance when a rung saturates at ~90%+ pass@1 at an acceptable cost):**
0. **Harness smoke tests (free, local).** Dockerized intentionally vulnerable apps such as DVWA, OWASP Juice Shop and WebGoat, plus PortSwigger Web Security Academy labs (labs are hosted by PortSwigger, so check their terms for automated use). Goal: tool plumbing, auth handling, crawling, report formatting. These are heavily contaminated, so use them for plumbing only, never as a capability claim. *(Recommendation; these targets were not separately verified this session.)*
1. **Regression gate: XBOW validation-benchmarks (104 web, levels 1–3).** Rebuild with random `FLAG=`. A frontier-model hackbot should be at ~100%; cheaper/open models give signal. Also run InterCode-CTF via Palisade's harness as a sanity check on general shell/CTF elicitation.
2. **Broad CTF capability: Cybench via Inspect (39–40 tasks; report the 8 web separately) + NYU CTF Bench web (19) and dev split.** Use pass@1 and pass@10 with ≥5 epochs. Mostly a check that the agent loop isn't the bottleneck.
3. **Real web exploitation: CVE-Bench v2.x, zero-day variant first, then one-day.** State-based grading is closest to bug-bounty "impact". Track per-objective success (RCE, DB access, admin login, SSRF).
4. **Bug-bounty economics: BountyBench Detect (and Patch for report quality).** Report dollar-weighted recall net of token cost, the same unit as your real-world ROI.
5. **White-box discovery, if the hackbot reads source: CyberGym level0/1 subset (cost-capped, e.g. $2/task) and SEC-bench.** Optional for black-box web hunters; expensive to host.
6. **Private, post-cutoff holdouts (the real test).** (a) Self-hosted OSS web apps pinned to versions before security fixes that landed after your models' training cutoffs (Shannon's Photoview method). (b) Retired HTB/VulnHub machines you haven't tuned on, with randomized flags. (c) Your own authored Inspect Cyber tasks for multi-step business-logic, IDOR and auth-chain bugs, which public suites underweight. Keep 30–50% of these hidden and never use them for prompt tuning.
7. **Live, authorized programs (VDPs/BBPs that allow automation).** Metrics: valid-report rate, duplicate rate, triage-accepted severity, mean cost per accepted report, program-owner complaints. Keep a human gate on every submission, as Big Sleep, Aardvark and XBOW all do.

**Metrics to log on every rung:**
- pass@1 / pass@k over ≥5 runs, with variance.
- Validated findings per $ and per hour.
- False-positive rate after the validator, plus a manually audited sample, because "passes validator but wrong" was 20–40% in AIxCC patching.
- Duplicate rate.
- Which model actually served each step, since Anthropic GA models may re-route cyber tasks to Opus 4.8.
- Trajectory provenance (exploit evidence vs a flag found by grep).

**Anti-overfitting rules:**
- Tune prompts only on dev splits (NYU dev, your own dev targets).
- Freeze the test sets.
- Rotate holdouts every model generation.
- Randomize flags and canaries at build time.
- Diff your trajectories for benchmark-specific hard-coding.
- Prefer state- or impact-based grading over flag strings.
- Report results with the harness version and model snapshot, given the CyberGym 66.6→73.8 harness effect.

**Cost guidance (rough):** rungs 0–3 can be run locally on one Docker host for tens to low hundreds of dollars per full pass with frontier APIs, if you set iteration caps and per-task budgets. CyberGym and SEC-bench at full scale need hundreds of GB to TB of storage and large token budgets. Always set per-task USD caps, as CyberGym's leaderboard and Buttercup/FuzzingBrain do. *(The order of magnitude is an inference, not a measured figure.)*

### Gaps
- No public, maintained benchmark was found that targets bug-bounty-style web logic flaws (IDOR, auth/business-logic chains) with realistic multi-account state. Builders must author these.
- Verified per-run dollar costs for CVE-Bench and the Cybench web subset with 2026 models were not found.
- Terms of service for automated agents on PortSwigger Academy, HTB and VulnHub, and HackerOne/Bugcrowd AI-agent submission policies, were not verified this session.
