# 📘 Claude AI & Claude Code — Comprehensive Technical Guide

> This is the master technical reference for the `Claude-Code-Mastery` repository. It is provided in two complete passes: a full **English** edition followed by a full **Roman Urdu** edition. Both cover the same five pillars: History, Technical Architecture, Setup & Pricing, Skill-Building Roadmap, and Hook-Based Security.

---

# 🇬🇧 PART 1 — ENGLISH EDITION

## 1. 🕰️ History — Who Built This, and When

**Anthropic** is the company behind Claude and Claude Code. It was founded in **2021** by **Dario Amodei** and **Daniela Amodei** (siblings), together with a group of former OpenAI researchers, with AI safety positioned as a core part of the company's mission rather than an afterthought.

A rough timeline of the products this repo documents:

| Date | Milestone |
|---|---|
| **2021** | Anthropic is founded. |
| **March 2023** | The first Claude model and Claude Instant are announced; the Anthropic API opens for early access. |
| **July 2023** | Claude 2 launches, alongside the public `claude.ai` chat interface. |
| **March 2024** | The Claude 3 family (Opus, Sonnet, Haiku) launches as Anthropic's first three-tier model lineup. |
| **June 2024** | Claude 3.5 Sonnet launches. |
| **February 2025** | Claude 3.7 Sonnet introduces hybrid reasoning (extended thinking) in a production model. |
| **February 24, 2025** | **Claude Code** launches as a limited **research preview** — a terminal-based agentic coding tool, initially bundled with Claude 3.7 Sonnet. |
| **May 2025** | Claude 4 (Opus 4 / Sonnet 4) launches; Claude Code becomes generally available and is added to the Pro plan. |
| **Mid–late 2025** | MCP support, subagents, remote MCP servers, and Claude Code on the Web all roll out. |
| **2026** | Continued rapid iteration across the Sonnet, Opus, and Haiku lines, plus ecosystem growth (Agent SDK, Managed Agents, deeper IDE integrations). |

