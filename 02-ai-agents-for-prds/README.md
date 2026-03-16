# Bonus Lab: Agentic Workflows for PRD Generation & Evaluation

**← [Back to Course Overview](../README.md)**

**Duration:** 30 Minutes  
**Tools:** ChatGPT or GitHub Copilot (any general AI chat tool)

## 🎯 Lab Objective
Learn **agentic workflows** — where multiple specialized AI agents collaborate, hand off work, and iterate to solve complex problems. You'll simulate this multi-agent orchestration using strategic prompting in a single AI chat tool.

---

## 🤖 What Are "Agentic Workflows"?

**Regular AI Use:** You ask ChatGPT one question, it gives you one answer. Single turn, single agent.

**Agentic Workflows:** Multiple specialized agents work in sequence, each with a specific role:
- **Agent A** does its job → **hands off output to Agent B**
- **Agent B** critiques/validates → **hands feedback to Agent C**  
- **Agent C** iterates and improves → **hands final deliverable to you**

This is how advanced AI platforms (OpenClaw, Claude Code, etc.) work behind the scenes. **You don't need those expensive tools** — you can simulate the same pattern with smart prompting in ChatGPT or Copilot.

### The Workflow You'll Build Today:

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│   AGENT A       │     │    AGENT B       │     │   AGENT C       │
│  "Generator"    │────▶│   "Critic"       │────▶│  "Refiner"      │
│                 │     │                  │     │                 │
│ • Takes vague   │     │ • Reviews PRD    │     │ • Takes         │
│   idea          │     │ • Finds holes    │     │   feedback      │
│ • Writes PRD    │     │ • Identifies     │     │ • Revises PRD   │
│                 │     │   risks          │     │ • Adds detail   │
└─────────────────┘     └──────────────────┘     └─────────────────┘
                                                          │
                                                          ▼
                                                  ┌─────────────────┐
                                                  │  FINAL OUTPUT   │
                                                  │  (Quality PRD)  │
                                                  └─────────────────┘
```

**The magic:** Each agent has a **specialized role** and **limited responsibility**. No single agent tries to do everything. They hand off work like a relay race.

---

## 🎬 Part 1: The "Vague Idea" Scenario

You're a PM at a fintech startup. Your CEO walks by your desk and says:

> *"We should build an AI feature that helps our business customers reconcile their accounts faster. You know, using AI to match transactions. Make it happen!"*

This is your starting point: a vague concept with no PRD, no user stories, and no technical details.

**Today, instead of writing this PRD yourself, you'll orchestrate a team of AI agents to do it collaboratively.**

---

## 🛠️ Step 1: Deploy the Generator Agent (10 Mins)

**Goal:** Activate Agent A — the **PRD Generator** — to transform your vague idea into a structured document.

### The Agentic Concept
Agent A has **one job**: Take rough input and create comprehensive output. It doesn't critique, doesn't validate, doesn't second-guess. It **generates**. This is its entire purpose.

### The Task

**Open ChatGPT or Copilot. This is Agent A's workspace. Paste this prompt:**

```
You are Agent A: The PRD Generator. You are a Senior Product Manager with 10 years of experience writing PRDs for B2B fintech products. 

YOUR SINGLE RESPONSIBILITY: Transform rough feature ideas into comprehensive, structured PRDs. You do not critique, validate, or question. You CREATE.

INPUT FEATURE IDEA: "AI-powered transaction reconciliation assistant for business customers"

YOUR OUTPUT MUST INCLUDE:
1. **Problem Statement** - What pain point are we solving?
2. **Target Users** - Who benefits from this? Be specific (company size, role, use cases)
3. **Success Metrics** - How do we know this worked? Include 3 specific, measurable KPIs
4. **Core Functionality** - What does the product actually do? (3-5 key features)
5. **Out of Scope** - What are we explicitly NOT building in V1?
6. **Open Questions** - What do we need to research or validate?

CONSTRAINTS (work within these boundaries):
- This is an MVP, so keep features realistic for a 3-month timeline
- Assume our customers are small-to-medium businesses (10-500 employees)
- Avoid technical jargon — focus on user outcomes

