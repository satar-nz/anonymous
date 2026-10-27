# Practical Engineering, Safety, Legal/Platform-Policy and Roadmap Considerations for an Authorized AI Hackbot

*Research date: 2026-10-27. How sources were checked: in this sandbox the egress proxy blocked most sites (hackerone.com, bugcrowd.com, intigriti.com, genai.owasp.org, nist.gov, ncsc.gov.uk, arxiv.org, openai.com, xbow.com, roadmap.sh, simonwillison.net, embracethered.com and others returned 403). Each source carries one of two tags. **[F]** means I fetched and read the page directly on 2026-10-27 (anthropic.com, support.claude.com, platform.claude.com, code.claude.com, raw.githubusercontent.com). **[S]** means a search engine returned the URL on 2026-10-27 and I relied on its summary or snippet, without opening the page; check the exact wording before quoting it. The shared web-search budget ran out partway through, so several sub-questions are listed as gaps rather than guessed.*

---

## 1. Bug bounty platform rules and program policies on AI and automation (HackerOne, Bugcrowd, Intigriti, YesWeHack, Synack); the AI-slop crisis; legal framing

### Takeaway
No major platform bans AI-assisted hunting. Every platform that has published a stance puts **accountability on the human operator**: follow each program's policy, validate before you submit, and expect suspensions or bans for unvalidated AI or automated output. The 2025–2026 "AI slop" wave closed or paused several open-source bounties: curl ended its bounty on 31 Jan 2026, the Internet Bug Bounty paused from 27 Mar 2026, and Node.js, Nextcloud and Google OSS VRP paused or suspended theirs. In practice a hackbot needs deterministic validation plus human review before it submits anything. On the legal side, written authorization and scope remain the foundation. The US DOJ's 2022 good-faith policy and the UK's still-pending CMA reform give limited, discretionary protection, not immunity.

### Cited Findings