> ⚠️ **Important note on versions:** Anthropic ships new model versions frequently. Rather than hard-coding a specific version number in this guide (which will go stale), always check **[anthropic.com/news](https://www.anthropic.com/news)** and **[docs.claude.com](https://docs.claude.com)** for the current model lineup before making purchasing or architecture decisions.

---

## 2. 🔬 Technical Deep-Dive — How Claude Code Actually Works

Claude Code is best understood as **three components working together**, not a single monolithic "AI app."

### 2.1 The Reasoning Engine (LLM)
At the core sits a **Claude model** (from the Haiku, Sonnet, or Opus family). This is the part that:
- Reads and understands your natural-language instruction.
- Builds a mental model of your codebase by reasoning over file contents it requests.
- Plans a multi-step approach — which files to touch, in what order, and why.
- Writes the actual code, commands, and explanations.

The LLM itself has **no inherent ability to touch your disk or run anything**. It only produces text. Everything it "does" in your project happens through the next layer.

### 2.2 Terminal & Tool Access (The Agent Loop)
Claude Code wraps the model in an **agent loop** that gives it real capabilities:
- **File tools** — read, write, edit, and search files in your working directory.
- **Bash/shell access** — run build commands, tests, linters, git operations.
- **Iteration** — after running a command, Claude Code feeds the *result* (stdout/stderr, test output, errors) back to the model, which then decides its next step.

This read → plan → act → observe → re-plan cycle is what makes it "agentic" rather than a one-shot autocomplete. Crucially, by default Claude Code **asks for permission** before file-modifying or potentially destructive actions — autonomy is a dial the developer controls, not an all-or-nothing switch.

### 2.3 MCP — Model Context Protocol (The Connector Layer)
**MCP** is an open protocol that lets Claude Code (and other AI applications) connect to external tools and data sources in a standardized way — instead of every integration being a one-off, bespoke API wrapper.

In practice, MCP servers expose tools to Claude such as:
- Reading issues from a project tracker.
- Querying a database.
- Searching internal documentation.
- Posting messages to a team chat tool.

Architecturally, an MCP server sits **alongside** Claude Code and offers a menu of callable tools; the LLM decides *when* to call them based on the conversation, and the agent loop executes the call and returns the result — the same observe-and-react cycle as terminal tools, just extended to anything that speaks MCP.

### 2.4 Putting It Together
A single Claude Code request might flow like this:
1. You type a natural-language instruction.
2. The **LLM** reasons about what's needed and proposes a plan.
3. The **agent loop** executes file reads/edits and shell commands to gather context and make changes.
4. If the task needs outside data, the LLM calls an **MCP tool** (e.g., "fetch the related ticket").
5. Results flow back to the LLM, which adjusts its plan and continues until the task is done or it hands control back to you.

---

## 3. 💰 Setup & Pricing — Official Plans vs. Ollama (Free, Local)

### 3.1 Official Pricing Model (Subscription + API)

Claude Code is **not priced as a separate standalone product** — it draws on whichever Claude access you already have: a consumer subscription or pay-as-you-go API credits. As of mid-2026, the general shape of official pricing is:

| Tier | Approx. Price | Notes |
|---|---|---|
| **Free (Claude.ai chat)** | $0 | Chat only — **does not** include Claude Code / terminal access. |
| **Pro** | ~$20/month | Entry point for individual developers; includes Claude Code with a pooled usage budget. |
| **Max 5x** | ~$100/month | Higher usage ceiling for daily, heavy Claude Code use. |
| **Max 20x** | ~$200/month | For power users running long sessions, multiple agents, or large codebases. |
| **Team (Premium seats)** | ~$100–125/seat/month | Claude Code access is typically gated to the higher "Premium" seat tier; minimum seat counts usually apply. |
| **Enterprise** | Custom | Larger context windows, compliance/SSO features, custom contracts. |
| **API (pay-as-you-go)** | Per-token | No monthly fee; billed by input/output tokens consumed. Useful for CI pipelines and automation, less economical for sustained interactive daily use. |

> 💡 **Always verify current numbers** at **[anthropic.com/pricing](https://www.anthropic.com/pricing)** — token rates and plan tiers are revised periodically, and this table will age.

**Rule of thumb:** if you use Claude Code for a few focused hours most days, a subscription tier is usually cheaper than paying per token. If your usage is bursty or you're running it inside automated pipelines (e.g., GitHub Actions), API billing gives better cost visibility.

### 3.2 The Pakistan Problem: USD Pricing vs. PKR Income

For developers earning in PKR, a recurring USD subscription is a real cost barrier — exchange-rate exposure, international card requirements, and the simple fact that $20–200/month is a very different proportion of income locally versus in the US/EU. This is precisely why the **Ollama local route** below matters for this community: it is not a "nice to have," it's often the realistic entry point.

### 3.3 Free, Local Usage via Ollama

**Ollama** is an open-source runtime for downloading and running large language models entirely on your own machine — no API costs, no internet dependency once the model is downloaded, and no data leaving your hardware.

As of early 2026, Ollama added **native support for the Anthropic Messages API**, which means Claude Code's CLI can talk to Ollama-served open models **directly**, without a third-party translation proxy, by pointing Claude Code's environment variables at your local Ollama server.

**Typical setup:**

```bash
# 1. Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# 2. Pull a coding-capable open model
ollama pull qwen2.5-coder:32b   # or a smaller variant if VRAM/RAM is limited

# 3. Point Claude Code at your local Ollama server
export ANTHROPIC_AUTH_TOKEN=ollama
export ANTHROPIC_API_KEY=""
export ANTHROPIC_BASE_URL=http://localhost:11434

# 4. Launch
claude
```

**Older / alternative method (still widely used for non-Anthropic-compatible local servers):** route through **LiteLLM**, a proxy that translates Claude Code's Anthropic-format requests into the OpenAI-compatible format that many local servers (older Ollama versions, LM Studio, vLLM) expect.

| Factor | Official Cloud (Sonnet/Opus) | Ollama (Local) |
|---|---|---|
| Cost | Subscription or per-token | Free after hardware/electricity |
| Code quality | State-of-the-art, best for complex multi-file reasoning | Good for simpler tasks; weaker on long-horizon, complex reasoning |
| Privacy | Data sent to Anthropic's API | Stays entirely on your machine |
| Hardware needs | None (cloud-hosted) | Needs a capable GPU/CPU + RAM (16–32GB+ recommended for larger models) |
| Context window | Large, production-grade | Often smaller on local models — can drop context on big codebases |
| Best use case | Production work, client deliverables, complex refactors | Learning, prototyping, offline work, privacy-sensitive experimentation |

**Practical recommendation for Pakistani developers:** learn the *workflow* (prompting patterns, CLAUDE.md conventions, hooks, MCP) for free using Ollama, then selectively use a Pro subscription or API credits for client-facing or genuinely complex work where model quality directly affects billable outcomes.

---

## 4. 🛣️ Skill-Building Roadmap — Becoming a Claude Code Expert

### Level 1 — Foundations
- Install Claude Code and authenticate (subscription or API key).
- Learn the basic loop: instruct → review diff → approve/reject → iterate.
- Write a `CLAUDE.md` file in your project root — project-specific conventions, build commands, and style rules the agent should always follow.
- Get comfortable with plan mode (review a proposed approach before execution) versus direct execution.

### Level 2 — Intermediate Workflows
- Use **slash commands** for repeatable workflows (e.g., a custom `/review` or `/test` command).
- Delegate focused sub-tasks to **subagents** to keep the main session's context clean.
- Connect your first **MCP server** (e.g., a GitHub or filesystem connector) and observe how tool calls appear in the session.
- Practice context management — knowing when to start a fresh session vs. continuing a long one.

### Level 3 — Advanced Automation
- Write your first **hooks** (see Section 5) to auto-format code after every edit.
- Integrate Claude Code into **CI/CD** — automated PR review, test-failure triage, or scheduled maintenance tasks.
- Build or configure a **custom MCP server** exposing your team's internal tools or data.
- Experiment with multi-agent patterns — one orchestrator agent delegating to specialized builder/validator subagents.

### Level 4 — Expert / Production Governance
- Design a **security hook layer**: file-protection rules, command allow/deny lists, mandatory review gates.
- Standardize team-wide configuration via shared `.claude/settings.json` checked into version control.
- Use the **Agent SDK** to embed Claude Code-style agentic behavior into your own products.
- Contribute back: publish reusable hooks, MCP servers, or `CLAUDE.md` templates for your team or the open-source community.

> 🎯 **Mastery checkpoint:** you know you've moved from "user" to "expert" when you stop thinking of Claude Code as a chat tool and start thinking of it as a **configurable agent runtime** you architect around your codebase and team process.

---

## 5. 🔐 Security — Advanced Hook-Based Configuration (PreToolUse / PostToolUse)

**Hooks** are shell commands (or prompts/subagents) that Claude Code executes automatically at specific points in its lifecycle. They run **outside the model** — as deterministic scripts — which makes them the right place to enforce rules you never want to depend on the LLM "remembering."

### 5.1 Where Hooks Live
Hooks are configured in a `.claude/settings.json` file (per-project or user-level), using a `matcher` (a regex on tool names) and a list of handlers to run when that matcher fires.

### 5.2 The Two Most Important Events

**`PreToolUse`** — fires *before* a tool call executes. This is your **enforcement point**: it can inspect the proposed action and **allow**, **deny**, or **ask** the user for confirmation. A hook that exits with code `2` blocks the action and tells the model why; exit `0` allows it through.

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "./scripts/block-dangerous-commands.sh"
          }
        ]
      }
    ]
  }
}
```

Example use cases:
- Block edits to protected files (`.env`, CI configs, `main`/`master` branch).
- Block destructive shell commands (`rm -rf`, force-pushes).
- Require human confirmation before any database-writing command runs.

**`PostToolUse`** — fires *after* a tool call completes. It cannot undo the action, but it can **validate, format, or give the model feedback** to correct course on the next step.

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit|MultiEdit",
        "hooks": [
          { "type": "command", "command": "npx prettier --write \"$CLAUDE_TOOL_INPUT_FILE_PATH\"" },
          { "type": "command", "command": "npx eslint --fix \"$CLAUDE_TOOL_INPUT_FILE_PATH\"" }
        ]
      }
    ]
  }
}
```