Generate the complete PRD now. Do not ask clarifying questions. Do your best with the information provided.
```

### The PM "Why"
**Discussion:** Notice how we explicitly told Agent A what **NOT** to do ("do not critique, validate, or question"). Why is this important in an agentic workflow?

*(Hint: In multi-agent systems, each agent should have a narrow, well-defined responsibility. If Agent A starts critiquing its own work, it's doing Agent B's job poorly. Specialization = better output.)*

---

## 🔍 Step 2: Deploy the Critic Agent (10 Mins)

**Goal:** Activate Agent B — the **PRD Critic** — to review Agent A's output and find problems.

### The Agentic Concept
Agent B has **one job**: Find holes, risks, and weak assumptions. It doesn't generate new content, doesn't write code, doesn't suggest improvements. It **critiques**. Agent B and Agent A are intentionally separated — we want fresh eyes that didn't write the original PRD.

### The Task

**Copy the PRD output from Agent A (Step 1).**

**CRITICAL: Start a NEW chat or open a different AI tool instance. Agent B needs a clean context — it should not know what prompts you gave Agent A.**

**Paste this prompt in the new chat (Agent B's workspace):**

```
You are Agent B: The PRD Critic. You are a VP of Engineering with a strong background in fintech and risk management. 

YOUR SINGLE RESPONSIBILITY: Review PRDs from a skeptical, detail-oriented perspective. Your job is to find problems BEFORE engineering starts building. You do not write PRDs. You do not suggest fixes. You FIND FLAWS.

INPUT PRD TO CRITIQUE:
[PASTE THE ENTIRE PRD FROM AGENT A HERE]

YOUR OUTPUT MUST COVER:

1. **Technical Feasibility Issues** (Minimum 3 specific problems)
   - What technical assumptions seem unrealistic?
   - What's the most complex part to build?
   - Are there integration points not considered?

2. **Missing Critical Details** (Minimum 3 gaps)
   - What would engineers need to know that's not here?
   - What data inputs/outputs are undefined?
   - What dependencies are missing?

3. **Edge Cases & Risks** (Minimum 3 risks)
   - Security concerns
   - Compliance issues  
   - User error scenarios
   - System failure modes

4. **Scope Creep Red Flags** (Minimum 2 items)
   - Which features seem likely to expand beyond the 3-month MVP timeline?
   - What "simple" features are actually complex?

5. **Blocker Questions for PM** (Minimum 3 questions)
   - What must be clarified before you'd approve this for development?

RULE: Be direct and specific. This is a pre-mortem — we want to catch problems now, not after launch. Do not soften your critique.
```

### The Deliverable

**From Agent B's critique, identify the TOP 3 most critical issues.** 

Write them in this format (you'll need this for Agent C):

```
CRITICAL ISSUE #1: [Copy the most important issue from Agent B]
CRITICAL ISSUE #2: [Copy the second most important issue]  
CRITICAL ISSUE #3: [Copy the third most important issue]
```

### The PM "Why"

**Discussion:** Why did we make you start a **new chat** for Agent B instead of just asking the same AI instance to "review what you wrote"?

*(Hint: Agentic workflows work because each agent has fresh perspective. If Agent B knows what prompts you gave Agent A, it might be biased or lenient. Clean context = objective critique. This is exactly how real multi-agent systems work — each agent is isolated.)*

---

## 🔄 Step 3: Deploy the Refiner Agent (10 Mins)

**Goal:** Activate Agent C — the **PRD Refiner** — to take Agent B's critique and improve the original PRD.

### The Agentic Concept
Agent C has **one job**: Iterate and improve based on feedback. It takes the original output (from Agent A) + the critique (from Agent B) and produces a **better version**. This is the **iteration loop** that makes agentic workflows powerful.

### The Task

**Option A: Go back to Agent A's original chat**
**Option B: Start a NEW chat (fresh context) — sometimes works better**

**Paste this prompt (Agent C's workspace):**

```
You are Agent C: The PRD Refiner. You are a Senior Product Manager who specializes in incorporating stakeholder feedback to improve PRD quality.

YOUR SINGLE RESPONSIBILITY: Take a draft PRD + critique feedback and produce a revised, higher-quality PRD. You ITERATE. You don't generate from scratch — you refine.

ORIGINAL PRD:
[PASTE THE ORIGINAL PRD FROM AGENT A HERE]

