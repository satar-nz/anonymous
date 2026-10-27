# Structured Learning Resources for an AI Hackbot Builder: Courses, Lectures, Videos, Books (as of 2026-10-27)

**How these were checked (read this first):** Every URL below came back in live web-search results during this session (2026-10-27). Direct page fetches (WebFetch) failed with DNS errors for every host I tried (rdi.berkeley.edu, agenticai-learning.org, anthropic.skilljar.com, portswigger.net). The proxy blocked curl and the GitHub API for public repos. The shared web-search budget then ran out partway through. So "verified" here means the URL and its key facts showed up in search-indexed results. It does not mean I loaded the live page. I flag items as **[from memory, not verified this session]** or **[weak/aggregator source]** where that applies. Items are numbered across all sections (#1–#80) so the report writer can count them. Lines starting "Why:" are my own judgment of whether the item is worth the time, not cited fact. Levels: B = beginner, I = intermediate, A = advanced.

---

## 1. University courses: which have free recorded lectures? (2024–2026 offerings)

### Takeaway
The Berkeley RDI series is the main free, recorded university track on LLM agents: LLM Agents (F24), then Advanced LLM Agents (SP25), then Agentic AI (F25). For hackbot builders specifically, CMU 11-768 "AI Agents" (Fall 2026, Neubig & Fried) stands out because it has students build an agent harness from scratch and design agent evals, with recordings posted as the term runs. Stanford CS336 and CS25 are freely recorded foundations courses. CS329A and Princeton COS 597C are newer agent seminars whose recordings are uncertain or missing.

### Cited Findings
**#1. CS 294/194-196 "Large Language Model Agents" MOOC (Fall 2024)**
- UC Berkeley RDI (Dawn Song et al.). URL: https://rdi.berkeley.edu/llm-agents/f24
- Format: Lectures were Mondays 3–5pm PT in Latimer 120. Each schedule entry links slides, an "Original Recording" and an "Edited Video". Example: Sept 16, "LLM agents: brief history and overview", Shunyu Yao (OpenAI).
- Cost: free to audit. Status: archived but fully viewable. Level: I. Prereqs: ML/LLM basics.
- [Berkeley RDI F24 page](https://rdi.berkeley.edu/llm-agents/f24); [intro slides](https://rdi.berkeley.edu/llm-agents/assets/intro.pdf)
- A Denny Zhou "LLM Reasoning" lecture is mentioned only by an unreliable marketplace page. I could not confirm it on the official schedule.
- Why: the best single overview of the ReAct-to-agents lineage, given by the people who wrote the papers.

**#2. CS 294/194-280 "Advanced Large Language Model Agents" (Spring 2025)**
- Dawn Song, with guest co-instructors Xinyun Chen (Google DeepMind) and Kaiyu Yang (Meta FAIR); head TA Alex Pan. URL: https://rdi.berkeley.edu/adv-llm-agents/sp25
- Dates: late January to late April 2025.
- Topics: inference-time techniques (Xinyun Chen), reasoning and planning for language agents (Yu Su), open training recipes (Hanna Hajishirzi), theorem proving (Sean Welleck), and agent security, including using LLMs to catch vulnerabilities in real-world code.
- Cost: free MOOC. Level: A. Prereqs: the F24 course or equivalent.
- [RDI SP25 page](https://rdi.berkeley.edu/adv-llm-agents/sp25); [SP25 intro slides](https://rdi.berkeley.edu/adv-llm-agents/slides/llm-agents-berkeley-intro-sp25.pdf); [Substack recap](https://builditdeploy.substack.com/p/join-me-in-uc-berkeleys-rdi-advanced-llm-agents-mooc)
- Why: the security and code-vulnerability lectures bear directly on hackbots.

**#3. CS 294/194-196 "Agentic AI" MOOC (Fall 2025)**
- Dawn Song; TAs Xiuyu Li and Baifeng Shi. URLs: https://rdi.berkeley.edu/agentic-ai/f25; the course slides give the course site as agenticai-learning.org/f25.
- Third-party listings describe about 12 weekly lectures of roughly 2 hours, free and self-paced, with recordings on YouTube and guest speakers from OpenAI, Google DeepMind, NVIDIA and Microsoft.
- Confirmed lectures: Peter Stone on autonomous agents, and Dawn Song on agentic AI safety and security (Dec 8).
- Level: I–A.
- [RDI F25 page](https://rdi.berkeley.edu/agentic-ai/f25); [F25 intro slides](https://rdi.berkeley.edu/agentic-ai/slides/introduction_25.pdf); [beri.net listing, aggregator](https://www.beri.net/learning/berkeley-agentic-ai-mooc-f25); [DEV recap](https://dev.to/ajay_kumarkv_ad4e59dc31/shaping-the-future-with-agentic-ai-reflections-from-the-uc-berkeley-agentic-ai-mooc-fall-2025-3cp2)
- Related: the Berkeley RDI Agentic AI Summit 2025, https://rdi.berkeley.edu/events/agentic-ai-summit-2025 ([RDI](https://rdi.berkeley.edu/events/agentic-ai-summit-2025)). RDI's education hub: https://rdi.berkeley.edu/education ([RDI](https://rdi.berkeley.edu/education)).
- Why: the most current free university agent series. The safety/security lecture covers attacks on agents, which a hackbot builder also needs to defend against.

**#4. CMU 11-768 "AI Agents" (Fall 2026, Aug 25 – Dec 3, 2026)**
- Graham Neubig and Daniel Fried (LTI). Site: cmu-agents.com. I could not read it directly because it is a JavaScript app, so details come from secondary sources.
- Format: sessions of roughly 60–75 minutes; recordings posted to YouTube as the term runs (the first four were up by early September 2026); described as free.
- Content: building an agent harness from scratch; designing agent evaluations; training agentic LLMs with SFT and RL; sandboxing and credential management; observability; GUI and deep-research agents; multi-agent interaction. Guest lectures by Sasha Rush and Karthik Narasimhan. One blog counts 28 sessions and 3 assignments.
- Prereqs: graduate NLP and Python; Assignment 1 needs a Modal account and access to an OpenAI-compatible API. Level: A.
- [beri.net listing](https://www.beri.net/learning/cmu-11-768-ai-agents-fall-2026); [heyuan110 course breakdown (Chinese)](https://www.heyuan110.com/zh/posts/ai/2026-09-08-cmu-11-768-ai-agents-course/)
- A deep-research lecture by Akari Asai (Oct 10) is reported only by a social-media aggregator and is unverified.
- Why: the closest academic match to "build a harness, sandbox it, evaluate it". Neubig is a co-creator of OpenHands.

**#5. CMU 11-711 Advanced NLP**
- Fall 2024 (Neubig) has all 23 lectures on YouTube. Spring 2026 was taught by Sean Welleck, includes agent lectures, and reportedly posted no videos.
- Level: I–A. Free (Fall 2024 videos).
- [Digg post linking the playlist](https://digg.com/tech/c2rmqwzp); [beri.net SP26 listing](https://www.beri.net/learning/cmu-11-711-advanced-nlp-spring-2026)
- The official course URL was not verified (cmu-anlp.github.io appears only through a domain-index site).

**#6. Stanford CS25 "Transformers United V6" (Spring 2026, roughly Mar 30 – Jun 3, 2026 per an aggregator)**
- Stanford seminar with guest researchers; past speakers include Hinton, Vaswani and Karpathy. URLs: https://web.stanford.edu/class/cs25/; recordings at https://web.stanford.edu/class/cs25/recordings
- The Stanford bulletin says lectures are livestreamed and recorded. V6 titles reported by aggregators include "Overview of Transformers", "The Ultra-Scale Talk: Scaling Training to Thousands of GPUs" and "Serving Transformers: Lessons from the Trenches".
- Free; about 75-minute talks. Level: I.
- [CS25 site](https://web.stanford.edu/class/cs25/index.html); [recordings page](https://web.stanford.edu/class/cs25/recordings); [V5 archive](https://web.stanford.edu/class/cs25/past/cs25-v5); [Stanford bulletin](https://bulletin.stanford.edu/courses/2233491)
- Why: a cheap way to keep up with frontier research; optional for builders.

**#7. Stanford CS336 "Language Modeling from Scratch" (Spring 2025; Spring 2026 edition also exists)**
- Tatsunori Hashimoto and Percy Liang.
- Site: https://stanford-cs336.github.io/spring2025/. Spring 2025 YouTube playlist: https://www.youtube.com/watch?v=SQ3fZ1sAqXI&list=PLoROMvodv4rOY23Y0BoGoBGgQ1zmU_MT_
- Videos are titled "Stanford CS336 Language Modeling from Scratch | Spring 2025 | Lecture N: …" (e.g., L4 Mixture of Experts, L10 Inference, L16 Alignment-RL). Spring 2026 lecture titles also circulate, e.g., "Spring 2026 Lecture 10 Inference".
- Free. Level: A. Prereqs: strong PyTorch and systems skills.
- [BUAA page listing the playlist and site](https://shi.buaa.edu.cn/zhangtianhan/zh_CN/jxzy/227780/content/1452.htm); [Stanford bulletin](https://bulletin.stanford.edu/courses/2255541); [aggregator mention of SP26 lecture](https://git.handmade.network/stanford-cs336-language-modeling-from-scratch-spring-2026-lecture-10-inference.html)
- Why: optional for hackbot builders. Take it only if you plan to fine-tune or RL-train your own models.

**#8. Stanford CS329A "Self-Improving AI Agents" (Autumn 2025)**
- Aakanksha Chowdhery and Azalia Mirhoseini. URL: https://cs329a.stanford.edu/
- Graduate seminar with lab guest speakers and a research final project; students get API credits; no auditing. 20 sessions.
- Topics: verifiers, train-time scaling/RL, search and deep-research agents, agent evaluation.
- Prereqs: CS224N or CS229S, Python, experience with LLM APIs. Level: A.
- [CS329A site](https://cs329a.stanford.edu/); [Stanford bulletin](https://bulletin.stanford.edu/courses/2263721)
- Recordings: conflicting. A Notion guide says it was posted free on YouTube ([Notion guide](https://resources-evolvingai.notion.site/Stanford-CS329A-Self-Improving-AI-Agents-Lecture-by-Lecture-3b4ff95fa05981889104d9646543d9da)); beri.net says no recordings were published ([beri.net](https://www.beri.net/learning/stanford-cs329a-self-improving-ai-agents)).

**#9. Princeton COS 597C "Advanced Topics in Computer Science: AI Agents" (Fall 2026)**
- Karthik Narasimhan (co-author of ReAct, SWE-bench and SWE-agent).
- Graduate seminar covering agent architectures, LLM planning and reasoning, tool use, multi-agent systems, benchmarks and safety. No public recordings found.
- [Princeton registrar listing](https://paws.princeton.edu/courses/fall-2026/advanced-topics-computer-science-ai-agents)
- Older related course: COS 597G "Understanding Large Language Models" (Fall 2023, Danqi Chen) ([listing](https://paws.princeton.edu/courses/fall-2023/advanced-topics-computer-science-understanding-large-language-models)).
- Why: read the reading list if one is published. Not watchable.

### Inferences
- Only Berkeley RDI (F24, SP25, F25), CS336, CS25 and, partway through the term, CMU 11-768 have confirmed free recordings. CS329A is uncertain. Princeton COS 597C has none.
- CMU 11-768 is the highest-signal university course for building a hackbot harness (sandboxing, credentials, evals, RL). Berkeley SP25 and F25 are the best academic sources on agent security.

### Gaps
- Stanford CS224N (2024/2025 recordings), MIT courses, and university courses specifically on "AI for offensive security / AI-assisted pentesting" (NYU, Georgia Tech, Purdue, CMU security) were not verified because the search budget ran out. The one hit was a Warsaw University of Technology document titled "Agentic AI for Offensive Cybersecurity" ([PW .doc](https://www.elka.pw.edu.pl/content/download/34731/228919/file/01-Agentic%20AI%20for%20Offensive%20Cybersecurity.doc)); I did not determine whether it is a course.
- A 2026 Berkeley RDI offering (spring or fall) was neither confirmed nor ruled out.
- I could not load the full F25 Agentic AI lecture list or YouTube playlist URLs.

---

## 2. Online courses for LLM/agent engineering (prompting, context, agent loops, LangGraph, multi-agent, MCP, evals)

### Takeaway
The best free core stack:
- DeepLearning.AI "Agentic AI" (Andrew Ng) for design patterns.
- Anthropic Academy (Skilljar) plus the anthropics/courses notebooks for the Claude API, tool use, MCP and Claude Code.
- Hugging Face Agents and MCP courses for open-framework hands-on work.
- LangChain Academy for graph-based orchestration and Deep Agents.
- DeepLearning.AI short courses for memory, evals and multi-agent work.

The one paid course worth its premium is Hamel Husain and Shreya Shankar's evals course on Maven.

### Cited Findings
**#10. DeepLearning.AI "Agentic AI" (2025)**
- Andrew Ng. URL: https://www.deeplearning.ai/courses/agentic-ai/ (also on Coursera: https://www.coursera.org/learn/dlai-agentic-ai)
- Intermediate; about 9h55m; 31 video lessons; 7 code examples; graded assignments for PRO members.
- Covers four patterns: reflection, tool use, planning (including planning with code execution) and multi-agent workflows.
- The Coursera listing shows a smaller package, so the two listings conflict.
- [DLAI course page](https://www.deeplearning.ai/short-courses/agentic-ai/); [DLAI info](https://learn.deeplearning.ai/courses/agentic-ai/information); [Coursera](https://www.coursera.org/learn/dlai-agentic-ai)
- Why: the clearest vendor-neutral grounding in agent design patterns before you pick a framework.

**#11. "MCP: Build Rich-Context AI Apps with Anthropic"**
- DeepLearning.AI with Anthropic; taught by Elie Schoppik (Anthropic Head of Technical Education).
- URL: https://www.deeplearning.ai/short-courses/mcp-build-rich-context-ai-apps-with-anthropic
- Intermediate; 1h38m (some pages say 1h48m); 11 videos and 6 code examples. You build an MCP chatbot, then an MCP server with FastMCP, tested with MCP Inspector.
- Was free during the DLAI platform beta. Prereqs: Python and basic LLM API use.
- [DLAI](https://deeplearning.ai/short-courses/mcp-build-rich-context-ai-apps-with-anthropic); [learn.deeplearning.ai](https://learn.deeplearning.ai/courses/mcp-build-rich-context-ai-apps-with-anthropic/information)

**#12. "Claude Code: A Highly Agentic Coding Assistant"**
- DeepLearning.AI with Anthropic; Elie Schoppik. URL: https://www.deeplearning.ai/short-courses/claude-code-a-highly-agentic-coding-assistant
- Intermediate; 1h50m; 10 lessons and 1 graded assignment (PRO). Projects include a RAG chatbot codebase, notebook-to-dashboard and Figma-to-web-app.
- [DLAI](https://deeplearning.ai/short-courses/claude-code-a-highly-agentic-coding-assistant)
- Why: Claude Code is a reference agent harness. Studying how it is driven (CLAUDE.md, subagents, hooks) feeds directly into harness design.

**#13. "Evaluating AI Agents"**
- DeepLearning.AI with Arize; John Gilhuly and Aman Khan. URL: https://www.deeplearning.ai/short-courses/evaluating-ai-agents
- Beginner; 2h16m; 15 videos and 6 code examples. Covers tracing and observability, component-wise evals, code-based and LLM-as-judge evals.
- [DLAI](https://www.deeplearning.ai/short-courses/evaluating-ai-agents); [corporate page](https://corporate.deeplearning.ai/courses/evaluating-ai-agents/information)

**#14. "Long-Term Agentic Memory with LangGraph"**
- DeepLearning.AI with LangChain; Harrison Chase. URL: https://www.deeplearning.ai/short-courses/long-term-agentic-memory-with-langgraph
- Intermediate; 1h04m; 7 lessons and 5 code examples. Covers semantic, episodic and procedural memory through an email-assistant build.
- [DLAI](https://www.deeplearning.ai/short-courses/long-term-agentic-memory-with-langgraph)
- Why: maps onto hackbot needs such as remembering target state across sessions and storing learned techniques as procedural memory.

**#15. "AI Agents in LangGraph"**
- DeepLearning.AI; Harrison Chase (LangChain) and Rotem Weiss (Tavily). URL: https://www.deeplearning.ai/courses/ai-agents-in-langgraph
- Intermediate; 1h32m (another page says 1h42m); 9 lessons. Builds an agent from scratch in Python, then rebuilds it in LangGraph; covers persistence, streaming and human-in-the-loop.
- [DLAI](https://www.deeplearning.ai/courses/ai-agents-in-langgraph); [DLAI alt](https://www.deeplearning.ai/alpha/short-courses/ai-agents-in-langgraph)

**#16. "Multi AI Agent Systems with crewAI"**
- DeepLearning.AI; João Moura (CrewAI founder). URL: https://www.deeplearning.ai/short-courses/multi-ai-agent-systems-with-crewai
- Beginner; 2h41m; 18 lessons and 7 code examples. Covers role-based agents and short-term, long-term and shared memory.
- Follow-up course: "Design, Develop, and Deploy Multi-Agent Systems with CrewAI" (about 12h58m; MCP, Flows, production; graded).
- [DLAI](https://deeplearning.ai/short-courses/multi-ai-agent-systems-with-crewai); [follow-up](https://deeplearning.ai/courses/design-develop-and-deploy-multi-agent-systems-with-crewai)

**#17. Hugging Face Agents Course**
- URL: https://huggingface.co/learn/agents-course/unit0/introduction
- Free. Unit 1 fundamentals; Unit 2.1 smolagents, 2.2 LlamaIndex, 2.3 LangGraph; Unit 3 agentic RAG; Unit 4 final project.
- Certificates: fundamentals requires 80% on the Unit 1 quiz; completion requires a score above 30% on Unit 4 (one third-party review says 80%).
- Level: B–I.
- [HF Unit 0](https://huggingface.co/learn/agents-course/unit0/introduction); [HF Unit 1 cert](https://huggingface.co/learn/agents-course/unit1/get-your-certificate); [HF Unit 4 cert](https://huggingface.co/learn/agents-course/en/unit4/get-your-certificate)
- Why: compare three frameworks side by side for free.

**#18. Hugging Face MCP Course (built with Anthropic)**
- URL: https://huggingface.co/learn/mcp-course/unit0/introduction
- Free with a certificate. Covers MCP architecture, an end-to-end app (Unit 2) and deployment with partner services (Unit 3).
- Time estimates vary in third-party listings (10–15 hours total, or 3–4 hours per unit). Level: B–I.
- [HF MCP Unit 0](https://huggingface.co/learn/mcp-course/unit0/introduction); [HF org](https://huggingface.co/mcp-course/spaces)

**#19. LangChain Academy, "Introduction to LangGraph"**
- In the Foundation collection: https://academy.langchain.com/collections/foundation
- Free. Covers state, nodes, edges, memory and human-in-the-loop. Third parties estimate 5–8 hours. Level: I.
- [LangChain Academy Foundation](https://academy.langchain.com/collections/foundation); [aiagentrank review, aggregator](https://aiagentrank.io/learn/course/langchain-academy-langgraph-intro)

**#20. LangChain Academy, "Introduction to Deep Agents"**
- Fundamentals of Deep Agents and building a custom one for long-running tasks. Free.
- beri.net says 43 lessons and about 30 minutes of video, launched July 2026. Unverified: I could not check the date on the academy site.
- [academy listing](https://academy.langchain.com/collections/products?page=2); [beri.net](https://www.beri.net/learning/langchain-academy-intro-to-deep-agents)
- Why: the Deep Agents pattern (planner, subagents, filesystem, long horizon) is close to what a multi-hour pentest agent needs.

**#21–#24. Anthropic Academy (Skilljar)**
- Free with a Skilljar account. Catalog counts vary by date: 13, 17 or 19 courses.
- URL: https://anthropic.skilljar.com/ ([Skilljar home](https://anthropic.skilljar.com/); [i-programmer](https://www.i-programmer.info/news/105-artificial-intelligence/18735-anthropic-releases-free-courses-on-ai-.html); [catalog update April 2026](https://pasqualepillitteri.it/news/370/anthropic-academy-corsi-gratuiti-claude))
- #21 **"Building with the Claude API"**: the full Claude API surface; a common starting point for developers. https://anthropic.skilljar.com/claude-with-the-anthropic-api ([Skilljar](https://anthropic.skilljar.com/claude-with-the-anthropic-api))
- #22 **"Claude Code in Action"**: integrating Claude Code into development workflows ([Skilljar home](https://anthropic.skilljar.com/))
- #23 **"Introduction to Model Context Protocol"**: hands-on Python; build MCP servers and clients from scratch; covers the tools, resources and prompts primitives.
- #24 **"MCP: Advanced Topics"**: follow-on course, per a partner listing ([partner catalog](https://anthropic-partners.skilljar.com/page/all-courses)).
- Exact lengths were not verified.

**#25. GitHub anthropics/courses**
- URL: https://github.com/anthropics/courses
- Five Jupyter courses: API fundamentals, prompt engineering interactive tutorial, real-world prompting, prompt evaluations, and tool use (6 lessons, including tool_choice and a multi-tool chatbot).
- Free; needs an API key. Some notebooks reference Claude 3 Haiku, so model names are dated but techniques still apply.
- [GitHub](https://github.com/anthropics/courses); [Maginative](https://www.maginative.com/article/anthropic-releases-two-new-resources-for-learning-prompt-engineering/)

**#26. GitHub anthropics/prompt-eng-interactive-tutorial**
- Nine-chapter interactive prompt-engineering tutorial; Chapter 9 covers complex industry prompts.
- [sourcepulse mirror, aggregator](https://www.sourcepulse.org/projects/1841853)
- Canonical URL: https://github.com/anthropics/prompt-eng-interactive-tutorial

**#27. Microsoft "AI Agents for Beginners"**
- URL: https://github.com/microsoft/ai-agents-for-beginners
- Free. Grew from 10 lessons to 12, with later listings citing 18 (through "securing AI agents"). Short videos and Python code; Azure AI Foundry or free/limited GitHub Models. Level: B.
- [Microsoft DevBlog](https://devblogs.microsoft.com/semantic-kernel/ai-agents-for-beginners-course-10-lessons-teaching-you-how-to-start-building-ai-agents); [KDnuggets](https://www.kdnuggets.com/12-essential-lessons-for-building-ai-agents)

**#28. Google x Kaggle "5-Day AI Agents Intensive" (Nov 10–14, 2025)**
- Free. Live run with livestreams and daily Kaggle codelabs; Day 2 covered tools and MCP; Day 4 covered quality, observability and evaluation; optional capstone with a badge.
- More than 1.5M learners. Now available as a self-paced Kaggle Learn Guide.
- A later "5-Day AI Agents Intensive Vibe Coding" iteration tagged 2026 also exists.
- [Google blog announcement](https://blog.google/technology/developers/ai-agents-intensive/); [Google recap](https://blog.google/technology/developers/ai-agents-intensive-recap/); [KDnuggets](https://www.kdnuggets.com/kaggle-googles-free-5-day-agentic-ai-course)
- Why: the course whitepapers (agents, tools/MCP, quality/evals) are compact references.

**#29. Maven "AI Evals For Engineers & PMs"**
- Hamel Husain and Shreya Shankar. Live cohort: 4 weeks, about 3–5 hours per week; sessions recorded with lifetime access.
- Latest cohort seen: Sep 5 – Oct 3, 2026.
- Price conflicts: $5,000 list discounted 25% to $3,750, versus $4,200 list with a 25% promo. A $1,995 figure appears on a low-reliability site.
- Level: I.
- [Product Compass review](https://www.productcompass.pm/p/best-ai-product-management-courses); [beri.net](https://www.beri.net/learning/maven-ai-evals-for-engineers-and-pms); [Maven free lecture: cold-start eval problem](https://maven.com/p/5fb214/the-cold-start-eval-problem-evals-for-new-ai-products); [Lenny's Newsletter episode](https://www.lennysnewsletter.com/p/why-ai-evals-are-the-hottest-new-skill)
- The canonical Maven course URL was not captured.
- Why: evals are what separates a demo hackbot from one that reliably finds true positives. This is the most practitioner-respected evals curriculum.

### Inferences
- Coverage of the requested topics:
  - Agent loop and design patterns: #10, #15, #17, #27.
  - Graphs and LangGraph: #15, #19, #14.
  - Long-horizon and subagents: #20, #12, #22.
  - MCP: #11, #18, #23, #24.
  - Multi-agent: #10, #16.
  - Evals: #13, #29.
- Many DLAI short courses were "free for a limited time during the platform beta". Expect some to now need DLAI Pro for graded labs.

### Gaps
- Not verified this session because the search budget ran out:
  - DLAI "ChatGPT Prompt Engineering for Developers" (Isa Fulford and Andrew Ng, 2023; known to exist but URL and status unverified).
  - OpenAI Academy agent content.
  - fast.ai.
  - The Full Stack Deep Learning LLM Bootcamp (2023; probably dated).
  - NVIDIA DLI agent courses.
- Exact lengths of the Anthropic Skilljar courses and the canonical Maven URL were not confirmed.

---

## 3. Training on AI for offensive security, pentesting and bug bounty (2025–2026)

### Takeaway
Most commercial AI-security training teaches **attacking AI systems** (prompt injection, RAG/agent abuse): PortSwigger, TCM, HTB AI Red Teamer, OffSec OSAI, SANS SEC536, Antisyphon, Arcanum "Attacking AI". Far fewer teach **building AI agents that hack**. The ones that do:
- SANS SEC535 (AI-automated recon and offensive AI tooling)
- Arcanum "Red Blue Purple AI" (using LLMs across security work)
- CanSecWest 2026 "Building Your Own Offensive Security Agentic Harness"
- The Packt and Manning offensive-agent books (see section 6)

A hackbot builder needs both: the first group to understand LLM attack surfaces, the second to build the bot.

### Cited Findings
**#30. SANS SEC535 "Offensive AI – Attack Tools and Techniques"**
- URL: https://www.sans.org/cyber-security-courses/offensive-ai-attack-tools-techniques
- 3-day instructor-led course covering guardrail bypass, **automated reconnaissance** and AI-driven malware. Aligned to the GIAC Offensive AI Analyst (GOAA) certification.
- Price: a single-user 12-month license on AWS Marketplace is $5,250. The Amsterdam May 2027 run is already waitlisted. One SANS page still says "coming soon".
- Level: I–A.
- [SANS SEC535](https://www.sans.org/cyber-security-courses/offensive-ai-attack-tools-techniques); [GIAC GOAA](https://www.sans.org/cyber-security-certifications/offensive-ai-analyst-goaa); [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-7ysn3pvobfolw)

**#31. SANS SEC536 "Adversarial AI – Penetration Testing AI Systems"**
- Bundled with SEC535 as **SEC537 "AI for Offensive Security"**, which gives two completion certificates.
- [SANS AI hub](https://www.sans.org/mlp/ai/); [SANS AI offensive security](https://www.sans.org/cyber-security-courses/ai-offensive-security)

**#32. SANS SEC545 "GenAI and LLM Application Security"**
- Defensive/build course: students build a GenAI app with a frontend, backend and Weaviate vector DB.
- 5 days instructor-led or 30 hours self-paced; 20 labs. Level: A.
- SEC495 is a separate, build-oriented LLM course with few details found.
- [SANS blog on SEC545](https://www.sans.org/blog/why-sec545-genai-and-llm-application-security)

**#33. Arcanum Information Security "Red, Blue, Purple AI" (and "RBP AI 2.0")**
- Jason Haddix. Two-day live class on using LLMs (GPT, Claude and others) across all major security domains; sold through Payhip.
- The 2.0 edition lists Dec 3 and Dec 5 sessions, 10am–5pm MST; the year was not shown.
- Whether it covers building autonomous agents is **unverified**. Haddix's public material (CT podcast Ep. 102, "micro agents") suggests it covers small task agents.
- [Payhip listing](https://payhip.com/b/GpkZv); [Payhip listing 2](https://payhip.com/b/JOzXy); [undercode blog, weak source](https://undercodetesting.com/red-blue-purple-ai-class-with-arcanum-security-a-deep-dive-into-ai-and-cybersecurity/)

**#34. Arcanum "Attacking AI"**
- Jason Haddix. Two-day training at OWASP Global AppSec USA, Nov 4–5, 2025.
- Also a free one-hour Antisyphon Anti-Cast, "Attacking AI v1.1" (July 9, 2025): Haddix's seven-point AI-app assessment methodology and the release of Arcanum's prompt-injection taxonomy.
- Talk version: "Attacking AI – Jason Haddix – NDC Security 2026".
- [OWASP sched](https://owaspglobalappsecusa2025.sched.com/event/268qk/2-day-training-attacking-ai); [Antisyphon Anti-Cast](https://www.antisyphontraining.com/event/anti-cast-attacking-ai-version-1-1-with-jason-haddix/)

**#35. Hack The Box Academy "AI Red Teamer" job-role path (built with Google, aligned to SAIF)**
- URL: https://academy.hackthebox.com/path/preview/ai-red-teamer
- Modules in the changelog: Fundamentals of AI; **Applications of AI in InfoSec**; Introduction to Red Teaming; Prompt Injection Attacks; LLM Output Attacks; AI Data Attacks; Attacking AI – Application and System; AI Evasion – Foundations. One article counts 12 modules now.
- Paid through an HTB Academy subscription. Level: I.
- [HTB path](https://academy.hackthebox.com/path/preview/ai-red-teamer); [HTB/Google blog](https://www.hackthebox.com/blog/google-ai-red-teamer); [HTB changelog](https://roadmap.hackthebox.com/changelog/full-ai-red-teamer-job-role-path-now-available)

**#36. HTB Certified Offensive AI Expert (HTB COAE)**
- Final assessment for the AI Red Teamer path: a 7-day practical engagement. Level: A.
- [HTB changelog](https://roadmap.hackthebox.com/changelog/introducing-the-new-htb-certified-offensive-ai-expert-htb-coae)

**#37. OffSec AI-300 "Advanced AI Red Teaming", leading to the OSAI ("OffSec AI Red Teamer") certification**
- Course planned for Spring 2026 and certification for Summer 2026. Attacks on GenAI, LLMs and multi-agent environments.
- 24-hour practical exam; from $1,749. OSAI does not expire; OSAI+ expires after 3 years.
- One OffSec page still read "coming". CODE BLUE lists tentative training on 2026-11-13 to 15.
- Level: A.
- [OffSec AI-300](https://offsec.com/courses/ai-300); [OffSec OSAI](https://www.offsec.com/courses/osai); [OffSec FAQ](https://help.offsec.com/hc/en-us/articles/46593095198740-OSAI-Advanced-AI-Red-Teaming-AI-300-FAQ); [OffSec Jan 2026 update](https://www.offsec.com/resources/product-updates/january-2026-content-platform-update/)
- OffSec also has an "LLM Red Teaming" learning path on the Learn Enterprise plan ([OffSec blog](https://www.offsec.com/blog/red-teaming-llm/)).

**#38. TCM Security "AI Hacking 101"**
- URL: https://tcm-sec.com/academy/ai-hacking-101/
- Self-paced video with guided labs: prompt injection, jailbreaks, exploiting RAG to leak data or PII.
- Recommended prereq: the free "AI Fundamentals: 100" (about 4 hours).
- Included in All-Access at $29.99/month. A one-day live version is listed at $699 through New Horizons.
- Companion defensive course: "Securing AI Applications". Level: B–I.
- [TCM AI Hacking 101](https://tcm-sec.com/academy/ai-hacking-101/); [TCM Securing AI Applications](https://tcm-sec.com/tcm-academy-course-release-securing-ai-applications/); [New Horizons](https://www.newhorizons.com/course-outline/courseid/300202015)

**#39. PortSwigger Web Security Academy, "Web LLM attacks" topic and learning path (Practitioner)**
- URLs: https://portswigger.net/web-security/llm-attacks and https://portswigger.net/web-security/learning-paths/llm-attacks
- Free. Labs use a live LLM, so responses vary between attempts.
- Labs include "Exploiting LLM APIs with excessive agency" and "Exploiting insecure output handling in LLMs" (indirect prompt injection leading to XSS). The lab count conflicts between 8 and 12.
- [PortSwigger topic](https://portswigger.net/web-security/llm-attacks); [learning path](https://portswigger.net/web-security/learning-paths/llm-attacks); [lab page](https://www.portswigger.net/web-security/llm-attacks/lab-exploiting-insecure-output-handling-in-llms)
- Why: free, and the best short hands-on intro to excessive agency, the core risk of your own hackbot's tool layer.

**#40. Antisyphon Training "Attacking and Defending AI/LLM Systems"**
- Brian Fehrman, Joff Thyer and Derek Banks. 8 hours, hands-on: adversarial prompts, guardrail bypass, abusing agentic and interpreter capabilities, RAG attacks, CTF. Mapped to the OWASP LLM Top 10, MITRE ATLAS and NIST AI RMF.
- A free Anti-Cast on the OWASP LLM Top 10 ran Feb 25, 2026.
- [Antisyphon product](https://www.antisyphontraining.com/product/attacking-and-defending-ai-with-brian-fehrman-joff-thyer-and-derek-banks/)

**#41. DEF CON Training 2026 "Strategic AI Penetration: Mastering Offensive Techniques for LLMs"**
- Marek Zmysłowski and Konrad Jędrzejczyk. Aug 10–11, 2026, Las Vegas, $3,500. Labs on PyRIT, garak, Llama Guard, and RAG and agent attacks.
- Repeated at Nullcon Berlin, Nov 2–4, 2026.
- [DEF CON Training](https://training.defcon.org/products/strategic-ai-penetration-mastering-offensive-techniques-for-llms-marek-zmyslowski-konrad-jedrzejczyk-dclv2026)

**#42. CanSecWest 2026 "AI-Powered Offensive Security: Building Your Own Offensive Security Agentic Harness"**
- 4-day in-person dojo, Sept 26–29, 2026. Listed as sold out; details marked not final.
- [CanSecWest registration](https://www.register.cansecwest.com/csw26/ai-powered-offensive-security-building-and-your-own-offensive-security-agentic-harness)
- Why: the only 2026 conference training found whose stated goal is building an offensive agent harness. Watch for a 2027 repeat.

**#43. AppSecEngineer "AI Agent Security Masterclass – 2026 Edition" (Black Hat USA 2026, Aug 1–2)**
- [AppSecEngineer](https://www.appsecengineer.com/workshop/blackhat-usa-2026-ai-agent-security-masterclass-2026-edition)

**#44. Learn Prompting "AI Red Teaming & AI Security Masterclass" (Maven)**
- Exists; details not verified. [Maven](https://maven.com/learn-prompting-company/ai-red-teaming-and-ai-safety-masterclass)

### Inferences
- If the budget allows only one paid offensive-AI course for a builder: SANS SEC535 is the only major-vendor course that explicitly teaches *using* AI offensively (automated recon). OffSec OSAI, HTB COAE and SANS SEC536 teach *attacking* AI.
- For a low budget: PortSwigger Web LLM attacks (free), TCM AI Hacking 101 ($29.99/month), and Haddix's free Anti-Cast give most of the attack-surface knowledge.

### Gaps
- Not verified (search budget): TryHackMe AI rooms, NVIDIA DLI security-AI courses, Pentester Academy, and well-rated Udemy AI-pentest courses.
- I could not find official Black Hat USA 2026 training titles on building pentest agents in the official catalog. Only aggregator coverage exists, e.g., [NCC Group recap](https://www.nccgroup.com/research/postcards-from-las-vegas-blackhat-usa-2026-and-defcon-34/).
- The exact syllabi for Arcanum RBP AI and SANS SEC535, specifically whether agent-building labs are included, are unverified.

---

## 4. Pentest and bug-bounty fundamentals a hackbot builder must know

### Takeaway
A hackbot is only as good as the methodology you encode. The free PortSwigger Web Security Academy plus three No Starch books (Bug Bounty Bootcamp, Real-World Bug Hunting, Hacking APIs) remain the core reading.

### Cited Findings
**#45. PortSwigger Web Security Academy (all labs)**
- URL: https://portswigger.net/web-security/all-labs. Free; levels Apprentice, Practitioner and Expert.
- [PortSwigger all-labs, referenced](https://portswigger.net/web-security/all-labs); [community lab index](https://github.com/hanzalaghayasabbasi/portswigger-all-labs)
- Why: lab descriptions double as ready-made eval tasks for a hackbot.

**#46. *Bug Bounty Bootcamp: The Guide to Finding and Reporting Web Vulnerabilities***
- Vickie Li (No Starch, Dec 7, 2021, 416 pp). Recon, vulnerability classes, exploitation; more recon automation than Yaworski's book.
- [O'Reilly listing](https://www.oreilly.com/library/view/bug-bounty-bootcamp/9781098129088/); [systemoverlord review](https://systemoverlord.com/2021/11/05/book-review-bug-bounty-bootcamp.html)

**#47. *Real-World Bug Hunting: A Field Guide to Web Hacking***
- Peter Yaworski (No Starch, July 2019, 264 pp). Real paid-report examples per vulnerability class.
- [Bookshare](https://bookshare.org/browse/book/6237451)

**#48. *Hacking APIs: Breaking Web Application Programming Interfaces***
- Corey J. Ball (No Starch, July 2022, 368 pp). Lab-based REST API testing.
- [Security Boulevard](https://securityboulevard.com/2022/09/5-books-every-api-hacker-should-read)

**#49. HTB Academy job-role paths / HTB CWES**
- HTB notes the transition from the CBBH certification to CWES, which signals the bug-bounty path was renamed or reworked.
- [HTB Academy banner on path page](https://academy.hackthebox.com/path/preview/ai-red-teamer)

### Inferences
- Read Bug Bounty Bootcamp first (methodology and recon), then Hacking APIs (modern targets are API-heavy), then Real-World Bug Hunting (case studies, good few-shot material). *The Web Application Hacker's Handbook* (2011) is dated; use the PortSwigger Academy instead.

### Gaps
- HackerOne Hacker101, Bugcrowd University, the OWASP WSTG and TryHackMe paths were not verified this session (search budget). All are well-known free resources, but their URLs and current status are unconfirmed here.

---

## 5. YouTube videos, channels, conference talks and podcasts

### Takeaway
Must-watch talks on agent engineering:
- Karpathy: Intro to LLMs, Deep Dive, How I Use LLMs, Software 3.0.
- Barry Zhang (Anthropic), "How We Build Effective Agents".
- Dex Horthy, "12-Factor Agents" (origin of the "context engineering" framing).

For security plus AI, the densest material is the Critical Thinking Bug Bounty Podcast hackbot episodes (Joseph "rez0" Thacker, Justin Gardner), Jason Haddix's talks, XBOW's write-ups, and the DARPA AIxCC DEF CON 33 materials.

### Cited Findings
**Agent and LLM engineering**

**#50. Andrej Karpathy, "Software Is Changing (Again)"**
- YC AI Startup School keynote, June 17/18, 2025, about 39 minutes. Introduces the Software 1.0/2.0/3.0 framing.
- YouTube: https://www.youtube.com/watch?v=LCEmiRjPEtQ (ID taken from the transcript page URL).
- [Gigazine](https://gigazine.net/gsc_news/en/20250620-andrej-karpathy-software-is-changing); [transcript](https://cdn.billmill.org/static/yt-transcribe/karpathy/httpswwwyoutubecomwatchvLCEmiRjPEtQ.html)

**#51. Karpathy, "Deep Dive into LLMs like ChatGPT"**
- Feb 2025; about 3.5 hours (listed durations conflict). Level: B–I.
- [Analytics Vidhya / aggregator](https://www.beri.net/learning/karpathy-deep-dive-into-llms-like-chatgpt); [summary](https://lawrencewu.net/posts/2025-02-24-karpathy-deep-dive-llms/)
- YouTube https://www.youtube.com/watch?v=7xTGNNLPyMI **[from memory, not verified this session]**

**#52. Karpathy, "How I use LLMs"**
- Feb 27, 2025. Practical tool use: search, deep research, Python interpreter.
- [Analytics Vidhya](https://www.analyticsvidhya.com/blog/2025/03/how-andrej-karpathy-uses-llms/)
- YouTube https://www.youtube.com/watch?v=EWvNQjAaOHw **[from memory, not verified this session]**

**#53. Karpathy, "[1hr Talk] Intro to Large Language Models"**
- Nov 23, 2023. Its "LLM security" section (jailbreaks, prompt injection) is still useful.
- [summify channel listing, aggregator](https://summify.io/discover/channels/andrej-karpathy)
- YouTube https://www.youtube.com/watch?v=zjkBMFhNj_g **[from memory, not verified this session]**

**#54. Barry Zhang (Anthropic), "How We Build Effective Agents"**
- AI Engineer Summit 2025, NYC; about 15 minutes. When not to build agents, keep it simple, think like the agent. Agent = model using tools in a loop.
- [ai.engineer talk page](https://ai.engineer/talks/effective-ai-agents); [alt page with YouTube ID D7_ipDqhtwk](https://ai.engineer/talks/D7_ipDqhtwk-effective-ai-agents)
- YouTube URL inferred from the slug: https://www.youtube.com/watch?v=D7_ipDqhtwk

**#55. Dex Horthy (HumanLayer), "12-Factor Agents: Patterns of reliable LLM applications"**
- AI Engineer World's Fair, June 3, 2025; about 17 minutes. Chapters include control flow, pause/resume, prompts and context, micro-agents, stateless agents.
- Repo: github.com/humanlayer/12-factor-agents.
- [ai.engineer](https://ai.engineer/talks/12-factor-agents); [MLOps Community re-recording](https://home.mlops.community/public/videos/12-factor-agents-patterns-of-reliable-llm-applications-dexter-horthy-agents-in-production-2025-2025-08-06)
- Follow-ups on context engineering: [Pragmatic Engineer episode](https://newsletter.pragmaticengineer.com/p/context-engineering-with-dex-horthy) and [Chroma interview](https://www.trychroma.com/interviews/dex-horthy-context-engineering-ep-1).
- Why: "micro-agents inside deterministic software" is the architecture most successful hackbots converge on.

**#56. Michael Albada, "Building Applications with AI Agents"**
- AI Engineer World's Fair 2025; a distillation of his O'Reilly book (#73). [ai.engineer](https://ai.engineer/talks/building-applications-with-ai-agents)

**#57. "Building Claude Code with Boris Cherny"**
- Pragmatic Engineer podcast; origins and design of Claude Code. [Pragmatic Engineer](https://newsletter.pragmaticengineer.com/p/building-claude-code-with-boris-cherny)
- His AI Engineer talk: [speaker page](https://www.ai.engineer/speakers/boris-cherny)

**#58. Hamel Husain & Shreya Shankar, "Why AI evals are the hottest new skill for product builders"**
- Lenny's Podcast. [Lenny's Newsletter](https://www.lennysnewsletter.com/p/why-ai-evals-are-the-hottest-new-skill)

**#59. Harrison Chase, "Long-term memory with LangGraph" (AI Dev 25)**
- [CO/AI](https://getcoai.com/video/ai-dev-25-harrison-chase-long-term-memory-with-langgraph/)

**Security and AI**

**#60. Critical Thinking – Bug Bounty Podcast**
- Hosts: Justin Gardner (Rhynorater); Joseph "rez0" Thacker (co-host since Jan 2025); Brandyn Murtagh (since Oct 2025). Episodes most relevant to hackbots:
  - Ep. 102 "Building Web Hacking Micro Agents" (with Jason Haddix: fuzzing, WAF bypass, report writing)
  - Ep. 117 "Hacking AI Series: Vulnus ex Machina – Part 1"
  - Ep. 129 "Is this how Bug Bounty Ends?" (hackbots and human-AI collaboration; links HackerOne's "Welcome, Hackbots" post)
  - Ep. 137 "How We Do AI-Assisted Whitebox Review, New CSPT Gadgets, and Tools from SLCyber"
  - Ep. 165 "Protobuf Hacking, AI-Powered Bug Hunting, and Self-Improving Claude Workflows"
  - Ep. 173 "Bug Bounty is Dead and AI Killed It"
  - Ep. 175 "Rhyno's Hackbot Setup, Sick Bugs, and ZDI Drama" (May 2026)
  - Ep. 181 (Joseph and XSSDoctor on building a hackbot)
- [guptadeepak overview](https://guptadeepak.com/podcasts/critical-thinking-bug-bounty/); [Ep 137](https://pocketcasts.com/podcasts/946adcc0-728b-013b-f272-0acc26574db2/6ec2066c-8710-4117-8241-d42017e04b51); [Ep 129](https://pocketcasts.com/podcasts/946adcc0-728b-013b-f272-0acc26574db2/0e377146-7294-40c5-a086-1c09563479dd); [Ep 175](https://www.castfox.net/podcast/critical-thinking-bug-bounty-podcast-5951018/episode/episode-175-rhynos-hackbot-setup-sick-bugs-and-zdi-drama-1681968); [Ep 165](https://www.castfox.net/podcast/critical-thinking-bug-bounty-podcast-5951018/episode/episode-165-protobuf-hacking-ai-powered-bug-hunting-and-self-improving-claude-workflows-462101); [Ep 173](https://www.castfox.net/podcast/critical-thinking-bug-bounty-podcast-5951018/episode/episode-173-bug-bounty-is-dead-and-ai-killed-it-1450621); [Ep 117](https://metacast.app/podcast/critical-thinking-bug-bounty-podcast/RxFQfeif/episode-117-hacking-ai-series-vulnus-ex-machina-part-1/koRkMlzG)
- Episode numbering and dates vary slightly across directories.
- Why: the most practitioner-grounded discussion of real hackbot setups anywhere.

**#61. Jason Haddix, "Red, Blue, and Purple AI" keynote**
- OWASP Global AppSec SF, 2024. YouTube https://www.youtube.com/watch?v=XHeTn7uWVQM (link from the CT Ep. 102 show notes, as relayed by search).
- [OWASP sched](https://owasp2024globalappsecsanfra.sched.com/event/1g3UA/red-blue-and-purple-ai-keynote)

**#62. Haddix with NetworkChuck: "Hacking AI is TOO EASY" series and "become an AI HACKER (it's easier than you think)"**
- The series includes the self-hostable Agent Breaker CTF.
- YouTube https://www.youtube.com/watch?v=Qvx2sVgQ-u0 and https://www.youtube.com/watch?v=_yfiUQSbdPY **[IDs from search summaries and aggregator pages; weak]**
- [videohighlight, aggregator](https://videohighlight.com/v/_yfiUQSbdPY); [DEV write-up](https://dev.to/xenocoregiger31/autonomous-pentesting-is-closer-than-expected-jason-haddix-on-where-the-ceiling-actually-is-420e)

**#63. Joseph "rez0" Thacker, "AI and hacking – opportunities and threats"**
- YouTube ID zY7dz4Dx5tc **[from a summarizer URL; weak]**. [chaindesk summary](https://www.chaindesk.ai/tools/youtube-summarizer/ai-and-hacking-opportunities-and-threats-joseph-rez0-thacker-zY7dz4Dx5tc)
- His blog feed: [josephthacker.com](https://josephthacker.com/feed)

**#64. XBOW material**
- "Black Hat and DEF CON 2025 recap" (Aug 13, 2025; XBOW was run live against real targets): [XBOW blog](https://xbow.com/blog/black-hat-2025)
- "XBOW on HackerOne: what's next" (Aug 18, 2025; XBOW is ending its leaderboard run): [XBOW blog](https://xbow.com/blog/xbow-on-hackerone-whats-next)
- Bloomberg on XBOW topping HackerOne's US leaderboard after 1,000+ reports: [Bloomberg](https://www.bloomberg.com/news/articles/2025-06-24/one-of-the-best-hackers-in-the-country-is-an-ai-bot)
- Nico Waisman, "Finding Large Bounties with Large Language Models", Application Security Weekly #351: [SC World](https://www.scworld.com/podcast-episode/asw-351)
- Sceptical counterpoint: [LessWrong, "Be skeptical of milestone announcements by young AI startups"](https://www.lesswrong.com/posts/qefrWyeiMvWEFRitN/be-skeptical-of-milestone-announcements-by-young-ai-startups)

**#65. DARPA AI Cyber Challenge (AIxCC) finals, DEF CON 33 (Aug 2025)**
- Team Atlanta won ($4M), Trail of Bits second ($3M), Theori third ($1.5M). Fully autonomous vulnerability discovery and patching.
- DEF CON 33 video-team captions for the "DARPA AIxCC" session are on media.defcon.org.
- [DARPA](https://darpa.mil/news/2025/ai-cyber-challenge-winners-def-con-33); [Infosecurity Magazine](https://www.infosecurity-magazine.com/news/defcon-ai-cyber-challenge-winners/); [DARPA podcast with Taesoo Kim](https://www.darpa.mil/news/podcast/aixcc-challenge-89); [DEF CON media captions](https://media.defcon.org/DEF%20CON%2033/DEF%20CON%2033%20video%20team/DEF%20CON%2032%20video%20team%20captions/DEF%20CON%2033%20-%20Video%20Team%20-%20DARPA%20AIxCC.eng.txt); [Georgia Tech story](https://cc.gatech.edu/news/google-cybersecurity-team-inspired-georgia-techs-aixcc-win); [AIxCC site](https://aicyberchallenge.com/)
- Why: the most rigorous public evidence on cyber reasoning system architecture. Watch the team talks and read the CRS write-ups.

**#66. Black Hat USA 2026 and DEF CON 34 (Aug 2026)**
- Reports describe heavy agent-security content and the debut of HalCTF, billed as the first public autonomous AI security competition, at DEF CON 34.
- [NCC Group recap](https://www.nccgroup.com/research/postcards-from-las-vegas-blackhat-usa-2026-and-defcon-34/); [Straiker talk guide](https://www.straiker.ai/blog/black-hat-usa-2026-ai-security-talks)
- Talk counts conflict (35 of 121 briefings versus "7+ in the AI track").

### Inferences
- For hackbot engineering, the highest-yield viewing order: Zhang (#54), then Horthy (#55), then CT Ep. 102/129/175/181 (#60), then the XBOW posts and AIxCC materials (#64, #65).
- Karpathy's videos are background, not requirements, for someone who already ships LLM code.

### Gaps
- Not verified this session (search budget): exact URLs for the AI Engineer YouTube channel, the LangChain and Anthropic YouTube channels, Sam Witteveen, Dave Ebbelaar, IndyDevDan, NahamSec AI videos, Daniel Miessler, John Hammond, LiveOverflow, STÖK, DEF CON 33 AI Village talks, and XBOW's own Black Hat/DEF CON talk recordings.
- The "Prompting for Agents | Code w/ Claude" talk: the request named Hannah Moran and Jeremy Hadfield, but the only listing found describes an Alex Albert / Erik Schluntz conversation ([neura.market directory, weak](https://www.neura.market/directories/claude/videos/%2Fde%2Fprompting-101-code-with-claude)). Speakers unresolved.
- An Anthropic workshop titled "Build agents that run for hours" (Ash Prabaker, Andrew Wilson) appeared only on a spam-like aggregator. Unverified.

---

## 6. Books (2024–2026)

### Takeaway
The core shelf:
- Chip Huyen, *AI Engineering* (evals, RAG, agents, finetuning).
- Berryman & Ziegler, *Prompt Engineering for LLMs* (context assembly; Ziegler is now Head of AI at XBOW).
- One agent-architecture book: Albada (O'Reilly 2025) or Lanham, *AI Agents in Action, 2nd ed.* (Manning, 2026, covers MCP and A2A).

For hackbot-specific reading, two 2026 titles exist: Yildirim, *Agentic AI for Offensive Cybersecurity* (Packt, Feb 2026), and Foudy, *AI Agents for Offensive Security* (Manning MEAP, final early 2027).

### Cited Findings
**#67. *AI Engineering: Building Applications with Foundation Models***
- Chip Huyen (O'Reilly; Dec 2024 per O'Reilly, Jan 2025 print; ISBN 9781098166304; about 500+ pp). Chapters cover evaluation methodology, AI system evaluation, prompt engineering, RAG and agents, and finetuning. Called the most-read O'Reilly title of 2025. Level: I.
- [O'Reilly](https://www.oreilly.com/library/view/ai-engineering/9781098166298/afterword01.html); [MIT Press Bookstore](https://mitpressbookstore.mit.edu/book/9781098166304); [SuperDataScience](https://superdatascience.com/999)

**#68. *Prompt Engineering for LLMs: The Art and Science of Building LLM-Based Applications***
- John Berryman & Albert Ziegler (O'Reilly, Nov 2024, 282 pp; ISBN 9781098156145 ebook / 9781098156152 print). Both were early GitHub Copilot engineers; **Ziegler is Head of AI at XBOW**. Level: I.
- [O'Reilly](https://www.oreilly.com/library/view/-/9781098156145); [MIT Press Bookstore](https://mitpressbookstore.mit.edu/book/9781098156152)
- Why: the best treatment of context assembly, which is what context engineering means in practice.

**#69. *AI Agents in Action, Second Edition***
- Micheal Lanham (Manning, June 2026; ISBN 9781633434530; about 392 pp; retailer listings vary). A near-total rewrite covering personas, tools, memory, retrieval, reasoning, evaluation, multi-agent patterns, **MCP** and **A2A**. Level: I.
- [Manning](https://manning.com/books/ai-agents-in-action-second-edition); [O'Reilly](https://www.oreilly.com/library/view/ai-agents-in/9781633434530/)

**#70. *Building Agentic AI Systems***
- Anjanava Biswas & Wrick Talukdar (Packt, Apr 2025; ISBN 9781803238753). Reflection, introspection, tool use, planning, multi-agent coordination. Code on GitHub. Small rating sample (3.8/5 from 4 reviews). Level: I.
- [Packt](https://www.packtpub.com/en-us/product/9781803238753); [GitHub](https://github.com/PacktPublishing/Building-Agentic-AI-Systems)

**#71. *Hands-On Large Language Models: Language Understanding and Generation***
- Jay Alammar & Maarten Grootendorst (O'Reilly, Sept 2024; about 400–428 pp; ISBN 9781098150952). Visual, code-first intro; official code in the HandsOnLLM GitHub repo. Level: B–I.
- [O'Reilly](https://www.oreilly.com/library/view/-/9781098150952)

**#72. *Build a Reasoning Model (From Scratch)***
- Sebastian Raschka (Manning, mid-2026; ISBN 9781633434677). Inference-time scaling (self-consistency, self-refinement), RL with verifiable rewards, distillation. Follow-on to his *Build a Large Language Model (From Scratch)* (Manning, 2024). Level: A.
- [i-programmer](https://www.i-programmer.info/book-watch-archive/19064-build-a-reasoning-model-from-scratch-manning.html); [Tenlong](https://www.tenlong.com.tw/products/9781633434677)

**#73. *Building Applications with AI Agents: Designing and Implementing Multiagent Systems***
- Michael Albada (O'Reilly, Sept/Oct 2025, 354 pp; ISBN 9781098176495 / 9781098176501). Single- and multi-agent design trade-offs. Level: B–I.
- [O'Reilly](https://www.oreilly.com/library/view/building-applications-with/9781098176495/); [O'Reilly Radar](https://www.oreilly.com/radar/building-applications-with-ai-agents)

**#74. *The Developer's Playbook for Large Language Model Security***
- Steve Wilson, lead of the OWASP Top 10 for LLM Apps (O'Reilly, Sept 2024, 200 pp; ISBN 9781098162207). Trust boundaries, prompt injection, OWASP LLM Top 10. Level: I.
- [O'Reilly](https://oreilly.com/library/view/the-developers-playbook/9781098162191); [MIT Press Bookstore](https://mitpressbookstore.mit.edu/book/9781098162207)

**#75. *AI Agents for Offensive Security***
- Mark Foudy (Manning; MEAP from April 2026; final edition estimated early 2027; ISBN 9781633434172). AI agents and multi-agent pipelines for recon, triage, vulnerability discovery, reporting and agentic pentesting. Level: I–A.
- [Manning](https://manning.com/books/ai-agents-for-offensive-security); [liveBook](https://livebook.manning.com/book/ai-agents-for-offensive-security); [author page](https://www.manning.com/authors/mark-foudy)
- Why: the most on-target hackbot book found. Its MEAP status means chapters are still changing.

**#76. *Agentic AI for Offensive Cybersecurity: Build and automate smarter penetration testing workflows using AI-driven agents***
- Orhan Yildirim (Packt, Feb 2026, 412 pp; ISBN 9781806114467 ebook / 9781806114474 print). End-to-end agentic pentest workflows with **n8n and MCP**, human in the loop. Level: I.
- [Packt](https://www.packtpub.com/en-us/product/agentic-ai-for-offensive-cybersecurity-9781806114467); [O'Reilly](https://www.oreilly.com/library/view/-/9781806114474/)

**#77. *Penetration Testing Using Artificial Intelligence* (2026, about 794 pp)**
- Covers adversarial ML, LLM red teaming and GenAI social engineering. **Publisher not confirmed**; treat with caution.
- [retailer listing](https://andoffice.com/products/penetration-testing-using-artificial-intelligence-methods-to/231874362/)

**#78. *Generative AI for Hackers: Hands-on Cyber Operations in the Cybersecurity Era***
- Self-published on Leanpub; about 90% complete as of the listing. Quality unknown.
- [Leanpub](https://leanpub.com/generativeaiforhackershands-oncyberoperationsinthecybersecurityera)

**Pentest fundamentals books:** #46, #47, #48 in section 4.

### Inferences
- *Prompt Engineering for LLMs* has an unusually direct link to the hackbot field because co-author Ziegler leads AI at XBOW.
- The two offensive-agent books differ in approach. Packt/Yildirim is low-code (n8n plus MCP). Manning/Foudy is code-first multi-agent pipelines, which better suits someone building a custom harness.

### Gaps
- Not re-verified this session (search budget): *Build a Large Language Model (From Scratch)* edition details; *Building LLM-Powered Applications* (Valentina Alto, Packt); *Adversarial AI Attacks, Mitigations, and Defense Strategies* (Packt, 2024); the *LLM Engineer's Handbook*. I found no "Hacking with AI" No Starch title.
- Reader reviews for #75–#78 are sparse or missing.

---

## 7. Recommended sequence (courses, videos, books)

### Takeaway
Run two tracks in parallel and merge them in Phase 4. Track A is agent engineering; Track B is offensive-security fundamentals. Total is roughly 4–6 months part-time if the free items are taken and paid ones are optional.

### Cited Findings
The sequence uses only the items catalogued above (sources as cited in sections 1–6).

**Phase 0: Orientation (about 1 week)**
- Watch: Karpathy "Intro to LLMs" (#53) and "Software Is Changing (Again)" (#50).
- Then: Barry Zhang "How We Build Effective Agents" (#54) and Dex Horthy "12-Factor Agents" (#55).

**Phase 1: Agent fundamentals and prompting/context (2–3 weeks)**
- Prompting: anthropics/courses prompt tutorial and tool-use course (#25, #26).
- Anthropic Skilljar "Building with the Claude API" (#21).
- DLAI "Agentic AI" (#10).
- Read: Berryman & Ziegler (#68), and Huyen chapters on prompting and agents (#67).

**Phase 2: Harness, graphs, MCP, subagents (3–4 weeks)**
- Graphs: DLAI "AI Agents in LangGraph" (#15), then LangChain Academy "Intro to LangGraph" (#19), then "Intro to Deep Agents" (#20).
- MCP: DLAI MCP course (#11), or the HF MCP course (#18) and Anthropic "Intro to MCP" / "MCP Advanced Topics" (#23, #24).
- Claude Code as a reference harness: Claude Code in Action (#22) or the DLAI Claude Code course (#12).
- Memory and multi-agent: DLAI memory (#14) and crewAI (#16) short courses.
- Optional: HF Agents Course (#17) for framework breadth; Microsoft AI Agents for Beginners (#27).

**Phase 3: Evals and observability (2 weeks, then ongoing)**
- DLAI "Evaluating AI Agents" (#13).
- Huyen's evaluation chapters (#67).
- Hamel & Shreya interview (#58).
- If budget allows, the Maven AI Evals cohort (#29).
- Use PortSwigger labs (#45) as an eval corpus for the bot.

**Track B, in parallel from Phase 1: offensive fundamentals**
- PortSwigger Web Security Academy (#45).
- Bug Bounty Bootcamp (#46), then Hacking APIs (#48), then Real-World Bug Hunting (#47).
- Optional: HTB Academy paths (#49).

**Phase 4: AI attack surface, attacking and defending agents (2–3 weeks)**
- PortSwigger "Web LLM attacks" (#39).
- TCM "AI Hacking 101" (#38).
- Haddix's Anti-Cast "Attacking AI" (#34).
- Steve Wilson's playbook (#74).
- Optional depth: HTB AI Red Teamer path, including "Applications of AI in InfoSec" (#35).

**Phase 5: Building the hackbot (4–6 weeks)**
- Read Foudy, *AI Agents for Offensive Security* (MEAP) (#75) and/or Yildirim, *Agentic AI for Offensive Cybersecurity* (#76).
- Listen to CT Podcast Ep. 102, 129, 137, 165, 175 and 181 (#60).
- Study XBOW posts and the Waisman podcast (#64) and the AIxCC finals materials (#65).
- Follow CMU 11-768 recordings: harness from scratch, sandboxing, evals (#4).

**Phase 6: Advanced and research (ongoing)**
- Berkeley Advanced LLM Agents SP25 (#2), especially the code-vulnerability lectures.
- Berkeley Agentic AI F25 safety/security lecture (#3).
- Stanford CS329A (#8) for verifiers and self-improvement.
- Raschka's reasoning-model book (#72) or CS336 (#7) if training or fine-tuning your own models.

**Optional paid credentials**
- SANS SEC535 (#30), the only one centred on using AI offensively.
- OffSec OSAI (#37), HTB COAE (#36), or a conference course like CanSecWest's agentic-harness dojo (#42) or DEF CON's Strategic AI Penetration (#41).

### Inferences
- Phases 2, 3 and 5 are the critical path. Most hackbot failures come from weak harness/context design and missing evals, not model choice. This fits Horthy's "mostly deterministic software with LLM steps" thesis (#55) and Zhang's "keep it simple" guidance (#54).
- Do Track B and Phase 4 before Phase 5. Encoding methodology needs domain knowledge, and understanding excessive agency and prompt injection (#39) is needed to keep your own bot safe when it reads attacker-controlled content.

### Gaps
- Time estimates per phase are my judgment, not sourced.
- Because live-page verification was impossible and the search budget ran out, re-check prices, cohort dates and lesson counts (DLAI Pro status, Maven price, OffSec OSAI availability, SANS dates) before publishing.