Example use cases:
- Auto-format and lint every file the model touches.
- Run the test suite after a code change and feed failures back to the model.
- Strip secrets or noisy output before it re-enters the model's context.

### 5.3 Other Lifecycle Events Worth Knowing
- **`SessionStart` / `SessionEnd`** — setup and teardown (e.g., loading environment context, writing session summaries).
- **`UserPromptSubmit`** — fires when you submit a prompt; can inject extra context but cannot rewrite your prompt.
- **`Notification`** — route alerts to Slack, desktop notifications, or logging.
- **`Stop`** — fires when the agent finishes responding; can force continuation if validation hasn't passed yet.

### 5.4 Hardening Principles for Production Use
1. **Treat `PreToolUse` as your real security boundary** — never rely on prompt instructions alone ("please don't touch production files") for anything that actually matters.
2. **Use explicit exit codes** — a security hook that doesn't exit `2` on failure provides no real enforcement, only a warning.
3. **Check `tool_input` precisely** — match on the actual command/file path being sent, not just the tool name, to avoid both false positives and bypass loopholes.
4. **Version-control your hooks** — commit `.claude/settings.json` and hook scripts so every team member inherits the same guardrails automatically.
5. **Keep hooks fast** — slow hooks degrade the interactive experience; offload heavy validation to background CI where possible.