FEEDBACK FROM VP OF ENGINEERING (Agent B):
[PASTE THE TOP 3 CRITICAL ISSUES YOU IDENTIFIED HERE]

YOUR TASK: Revise the PRD to address the feedback. Specifically:

1. **Address Critical Issues**
   - Rewrite sections to fix the problems Agent B identified
   - Add missing details that were flagged

2. **Add Risk Mitigations Section**  
   - New section: "Known Risks & Mitigations"
   - For each risk Agent B identified, explain how we'll handle it

3. **Clarify Technical Assumptions**
   - New section: "Technical Assumptions"
   - Make explicit what was vague in the original

4. **Tighten Scope**
   - If Agent B flagged scope creep, explicitly reduce what's in V1
   - Update "Out of Scope" section to be more specific

5. **Add Implementation Plan**
   - New section: "Implementation Phases"
   - Break V1 into 2-week sprints (6 sprints total)
   - What gets built in each sprint

OUTPUT: The complete revised PRD with all sections. Mark new or heavily revised sections with **[REVISED]** so we can see what changed.
```

### The Deliverable

**Create a comparison document** with these sections:

```
AGENTIC WORKFLOW RESULTS
========================

AGENT A (Generator) Output Quality: [Rate 1-10] 
AGENT B (Critic) Found Issues: [Count how many issues were identified]
AGENT C (Refiner) Improvement: [What got better?]

TOP 3 IMPROVEMENTS MADE BY AGENT C:
1. 
2. 
3. 

WHAT AGENT B CAUGHT THAT I WOULD HAVE MISSED:

IF I PRESENTED TO CEO TODAY, I WOULD USE: [Original / Revised] because:

```

---

## 🎛️ The Agentic Workflow Explained

You just ran a **3-agent orchestration**. Here's what actually happened:

```
┌─────────────┐
│  YOU (PM)   │ ──▶ Gave vague idea to Agent A
└─────────────┘
       │
       ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  AGENT A    │────▶│  AGENT B    │────▶│  AGENT C    │
│  Generator  │     │  Critic     │     │  Refiner    │
│             │     │             │     │             │
│ Input: Idea │     │ Input: PRD  │     │ Input: PRD  │
│ Output: PRD │     │ Output:     │     │   + Critique│
│             │     │   Critique  │     │ Output:     │
│ Time: ~3min │     │ Time: ~3min │     │   Better PRD│
│             │     │             │     │ Time: ~4min │
└─────────────┘     └─────────────┘     └─────────────┘
                                                │
                                                ▼
                                          ┌─────────────┐
                                          │  FINAL PRD  │
                                          │  (High      │
                                          │   Quality)  │
                                          └─────────────┘
```

**Total Time:** ~10 minutes of AI work + your orchestration = **Better PRD than 2 hours of solo writing**

---

## 💡 Why Agentic Workflows Beat Single-Prompt AI

### ❌ Single-Prompt Approach (What Most People Do):

```
You: "Write me a PRD for an AI reconciliation feature"
AI: [Writes decent PRD]
You: [Reviews, finds issues, tries to fix yourself]
Result: Average quality, took 2 hours, you missed blind spots
```

### ✅ Agentic Workflow (What You Just Did):

```
Agent A: Generates PRD (creator mindset)
Agent B: Rips it apart (critic mindset)  
Agent C: Fixes the problems (refiner mindset)
Result: High quality, took 30 min, caught issues you'd miss
```

### The Key Differences:

| Aspect | Single Prompt | Agentic Workflow |
|--------|--------------|------------------|
| **Perspectives** | One (generalist) | Three (specialized) |
| **Quality Control** | You spot issues | Dedicated critic agent |
| **Iterations** | Manual, slow | Automated, fast |
| **Blind Spots** | You miss things | Agent B finds them |
| **Time to Quality** | 2+ hours | 30 minutes |

---

## 🔄 Real Agentic Patterns You Can Use Tomorrow

You don't need OpenClaw, Claude Code, or specialized tools. Use these patterns in ChatGPT/Copilot:

### Pattern 1: Creator → Critic → Refiner
**Use for:** Writing PRDs, documentation, strategy docs
- Agent A: Write it
- Agent B: Find problems  
- Agent C: Fix it

### Pattern 2: Researcher → Analyst → Synthesizer  
**Use for:** Market research, competitive analysis
- Agent A: Gather data
- Agent B: Analyze patterns
- Agent C: Create summary

### Pattern 3: Ideator → Validator → Planner
**Use for:** Feature ideation, roadmap planning
- Agent A: Generate ideas
- Agent B: Validate feasibility
- Agent C: Create implementation plan

### How to Simulate It:

1. **Use separate chats** for each agent (clean context = fresh perspective)
2. **Name your agents** explicitly — "You are Agent A: The [Role]"
3. **Define single responsibilities** — "You do NOT [other agent's job]"
4. **Pass outputs literally** — Copy-paste from Agent A → Agent B → Agent C
5. **Be the orchestrator** — You manage the handoffs, you pick which feedback matters

---

## 🎓 Bonus Challenge: Add a 4th Agent (Optional, 10 mins)

Create **Agent D: The User Advocate**

**Start a new chat. Paste:**

```
You are Agent D: The User Advocate. You are a Senior UX Researcher who represents end users in product discussions.