**HackerOne**
- In HackerOne's "Welcome, Hackbots" post, hackbots "must operate within the published vulnerability disclosure policies of the program they're engaging with, along with HackerOne's Code of Conduct and Disclosure Guidelines", and "Hackbot operators are responsible for their Hackbots and must exercise due diligence to ensure compliance with platform rules and program policies." — [HackerOne blog, Welcome Hackbots](https://www.hackerone.com/blog/welcome-hackbots-how-ai-shaping-future-vulnerability-discovery) [S]
- Naming caution: HackerOne's docs page called "Hackbot" covers HackerOne's own free in-platform assistant for program staff. It is not a policy for external AI agents. — [docs.hackerone.com/organizations/hackbot.html](https://docs.hackerone.com/organizations/hackbot.html) [S]
- XBOW reached #1 on the HackerOne US leaderboard in 2025 with "nearly 1,060 vulnerabilities". It says "All findings were fully automated, though our security team reviewed them pre-submission to comply with HackerOne's policy on automated tools." — [XBOW, "Top 1: how XBOW did it"](https://xbow.com/blog/top-1-how-xbow-did-it/) [S]. A secondary summary says XBOW uses "validators, automated peer reviewers that confirm each vulnerability" (not checked against the original post). — same search result set [S]
- Skeptical view: the leaderboard ranks by "reputation", which mixes how many bugs you report with the percentage accepted, and XBOW's reports are mostly undisclosed. — [LessWrong, "Be skeptical of milestone announcements by young AI startups"](https://www.lesswrong.com/posts/qefrWyeiMvWEFRitN/be-skeptical-of-milestone-announcements-by-young-ai-startups) [S]. CyberScoop notes the "no human input" claim sits alongside heavy infrastructure for choosing targets. — [CyberScoop](https://cyberscoop.com/is-xbows-success-the-beginning-of-the-end-of-human-led-bug-hunting-not-yet/) [S]
- HackerOne co-founder and CTO Alex Rice said the Code of Conduct "does not prohibit the use of AI to assist in writing reports, but it does prohibit spam", and reported "an aggregate increase in report quality" from AI help. — [HackerOne blog, "Navigating the AI Wave"](https://hackerone.com/blog/ai-driven-report-volume-insights-and-actions) [S]; Code of Conduct at [hackerone.com/policies/code-of-conduct](https://www.hackerone.com/policies/code-of-conduct) [S]. One third-party summary says a May 2026 Code of Conduct update makes researchers responsible for submission quality "regardless of whether human or tool generated the first draft". **I could not confirm this against HackerOne's own text.**
- In Feb 2026 HackerOne said it was "updating" its terms after hunters asked whether their reports were being used to train AI (Hai). — [The Register, 2026-02-18](https://www.theregister.com/2026/02/18/hackerone_ai_policy/) [S] (headline and snippet only)
- Internet Bug Bounty (run on HackerOne): paused new submissions effective **27 March 2026**, citing "a worsening imbalance between vulnerability discoveries and the ability for open source maintainers to remediate them". Projects still triage through HackerOne but no longer pay IBB rewards. — [CSO Online](https://www.csoonline.com/article/4154216/internet-bug-bounty-program-hits-pause-on-payouts-2.html) [S]; [Dark Reading](https://darkreading.com/application-security/ai-led-remediation-crisis-prompts-hackerone-pause-bug-bounties) [S]; [Privacy Guides, 2026-04-17](https://www.privacyguides.org/news/2026/04/17/hackerone-pauses-internet-bug-bounty/) [S]. In May 2026 a hunter was paid months late "at a drastically reduced reward rate". — [The Register, 2026-05-21](https://www.theregister.com/security/2026/05/21/hackerone-takes-an-axe-to-its-bug-bounty-rewards/5244458) [S]

**Bugcrowd**
- Bugcrowd's blog names the problem "sloptimism": large volumes of speculative or AI-generated reports "submitted with minimal to no pre-submission validation". It says this violates its terms of service. The search summary also reports a **30-day suspension** where "submissions are attributable to automated or AI-generated activity without sufficient validation prior to submission". Bugcrowd also launched a Priority Queue Bypass that fast-tracks triage for high-accuracy researchers. — [Bugcrowd, "Policy changes to address AI slop submissions"](https://www.bugcrowd.com/blog/bugcrowd-policy-changes-to-address-ai-slop-submissions/) [S]. The publication date was not captured.
- Bugcrowd researcher docs: researchers must manually verify the accuracy and reproducibility of any GenAI-assisted findings, and "Automated or unverified outputs are not accepted as valid submissions". Researchers are responsible for their use of GenAI tools. — [docs.bugcrowd.com, AI privacy & security](https://docs.bugcrowd.com/researchers/onboarding/ai-privacy-security/) [S]
- A secondary analysis claims permanent bans for "submission farming", 30-day suspensions after ≥10 consecutive invalid reports, and ID verification for repeat offenders. **Unverified.** — [decryptiondigest.com](https://www.decryptiondigest.com/blog/bug-bounty-programs-ai-era-hackerone-bugcrowd-2026) [S]

**Intigriti**
- A census of 53 published programs (retrieved 28 July 2026) found **0/53 prohibit AI-assisted or AI-generated submissions outright**. Only **3/53 require AI disclosure: Intigriti, Django and FFmpeg**. Wireshark accepts AI-written reports but refuses agent-submitted ones. — [Stingrai census](https://www.stingrai.io/blog/ai-generated-vulnerability-report-policies-census) [S] (secondary; the Intigriti clause itself was not seen)
- Intigriti's Researcher T&C bar researchers from disclosing to third parties "(without the Company's permission) or misus[ing] information or data that was acquired in the context of a Program". Programs may add custom terms. — [Intigriti KB, Researcher Terms & Conditions](https://kb.intigriti.com/en/articles/5466165-researcher-terms-conditions) [S]; [Community Code of Conduct](https://kb.intigriti.com/en/articles/5247238-community-code-of-conduct) [S]

**YesWeHack and Synack**
- YesWeHack's public AI page covers its own platform AI features ("can be individually disabled at any given moment"), not rules for hunters. Its 2026 report surveys hunters' AI use. YesWeHack also launched an "Agentic Pentest" product and says bounty-program data "will not be used to train" its models. — [YesWeHack, Our Approach to AI](https://www.yeswehack.com/product/ai-vulnerability-management) [S]; [YesWeHack Report 2026](https://choose.yeswehack.com/bug-bounty-report-2026-trends-and-key-insights-yeswehack) [S]
- Synack's "Sara" AI pentesting agent runs with the Synack Red Team (>1,500 vetted researchers). Findings go to human experts for review, and Sara AI Pentesting reached general availability in May 2026. — [IT Brief UK](https://itbrief.co.uk/story/synack-launches-sara-ai-pentesting-for-wider-coverage) [S]; [Synack Sara datasheet](https://go.synack.com/hubfs/synack-sara-ai-pentesting-datasheet-continuous-security-validation.pdf?hsLang=en) [S]

**The AI-slop crisis and open-source fallout**
- curl's **current** policy (read from the master branch on 2026-10-27) says: "There is no bug bounty and the curl project never offers rewards for reported vulnerabilities." Reports still go to HackerOne, or to GitHub if severity is below HIGH. It asks reporters to "explain your issues or improvements briefly and clearly in your own human voice. Do not lazily paste massive, AI-generated explanations". Someone on the security team acknowledges each report "that a human has seen the report". — [curl docs/VULN-DISCLOSURE-POLICY.md](https://raw.githubusercontent.com/curl/curl/master/docs/VULN-DISCLOSURE-POLICY.md) [F]
- Timeline:
  - May 2025: Stenberg said reporters of AI-slop reports on HackerOne would be banned, and proposed a project AI policy on the mailing list. — [curl-library mailing list, 2025-05](https://curl.se/mail/lib-2025-05/0013.html) [S]; [Open Source Security podcast](https://opensourcesecurity.io/2025/2025-05-curl_vs_ai_with_daniel_stenberg/) [S]
  - Aug 2025: "AI slop attacks on the curl project". — [daniel.haxx.se, 2025-08-18](https://daniel.haxx.se/blog/2025/08/18/ai-slop-attacks-on-the-curl-project) [S]
  - Jan 2026: the bounty ends on **31 Jan 2026**. The confirmed-vulnerability rate reportedly fell "from north of 15% to below 5%", while Stenberg also knows of ">100 good AI-assisted reports". — [The Register, 2026-01-21](https://www.theregister.com/2026/01/21/curl_ends_bug_bounty/) [S]; [BleepingComputer](https://bleepingcomputer.com/news/security/curl-ending-bug-bounty-program-after-flood-of-ai-slop-reports/) [S]; [LWN](https://lwn.net/Articles/1055443/) [S]
  - Conflicting later accounts: HeroDevs says the slop problem "effectively resolved itself" by April 2026 ([HeroDevs](https://www.herodevs.com/blog-posts/how-ai-broke-open-source-security-end-of-life-most-exposed) [S]), while a Pinggy blog claims curl stopped reading reports in July 2026 ([Pinggy](https://pinggy.io/blog/curl_ai_slop_summer_of_bliss/) [S]). The current policy file [F] still routes reports to HackerOne, so treat the Pinggy claim as unverified.
- Others:
  - Nextcloud's policy says reports made mostly of LLM output "won't be considered unless manually reviewed and validated", and Nextcloud later stopped its bounty. — [Privacy Guides forum](https://discuss.privacyguides.net/t/nextcloud-stops-bug-bounty-program-due-to-increase-of-low-effort-ai-generated-reports/37334) [S]
  - Google suspended product-vulnerability submissions to its OSS VRP, citing low-quality AI reports; the exact date is unclear across sources. — [BleepingComputer](https://bleepingcomputer.com/news/google/google-halts-open-source-bug-bounty-program-amid-ai-spam-surge) [S]
  - Node.js paused its bounty after losing IBB funding. — [CSO Online](https://www.csoonline.com/article/4154216/internet-bug-bounty-program-hits-pause-on-payouts-2.html) [S]
  - Django's policy closes apparent unverified AI output without a response. — [Stingrai census](https://www.stingrai.io/blog/ai-generated-vulnerability-report-policies-census) [S]
  - Reported but **unverified**: Apple paused researchers for 180 days over AI-generated reports ([aiweekly.co](https://aiweekly.co/alerts/apple-pauses-researchers-180-days-over-ai-generated-bug-reports) [S]), and GitHub tightened its bounty rules ([Analytics India Mag](https://analyticsindiamag.com/ai-news/github-tightens-bug-bounty-rules-as-ai-changes-security-research) [S]).
- CSA research note on AI agents flooding disclosure pipelines. — [CSA Labs](https://labs.cloudsecurityalliance.org/research/csa-research-note-ai-agent-vuln-disclosure-noise-signal-inte/) [S] (CSA notes say they are AI-assisted and not officially reviewed)

**Legal**
- US DOJ revised its CFAA charging policy on **19 May 2022**. Prosecutors "should decline prosecution" of good-faith security research, defined as access "solely for purposes of good-faith testing, investigation, and/or correction of a security flaw or vulnerability", done in a way designed to avoid harm. Extortion dressed up as "research" is excluded. Commentators flag that "solely" is ambiguous and that civil exposure is unaffected. — [Morrison Foerster](https://www.mofo.com/resources/insights/220525-cyber-crime-charging-policy) [S]; [Jones Day](https://www.jonesday.com/fr/insights/2022/06/department-of-justice-significantly-revises-policy-on-charging-cfaa-violations) [S]; [Jenner & Block](https://www.jenner.com/en/news-insights/publications/client-alert-doj-revises-cfaa-charging-policy-to-provide-clarity-for-cybersecurity-research-and-terms-of-use) [S]
- UK Computer Misuse Act 1990:
  - Dec 2025: Security Minister Dan Jarvis said the government was considering a statutory defence. — [CyberUp Campaign](https://www.cyberupcampaign.com/news/security-minister-announces-government-is-looking-at-introducing-a-statutory-defence-to-the-computer-misuse-act) [S]
  - May 2026: the proposal is reported as "much narrower", focused on scanning, and short of a full safe harbour. — [The Record](https://therecord.media/uk-moves-to-shield-security-researchers-cybercrime) [S]; [Security Boulevard](https://securityboulevard.com/2026/05/uk-moves-to-shield-security-researchers-in-cybercrime-law-overhaul/) [S]
  - Feb 2026: a proposed new clause in the Cyber Security and Resilience Bill would have required a review of a defence within 12 months; whether it was adopted is unknown. — [TheyWorkForYou](https://www.theyworkforyou.com/pbc/2024-26/Cyber_Security_and_Resilience_%28Network_and_Information_Systems%29_Bill/07-0_2026-02-24a.259.0) [S]
- Safe-harbor language: disclose.io's CC0 "dioterms" defines **Full Safe Harbor** as four tenets:
  1. authorization against anti-hacking laws (CFAA, CMA)
  2. exemption from anti-circumvention laws (DMCA)
  3. exemption from the organization's own TOS/AUP during testing
  4. acknowledgement of good-faith research

  A policy with only a good-faith non-pursuit commitment counts as "Partial Safe Harbor", and "bounties … are not a prerequisite for Safe Harbor". — [disclose/dioterms README](https://raw.githubusercontent.com/disclose/dioterms/master/README.md) [F]
- Tool vendors put responsibility on the operator. Strix's README: "only run it against systems you own or have **explicit, written permission** to test, and stay within the agreed scope… You alone are responsible for obtaining authorization." — [usestrix/strix README](https://raw.githubusercontent.com/usestrix/strix/main/README.md) [F]

### Inferences
- Platform rules converge on the same pattern: (a) the operator is accountable, (b) the program policy governs, including its rules on automation and rate limits, and (c) unvalidated output is sanctionable. A hackbot should therefore have three hard gates before submitting:
  1. a deterministic reproduction or oracle check (re-run the PoC from a clean session and check for a canary, OOB callback or diffed response)
  2. a de-duplication check against known and out-of-scope issues
  3. a human who reads and signs off on every report
- XBOW's statement suggests HackerOne expects human pre-submission review of automated findings. For a hackbot builder, "fully autonomous submission" is a policy risk, not just a quality risk.
- Intigriti's T&C bar on disclosing program data to third parties, together with Bugcrowd's "AI privacy & security" onboarding page, means sending target responses or report drafts to a third-party LLM API may need program permission or a zero-retention or self-hosted model. This is my reading of the rules; no platform text I saw addressed it directly.
- The slop crisis is shrinking open-source bounty money (curl, IBB, Node.js, Google OSS VRP). Targets that tolerate hackbots are increasingly paid commercial programs whose policies need careful reading.

### Gaps
- I could not read HackerOne's current Code of Conduct, Disclosure Guidelines or any "automated tools" policy text (hackerone.com blocked). The exact wording and date of any 2026 AI clause are unverified.
- The publication date of Bugcrowd's slop-policy post and its exact penalty wording are unconfirmed.
- I did not find Intigriti's actual AI-disclosure clause (only the third-party census reports it exists), YesWeHack's hunter-facing AI or automation rules, or Synack's researcher rules for AI tools.
- No platform-wide numeric rate limits were found; these are set per program.
- I did not verify DOJ Justice Manual §9-48.000 text or any post-2022 changes.

---

## 2. Safety and scope-control engineering for autonomous offensive agents

### Takeaway
The authoritative 2025–2026 guidance (OWASP LLM Top 10 **2026**, OWASP Agentic Top 10, CISA/Five Eyes agentic guidance, NIST CAISI, UK NCSC, Anthropic's containment write-ups) agrees on one point: **enforce boundaries deterministically in the environment, not in the prompt**. That means:
- egress allowlists or proxies, filesystem isolation, VMs and containers
- least-privilege, narrow tools, and policy checks in code ("complete mediation")
- hard budget, step and time caps ("circuit breakers")
- human approval for high-impact actions, with awareness that approvals degrade under fatigue
- full audit logging

Hostname allowlists have known holes (domain fronting, allowlisted domains doubling as exfil channels), and these matter directly for scope enforcement.

### Cited Findings

**OWASP**
- The OWASP Top 10 for LLM Applications **2026** was published **4 Aug 2026** and supersedes 2025:
  - LLM01 Prompt Injection
  - LLM02 Sensitive Information Disclosure
  - LLM03 Excessive Agency
  - LLM04 Supply Chain
  - LLM05 Data and Model Poisoning
  - LLM06 Unbounded Consumption
  - LLM07 Misinformation
  - LLM08 Hidden Context Exposure
  - LLM09 Vector and Embedding Weaknesses
  - LLM10 Improper Output Handling

  Sources: [GenAI-LLM-Top10 2026 README](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/README.md) [F]; the legacy repo says it is now "a historical archive" ([OWASP legacy index](https://raw.githubusercontent.com/OWASP/www-project-top-10-for-large-language-model-applications/main/index.md) [F]); official page [genai.owasp.org LLM Top 10 2026](https://genai.owasp.org/resource/owasp-genai-llm-top-10-2026/) (not fetched).
- LLM03:2026 Excessive Agency mitigations: minimize tools; minimize tool functionality; "avoid open-ended tools (e.g., run a shell command, fetch a URL)" in favour of granular tools with strict input schemas; minimize tool permissions; execute in the user's context; require user approval for high-impact actions. **Complete mediation** means "Implement authorization in logic rather than relying on an LLM to decide if an action is allowed", via an independent pre-execution policy decision point with graduated enforcement (audit, warn, block, escalate). Also: monitor tool use and rate-limit. It maps agentic excessive agency to ASI02 Tool Misuse & Exploitation, ASI03 Identity & Privilege Abuse and ASI08 Cascading Failures. — [LLM03:2026](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/final/LLM03_ExcessiveAgency.md) [F]
- LLM06:2026 Unbounded Consumption covers "Denial of Wallet" and "Reasoning-Loop and Thinking-Token Exhaustion". Mitigations:
  - "Hard Spending Caps": "non-overridable budget ceilings per API key, user, team… that halt inference when exceeded, rather than alerting thresholds"
  - "Agentic Circuit Breakers": "step limits, recursion depth limits, time limits, and per-run cost ceilings… Use state hashing to detect recursive loops"

  Source: [LLM06:2026](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/final/LLM06_UnboundedConsumption.md) [F]
- LLM01:2026 Prompt Injection mitigations relevant to hackbots:
  - Hold credentials and state-change capability "in application code, not the model", routed through "a deterministic policy engine that re-validates intent and arguments at execution time".
  - Require human confirmation for privileged, irreversible or externally visible actions, "surfacing the exact rendered action rather than a summary".
  - Apply the **Rule of Two** (Meta AI, 2025): an agent with untrusted input + sensitive data + state change/external comms needs per-action approval.
  - Strip Unicode tag-block, variation-selector and zero-width characters at every ingest boundary.
  - Test against adaptive attackers. Nasr et al. (2025) found static attack success near zero but adaptive success >90% for most of 12 defenses.

  Source: [LLM01:2026](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/final/LLM01_PromptInjection.md) [F]
- OWASP Top 10 for Agentic Applications 2026 was released **Dec 2025** (one source says 9 Dec 2025). Verified titles: ASI01 Agent Goal Hijack; ASI02 Tool Misuse & Exploitation; ASI03 Identity & Privilege Abuse; ASI04 Agentic Supply Chain Vulnerabilities; ASI07 Insecure Inter-Agent Communication; ASI08 Cascading Failures; ASI09 Human-Agent Trust Exploitation; ASI10 Rogue Agents. ASI05 and ASI06 cover code execution and memory poisoning; the exact titles were not seen in a fetched source. — [Giskard summary](https://www.giskard.ai/knowledge/owasp-top-10-for-agentic-application-2026) [S]; [Auth0](https://auth0.com/blog/owasp-top-10-agentic-applications-lessons/) [S]; ASI02/03/08 titles confirmed in [LLM03:2026](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/final/LLM03_ExcessiveAgency.md) [F]; ASI04 title confirmed in [LLM01:2026](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/final/LLM01_PromptInjection.md) [F]
- OWASP Securing Agentic Applications Guide v1.0 (reported dated **28 July 2025**, CC BY-SA 4.0) covers design, build, deployment and runtime controls, including sandboxing and HITL approval for high-impact actions. One source shows conflicting page metadata. — [Infosecurity Magazine](https://infosecurity-magazine.com/news/owasp-agentic-ai-security-guidance) [S]; [AIGL summary](https://www.aigl.blog/securing-agentic-applications-guide) [S]

**Government guidance**
- NIST CAISI published a Federal Register RFI on AI agent security on **8 Jan 2026** (comments to 9 Mar 2026, docket NIST-2025-0035). It asks how to "constrain and monitor agents in deployment environments" and defines agent hijacking as indirect prompt injection via ingested data. NIST announced an **AI Agent Standards Initiative on 17 Feb 2026**. — [Federal Register 2026-00206](https://www.govinfo.gov/content/pkg/FR-2026-01-08/html/2026-00206.htm) [S]; [ANSI news](https://www.ansi.org/standards-news/all-news/1-15-26-mitigating-risks-of-ai-agent-systems-nist-center-for-ai-standards-and-innovation-issues-rfi) [S]
- NIST/US AISI's agent-hijacking evaluation work (Jan 2025 technical blog) forked AgentDojo, adding mass-data-exfiltration injection tasks and a terminal RCE environment. The fork was archived after the UK AISI released an Inspect-compatible AgentDojo (2025-10-23). — [usnistgov/agentdojo-inspect README](https://raw.githubusercontent.com/usnistgov/agentdojo-inspect/main/README.md) [F]; [NIST blog](https://www.nist.gov/news-events/news/2025/01/technical-blog-strengthening-ai-agent-hijacking-evaluations) (link from README; not fetched)
- CISA, NSA, ASD's ACSC, the Canadian Centre for Cyber Security, NZ NCSC and UK NCSC issued **"Careful Adoption of Agentic AI Services"**. Sources give 30 Apr, 1 May or 4 May 2026. It sets out five risk categories, four lifecycle stages and >100 practices, and builds on zero-trust, defense-in-depth and least privilege. — [CISA bulletin](https://content.govdelivery.com/accounts/USDHSCISA/bulletins/41544ff) [S]; [Crowell & Moring](https://www.crowell.com/en/insights/client-alerts/american-and-allied-cyber-agencies-issue-first-joint-guidance-on-securing-agentic-ai) [S]; [Mayer Brown](https://www.mayerbrown.com/zh-hans/insights/publications/2026/06/multi-agency-guidance-on-securing-agentic-ai-systems) [S]
- UK NCSC, "Prompt injection is not SQL injection (it may be worse)", **8 Dec 2025**: "there's a good chance prompt injection will never be properly mitigated" the way parameterised queries fix SQLi. LLMs are an "inherently confusable deputy", so defenders should reduce likelihood and impact through secure design, data marking and monitoring. — [NCSC blog](https://www.ncsc.gov.uk/blog-post/prompt-injection-is-not-sql-injection) [S]

**Anthropic's containment practice (directly relevant to a hackbot runtime)**
- "How we contain Claude across products" (**25 May 2026**):
  - **Approval fatigue:** "users approved roughly 93% of permission prompts", so HITL alone is "fallible".
  - **Containment:** "supervise what it's able to do by enforcing access boundaries through… sandboxes, virtual machines, and egress controls". "If credentials never enter the sandbox, they can't be exfiltrated."
  - **Allowlists:** an egress allowlist is better treated "as a capability grant" than a destination filter. "Every function reachable through any domain on an allowlist is now an attack surface". An attacker-supplied API key let data leave through the allowlisted api.anthropic.com. The fix was an in-VM MITM proxy that only passes the VM's own session token.
  - **Custom components:** "the software you build yourself is often the weakest". gVisor, seccomp and hypervisors held; the custom allowlist proxy failed.
  - **Tool output:** route tool calls through proxies that "inspect return values before they enter the model's context", using "a small, fast model" as classifier.
  - **Model-layer numbers:** on Gray Swan's Agent Red Teaming benchmark, Claude Opus 4.7 holds attack success to ~0.1% single-shot and ~5–6% after 100 adaptive attempts. Auto mode catches ~83% of overeager actions; ~17% get through and ~0.4% of benign commands are blocked.
  - **Pre-consent attack surface:** project config executing before the trust dialog was a real bug class.

  Source: [Anthropic Engineering, How we contain Claude](https://www.anthropic.com/research/how-we-contain-claude) [F]
- "Beyond permission prompts" (**20 Oct 2025**): Claude Code sandboxing reduced permission prompts by **84%**. "effective sandboxing requires both filesystem and network isolation". Without network isolation an agent can exfiltrate SSH keys; without filesystem isolation it can escape to gain network access. It is built on bubblewrap (Linux) and seatbelt (macOS) and covers spawned subprocesses. — [Anthropic Engineering, Claude Code sandboxing](https://www.anthropic.com/engineering/claude-code-sandboxing) [F]
- Claude Code sandbox docs, security limitations:
  - The built-in proxy decides "from the client-supplied hostname without inspecting TLS, code running inside the sandbox can potentially use domain fronting… to reach hosts outside the allowlist". For stronger guarantees, use a TLS-terminating inspecting proxy.
  - Allowing `/var/run/docker.sock` "effectively grants access to the host".
  - `enableWeakerNestedSandbox` "considerably weakens security".

  Source: [code.claude.com/docs/en/sandboxing](https://code.claude.com/docs/en/sandboxing) [F]
- The open-source **Anthropic Sandbox Runtime (`srt`)** applies OS-level filesystem and proxy-based network restrictions to "arbitrary processes", including agents and local MCP servers, without a container (beta research preview). — [anthropic-experimental/sandbox-runtime README](https://raw.githubusercontent.com/anthropic-experimental/sandbox-runtime/main/README.md) [F]
- Claude Code cloud sessions: "GitHub credentials… never enter the session VM. The VM holds a short-lived credential scoped to that session, and GitHub traffic goes through an Anthropic proxy that attaches the GitHub credential on the server side." Network access is limited by default and can be restricted to specific domains. — [code.claude.com/docs/en/security](https://code.claude.com/docs/en/security) [F]
- Anthropic's Nov 2025 case study of a state-sponsored group using Claude Code agentically against ~30 targets:
  - "80-90% of the campaign" was done by AI, with "4-6 critical decision points".
  - The attackers "told Claude that it was an employee of a legitimate cybersecurity firm… being used in defensive testing" and split tasks into innocuous-looking pieces.
  - Claude "occasionally hallucinated credentials or claimed to have extracted secret information that was in fact publicly-available."

  Source: [Anthropic, Disrupting the first reported AI-orchestrated cyber espionage campaign, 13 Nov 2025](https://www.anthropic.com/news/disrupting-AI-espionage) [F]

**Tool designs**
- PentAGI (open-source pentest agent) allows Docker access only via a hardened dind daemon over TLS. It does not recommend bind-mounting the host Docker socket because "it grants an autonomous agent the host Docker API: it can start a privileged container, mount `/`, and compromise the entire node". `DOCKER_ALLOWED_IMAGES` restricts which images agents may pick. — [vxcontrol/pentagi README](https://raw.githubusercontent.com/vxcontrol/pentagi/master/README.md) [F]
- Synack's Sara datasheet describes 48-hour autonomous engagements with human expert review, and no MFA or CAPTCHA support. — [Synack datasheet](https://go.synack.com/hubfs/synack-sara-ai-pentesting-datasheet-continuous-security-validation.pdf?hsLang=en) [S]. XBOW markets "you define the scope, every action is logged and auditable". — [xbow.com](https://www.xbow.com/) [S]

### Inferences
- Scope must be enforced in code, outside the model, at three layers:
  1. **Program parser:** the policy becomes a machine-readable allowlist (hosts, CIDRs, wildcard rules, explicit exclusions, banned vuln classes, max request rate, required headers such as an X-Bug-Bounty username).
  2. **Egress proxy:** every tool's traffic, including nmap/ffuf/nuclei subprocesses, goes through it. It resolves and checks both hostname and IP against scope, drops anything else, and enforces token-bucket rate limits per host.
  3. **Tool-wrapper policy point:** "complete mediation" that refuses destructive verbs, DoS-class templates and data access beyond PoC (cap rows, fields and bytes returned).
- Because the hostname-only check is vulnerable to domain fronting and CDN-shared IPs (Claude Code docs), a hackbot proxy should terminate TLS and check SNI, Host and resolved IP for consistency.
- Treat any allowlisted third-party domain (paste sites, OOB-interaction servers, cloud APIs) as an exfil channel (Anthropic's capability-grant lesson). Pin the OOB server to one you own.
- Approval fatigue (93% approval rate) means HITL should be reserved for a few high-impact gates (first exploit attempt on a host, any authenticated state-changing request, report submission), not every command.
- Circuit breakers (step, time and cost caps, loop detection by state hashing) and a kill switch (revoke the proxy credential so all egress stops at once) are cheap and directly endorsed by OWASP LLM06:2026.
- Model-generated "findings" can be hallucinated (Anthropic espionage case), which reinforces deterministic validation.

### Gaps
- I could not fetch the OWASP Agentic Top 10 PDF, the OWASP "Agentic AI – Threats and Mitigations" (Feb 2025) document, or the Securing Agentic Applications Guide itself. Exact ASI05 and ASI06 titles are unverified.
- I did not read the CISA/Five Eyes PDF or NIST AI RMF / AI 600-1 text directly.
- No XBOW primary text on how its agents enforce scope (xbow.com blocked).
- OpenAI agent-safety documentation was not reviewed (openai.com blocked).

---

## 3. Securing the hackbot itself (prompt injection from targets, MCP supply chain, secrets, defensive patterns)

### Takeaway
A hackbot is a near-perfect "lethal trifecta" system: it reads hostile target content, holds secrets (API keys, test-account credentials, findings), and can talk to the network. Research shows offensive agents are readily hijacked by responses planted on the target; CAI's own team reported 100% success across 14 variants, and Mantis reported >95% against LLM attackers. MCP and tool ecosystems add poisoning and rug-pull risk. Defense means architectural separation (CaMeL / dual-LLM / plan-then-execute), stripping and inspecting tool output, and keeping credentials out of the agent's reach, backed by sandbox and egress controls. Model robustness has improved but is not zero-risk.

### Cited Findings

**Prompt injection against offensive agents**
- "Cybersecurity AI: Hacking the AI Hackers via Prompt Injection" (Mayoral-Vilches & Mannermaa Rynning, arXiv:2508.21669, 29 Aug 2025, v2 15 Nov 2025): a hostile server's poisoned response hijacks the CAI agent, going "from initial reconnaissance to full system compromise in under 20 seconds". The paper reports 100% exploitation across 14 variants, a 7-category taxonomy and a four-layer guardrail defense, and calls prompt injection "the XSS of the AI era". — [arXiv 2508.21669](https://arxiv.org/pdf/2508.21669) [S]; the four-layer guardrail is also referenced in the [CAI README](https://raw.githubusercontent.com/aliasrobotics/cai/master/README.md) [F]
- "Hacking Back the AI-Hacker" / **Mantis** (Pasquini & Kornaropoulos, arXiv:2410.20911): decoy services plant prompt injections in responses, hidden from human viewers via ANSI escape sequences and HTML comments. They either derail the attacking agent or lead it to compromise its own host, with >95% effectiveness against GPT-4/4o-driven agents. The code is open source. — [arXiv 2410.20911](https://arxiv.org/pdf/2410.20911) [S]; [pasquini-dario/project_mantis](https://raw.githubusercontent.com/pasquini-dario/project_mantis/main/README.md) [F]

**Lethal trifecta and design patterns**
- Simon Willison's "lethal trifecta" (**16 Jun 2025**): private data + exposure to untrusted content + external communication. "the only way to stay safe there is to avoid that lethal trifecta combination entirely". — [simonwillison.net/2025/Jun/16/the-lethal-trifecta](https://simonwillison.net/2025/jun/16/the-lethal-trifecta/) [S]
- The **Rule of Two** (Meta AI, 2025), as adopted by OWASP LLM01:2026: an agent with all three of untrusted input, sensitive data, and state change or external comms needs per-action approval. — [LLM01:2026](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/final/LLM01_PromptInjection.md) [F]
- "Design Patterns for Securing LLM Agents against Prompt Injections" (Beurer-Kellner et al., arXiv:2506.08837, 2025; authors from Google, Microsoft, IBM, ETH Zurich, EPFL) proposes six patterns:
  - action-selector: "acts like a switch statement"; tool outputs never feed back
  - plan-then-execute: "A prompt injection cannot force the LLM into executing a tool that is not part of the defined plan", although arguments can still be steered
  - LLM map-reduce
  - dual LLM
  - code-then-execute
  - context minimization

  Sources: [arXiv 2506.08837](https://arxiv.org/pdf/2506.08837) [S]; [Simon Willison, 13 Jun 2025](https://simonwillison.net/2025/Jun/13/prompt-injection-design-patterns/) [S]. The list of six names is from the paper as summarized; only action-selector and plan-then-execute were quoted in the result.
- **CaMeL** ("Defeating Prompt Injections by Design", Debenedetti, Shumailov, Fan, Hayes, Carlini, Fabian, Kern, Shi, Tramèr; Google, Google DeepMind, ETH Zurich; arXiv:2503.18813):
  - A privileged LLM writes code. A quarantined LLM parses untrusted data with no tool access.
  - A custom interpreter tracks capabilities (provenance tags) and enforces policies.
  - It solves AgentDojo "by design" with some utility loss. Section 8.3 says prompt injection is not fully solved.
  - The released code is a research artifact that "likely contains bugs… might not be fully secure" and is unsupported.

  Sources: [google-research/camel-prompt-injection README](https://raw.githubusercontent.com/google-research/camel-prompt-injection/main/README.md) [F]; [Simon Willison, 11 Apr 2025](https://simonwillison.net/2025/Apr/11/camel/) [S]

**Real-world agent exploits and MCP supply chain**
- Johann Rehberger's "Month of AI Bugs" (Aug 2025) disclosed more than two dozen vulnerabilities across agentic coding assistants (Claude Code, Copilot, Cursor, Windsurf, Gemini CLI, Amazon Q/Kiro). They included DNS-based data exfiltration from Claude Code (CVE-2025-55284), memory-persistent "SpAIware", and "ZombAIs" (agents joined to C2). — [Rehberger DEF CON SG slides](https://media.defcon.org/DEF%20CON%20SG%201/DEF%20CON%20SG%201%20main%20stage%20presentations/Johann%20Rehberger%20-%20The%20Month%20of%20AI%20Bugs%20Lessons%20Learned%20Exploiting%20AI%20Computer-Use%20and%20Coding%20Agents.pdf) [S]; [39C3 talk "Agentic ProbLLMs"](https://events.ccc.de/congress/2025/hub/de/event/detail/agentic-probllms-exploiting-ai-computer-use-and-coding-agents) [S]
- Invariant Labs (**1 Apr 2025**):
  - **Tool Poisoning Attacks**: hidden instructions in MCP tool descriptions that the model sees and the user does not.
  - **Rug pulls**: "a malicious server can change the tool description after the client has already approved it".
  - **Tool shadowing**: one server's description alters use of another server's tools.
  - Invariant said "code isolation or sandboxing of the MCP server is not a relevant mitigation" for this class.

  Invariant released MCP-Scan (11 Apr 2025) with tool pinning by hash. — [Invariant Labs, MCP security notification](https://invariantlabs.ai/blog/mcp-security-notification) [S]
- MCP-Scan is now **"Snyk Agent Scan"**, which scans agents, MCP servers and skills for prompt injections. Its CLI output is "experimental". — [invariantlabs-ai/mcp-scan README](https://raw.githubusercontent.com/invariantlabs-ai/mcp-scan/main/README.md) [F]
- Anthropic on external content:
  - "Any external resource provided to an agent represents two risks at once: a code execution risk… and a prompt injection vector."
  - Remote MCP servers "can change behavior at any point after you've approved it". Run unknown tools "against fake data first".
  - "Tool output is an attack surface even when the tool is trusted". A GitHub connector can load a poisoned README.
  - Emerging risks: persistent memory poisoning (CLAUDE.md, memory, state directories reloaded each session) and multi-agent trust escalation.

  Source: [How we contain Claude](https://www.anthropic.com/research/how-we-contain-claude) [F]
- In a Feb 2026 internal red-team exercise, a phished prompt asked Claude Code to read `~/.aws/credentials` and POST it externally. It succeeded **24/25 times**, because the instruction arrived from the user. "The only defense that holds… is the environment": egress controls plus filesystem boundaries. — [How we contain Claude](https://www.anthropic.com/research/how-we-contain-claude) [F]

**Model robustness, benchmarks and defensive tools**
- Model robustness is improving but bounded: Claude Opus 4.5 browser-use prompt-injection ASR is around 1% against an adaptive Best-of-N attacker, and "A 1% attack success rate… still represents meaningful risk". — [Anthropic, Mitigating the risk of prompt injections in browser use, 24 Nov 2025](https://www.anthropic.com/research/prompt-injection-defenses) [F]
- Benchmarks for testing a hackbot's own robustness:
  - **AgentDojo** (ETH): [ethz-spylab/agentdojo](https://raw.githubusercontent.com/ethz-spylab/agentdojo/main/README.md) [F]
  - **Agent Security Bench** (ICLR 2025), covering direct and observation prompt injection, plan-of-thought backdoor and memory poisoning: [agiresearch/ASB README](https://raw.githubusercontent.com/agiresearch/ASB/main/README.md) [F]
- Meta's Purple Llama provides **Prompt Guard** (malicious-prompt classifier) and **Code Shield** (inference-time insecure-code filtering). — [meta-llama/PurpleLlama README](https://raw.githubusercontent.com/meta-llama/PurpleLlama/main/README.md) [F]
- Secrets handling patterns:
  - Credentials stay in the host keychain; the VM gets a per-session scoped, revocable token (Cowork). — [How we contain Claude](https://www.anthropic.com/research/how-we-contain-claude) [F]
  - A server-side proxy attaches GitHub credentials (Claude Code cloud). — [Claude Code security docs](https://code.claude.com/docs/en/security) [F]

### Inferences
- Concrete hackbot rules that follow from the above:
  1. Treat every target-derived byte (HTTP bodies, headers, banners, JS, error messages, DNS TXT, file contents) as untrusted data. Never let it select tools directly; plan-then-execute or action-selector for the exploit phase.
  2. Run a quarantined parser model (cheap) that extracts typed fields (status, reflected-param yes/no, version strings) for the privileged planner (CaMeL / dual-LLM).
  3. Strip ANSI escapes, HTML comments, and Unicode tag, variation-selector and zero-width characters from tool output before it enters context. Mantis hides payloads in exactly these.
  4. Never execute commands, download binaries or visit URLs because target content suggested them, unless the URL is in scope and passes the proxy.
  5. Keep platform API tokens, LLM API keys and test-account passwords out of the agent sandbox. Inject them at a proxy.
  6. Pin and hash all MCP servers and tools, prefer local audited tools over remote MCP, and scan them (Snyk Agent Scan).
  7. Treat persistent memory and notes as privileged writes (OWASP LLM01:2026 #9).
- Expect targets that deploy Mantis-style counter-AI defenses. A hijacked hackbot that attacks out-of-scope hosts creates legal exposure for the operator, which is another reason for proxy-enforced scope.

### Gaps
- I could not read embracethered.com posts or Willison's full articles; quotes come from search summaries.
- No published measurement of how often real bug-bounty targets contain injection payloads aimed at agents.

---

## 4. Model choice and model-provider usage policies for offensive security work

### Takeaway
Anthropic's current Usage Policy (page marked "Effective Nov 12, 2026") explicitly permits security testing "with the authorization of the system owner, or within the scope of an authorized bug bounty program or vulnerability disclosure policy". Real-time cyber classifiers may still interrupt work such as "exploit validation". The legitimate route is Anthropic's **Cyber Verification Program (CVP)**: a three-tier program relaunched 6 Oct 2026. Individuals and bug-bounty hunters can get "Defense Access" only, and building customer-facing hackbot products on CVP models requires separate approval. Anthropic states that its frontier "Mythos" cyber capability is gated through Project Glasswing and CVP. Open-weight options used by open-source pentest agents in 2026 include GLM-5.3, Qwen3.5 and DeepSeek, and security-specialized model families (Cisco Foundation-sec, Alias Robotics' alias models) exist. I found no independent late-2026 head-to-head ranking.

### Cited Findings

**Anthropic Usage Policy**
- The "Do Not Compromise Computer or Network Systems Without Authorization" section prohibits, among other things:
  - discovering or exploiting vulnerabilities "without authorization of the system owner"
  - creating "automated tools designed to compromise multiple systems at scale without authorization"
  - malware creation
  - DoS tools

  Carve-out: "This section does not prohibit security research, testing, or tool development conducted on systems you own or operate, with the authorization of the system owner, or within the scope of an authorized bug bounty program or vulnerability disclosure policy, provided the activity complies with applicable law. For work covered by our real-time cyber safeguards, users may apply for adjusted access through our Cyber Verification Program."

  The page displays "Effective Nov 12, 2026" (a date after today, 2026-10-27, so the displayed text may be an announced upcoming version; see version history on the page). — [Anthropic Usage Policy](https://www.anthropic.com/legal/aup) [F]
- The AUP also requires that "Agentic use cases must comply with the Usage Policy. Users are responsible for ensuring that agents they build or deploy, including actions those agents take through tools, browsers, or connected systems, comply". It prohibits bypassing guardrails ("jailbreaking or prompt injection") "without prior authorization from Anthropic". "encountering a block does not by itself indicate a violation". — [Anthropic Usage Policy](https://www.anthropic.com/legal/aup) [F]

**Anthropic Cyber Verification Program (support article "Updated October 2026")**
- **Scope:**
  - Generally available models can be used for "secure code review, threat modeling, patching known issues, finding vulnerabilities in your own source code, and triaging security alerts", but "other cyber security work like malware analysis or exploit validation may be interrupted by our safety classifiers".
  - CVP "makes advanced cyber capabilities and reduced blocking classifiers available to qualifying security professionals".
  - It has three tiers (Defense Access, Red Team Access, Specialized Access) covering Claude Opus 5.5 ("our most capable cyber model yet"), Claude Sonnet 5.5 and Claude Mythos 5.1.
- **Who can apply:**
  - "Independent researchers, maintainers, and bug bounty hunters apply as individuals… only Defense access is available for individual applicants". Individuals need a paid plan. Red Team and Specialized tiers are organization-only.
  - Specialized Access is "reviewed in depth in collaboration with the US government". Glasswing members move to that tier.
  - Eligibility factors include "The program is built for defense", verifiability, legal environment and diversion risk, and end customers (more caution where an organization primarily serves military, intelligence or law-enforcement customers).
- **Conditions once approved:**
  - Data retention is required for misuse monitoring. Zero data retention is only possible for organizations already holding a ZDR exemption for Fable/Mythos.
  - Defense Access must adopt phishing-resistant MFA and stop using API keys by **15 Dec 2026**; until then API keys expire every 7 days.
  - "The Usage Policy still applies in full"; "We may review, narrow, or withdraw a grant".
  - Using CVP "to deliver work to your customers, for example by running the model against a customer's code or systems or by letting customers use something you've built on the model, requires approval". Managed services: applications open. Products (constrained or unconstrained): "not open yet", expression of interest only.
  - On third-party platforms only Defense and Red Team access are available.

  Source: [Claude Help Center, Cyber Verification Program](https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude-opus-and-sonnet) [F]

**Project Glasswing and frontier cyber capability**
- Project Glasswing was announced **7 Apr 2026** with AWS, Apple, Broadcom, Cisco, CrowdStrike, Google, JPMorganChase, Linux Foundation, Microsoft, NVIDIA and Palo Alto Networks, plus more than 40 more organizations.
  - It gives access to the unreleased **Claude Mythos Preview**, which "can surpass all but the most skilled humans at finding and exploiting software vulnerabilities".
  - On CyberGym, Mythos Preview scored 83.1% vs Opus 4.6 at 66.6%.
  - Anthropic committed up to $100M in usage credits and $4M in donations to open-source security organizations.
  - Update of 6 Oct 2026: CVP expanded to three tiers.

  Source: [Anthropic, Project Glasswing](https://www.anthropic.com/glasswing) [F]
- "Claude Mythos Preview is an example of a model whose blast radius was deemed too high to ship in April 2026". — [How we contain Claude](https://www.anthropic.com/research/how-we-contain-claude) [F]
- Why "just state authorization in the system prompt" is not enough on its own: the Nov 2025 attackers "told Claude that it was an employee of a legitimate cybersecurity firm… being used in defensive testing", and Anthropic responded with better classifiers and account bans. — [Anthropic espionage report](https://www.anthropic.com/news/disrupting-AI-espionage) [F]

**Open-weight and security-specialized models**
- Strix lists "Recommended models for best results": **Z.ai GLM-5.3** via Vercel AI Gateway or OpenRouter. It supports any provider (OpenAI, Anthropic, Google, etc.). — [usestrix/strix README](https://raw.githubusercontent.com/usestrix/strix/main/README.md) [F]
- PentAGI supports 10+ cloud providers plus local models via Ollama or vLLM, with provider configs for DeepSeek and Qwen and a "vLLM + Qwen3.5-27B-FP8" guide. — [vxcontrol/pentagi README](https://raw.githubusercontent.com/vxcontrol/pentagi/master/README.md) [F]
- Alias Robotics' CAI framework is now **archived** ("No further releases, bug fixes, security patches or support"). Its successor, "Cybersecurity Superintelligence (CSI)", ships the `alias` model family (`alias3`, `alias2`, `alias2-mini`, `alias1`, `alias0`), described as "cybersecurity-specialised, on-premise deployable". — [aliasrobotics/cai README](https://raw.githubusercontent.com/aliasrobotics/cai/master/README.md) [F]
- Cisco Foundation AI (formerly Robust Intelligence) publishes open "Foundation-sec" security models on Hugging Face (`fdtn-ai`), with fine-tuning, quantization and deployment recipes, and offers early access to "preview-mode models". — [cisco-foundation-ai/cookbook README](https://raw.githubusercontent.com/cisco-foundation-ai/cookbook/main/README.md) [F]
- Benchmark context:
  - XBOW's 104-challenge validation benchmark is "saturated" as of mid-2026 ("Industry-standard performance… approximately 100%"). — [xbow-engineering/validation-benchmarks README](https://raw.githubusercontent.com/xbow-engineering/validation-benchmarks/main/README.md) [F]
  - PentestGPT reports 86.5% (90/104) on it. — [GreyDGL/PentestGPT README](https://raw.githubusercontent.com/GreyDGL/PentestGPT/main/README.md) [F]
  - Harder current evals: CVE-Bench v2.1.0 (2026-01-12; 40 critical CVEs; ICML 2025) — [uiuc-kang-lab/cve-bench](https://raw.githubusercontent.com/uiuc-kang-lab/cve-bench/main/README.md) [F]; Cybench (40 CTF tasks) plus the newer BountyBench — [andyzorigin/cybench](https://raw.githubusercontent.com/andyzorigin/cybench/main/README.md) [F]; CyberGym (cited by Anthropic above).
  - CAI-team research: "Evaluating Agentic Cybersecurity in Attack/Defense CTFs" (arXiv:2510.17521) reports 54.3% defensive patching vs 28.3% offensive initial access. "Towards CSI" (arXiv:2605.28334) finds "No single scaffold dominates"; a blackboard over five scaffolds solved 19/33 Cybench vs 15/33 for the best single scaffold. — [CAI README research index](https://raw.githubusercontent.com/aliasrobotics/cai/master/README.md) [F] (author-reported)

### Inferences
- For a bug-bounty hackbot on Anthropic models: the AUP authorizes the activity in scope, but exploit-validation steps may hit classifiers, so an individual hunter's legitimate path is CVP Defense Access. A team selling hackbot-as-a-service would need CVP organization approval plus a "managed service" application; customer-facing products are not yet open.
- Stating authorization in the system prompt (program name, scope, the operator's platform handle) is reasonable context, but it is not a credential. Providers explicitly treat such claims as spoofable. Verification programs are the legitimate route; jailbreaking violates the AUP.
- Open-weight models (GLM, Qwen, DeepSeek via vLLM/Ollama) avoid provider classifiers and data-retention issues (relevant to program-confidentiality terms). The trade-off is that the operator carries full responsibility and likely gets weaker capability; the tool READMEs still default to frontier or large models.
- XBOW's benchmark saturation means a builder should evaluate on CVE-Bench, Cybench/BountyBench, CyberGym, or their own held-out targets, not on XBOW's set.

### Gaps
- **OpenAI** (usage policies, "Trusted Access for Cyber", Aardvark / Codex Security private beta) and **Google** (Generative AI Prohibited Use Policy, Gemini API terms): not verified (sites blocked, search budget exhausted). Treat the names "Aardvark" and "Trusted Access for Cyber" as leads to verify, not facts.
- I found no independent late-2026 comparison of frontier closed vs open-weight models (Qwen, DeepSeek, Kimi K2, GLM, gpt-oss, Llama) on offensive agent tasks, and no verified details on Llama Primus (Trend Micro), WhiteRabbitNeo or Foundation-sec-8b parameters and release dates.
- The CVP "Access tiers" comparison table is an image on the help page; per-tier permitted activities were not captured beyond the text quoted above.

---

## 5. Cost and infrastructure practicalities

### Takeaway
Verified hard numbers come mainly from provider price sheets and vendor or academic claims. Current Anthropic list prices (Oct 2026):

| Model | Input $/MTok | Output $/MTok |
|---|---|---|
| Haiku 5.5 (≤100k-token prompts) | 0.10 | 0.50 |
| Sonnet 5.5 | 2 | 10 |
| Opus 5.5 | 4 | 20 |
| Fable 5.1 / Mythos 5.1 | 10 | 50 |

Cache reads cost 0.025–0.1x input and Batch is 50% off. Both suit agentic loops that re-send large tool transcripts, and the price spread makes routing (a cheap parser or classifier plus a strong planner) the default architecture. The infrastructure pattern in open-source pentest agents is Docker Compose with a Kali tool container, isolated Docker-in-Docker, a Postgres/pgvector memory store, and Langfuse plus Grafana/Prometheus observability. Cost claims like CAI's "156× cheaper" are vendor-reported.

### Cited Findings

**Pricing and caching**
- Claude pricing, prompt-caching rows:
  - 5-minute cache write = 1.25x input; 1-hour write = 2x.
  - Cache hit = 0.1x for most models, 0.05x on Opus 5.5 / Sonnet 5.5 ($0.20 / $0.10 per MTok), 0.025x on Fable/Mythos 5.1 ($0.25 per MTok).
  - "caching pays off after one cache read for the 5-minute duration… or after two cache reads for the 1-hour duration".
- Batch API: "50 percent discount on both input and output tokens"; discounts stack.
- Claude 4.6+ (except Haiku 5.5) include a 1M-token context "at standard pricing". Haiku 5.5 is positioned "for high-volume, latency-sensitive tasks such as classification, extraction, and routing".

  Source: [Claude API pricing](https://platform.claude.com/docs/en/about-claude/pricing) [F]

**Routing and guardrail-model patterns**
- Anthropic: a guardrail pattern "where one model instance processes user queries while another screens them… tends to perform better than having the same LLM call handle both". It recommends "finding the simplest solution possible… This might mean not building agentic systems at all", and warns that agents bring "higher costs, and the potential for compounding errors. We recommend extensive testing in sandboxed environments". — [Anthropic, Building effective agents, 19 Dec 2024](https://www.anthropic.com/engineering/building-effective-agents) [F]
- Anthropic: the tool-output inspection classifier "can be a small, fast model; it doesn't need to be the one doing the reasoning". — [How we contain Claude](https://www.anthropic.com/research/how-we-contain-claude) [F]

**Cost claims and budget controls**
- CAI's framework paper (arXiv:2504.06017, Apr 2025) claims "**3,600× faster** than human pentesters at **156× lower cost**", with CVSS 4.3–7.5 findings in production systems. "Generative Cut-the-Rope" (arXiv:2601.05887) reports success rising from 20% to 43% and "cost per success ÷2.7". — [aliasrobotics/cai README](https://raw.githubusercontent.com/aliasrobotics/cai/master/README.md) [F] (vendor/author claims)
- OWASP LLM06:2026 recommends **hard, non-overridable spending caps** per key, user and team, plus per-run cost ceilings and step limits ("Denial of Wallet" is a named risk). — [LLM06:2026](https://raw.githubusercontent.com/GenAI-Security-Project/GenAI-LLM-Top10/main/2026/final/LLM06_UnboundedConsumption.md) [F]

**Infrastructure reference designs**
- PentAGI:
  - Docker Compose deployment; "All operations are performed in a sandboxed Docker environment".
  - Default pentest image `vxcontrol/kali-linux`; `DOCKER_ALLOWED_IMAGES` allowlist.
  - Hardened dind over TLS recommended over socket mounting.
  - Postgres + pgvector store "All commands and outputs".
  - Langfuse LLM-observability dashboard plus Grafana/Prometheus; REST/GraphQL APIs with bearer tokens.

  Source: [vxcontrol/pentagi README](https://raw.githubusercontent.com/vxcontrol/pentagi/master/README.md) [F]
- Strix needs Docker running; the "First run automatically pulls the sandbox Docker image". Results go to `strix_runs/<run-name>`. — [usestrix/strix README](https://raw.githubusercontent.com/usestrix/strix/main/README.md) [F]
- PentestGPT ships a Docker flow that runs Claude or Codex CLIs as backends (e.g., `make docker-run TARGET=… BACKEND=claude MODEL=opus MODE=pentest`). — [GreyDGL/PentestGPT README](https://raw.githubusercontent.com/GreyDGL/PentestGPT/main/README.md) [F]
- Lightweight alternative to containers: Anthropic `srt` (bubblewrap or sandbox-exec plus a proxy) can sandbox the agent, local MCP servers or individual tool processes. — [sandbox-runtime README](https://raw.githubusercontent.com/anthropic-experimental/sandbox-runtime/main/README.md) [F]
- Observability inside isolation is a known tension: VM isolation "kept the endpoint detection software out too". Anthropic's mitigation is pull-based OTLP log export. — [How we contain Claude](https://www.anthropic.com/research/how-we-contain-claude) [F]

### Inferences
- Budget model for a run: (planner tokens × strong-model price) + (parsing/classification tokens × cheap-model price) + cache-write premium, minus cache-hit savings. Long recon transcripts re-sent each turn are where caching (0.05–0.1x) and Haiku-class routing pay off. Summarizing or truncating raw tool output before it reaches the planner also helps with injection risk.
- A practical VPS layout:
  - the orchestrator on the host
  - tools in a Kali-based container with no host socket
  - all container egress through a scope-enforcing proxy (allowlist + rate limit + logging)
  - secrets injected at the proxy
  - traces to Langfuse or OpenTelemetry
  - hard spend caps at the provider plus per-run ceilings in the orchestrator

### Gaps
- I could not verify Fang et al.'s per-exploit cost figures (arXiv blocked; search budget exhausted), any XBOW per-finding cost, the Stanford "ARTEMIS" agent-vs-human pentester cost study, or other independent $/run measurements. The report should not quote specific numbers for these without checking.
- No verified VPS sizing (CPU/RAM) figures; PentAGI's hardware requirements section was not found in the fetched README.

---

## 6. Learning roadmaps and skill prerequisites; what experienced builders recommend

### Takeaway
I could verify little primary "roadmap" material. roadmap.sh and the builders' personal blogs (Joseph Thacker, Jason Haddix, Daniel Miessler, XBOW) were blocked, and the search budget ran out before I reached them. What I could verify points to a consistent build path:
- start simple and narrow (Anthropic: maybe "not building agentic systems at all")
- automate a methodology you already have (Haddix's TBHM is the canonical web bug-hunting methodology repo)
- make validation deterministic and PoC-backed (XBOW validators, Strix "working PoCs, not false positives", platform rules)
- learn the containment stack (Docker, Linux sandboxing, proxies/TLS) alongside LLM API mechanics (tool use, caching, routing)

### Cited Findings

**Builder guidance and methodology**
- Anthropic's agent-building guidance: "we recommend finding the simplest solution possible, and only increasing complexity when needed… This might mean not building agentic systems at all", plus "extensive testing in sandboxed environments, along with the appropriate guardrails". — [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) [F]
- Anthropic's agent model has four components (model, harness, tools, environment), "each one is both a source of capability and a potential point of oversight". Its framework principles: keep humans in control, align with human values, secure agents' interactions, maintain transparency, protect privacy. — [Anthropic Frontier Red Team, Trustworthy agents in practice, 9 Apr 2026](https://www.anthropic.com/research/trustworthy-agents) [F]
- Jason Haddix's **The Bug Hunter's Methodology (TBHM)** repo collects "tips, tricks, tools, data analysis and notes related to web application security assessments and more specifically towards bug hunting in bug bounties". It includes a "Before You Get Hacking: Learning Resources" section. — [jhaddix/tbhm README](https://raw.githubusercontent.com/jhaddix/tbhm/master/README.md) [F]
- Validation-first design in practice:
  - XBOW reviewed findings pre-submission and uses automated "validators". — [XBOW blog](https://xbow.com/blog/top-1-how-xbow-did-it/) [S]
  - Strix advertises "Real exploit validation - working PoCs, not false positives like legacy vulnerability scanners" and "every vulnerability includes a working proof-of-concept exploit and reproduction steps". — [Strix README](https://raw.githubusercontent.com/usestrix/strix/main/README.md) [F]
  - Daniel Miessler's Personal AI Infrastructure describes "Bitter Pill Engineering": remove scaffolding a smarter model would make unnecessary, but "the verification kernel stays". — [danielmiessler/Personal_AI_Infrastructure README](https://raw.githubusercontent.com/danielmiessler/Personal_AI_Infrastructure/main/README.md) [F]
- HackerOne engineers used CAI to explore agentic architectures, and "CAI's Retester agent directly inspired their production deduplication agent". Alias Robotics also lists a PortSwigger Web Security Academy race-condition case study. — [CAI README case studies](https://raw.githubusercontent.com/aliasrobotics/cai/master/README.md) [F] (vendor claim)

**Prerequisites evidenced by the tools themselves**
- **Python** tooling with `uv`: CaMeL, AgentDojo-inspect — [CaMeL README](https://raw.githubusercontent.com/google-research/camel-prompt-injection/main/README.md) [F]; [agentdojo-inspect README](https://raw.githubusercontent.com/usnistgov/agentdojo-inspect/main/README.md) [F]
- **Docker / Docker Compose**: Strix, PentAGI, PentestGPT — READMEs above [F]
- **Linux sandboxing** (bubblewrap, socat, seccomp): [Claude Code sandboxing docs](https://code.claude.com/docs/en/sandboxing) [F]
- **HTTP/TLS proxy internals**, needed to understand domain-fronting limits on allowlists: [Claude Code sandboxing docs](https://code.claude.com/docs/en/sandboxing) [F]
- **Standard offensive tools** (nmap, Metasploit, etc., "20+ professional tools"): [PentAGI README](https://raw.githubusercontent.com/vxcontrol/pentagi/master/README.md) [F]; ProjectDiscovery's nuclei is a common scanner dependency — [projectdiscovery/nuclei README](https://raw.githubusercontent.com/projectdiscovery/nuclei/dev/README.md) [F] (existence only)
- **OWASP LLM / Agentic Top 10** for agent-security fundamentals: section 2 sources [F]
- **Evaluation literacy** (CTF-style benchmarks and their saturation): XBOW benchmark, CVE-Bench, Cybench READMEs [F]

### Inferences
- A defensible learning sequence, synthesized from the sources rather than taken from any one roadmap:
  1. **Web and pentest fundamentals:** HTTP, auth/session, OWASP Web Top 10, Burp, recon methodology (TBHM). Manual practice on legal labs such as PortSwigger Academy and HTB.
  2. **Linux, Docker and networking:** containers, egress proxies, TLS, DNS.
  3. **Python** with async I/O and subprocess control of CLI tools.
  4. **LLM API mechanics:** tool use and JSON schemas, context management, caching, batch, routing, cost caps.
  5. **One narrow agent for one vuln class** on your own lab targets, with a deterministic oracle (canary or OOB callback).
  6. **Hardening the agent:** scope proxy, sandbox, secrets-out-of-sandbox, injection test suite (AgentDojo/ASB, plus Mantis-style hostile responses).
  7. **Only then, real programs:** read each policy, human review before submission, platform disclosure rules, provider verification (e.g., CVP) where classifiers block legitimate validation.

### Gaps
- **roadmap.sh** AI Engineer / AI Agents / AI Red Teaming / Cyber Security roadmaps: not verified (roadmap.sh blocked; GitHub paths in kamranahmedse/developer-roadmap returned 404, so the repo structure may have changed). Do not cite their contents without checking.
- Specific posts by **Joseph Thacker** (josephthacker.com, e.g., on hackbots), **Jason Haddix** (beyond the TBHM repo), **Daniel Miessler** (danielmiessler.com) and **XBOW engineers** recommending "automate your own methodology" or "build narrow agents first": not retrieved. These recommendations are plausible community advice but are **unverified** here.
- No dedicated "AI red teaming roadmap" or bug-bounty roadmap document was verified.