> 📚 For exact schema details, exit-code semantics, and the full list of lifecycle events, always cross-check the official Claude Code hooks reference at **[docs.claude.com](https://docs.claude.com)**, since hook capabilities have expanded rapidly release over release.

---
---

# 🇵🇰 PART 2 — ROMAN URDU EDITION

*(Yeh section Part 1 ka mukammal Roman Urdu tarjuma aur izafi tashreeh hai — professional aur technical, lekin samajhne mein asaan.)*

## 1. 🕰️ Tareekh — Kis Ne Banaya, Aur Kab

**Anthropic** wo company hai jis ne Claude aur Claude Code banaya. Yeh **2021** mein **Dario Amodei** aur **Daniela Amodei** (bhai-behan) ne, kuch dusre saabiq OpenAI researchers ke saath mil kar bunyad rakhi. AI safety is company ke mission ka core hissa hai, sirf ek baad ka khayal nahi.

In products ki mukhtasir timeline:

| Tareekh | Sangam (Milestone) |
|---|---|
| **2021** | Anthropic ki bunyad rakhi gayi. |
| **March 2023** | Pehla Claude model aur Claude Instant announce hue; Anthropic API early access ke liye khula. |
| **July 2023** | Claude 2 launch hua, sath hi public `claude.ai` chat interface bhi aaya. |
| **March 2024** | Claude 3 family (Opus, Sonnet, Haiku) launch hui — Anthropic ka pehla teen-tier model lineup. |
| **June 2024** | Claude 3.5 Sonnet launch hua. |
| **February 2025** | Claude 3.7 Sonnet ne ek production model mein hybrid reasoning (extended thinking) introduce ki. |
| **24 February 2025** | **Claude Code** ek limited **research preview** ke taur par launch hua — terminal-based agentic coding tool, jo shuru mein Claude 3.7 Sonnet ke saath bundled tha. |
| **May 2025** | Claude 4 (Opus 4 / Sonnet 4) launch hua; Claude Code generally available ho gaya aur Pro plan mein shamil kiya gaya. |
| **2025 ke darmiyani-akhri mahine** | MCP support, subagents, remote MCP servers, aur Claude Code on the Web — yeh sab roll out hue. |
| **2026** | Sonnet, Opus, aur Haiku lines mein musalsal tez raftaar improvements, sath sath ecosystem ki growth (Agent SDK, Managed Agents, deeper IDE integrations). |

> ⚠️ **Versions ke baare mein zaroori baat:** Anthropic naye model versions kaafi tezi se release karta hai. Is guide mein kisi specific version number ko hard-code karne ke bajaye (jo purana ho jayega), hamesha **[anthropic.com/news](https://www.anthropic.com/news)** aur **[docs.claude.com](https://docs.claude.com)** check karein takay current model lineup pata chal sake, khaas taur par kharidari ya architecture decisions se pehle.

---

## 2. 🔬 Technical Deep-Dive — Claude Code Asal Mein Kaam Kaise Karta Hai

Claude Code ko samajhne ka behtareen tareeqa yeh hai ke isay **teen components ka mil kar kaam karna** samjha jaye, na ke ek monolithic "AI app."

### 2.1 Reasoning Engine (LLM)
Core mein ek **Claude model** hota hai (Haiku, Sonnet, ya Opus family se). Yeh hissa:
- Aap ki natural-language instruction ko parhta aur samajhta hai.
- File contents request kar ke aap ke codebase ka ek mental model banata hai.
- Multi-step approach plan karta hai — kaun si files chhoni hain, kis order mein, aur kyun.
- Actual code, commands, aur explanations likhta hai.

LLM khud **disk ko touch nahi kar sakta ya kuch run nahi kar sakta** — yeh sirf text generate karta hai. Project mein jo bhi "kaam" hota hai, wo agle layer ke zariye hota hai.

### 2.2 Terminal aur Tool Access (Agent Loop)
Claude Code model ko ek **agent loop** mein wrap karta hai jo isay real capabilities deta hai:
- **File tools** — working directory mein files parhna, likhna, edit karna, search karna.
- **Bash/shell access** — build commands, tests, linters, git operations chalana.
- **Iteration** — ek command chalane ke baad, Claude Code uska *result* (stdout/stderr, test output, errors) model ko wapas deta hai, jo phir apna agla step decide karta hai.

Yeh read → plan → act → observe → re-plan cycle hi isay "agentic" banata hai, na ke ek-baar-mein-ho-jaane wala autocomplete. Sab se zaroori baat: by default Claude Code file-modify karne ya kisi bhi potentially nuqsaan-deh action se pehle **ijazat mangta hai** — autonomy ek dial hai jo developer control karta hai, all-or-nothing switch nahi.

### 2.3 MCP — Model Context Protocol (Connector Layer)
**MCP** ek open protocol hai jo Claude Code (aur dusre AI applications) ko external tools aur data sources se ek standard tareeqe se connect karne deta hai — is ke bajaye ke har integration apna alag, custom API wrapper ho.

Amal mein, MCP servers Claude ko aise tools muhayya karte hain jaise:
- Project tracker se issues parhna.
- Database query karna.
- Internal documentation search karna.
- Team chat tool par messages post karna.

Architecture ke lihaz se, ek MCP server Claude Code ke **sath** baitha hota hai aur callable tools ka menu offer karta hai; LLM decide karta hai ke *kab* unhein call karna hai, conversation ke hisab se, aur agent loop call execute kar ke result wapas laata hai — wahi observe-and-react cycle, sirf terminal tools se aage har us cheez tak extend hua jo MCP boltay ho.

### 2.4 Sab Kuch Saath Milana
Aik single Claude Code request shayad is tarah chale:
1. Aap natural-language instruction type karte hain.
2. **LLM** soch kar plan propose karta hai ke kya zaroori hai.
3. **Agent loop** context lene aur changes karne ke liye file reads/edits aur shell commands execute karta hai.
4. Agar task ko bahar ka data chahiye, LLM ek **MCP tool** call karta hai (misal ke taur par "related ticket fetch karo").
5. Results LLM ko wapas jate hain, jo apna plan adjust karta hai aur kaam mukammal hone tak ya control aap ko wapas dene tak jaari rakhta hai.

---

## 3. 💰 Setup aur Pricing — Official Plans vs. Ollama (Free, Local)

### 3.1 Official Pricing Model (Subscription + API)

Claude Code ek **alag standalone product ki tarah price nahi hota** — yeh wohi Claude access use karta hai jo aap ke paas pehle se hai: ek consumer subscription ya pay-as-you-go API credits. 2026 ke darmiyan tak, official pricing ka rough shape yeh hai:

| Tier | Taqreeban Price | Notes |
|---|---|---|
| **Free (Claude.ai chat)** | $0 | Sirf chat — **Claude Code / terminal access shamil nahi.** |
| **Pro** | ~$20/mahina | Individual developers ke liye entry point; Claude Code shamil hota hai ek pooled usage budget ke sath. |
| **Max 5x** | ~$100/mahina | Rozana, bhari Claude Code usage ke liye zyada usage ceiling. |
| **Max 20x** | ~$200/mahina | Power users ke liye jo lambi sessions, multiple agents, ya bare codebases chalate hain. |
| **Team (Premium seats)** | ~$100–125/seat/mahina | Claude Code access aam taur par sirf upper "Premium" seat tier mein milta hai; minimum seat counts aksar lagti hain. |
| **Enterprise** | Custom | Bare context windows, compliance/SSO features, custom contracts. |
| **API (pay-as-you-go)** | Per-token | Koi monthly fee nahi; consume kiye gaye input/output tokens ke hisab se bill hota hai. CI pipelines aur automation ke liye useful, musalsal rozana interactive use ke liye kam economical. |

> 💡 **Hamesha current numbers verify karein** **[anthropic.com/pricing](https://www.anthropic.com/pricing)** par — token rates aur plan tiers waqtan faqtan revise hotay hain, aur yeh table purana ho jayega.

**Aam usool:** agar aap zyada din kuch focused ghante Claude Code use karte hain, to ek subscription tier aam taur par per-token payment se sasta padta hai. Agar aap ka usage bursty hai ya aap isay automated pipelines (jaise GitHub Actions) mein chala rahe hain, to API billing behtar cost visibility deta hai.

### 3.2 Pakistan Ka Masla: USD Pricing vs. PKR Income

PKR mein kamane wale developers ke liye, ek recurring USD subscription ek haqeeqi cost barrier hai — exchange-rate ka asar, international card ki zaroorat, aur yeh simple fact ke $20–200/mahina locally income ka bohat alag proportion hai US/EU ke muqable mein. Isi liye neeche diya gaya **Ollama local route** is community ke liye itna ahem hai: yeh "hona to acha hai" wali cheez nahi, aksar yeh haqeeqi entry point hota hai.

### 3.3 Free, Local Usage Ollama Ke Zariye

**Ollama** ek open-source runtime hai jo large language models download kar ke poori tarah aap ki apni machine par chalata hai — koi API cost nahi, model download hone ke baad internet ki zaroorat nahi, aur koi data aap ke hardware se bahar nahi jata.

2026 ki shuruaat mein, Ollama ne **Anthropic Messages API ka native support** add kiya, jis ka matlab hai ke Claude Code ka CLI Ollama-served open models se **seedha** baat kar sakta hai, kisi third-party translation proxy ke bina, sirf Claude Code ke environment variables ko apne local Ollama server ki taraf point kar ke.

**Aam setup:**

```bash
# 1. Ollama install karein
curl -fsSL https://ollama.com/install.sh | sh

# 2. Ek coding-capable open model pull karein
ollama pull qwen2.5-coder:32b   # ya chhota variant agar VRAM/RAM mehdood hai

# 3. Claude Code ko apne local Ollama server ki taraf point karein
export ANTHROPIC_AUTH_TOKEN=ollama
export ANTHROPIC_API_KEY=""
export ANTHROPIC_BASE_URL=http://localhost:11434

# 4. Launch karein
claude
```

**Purana / alternative tareeqa (ab bhi un local servers ke liye istemal hota hai jo Anthropic-compatible nahi):** **LiteLLM** ke zariye route karein, ek proxy jo Claude Code ke Anthropic-format requests ko us OpenAI-compatible format mein translate karta hai jo kayi local servers (purane Ollama versions, LM Studio, vLLM) expect karte hain.

| Factor | Official Cloud (Sonnet/Opus) | Ollama (Local) |
|---|---|---|
| Cost | Subscription ya per-token | Hardware/bijli ke baad free |
| Code quality | State-of-the-art, complex multi-file reasoning ke liye behtareen | Simple tasks ke liye theek; long-horizon, complex reasoning mein kamzor |
| Privacy | Data Anthropic ke API ko jata hai | Bilkul aap ki machine par rehta hai |
| Hardware needs | Koi nahi (cloud-hosted) | Capable GPU/CPU + RAM chahiye (bare models ke liye 16–32GB+ recommend) |
| Context window | Bara, production-grade | Local models par aksar chhota — bare codebases par context drop ho sakta hai |
| Behtareen use case | Production kaam, client deliverables, complex refactors | Learning, prototyping, offline kaam, privacy-sensitive tajurba |

**Pakistani developers ke liye amali tajweez:** workflow seekhein (prompting patterns, CLAUDE.md conventions, hooks, MCP) Ollama ke zariye bilkul free, phir Pro subscription ya API credits ko selectively client-facing ya waqai complex kaam ke liye use karein jahan model quality directly billable result par asar dalti hai.

---

## 4. 🛣️ Skill-Building Roadmap — Claude Code Expert Banna

### Level 1 — Bunyadi Baatein (Foundations)
- Claude Code install karein aur authenticate karein (subscription ya API key).
- Bunyadi loop seekhein: instruct karna → diff review karna → approve/reject karna → iterate karna.
- Apne project root mein ek `CLAUDE.md` file likhein — project-specific conventions, build commands, aur style rules jo agent ko hamesha follow karni chahiyen.
- Plan mode (execution se pehle proposed approach review karna) aur direct execution mein farq samajh kar comfortable hon.

### Level 2 — Darmiyani Workflows (Intermediate)
- Repeatable workflows ke liye **slash commands** use karein (misal: custom `/review` ya `/test` command).
- Focused sub-tasks **subagents** ko delegate karein takay main session ka context saaf rahe.
- Apna pehla **MCP server** connect karein (misal: GitHub ya filesystem connector) aur dekhein ke tool calls session mein kaise dikhte hain.
- Context management ki practice karein — yeh samajhna ke kab nayi session shuru karni hai aur kab lambi session continue karni hai.

### Level 3 — Advanced Automation
- Apne pehle **hooks** likhein (Section 5 dekhein) takay har edit ke baad code auto-format ho.
- Claude Code ko **CI/CD** mein integrate karein — automated PR review, test-failure triage, ya scheduled maintenance tasks.
- Apni team ke internal tools ya data expose karne wala **custom MCP server** banayein ya configure karein.
- Multi-agent patterns ke sath tajurba karein — ek orchestrator agent jo specialized builder/validator subagents ko delegate kare.

### Level 4 — Expert / Production Governance
- Ek **security hook layer** design karein: file-protection rules, command allow/deny lists, mandatory review gates.
- Team-wide configuration ko shared `.claude/settings.json` ke zariye standardize karein jo version control mein check ho.
- Apne products mein Claude Code jaisa agentic behavior embed karne ke liye **Agent SDK** use karein.
- Wapas contribute karein: reusable hooks, MCP servers, ya `CLAUDE.md` templates apni team ya open-source community ke liye publish karein.

> 🎯 **Mastery checkpoint:** aap "user" se "expert" ban gaye hain jab aap Claude Code ko sirf ek chat tool samajhna chhor dete hain aur isay ek **configurable agent runtime** ki tarah dekhna shuru karte hain jise aap apne codebase aur team process ke gird architect karte hain.

---

## 5. 🔐 Security — Advanced Hook-Based Configuration (PreToolUse / PostToolUse)

**Hooks** shell commands (ya prompts/subagents) hote hain jinhein Claude Code apni lifecycle ke specific points par automatically execute karta hai. Yeh **model ke bahar** chalte hain — deterministic scripts ki tarah — jo unhein un rules ko enforce karne ke liye sahih jagah banata hai jin par aap kabhi LLM ki "yaad-dahani" par depend nahi karna chahte.

### 5.1 Hooks Kahan Configure Hote Hain
Hooks ek `.claude/settings.json` file mein configure hote hain (per-project ya user-level), jis mein ek `matcher` (tool names par regex) aur handlers ki list hoti hai jo us matcher ke fire hone par chalte hain.

### 5.2 Do Sab Se Ahem Events

**`PreToolUse`** — tool call execute hone se *pehle* fire hota hai. Yeh aap ka **enforcement point** hai: yeh proposed action inspect kar sakta hai aur **allow**, **deny**, ya user se confirmation **ask** kar sakta hai. Jo hook exit code `2` ke sath khatam hota hai wo action ko block karta hai aur model ko bataata hai kyun; exit `0` isay aage jaane deta hai.

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "./scripts/block-dangerous-commands.sh"
          }
        ]
      }
    ]
  }
}
```

Misal ke use cases:
- Protected files (`.env`, CI configs, `main`/`master` branch) par edits block karna.
- Nuqsaan-deh shell commands (`rm -rf`, force-pushes) block karna.
- Kisi bhi database-likhne wale command se pehle insaani confirmation zaroori karna.

**`PostToolUse`** — tool call mukammal hone ke *baad* fire hota hai. Yeh action ko undo nahi kar sakta, lekin **validate kar sakta hai, format kar sakta hai, ya model ko feedback de sakta hai** takay wo agle step mein sudhaar kare.

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit|MultiEdit",
        "hooks": [
          { "type": "command", "command": "npx prettier --write \"$CLAUDE_TOOL_INPUT_FILE_PATH\"" },
          { "type": "command", "command": "npx eslint --fix \"$CLAUDE_TOOL_INPUT_FILE_PATH\"" }
        ]
      }
    ]
  }
}
```

