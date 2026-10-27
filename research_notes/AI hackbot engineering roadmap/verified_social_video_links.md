# Verified social and video links: AI agent engineering and AI hackbots (YouTube, Reddit, HN, X)

**How items were verified.** Direct fetch paths were blocked in this session. `curl` to youtube.com/oembed, hn.algolia.com, reddit.com/*.json and publish.twitter.com returned proxy 403s. WebFetch got DNS ENOTFOUND for youtube.com and hn.algolia.com. WebSearch rejected `allowed_domains: reddit.com` ("not accessible to our user agent"). All verification therefore comes from WebSearch result blocks on the canonical domain (youtube.com, news.ycombinator.com, x.com).
- **[V]** means the exact URL and exact title came back as a search result on that domain.
- **[V2]** means the item is known only from a secondary mention, such as a handle quoted in another video's description.
- Content summaries ("why it matters") come from search snippets. Nothing was watched or read in full.
- **X dates are exact.** They were decoded from the status-ID snowflake: `(id >> 22) + 1288834974657` ms gives the UTC time.
- **HN dates are approximate** unless the underlying event date was confirmed. HN item IDs are monotonic but carry no timestamp.
- About 66 web searches were used.

## Q1. YouTube: exact video and channel URLs

### Takeaway
All four Karpathy videos and the Barry Zhang, Dex Horthy, Anthropic "Prompting for Agents" and "Claude Code best practices" talks are confirmed with exact IDs. So are XBOW's DEF CON 33 talk, several AIxCC finals videos, Jason Haddix's keynotes and about 9 Critical Thinking hackbot episodes. Not found: an XBOW Black Hat USA 2025 talk recording, any John Hammond video reviewing AI pentest tools, and an official LangChain-channel multi-agent tutorial (uploader not confirmed).

### Cited Findings

**Karpathy (general-audience LLM series)**
- [V] "[1hr Talk] Intro to Large Language Models", Andrej Karpathy. The canonical one-hour general-audience LLM intro. Date not shown in results. — [YouTube](https://www.youtube.com/watch?v=zjkBMFhNj_g)
- [V] "Deep Dive into LLMs like ChatGPT", Andrej Karpathy. About 3h31m (per a commenter); covers the full pretraining-to-RL stack. The results did not confirm the release date. — [YouTube](https://www.youtube.com/watch?v=7xTGNNLPyMI)
- [V] "How I use LLMs", Andrej Karpathy. 2025 (per a summary); a practical walkthrough of tools and features in the same series. — [YouTube](https://www.youtube.com/watch?v=EWvNQjAaOHw)
- [V] "Andrej Karpathy: Software Is Changing (Again)". Keynote at YC AI Startup School, San Francisco, June 17 2025; the "Software 3.0" framing. — [YouTube](https://www.youtube.com/watch?v=LCEmiRjPEtQ)

**Anthropic / agent-design talks**
- [V] "How We Build Effective Agents: Barry Zhang, Anthropic". Agent Engineering session day, AI Engineer Summit 2025, NYC; the talk version of the "Building effective agents" post. — [YouTube](https://www.youtube.com/watch?v=D7_ipDqhtwk)
- [V] "Prompting for Agents". Hannah Moran and Jeremy Hadfield (Anthropic Applied AI), Code w/ Claude, San Francisco, May 22 2025. — [YouTube](https://www.youtube.com/watch?v=XSZP9GhhuAc)
- [V] "Claude Code best practices". Cal Rueb (Anthropic MTS), Code w/ Claude, May 22 2025; the listing credits "@anthropic-ai". — [YouTube](https://www.youtube.com/watch?v=gv0WHhKelSE)
- [V] "Mastering Claude Code in 30 Minutes". Boris Cherny workshop. Several uploads share this title, and the results did not confirm which one is Anthropic's. — [YouTube](https://www.youtube.com/watch?v=B_KAEqiC-0Q)
- [V] "Inside Claude Code With Its Creator Boris Cherny". YC Lightcone episode; chapters cover subagents and plan mode. — [YouTube](https://www.youtube.com/watch?v=PQU9o_5rHC4)
- [V] "Boris Cherny: We Cut 80% of Claude Code's Prompt". Startup School 2026 with Diana Hu, after the Opus 5 launch; shows prompts getting smaller as models improve. — [YouTube](https://www.youtube.com/watch?v=qyPCVqFUyDo)
- [V] "Anthropic's Boris Cherny: Why Coding Is Solved, and What Comes Next". Sequoia AI Ascent 2026. — [YouTube](https://www.youtube.com/watch?v=SlGRN8jh2RI)
- [V] "A conversation on Claude Code". Boris Cherny with Alex Albert on Claude Code's origins as an internal tool. — [YouTube](https://www.youtube.com/watch?v=Yf_1w00qIKc)
- [V] "Claude for Long-Horizon Tasks — Lance Martin, Anthropic". Lessons on building agent harnesses for reliable, secure long-horizon work. — [YouTube](https://www.youtube.com/watch?v=9QebvrrY3KY)

**12-factor agents / context engineering**
- [V] "12-Factor Agents: Patterns of reliable LLM applications — Dex Horthy, HumanLayer". The "— Name, Company" title format matches AI Engineer uploads, but the uploader was not confirmed. — [YouTube](https://www.youtube.com/watch?v=8kMaTybvDUw)
- [V] "12-factor Agents - Patterns of reliable LLM applications // Dexter Horthy". An alternate recording; the description credits the Agents in Production 2025 conference (Databricks-sponsored). — [YouTube](https://www.youtube.com/watch?v=2yi4mAN3CtE)
- [V] "Context Engineering SF: Advanced Context Engineering for Agents - Dex Horthy". Scaling coding agents on real codebases (the research-plan-implement workflow). — [YouTube](https://www.youtube.com/watch?v=VvkhYWFWaKI)
- [V] "Everything We Got Wrong About Research-Plan-Implement - Dexter Horthy". Coding Agents Conference keynote, Computer History Museum, March 3 2026; he revisits his own workflow. — [YouTube](https://www.youtube.com/watch?v=YwZR6tc7qYg)
- [V] "Context Engineering for Agents - Lance Martin, LangChain". The venue was not confirmed; a "Deep Dive" version (rrzH0HQJH14) is associated with Latent Space. — [YouTube](https://www.youtube.com/watch?v=_IlTcWciEC4)
- [V] "Context Engineering for AI Agents with LangChain and Manus". A joint session with Lance Martin listed as a speaker. — [YouTube](https://www.youtube.com/watch?v=6_BcCthVvb8)
- [V] "Context Engineering SF: Context Engineering for Engineers - Jeff Huber" (Chroma). — [YouTube](https://www.youtube.com/watch?v=L8ZM78APDPk)
- [V] "RAG is Dead, Context Engineering is King: Chroma CEO Jeff Huber on Modern AI Retrieval & Context Rot". Context-rot framing. — [YouTube](https://www.youtube.com/watch?v=nLNuPsQ-Jlo)

**AI Engineer World's Fair streams (2025 / 2026)**
- [V] "AI Engineer World's Fair 2025 - Day 2 Keynotes & SWE Agents track". The full-day stream that includes the coding-agent talks. — [YouTube](https://www.youtube.com/watch?v=U-fMsbY-kHY)
- [V] "Sean Grove (OpenAI): The New Code @ AI Engineer World's Fair 2025". Specs as the primary artifact; adjacent to context engineering. — [YouTube](https://www.youtube.com/watch?v=BIvILtt164I)
- [V] "WF26: Harness Engineering & Startup Battlefield ft. Garry Tan, Mike Krieger, @t3dotgg, DSPy". The final-day stream of World's Fair 2026, with a harness-engineering block. — [YouTube](https://www.youtube.com/watch?v=I2cbIws9j10)
- [V] "WF2026: Autoresearch & Keynotes ft. Anthropic, Google DeepMind, Amazon AGI, Sonar, Arena, Recursive". — [YouTube](https://www.youtube.com/watch?v=4sX_He5c4sI)
- [V] "The Highest Loop — swyx". — [YouTube](https://www.youtube.com/watch?v=Fva8hFosqyQ)

**LangGraph multi-agent**
- [V] "LangChain Academy New Course: Introduction to LangGraph". Promoted by the official @LangChain channel. — [YouTube](https://www.youtube.com/watch?v=29XE10U6ooc)
- [V2] "Understanding multi-agent handoffs". Explains handoffs using LangGraph Swarm; the uploader was not shown, so it is not confirmed as LangChain's own. — [YouTube](https://www.youtube.com/watch?v=WTr6mHTw5cM)
- [V2] "Build a Supervisor Multi-agent Architecture with LangGraph". A supervisor-pattern tutorial; the uploader was not confirmed. — [YouTube](https://www.youtube.com/watch?v=KAwiykWkQ5Q)

**XBOW (DEF CON 33, interviews)**
- [V] "DEF CON 33 - Prompt Scan Exploit AI's Journey Through 0Days and 1000 Bugs - D. Jurado & J. Nogue". XBOW researchers on the HackerOne run; this is the key primary talk on XBOW's architecture. — [YouTube](https://www.youtube.com/watch?v=sOkgHfu4lXY)
- [V] "XBOW gives AMA after their hackbot hit #1 on HackerOne" (Short). Joel Noguera and Diego Jurado taking audience questions at DEF CON. — [YouTube](https://www.youtube.com/shorts/0oLPHgeRsYk)
- [V] "XBOW - AI Hacking Agent and Human in the Loop with Diego Jurado (Ep. 134)". XBOW history and human-in-the-loop review. — [YouTube](https://www.youtube.com/watch?v=rvA8IbyogJ0)
- [V] "Automating Offensive Security with XBOW". CISO Nico Waisman on continuous pentesting, false positives, production safety and audit trails. — [YouTube](https://www.youtube.com/watch?v=Q17hit4J3Cg)
- [V] "Cracking the Code on Offensive Security With AI ft XBOW CEO and GitHub Copilot Creator Oege de Moor". — [YouTube](https://www.youtube.com/watch?v=9mIphDV9m9c)
- [V] XBOW channel. — [YouTube @xbow-ai](https://www.youtube.com/@xbow-ai)

**DARPA AIxCC finals (DEF CON 33, Aug 2025)**
- [V] "DEF CON 33 Preview - AIXCC - Andrew Carney" (DARPA). — [YouTube](https://www.youtube.com/watch?v=lQDOIFZgBs4)
- [V] "AIxCC Winners Announcement". Carney on lessons learned; DARPA and ARPA-H award $9M. — [YouTube](https://www.youtube.com/watch?v=8F1j2zjRjZM)
- [V] "DEFCON 33 Video Team - AIxCC with ShellPhish". — [YouTube](https://www.youtube.com/watch?v=PiJwIUGJGmw)
- [V] "ShellPhish on the AI Cyber Challenge". How the team built its CRS. — [YouTube](https://www.youtube.com/watch?v=FIXBv8Idppk)
- [V] "Team All You Need IS A Fuzzing Brain - DARPA AI Cyber Challenge" (Texas A&M finalist). — [YouTube](https://www.youtube.com/watch?v=aGFEXm014vA)

**Jason Haddix**
- [V] "KEYNOTE: Attacking AI by Jason Haddix". DEF CON 33 Bug Bounty Village keynote (per the result summary). — [YouTube](https://www.youtube.com/watch?v=mYQgUHVgBPU)
- [V] "Keynote: Red, Blue, and Purple AI - Jason Haddix". OWASP Global AppSec San Francisco (2024, per the summary); shares a name with his course. — [YouTube](https://www.youtube.com/watch?v=XHeTn7uWVQM)
- [V] "Building Hackbots: Jason Haddix on Why AI Won't Replace Pentesters, But It Will Change How They Work". Recap of his CYBR.HAK.CON keynote. — [YouTube](https://www.youtube.com/watch?v=j7TWLF3YsUI)
- [V] "Building Web Hacking Micro Agents with Jason Haddix (Ep. 102)". The micro-agent design for web hacking. — [YouTube](https://www.youtube.com/watch?v=3y8dyeKmJQI)
- [V] "The Future of Hacking is Agentic w/ Jason Haddix". — [YouTube](https://www.youtube.com/watch?v=adV922E1ve0)

**NahamSec**
- [V] "Bug Bounty, Hacking, Community & Building with AI". NahamSec with Rishiraj Sharma (ProjectDiscovery) on where humans still matter. About 41 days before 2026-10-27, so roughly mid-Sept 2026. — [YouTube](https://www.youtube.com/watch?v=6ajeZq9Qtow)
- [V] "AI-Powered Bug Hunting • Ben Sadeghipour @NahamSec • GOTO 2024". — [YouTube](https://www.youtube.com/watch?v=lCZtgN7b4TE)

**Critical Thinking – Bug Bounty Podcast (hackbot episodes)**
- [V] Channel. — [YouTube @criticalthinkingpodcast](https://www.youtube.com/@criticalthinkingpodcast)
- [V] "Bug Bounty Singularity (Ep. 181)". Joseph Thacker and XSSDoctor on building a hackbot. — [YouTube](https://www.youtube.com/watch?v=QrVOFD2Xl2A)
- [V] "Partial Auth, Hackbot GraphQL, and AI's #1 Mission (Ep. 182)". — [YouTube](https://www.youtube.com/watch?v=IESlRFjT0G8)
- [V] "Ads Dawson - 750+ Bugs in 2026 - Destroying Hackbot Era (Ep. 184)". 0xMoose's report velocity; promoted on X 2026-07-23. — [YouTube](https://www.youtube.com/watch?v=0olk6lC1AGg)
- [V] "Rhyno's Hackbot Setup, Sick Bugs, and ZDI Drama (Ep. 175)". The hosts compare hackbot setups. — [YouTube](https://www.youtube.com/watch?v=v-XhQHy_jHM)
- [V] "Rez0s Sick Caching Bug & Local AI vs Subsidized tokens (Ep.191)". Scaling hackbots, and local versus API models. — [YouTube](https://www.youtube.com/watch?v=vSCUThW90Cw)
- [V] "Hackbot Proof-of-Concept Skill Creation (Ep. 192)". Writing PoC skills for hackbots. — [YouTube](https://www.youtube.com/watch?v=xxU6mq4JoNo)
- [V] "Protobuf Hacking, AI-Powered Bug Hunting, and Self-Improving Claude Workflows (Ep. 165)". — [YouTube](https://www.youtube.com/watch?v=J8wFq51TtMs)
- [V] "Is this how Bug Bounty Ends (Ep. 129)". The future of hackbots. — [YouTube](https://www.youtube.com/watch?v=XkmoloPKri8)

**Third-party reviews of open-source AI pentest agents (uploaders not confirmed)**
- [V] "I Let Strix Attack My App... Here's What It Found". — [YouTube](https://www.youtube.com/watch?v=vRTliPb8fjg)
- [V] "PentAGI: An AI Agent Team That Actually Runs the Pentest (29k★, Self-Hosted)". — [YouTube](https://www.youtube.com/watch?v=P3nCvrq20HU)
- [V] "Claude AI as a Pentester? Setting up HexStrike-AI MCP on Kali Linux". — [YouTube](https://www.youtube.com/watch?v=jXLB6xzrpzU)

**Practitioner channels**
- [V] AI Engineer (@aiDotEngineer). Talks from AI Engineer Summit and World's Fair, including 2026. — [YouTube](https://www.youtube.com/@aiDotEngineer)
- [V] LangChain (@LangChain). "How to build agents with LangChain products". — [YouTube](https://www.youtube.com/@LangChain)
- [V] IndyDevDan (@indydevdan). Agentic-coding channel. Example videos: "Claude 4 ADVANCED AI Coding: How I PARALLELIZE Claude Code with Git Worktrees" ([f8RnRuaxee8](https://www.youtube.com/watch?v=f8RnRuaxee8)) and "I'm HOOKED on Claude Code Hooks" ([J5B9UGTuNoM](https://www.youtube.com/watch?v=J5B9UGTuNoM)). — [YouTube](https://www.youtube.com/@indydevdan)
- [V] John Hammond (@_JohnHammond; channel ID UCVeW9qkBjo3zosnqUbG7CFw). — [YouTube](https://www.youtube.com/channel/UCVeW9qkBjo3zosnqUbG7CFw)
- [V2] Anthropic: handle "@anthropic-ai", credited in the Code w/ Claude video listings. The channel page itself did not come up. — [Cal Rueb video](https://www.youtube.com/watch?v=gv0WHhKelSE)
- [V2] Daniel Miessler / Unsupervised Learning: handle "@unsupervised-learning", quoted in a third-party video description. — [YouTube](https://www.youtube.com/watch?v=-aYxqKsh_Ho)
- [V] "Daniel Miessler - Anatomy of an Agentic Personal AI Infrastructure". [un]prompted 2026 talk. — [YouTube](https://www.youtube.com/watch?v=l9CPmPk2R-M)

### Inferences
- The richest verified video seams for a hackbot roadmap are the Critical Thinking hackbot episodes (Ep. 175–192, 2026) plus XBOW's DEF CON 33 talk. Practitioners are clearly converging on "agent file + skills + goal" setups; this matches rez0's X post below.
- Context-engineering material on YouTube is concentrated in the "Context Engineering SF" meetup series (Horthy, Huber) and Lance Martin talks, more than in World's Fair recordings, which mostly survive as full-day streams.

### Gaps
- **XBOW at Black Hat USA 2025:** no talk recording found. XBOW's own X post says only that it ran XBOW live at booth 3257.
- **John Hammond:** no video reviewing PentestGPT, PentAGI, Strix or HexStrike could be tied to his channel. Two targeted searches returned only third-party uploads.
- **NahamSec:** the channel handle URL was not returned directly. No NahamSec "hackbot" video was found.
- **LangGraph tutorials:** no multi-agent tutorial was confirmed as uploaded by the official LangChain channel.
- **AI Engineer World's Fair 2025:** no individual talk specifically on "context engineering" was found by title.
- **Dates:** exact upload dates for most YouTube items were not visible in search results.

## Q2. Reddit: exact thread URLs (2025–2026)

### Takeaway
No Reddit thread could be verified. reddit.com is blocked at every layer available here: proxy 403 on curl/JSON, WebSearch's `allowed_domains: reddit.com` rejected as inaccessible, and four unfiltered "r/…" queries that returned zero reddit.com URLs.

### Cited Findings
- None. No reddit.com URL appeared in any search result this session.
- Closest non-Reddit stand-in found for "Claude Code for bug bounty" community discussion: [V] YesWeHack, "Can Claude Code independently find and validate real vulnerabilities…". It reports that the LLM identifies vulnerabilities well but "often struggles to produce a proper proof of concept". — [YesWeHack](https://www.yeswehack.com/learn-bug-bounty/llm-series-claude)

### Inferences
- The search backend appears to exclude Reddit; the error message cites Anthropic crawler access. Reddit links will need manual collection, e.g. old.reddit.com/r/bugbounty/search?q=xbow&restrict_sr=on.

### Gaps
- None of the requested threads could be verified. That covers r/bugbounty (AI tools, hackbots, AI slop, XBOW), r/netsec (XBOW, Big Sleep, PentestGPT, Strix, Shannon, PentAGI), r/LocalLLaMA (local pentest models), r/ClaudeAI and r/ClaudeCode (security use, subagents), r/AI_Agents and r/LangChain (multi-agent lessons), and r/cybersecurity (autonomous pentest agents).
- An earlier note (community_social_sources.md) cites reddit.sentinel-team.org snapshot mirrors. Those were not re-checked here.

## Q3. Hacker News: exact item URLs

### Takeaway
HN is well indexed, and every requested thread was found except a dedicated item for Anthropic's "How we built our multi-agent research system" post. That post is discussed in comment threads; the exact story item was not confirmed. Strix, Shannon and an "open-source XBOW alternative" all have launch threads. No PentAGI-specific HN thread was found.

### Cited Findings

**XBOW / hackbots**
- [V] "XBOW, an autonomous penetration tester, has reached the top spot on HackerOne". June 2025. Comments are skeptical: "marketing gimmick", 3/43 valid at AT&T versus 22/24 at Disney, and pre-submission human review. — [HN 44367548](https://news.ycombinator.com/item?id=44367548)
- [V] "Show HN: Open-source alternative to XBOW". — [HN 45428526](https://news.ycombinator.com/item?id=45428526)
- [V] "Show HN: Strix - Open-source AI hackers for your apps". Strix launch, around Aug 2025 (inferred from item ID). — [HN 44945113](https://news.ycombinator.com/item?id=44945113)
- [V] "Shannon: OSS Autonomous AI Pentester" (KeygraphHQ). One commenter says it "smoked the output of one of the $10K pentests" in white-box mode. — [HN 46944416](https://news.ycombinator.com/item?id=46944416); also [V] "Shannon" — [HN 46915303](https://news.ycombinator.com/item?id=46915303)
- [V] "Comparing AI agents to cybersecurity professionals in real-world pen testing". — [HN 46518996](https://news.ycombinator.com/item?id=46518996)
- [V] "The Rise of AI Pentesting Agents: A Technical Analysis (2026)". A commenter argues that source-aware tools (XBOW, Big Sleep, ZeroPath) are the ones finding real CVEs. — [HN 47750350](https://news.ycombinator.com/item?id=47750350)
- [V] "Show HN: AI pentester". Recon, attack and verification agents, plus a judge agent that tries to disprove each finding. — [HN 47056759](https://news.ycombinator.com/item?id=47056759)
- [V] "Show HN: We post-trained a model that pen tests instead of refusing". — [HN 48609231](https://news.ycombinator.com/item?id=48609231)
- [V] "DeepSeek v4.1 Flash Is Now Our Best Hacking Model". — [HN 49725800](https://news.ycombinator.com/item?id=49725800)
- [V] "Small models also found the vulnerabilities that Mythos found". — [HN 47732020](https://news.ycombinator.com/item?id=47732020)
- [V] "AI agents find $4.6M in blockchain smart contract exploits". — [HN 46115214](https://news.ycombinator.com/item?id=46115214)

**Anthropic / Cognition agent-architecture posts**
- [V] "Building Effective Agents \ Anthropic". Dec 2024 thread on the workflows-versus-agents essay. — [HN 42468058](https://news.ycombinator.com/item?id=42468058); a second thread, [V] "Building Effective "Agents"". — [HN 42470541](https://news.ycombinator.com/item?id=42470541)
- [V] "Effective context engineering for AI agents" (Anthropic, ~late Sep 2025). Comments cover tool and MCP context budgets and output engineering. — [HN 45418251](https://news.ycombinator.com/item?id=45418251)
- [V] "Effective harnesses for long-running agents" (Anthropic). — [HN 46081704](https://news.ycombinator.com/item?id=46081704)
- [V] "Don't Build Multi-Agents" (Cognition). — [HN 45096962](https://news.ycombinator.com/item?id=45096962); [V] the comment thread "Whom to believe? Devin or Claude?" contrasts it with Anthropic's multi-agent post. — [HN 45097335](https://news.ycombinator.com/item?id=45097335)
- [V2] "Building Effective AI Agents". A thread where commenters, including links to Simon Willison's notes, discuss Anthropic's multi-agent research system (orchestrator plus parallel subagents). It may not be the dedicated story. — [HN 44301809](https://news.ycombinator.com/item?id=44301809)
- [V] "Orchestrate teams of Claude Code sessions". — [HN 46902368](https://news.ycombinator.com/item?id=46902368)
- [V] "Claude Skills". — [HN 45607117](https://news.ycombinator.com/item?id=45607117)

**curl and AI slop**
- [V] "cURL stopped HackerOne bug bounty program due to excessive slop reports" (Jan 2026, links the GitHub notice). — [HN 46678710](https://news.ycombinator.com/item?id=46678710)
- [V] "cURL removes bug bounties". — [HN 46701733](https://news.ycombinator.com/item?id=46701733)
- [V] "Curl ending bug bounty program after flood of AI slop reports". — [HN 46724225](https://news.ycombinator.com/item?id=46724225)
- [V] "Overrun with AI slop, cURL scraps bug bounties to ensure "intact mental health"" (Ars Technica). — [HN 46729871](https://news.ycombinator.com/item?id=46729871)
- [V] "Curl: We still have not seen a valid security report done with AI help" (May 2025, the precursor). — [HN 43907376](https://news.ycombinator.com/item?id=43907376)
- [V] "Potential issues in curl found using AI assisted tools". The counterpoint: Stenberg reportedly acknowledged more than 100 good AI-assisted reports. — [HN 45449348](https://news.ycombinator.com/item?id=45449348)

**Anthropic GTG-1002 report (Nov 13 2025)**
- [V] "Disrupting the first reported AI-orchestrated cyber espionage campaign". — [HN 45918638](https://news.ycombinator.com/item?id=45918638)
- [V] "Anthropic's paper smells like bullshit". The main skeptical response. — [HN 45944296](https://news.ycombinator.com/item?id=45944296)

**LLM vulnerability-discovery milestones**
- [V] "How I used o3 to find a remote 0-day vulnerability in the Linux kernel (ksmbd)" (Sean Heelan, CVE-2025-37899, post dated May 22 2025). — [HN 44060711](https://news.ycombinator.com/item?id=44060711); also [V] "I used o3 to find a remote zeroday in the Linux SMB implementation". — [HN 44081338](https://news.ycombinator.com/item?id=44081338)
- [V] "Using Large Language Models to Catch Vulnerabilities" (Project Zero/DeepMind Big Sleep, ~Nov 2024). — [HN 42017771](https://news.ycombinator.com/item?id=42017771)
- [V] "Google Says Its AI Found SQLite Vulnerability That Fuzzing Missed". AFL found nothing in 150 CPU-hours. — [HN 42043453](https://news.ycombinator.com/item?id=42043453)
- [V] "Google Big Sleep has found 77 Memory Corruption Issues with AI". — [HN 45899218](https://news.ycombinator.com/item?id=45899218)

### Inferences
- The HN threads are useful mainly as skepticism anthologies: XBOW's validity rates, GTG-1002 evidence quality, and Big Sleep's "exploitable" claims. That is good material for a roadmap's "limits of hackbots" section.

### Gaps
- No dedicated HN story for "How we built our multi-agent research system" (Anthropic, June 13 2025) was confirmed, even with an extended search.
- No PentAGI launch thread was found.
- Exact HN timestamps were not visible; dates above come from the underlying events or rough item-ID ordering.

## Q4. X/Twitter: exact status URLs

### Takeaway
All requested anchor posts were confirmed except Huntley's original Ralph post. That set covers Lütke's and Karpathy's context-engineering posts, Boris Cherny's Jan 2 2026 setup thread, rez0's hackbot posts, XBOW's milestone posts and Haddix's hackbot-course tips. Dates are exact (UTC, decoded from status IDs).

### Cited Findings

**Context engineering (June 2025)**
- [V] Tobi Lütke (@tobi), 2025-06-19: "I really like the term "context engineering" over prompt engineering. It describes the core skill better: the art of providing all the context for the task to be plausibly solvable by the LLM." — [X](https://x.com/tobi/status/1935533422589399127)
- [V] Andrej Karpathy (@karpathy), 2025-06-25, URL confirmed: "+1 for "context engineering" over "prompt engineering"… the delicate art and science of filling the context window…" — [X](https://x.com/karpathy/status/1937902205765607626)
- [V] Simon Willison (@simonw), 2025-06-27: "I think context engineering is going to stick…" — [X](https://x.com/simonw/status/1938745355916714448)

**Claude Code workflow (Boris Cherny, @bcherny)**
- [V] 2026-01-02: "I'm Boris and I created Claude Code… My setup might be surprisingly vanilla!" The thread covers 5 parallel terminal Claudes plus 5–10 web sessions, a shared CLAUDE.md in git, Plan mode first, slash commands and subagents, /permissions instead of --dangerously-skip-permissions, and a PostToolUse formatting hook. — [X](https://x.com/bcherny/status/2007179832300581177)
- [V] 2026-01-02, tip 13/: "give Claude a way to verify its work… it will 2-3x the quality". Directly relevant to hackbot validators. — [X](https://x.com/bcherny/status/2007179861115511237)
- [V] 2026-01-31: team tips thread (parallel worktrees, "Grill me on these changes…"). — [X](https://x.com/bcherny/status/2017742741636321619)
- [V] 2026-02-11: on customizability (hooks, plugins, skills, custom agents). — [X](https://x.com/bcherny/status/2021699851499798911)
- [V] 2026-03-30: hidden and under-used features. — [X](https://x.com/bcherny/status/2038454336355999749)

**Ralph loop (Geoffrey Huntley, @GeoffreyHuntley)**
- [V] Huntley, 2026-03-15: "ralph is a pattern for an mallocing orchestrator. the bash loop is a crude form for teaching… a watchdog should exist on top of the loop". — [X](https://x.com/GeoffreyHuntley/status/2033317516554944976)
- [V] Matt Pocock (@mattpocockuk), 2026-01-11: "I felt suspicious about Claude Code's Ralph plugin… Stick with a bash loop, you'll get better results". — [X](https://x.com/mattpocockuk/status/2010330604123144389)
- [V] Dex Horthy (@dexhorthy), 2025-11-01: "Running Agents in an Infinite loop w/ @GeoffreyHuntley's "ralph wiggum" technique" (session announcement). — [X](https://x.com/dexhorthy/status/1984765179452936649)

**Hackbots: Joseph Thacker (@rez0__)**
- [V] 2026-05-14: "Here's the sauce: agent md file with lots of disclaimers about how it's approved testing, a bunch of hacking skills, /goal find a crit on target.com. That's literally 90% of the way there". — [X](https://x.com/rez0__/status/2055067183168794953)
- [V] 2025-09-05: "the latest @Jhaddix newsletter is a full guide on how to actually engineer a good hackbot". — [X](https://x.com/rez0__/status/1964016722081403031)
- [V] 2025-02-10: TEDx talk "The Rise of AI Hackbots" now on the main TEDx channel. — [X](https://x.com/rez0__/status/1888781796885905857)
- [V] 2025-08-18: on XBOW's post, "XBOW isn't here to replace pentesters… Yes it is. And that's okay." — [X](https://x.com/rez0__/status/1957491543268270236)

**XBOW**
- [V] @Xbow, 2024-12-17: "…ended up at #11 in the US on HackerOne". Early milestone. — [X](https://x.com/Xbow/status/1869053482642362846)
- [V] @Xbow, 2025-07-31: "XBOW is now the #1 hacker on HackerOne, globally… Next week at #BlackHat… Booth 3257". — [X](https://x.com/Xbow/status/1951040733936492915)
- [V] Leandro Barragan (@lean0x2f), 2025-08-18: "Xbow concludes its HackerOne & Bug Bounty efforts". — [X](https://x.com/lean0x2f/status/1957493111124312148)
- [V] Oege de Moor (@oegerikus, XBOW co-founder), 2026-10-05: retrospective. He set the goal of topping the global leaderboard before Black Hat, and reports 54 critical bugs in 90 days. — [X](https://x.com/oegerikus/status/2107187332315336801)

**Jason Haddix (@Jhaddix) and the CTBB podcast**
- [V] Haddix, 2026-09-01, hackbots-course tip: give agents "the full path for everything… Skills, tools, and full defined syntax", otherwise the hackbot fails to find a skill or tool "and make[s] up its own solution". — [X](https://x.com/Jhaddix/status/2094803626950902056)
- [V] Haddix, 2025-01-25: AI "cannot replace mid/sr people… mostly helps scale small teams". — [X](https://x.com/Jhaddix/status/1883144233059500316)
- [V] @ctbbpodcast, 2026-07-08: "A wide-scope hackbot points at everything…", with a NoSQL `$where` injection example. — [X](https://x.com/ctbbpodcast/status/2074893912024498625)
- [V] @ctbbpodcast, 2026-07-23: Ep. 184 promo, "how the AI pros really build and use their hackbots". — [X](https://x.com/ctbbpodcast/status/2080231760235741188)
- [V] NahamSec (@NahamSec), 2025-05-22: "@rez0__ … with AI Findings in Bug Bounty live at #NahamCon2025". — [X](https://x.com/NahamSec/status/1925617660911366148)
- [V] Reuters, 2026-10-19: "EXCLUSIVE: How one hacker used AI agents and a few thousand dollars to break into scores of sites". Current-events signal. — [X](https://x.com/Reuters/status/2112189566765474049)

### Inferences
- The X posts give the clearest primary-source statements of hackbot design heuristics: rez0's "agent md + skills + goal", Haddix's "full paths, defined syntax" and Cherny's "give it a way to verify". All three map onto harness design, not model choice.

### Gaps
- Huntley's original Ralph announcement post on X was not found; the canonical write-up is reportedly ghuntley.com/ralph, which was not fetched.
- XBOW's own Aug 18 2025 "concluding our primary mission" post on X was not returned; only third-party posts quoting it were.
- Haddix's "Building AI Hackbots, Part 1" newsletter URL was not found; only rez0's link (t.co) to it.