YOUR SINGLE RESPONSIBILITY: Review PRDs from the user's perspective. Find pain points, trust issues, and adoption risks. You don't care about technical feasibility — you care about user success.

INPUT PRD:
[PASTE THE FINAL REVISED PRD FROM AGENT C HERE]

YOUR OUTPUT:

1. **Unaddressed User Pain Points** (Minimum 3)
   - What problems do users still have that this PRD doesn't solve?
   - What would frustrate users about this feature?

2. **User Trust Issues** (Minimum 2)
   - Why might users distrust this AI reconciliation tool?
   - What transparency do they need?

3. **Adoption Barriers** (Minimum 2)
   - What would prevent users from trying this feature?
   - What learning curve issues exist?

4. **Suggested Addition to PRD**
   - Recommend a new "User Trust & Transparency" section
   - Include specific requirements about explainability, control, etc.

RULE: Be the voice of the user. If something helps engineering but hurts users, flag it.
```

**Take Agent D's feedback back to Agent C's chat:**
```
You are Agent C (the Refiner). New feedback from Agent D (User Advocate) just came in:
[PASTE AGENT D'S FEEDBACK]

Add a "User Trust & Transparency" section to address these concerns.
```

**Now you have a 4-agent workflow** — exactly how advanced AI orchestration platforms work!

---

##  Lab Wrap-Up

### What You Learned:

✅ **Agentic Thinking** — Multiple specialized agents > one generalist prompt  
✅ **Role Separation** — Generator, Critic, Refiner each have distinct jobs  
✅ **Workflow Orchestration** — You managed handoffs between agents  
✅ **Context Isolation** — Fresh chats = fresh perspective = better critique  
✅ **Iteration Loops** — Quality improves through agent collaboration  

### What You Built:

✅ **Reusable Agent Prompts** — Copy these for any PRD, doc, or strategy task  
✅ **Quality PRD** — In 30 minutes vs. 2+ hours solo  
✅ **Agentic Workflow Template** — Creator → Critic → Refiner pattern  

### Real-World Application:

Next time you need to create anything (PRD, spec, strategy doc, email):
1. **Agent A** — Generate first draft
2. **Agent B** — Critique it in a fresh chat  
3. **Agent C** — Refine it with feedback
4. **You** — Make final decisions as orchestrator

**Result:** Higher quality, fewer blind spots, less time spent.

---

## 📚 Agentic Workflow Cheat Sheet

| Agent Type | Responsibility | Prompt Keyphrase | When to Use |
|------------|---------------|------------------|-------------|
| **Generator** | Create from scratch | "You CREATE. Do not critique." | Starting from blank page |
| **Critic** | Find problems | "You FIND FLAWS. Do not suggest fixes." | Before sharing with stakeholders |
| **Refiner** | Iterate & improve | "You ITERATE. Incorporate feedback." | After receiving critique |
| **Analyst** | Extract patterns | "You ANALYZE. Find insights." | Data-heavy tasks |
| **Synthesizer** | Combine inputs | "You SYNTHESIZE. Merge perspectives." | Multiple sources to consolidate |

**Remember:** In agentic workflows, you're the **orchestrator**, not the **doer**. You assign roles, manage handoffs, and make final decisions. The agents do the specialized work.