Misal ke use cases:
- Model jo bhi file touch kare usay auto-format aur lint karna.
- Code change ke baad test suite chalana aur failures model ko wapas dena.
- Secrets ya noisy output ko strip karna is se pehle ke wo dobara model ke context mein jaye.

### 5.3 Dusre Lifecycle Events Jo Janna Zaroori Hai
- **`SessionStart` / `SessionEnd`** — setup aur teardown (misal: environment context load karna, session summaries likhna).
- **`UserPromptSubmit`** — jab aap prompt submit karte hain fire hota hai; extra context inject kar sakta hai lekin aap ka prompt rewrite nahi kar sakta.
- **`Notification`** — alerts ko Slack, desktop notifications, ya logging ki taraf route karna.
- **`Stop`** — jab agent jawab dena khatam karta hai fire hota hai; agar validation pass nahi hui to continuation force kar sakta hai.

### 5.4 Production Use Ke Liye Hardening Usool
1. **`PreToolUse` ko apna asal security boundary samjhein** — kabhi bhi sirf prompt instructions ("please production files ko touch na karen") par depend na karen us cheez ke liye jo waqai matter karti hai.
2. **Explicit exit codes use karen** — jo security hook failure par exit `2` nahi karta wo koi haqeeqi enforcement nahi deta, sirf ek warning.
3. **`tool_input` ko theek se check karen** — sirf tool name nahi, balke jo actual command/file path bheja ja raha hai usay match karen, takay false positives aur bypass loopholes dono se bacha ja sake.
4. **Apne hooks version-control mein rakhen** — `.claude/settings.json` aur hook scripts commit karen takay har team member ko automatically wahi guardrails milein.
5. **Hooks ko fast rakhen** — slow hooks interactive experience kharab karte hain; bhari validation ko jahan mumkin ho background CI par bhej dein.

> 📚 Sahih schema details, exit-code semantics, aur lifecycle events ki mukammal list ke liye, hamesha official Claude Code hooks reference **[docs.claude.com](https://docs.claude.com)** par cross-check karen, kyun ke hooks ki capabilities har release ke saath tezi se barh rahi hain.
