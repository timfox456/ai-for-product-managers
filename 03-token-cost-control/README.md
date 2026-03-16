# Bonus Lab: Token Management & Cost Control for AI-Powered Products

**← [Back to Course Overview](../README.md)**

**Duration:** 30 Minutes  
**Tools:** ChatGPT, GitHub Copilot, Calculator (or spreadsheet), Word/Character Counter (any text editor)

---

## 📚 Part 1: Understanding Tokens, Cost & Why PMs Must Care

### What Are Tokens?

**Tokens are the "currency" of AI interactions.** They represent pieces of text that AI models process:
- **Input tokens:** The text you send to the AI (your prompt + any context)
- **Output tokens:** The text the AI generates in response
- **Total tokens:** Input + Output = what you pay for

**Rough Rule of Thumb:**
- 1 token ≈ 0.75 words (English)
- 100 tokens ≈ 75 words
- 1,000 tokens ≈ 750 words (about 1.5 pages)

**Example:**
```
Your prompt: "Write a summary of this quarterly report" 
→ ~10 tokens (input)

AI response: Full 500-word summary  
→ ~667 tokens (output)

Total: 677 tokens for one interaction
```

### Why Tokens = Product Management

As a PM building AI-powered features, **every token is a cost center**:

| AI-Powered Feature | Tokens per User | Monthly Cost (10K users) |
|-------------------|-----------------|-------------------------|
| Simple chatbot (short answers) | ~500 | ~$750 |
| Document summarizer | ~5,000 | ~$7,500 |
| Code generation assistant | ~10,000 | ~$15,000 |
| Multi-turn conversation | ~50,000+ | ~$75,000+ |

**Real Example:** A startup built an AI writing assistant. They didn't monitor tokens. After launch:
- 5,000 active users
- Average 15 AI calls per user per day
- 3,000 tokens per call
- **Monthly cost: $67,500** — more than their engineering payroll

They had to either:
- Raise prices (lose customers)
- Add usage limits (angry users)
- Optimize aggressively (2-week emergency project)

**As PM, you own the business model.** Token costs are your problem.

---

### Token Cost Drivers

**What makes AI features expensive?**

1. **Long inputs** — Sending lots of context (documents, history, data)
2. **Long outputs** — Asking for detailed responses, code, analysis
3. **High frequency** — Many AI calls per user session
4. **Large context windows** — Using 128K token models "just in case"
5. **Inefficient prompts** — Redundant instructions, unclear constraints

**The PM Challenge:** Build useful AI features while controlling these cost drivers.

---

### Cost Control Strategies

**Strategy 1: Right-Size Your Model**
- GPT-4 ($0.03/1K tokens) for complex reasoning
- GPT-3.5 ($0.002/1K tokens) for simple tasks — **15x cheaper**
- Use the cheapest model that can do the job

**Strategy 2: Compress Context**
- Summarize long documents before sending to AI
- Use embeddings + retrieval instead of sending full knowledge base
- Cache frequent queries

**Strategy 3: Limit Output Length**
- Specify "in 100 words or less" in prompts
- Request structured formats (JSON, bullet points) vs. prose
- Use "max_tokens" parameter to cap responses

**Strategy 4: Batch Operations**
- Combine multiple user requests into single AI call
- Process in background vs. real-time for non-urgent tasks

**Strategy 5: Hybrid AI/Human**
- Use AI for draft generation
- Human review for final output (also reduces hallucinations)
- AI only for specific sub-tasks, not end-to-end

---

## 🛠️ Step 1: Estimate Your Token Budget (10 Mins)

**Goal:** Learn to estimate token usage before building, so you can price and plan accurately.

### The Concept: Token Math for PMs

Before writing code, you should be able to answer:
- "How many tokens per user session?"
- "What's our monthly AI bill at scale?"
- "Where can we cut 50% of token usage without hurting UX?"

### The Task: Estimate a Feature's Token Cost

**Scenario:** You're PM for a customer support AI tool. Feature: "AI Email Response Generator"

**User Flow:**
1. Customer submits support ticket (average 200 words)
2. Support agent opens ticket, clicks "Generate AI Response"
3. AI reads the ticket + customer's history (last 5 emails) + company knowledge base article
4. AI drafts a personalized response
5. Agent reviews, edits, sends

**Your Job:** Estimate tokens per use and monthly cost.

**In ChatGPT, paste this prompt:**

```
You are an AI cost estimation specialist. Help me calculate token usage for a customer support feature.

SCENARIO: AI Email Response Generator for customer support

DATA INPUTS:
- Customer support ticket: 200 words
- Customer history (last 5 emails): 300 words each = 1,500 words total
- Relevant knowledge base article: 800 words
- System prompt (instructions to AI): 150 words

AI TASK: Generate personalized email response (estimated 250 words)

Please calculate:
1. Estimated input tokens (all context sent to AI)
2. Estimated output tokens (AI response)
3. Total tokens per interaction
4. Cost per interaction (use GPT-4 pricing: $0.03/1K input tokens, $0.06/1K output tokens)
5. Monthly cost if we have 5,000 support agents, each using this 10x per day
6. Cost if we switch to GPT-3.5 ($0.0015/1K input, $0.002/1K output)

Show your math clearly.
```

### The Deliverable

**Copy the AI's calculation and add this analysis:**

```
TOKEN BUDGET ANALYSIS
====================

Calculated Tokens Per Interaction: [number]
Calculated Cost Per Interaction (GPT-4): $[amount]
Monthly Cost (5K agents, 10x/day, GPT-4): $[amount]
Monthly Cost (same usage, GPT-3.5): $[amount]

POTENTIAL COST OPTIMIZATIONS:
1. [Idea #1]: Could reduce tokens by [X]% by [how]
2. [Idea #2]: Could reduce tokens by [X]% by [how]
3. [Idea #3]: Could reduce tokens by [X]% by [how]

COST VS. QUALITY TRADE-OFF:
- GPT-4 gives [quality level] but costs [cost]
- GPT-3.5 gives [quality level] but costs [cost]
- My recommendation: [which model and why]

PRICING IMPLICATIONS:
- If we charge $X/agent/month, AI costs represent [X]% of revenue
- To maintain 50% gross margin, we need to [action]
```

### The PM "Why"

**Discussion:** Why must PMs do this estimation *before* engineering starts building?

*(Hint: Token costs are a business model input. If you discover your AI feature costs $50/user/month to run but you can only charge $30, you have a problem. Better to know this in the PRD phase than at launch.)*

---

## 🔍 Step 2: Audit a Prompt for Token Waste (10 Mins)

**Goal:** Learn to identify and eliminate token inefficiencies in prompts.

### The Concept: The "Bloated Prompt" Problem

Most AI prompts are token-inefficient. Common wastes:
- Redundant instructions (saying the same thing 3 ways)
- Unnecessary pleasantries ("Please be so kind as to...")
- Over-explaining obvious context
- Not using structured formats

**Example of Waste:**
```
"I need you to help me with something. I have a document here, and I was 
hoping you could read it and then provide a summary. The document is about 
our Q3 sales performance. Please make sure the summary is comprehensive and 
covers all the main points. Also, please be thorough and detailed. And 
remember to be accurate. Thank you so much for your help!"
→ 72 words = ~96 tokens (waste: ~40 tokens of fluff)
```

**Token-Efficient Version:**
```
"Summarize this Q3 sales document. Cover: revenue trends, top products, 
regional performance, key challenges. 150 words max."
→ 22 words = ~29 tokens
```

**Same result, 70% fewer tokens.**

### The Task: Audit and Optimize a Bloated Prompt

**Here's a real (terrible) prompt from a product team:**

```
Hello there! I hope you are doing well today. I am working on a project 
and I need your assistance with something. I have a customer support ticket 
here from one of our users, and I would really appreciate it if you could 
help me draft a response to them. The customer is asking about a billing 
issue, specifically they were charged twice for their subscription this 
month and they are quite upset about it. They mentioned that they tried 
to contact us before but didn't hear back, which is unfortunate. I want 
to make sure we handle this professionally and show that we care about 
their experience. Could you please draft a response that acknowledges 
their frustration, apologizes for the inconvenience, explains what 
happened (it was a system error on our end), confirms that we have 
refunded the duplicate charge, and assures them that this won't happen 
again? Please make it empathetic but professional, and make sure it's 
comprehensive but not too long. Also, can you make sure to mention our 
24/7 support line in case they have any other issues? Thank you so much 
for your help with this, I really appreciate it!
```

**Step 1: Count the tokens**

Paste the bloated prompt into any character counter or word processor:
- Word count: ______
- Estimated token count (multiply by 1.33): ______

**Step 2: Ask AI to optimize it**

In ChatGPT, paste:

```
This prompt is too long and wastes tokens. Optimize it to be token-efficient 
while preserving all necessary instructions:

[BLOATED PROMPT HERE]

Requirements for optimized version:
- Remove all fluff, pleasantries, and redundancy
- Keep all functional requirements (what, why, constraints)
- Use imperative voice ("Draft response that...")
- Target: Under 50 words if possible

Show me:
1. Your optimized prompt
2. Word count of optimized version
3. Estimated token savings per call
4. Monthly savings if used 1,000x/day (30 days)
```

### The Deliverable

**Complete this audit template:**

```
PROMPT EFFICIENCY AUDIT
=======================

ORIGINAL PROMPT:
- Word count: [number]
- Estimated tokens: [number]
- Cost per call (GPT-4): $[amount]

OPTIMIZED PROMPT:
- Word count: [number]  
- Estimated tokens: [number]
- Cost per call (GPT-4): $[amount]

SAVINGS:
- Tokens saved per call: [number] ([X]% reduction)
- Cost saved per call: $[amount]
- Monthly savings (1,000 calls/day): $[amount]
- Annual savings: $[amount]

WHAT I LEARNED:
[Write 2-3 sentences about what made the original bloated and your 
takeaways for writing token-efficient prompts]

```

### The PM "Why"

**Discussion:** Why do you think product teams write such bloated prompts in the first place? And what process could prevent this?

*(Hint: Teams often write prompts like emails — polite, contextual, explanatory. But AI doesn't need pleasantries. A review step specifically for "token efficiency" before prompts go to production could catch this.)*

---

## 🎛️ Step 3: Design a Usage Limiter (10 Mins)

**Goal:** Design product mechanisms to control costs while maintaining user experience.

### The Concept: The "Guardrails" Approach

You can't just let users consume unlimited AI tokens — you'll go bankrupt. But you also can't be overly restrictive — users will churn.

**The PM Solution:** Design guardrails that:
1. Set clear expectations (users know their limits)
2. Provide value within limits (good UX even with constraints)
3. Offer upgrade paths (monetize heavy users)
4. Are transparent (users understand why limits exist)

### The Task: Design a Token Budget System

**Scenario:** You're PM for "DocuMind" — an AI document analysis tool for researchers.

**Current State:**
- Users upload PDFs, AI extracts insights, answers questions
- No limits = some users upload 500-page textbooks daily
- Your current monthly AI bill: $23,000 (unsustainable)
- Target: Reduce to $8,000 while keeping 90% of users happy

**Your Job:** Design a token budgeting system.

**In ChatGPT, paste this prompt:**

```
You are a Product Manager designing a token usage limit system. Help me 
create guardrails for an AI document analysis tool.

PRODUCT: DocuMind — AI document analysis for researchers
PROBLEM: No usage limits = $23K/month AI costs, need to reduce to $8K
CONSTRAINTS:
- Must keep 90% of users happy (light to moderate users)
- Heavy users (10%) should have clear upgrade path
- Must be transparent and fair
- Can't compromise core product value

Please design a token budget system including:

1. **Usage Tiers** 
   - How many tiers? (Free/Lite/Pro/Enterprise?)
   - Token limits per tier per month
   - What happens when users hit limits? (Hard stop? Throttled? Pay-as-you-go?)

2. **User Experience**
   - How do we communicate limits to users? (Dashboard? Warnings?)
   - What messaging when approaching limit?
   - What messaging when hitting limit?
   - How to make limits feel fair, not punitive?

3. **Technical Implementation**
   - How to track tokens per user? (API logging? Middleware?)
   - Real-time vs. daily aggregation?
   - How to prevent abuse/system gaming?

4. **Business Model**
   - Pricing for each tier
   - How to upsell heavy users without annoying them
   - Revenue projection: Will this pricing cover the $8K target + profit?

5. **Edge Cases**
   - What if a user's document is processing when they hit limit?
   - What about multi-user teams sharing token pools?
   - What if a user genuinely needs a one-time exception?

Output: Specific recommendations for each section above. Include example 
messaging and specific token numbers.
```

### The Deliverable

**Design your Token Budget System:**

```
TOKEN BUDGET SYSTEM DESIGN
==========================

TIER STRUCTURE:
┌─────────────┬──────────────┬───────────────┬─────────────┐
│ Tier        │ Monthly Cost │ Token Limit   │ Target User │
├─────────────┼──────────────┼───────────────┼─────────────┤
│ Free        │ $0           │ [X] tokens    │ [who]       │
│ Lite        │ $[X]         │ [X] tokens    │ [who]       │
│ Pro         │ $[X]         │ [X] tokens    │ [who]       │
│ Enterprise  │ Custom       │ Unlimited     │ [who]       │
└─────────────┴──────────────┴───────────────┴─────────────┘

LIMIT ENFORCEMENT STRATEGY:
[Describe what happens when users hit limits — soft cap vs hard stop, 
grace period, etc.]

USER COMMUNICATION:
- At 50% usage: [What message? Where shown?]
- At 80% usage: [What message? Where shown?]
- At 100% usage: [What message? Options given?]

UPSELL STRATEGY:
[How do you convert heavy users to paid tiers without being annoying?]

COST PROJECTION:
- Expected token usage per tier: [numbers]
- Expected revenue per tier: [numbers]
- Projected monthly AI costs: $[amount]
- Projected gross margin: [X]%

EDGE CASE HANDLING:
1. [Edge case]: [Solution]
2. [Edge case]: [Solution]
3. [Edge case]: [Solution]

MY CONCERNS / OPEN QUESTIONS:
[What would you want to validate with user research before implementing?]

```

### The PM "Why"

**Discussion:** Why is it better to design token limits as a **product feature** (clear tiers, transparent communication, upgrade paths) rather than just implementing a **technical restriction** (hard cutoff at X tokens)?

*(Hint: Users accept constraints when they understand them and see value. A technical restriction feels arbitrary and generates support tickets. A productized limit with clear tiers and benefits feels like choice and control. Same backend implementation, radically different user experience.)*

---

## 💡 The Token-Conscious PM Playbook

### What You Learned Today:

✅ **Token Math** — How to estimate costs before building features  
✅ **Prompt Efficiency** — How to audit and optimize token waste  
✅ **Limit Design** — How to build guardrails that users accept  

### Token Checklist for Every AI Feature:

**Before Building:**
- [ ] Estimate tokens per user interaction
- [ ] Calculate cost at projected scale
- [ ] Model unit economics (AI cost vs. revenue per user)
- [ ] Decide on model tier (GPT-4 vs GPT-3.5 vs fine-tuned)

**During Development:**
- [ ] Review all prompts for token efficiency
- [ ] Implement token tracking/logging
- [ ] Design usage limits and guardrails
- [ ] Build admin dashboard for cost monitoring

**After Launch:**
- [ ] Monitor token usage per user segment
- [ ] Identify power users and cost outliers
- [ ] Optimize top 20% most expensive operations
- [ ] Adjust pricing or limits based on real data

---

## 🎓 Bonus Challenge: The "Token Diet" (Optional, 10 mins)

**The Challenge:** Take a real AI-powered feature and put it on a "token diet"

**Your Task:**
1. **Find a real prompt** you or your team use (or use this example):

```
I want you to act as a product analyst. I will give you a user research 
interview transcript, and I want you to analyze it and extract key insights. 
Please identify: user pain points, feature requests, sentiment analysis, 
and actionable recommendations. Please format your response as a structured 
report with sections for each category. Please be thorough and 
comprehensive. Make sure to include specific quotes from the transcript 
to support each insight. Also, please provide a summary at the beginning 
and a conclusion at the end. The transcript is quite long, about 5,000 
words, so please make sure you process all of it carefully.
```

2. **Count current tokens:**
   - Word count: ~130 words
   - Tokens: ~173 tokens
   - Cost per call (GPT-4): $0.0052 input + output

3. **Optimize aggressively** — Rewrite to achieve same results with 50% fewer tokens

4. **Test both versions** — Run both prompts with the same transcript, compare output quality

5. **Report results:**

```
TOKEN DIET RESULTS
==================

ORIGINAL PROMPT:
- Tokens: [X]
- Cost: $[X]
- Output quality: [Rate 1-10]

DIET PROMPT:
- Tokens: [X] ([X]% reduction)
- Cost: $[X] ([X]% savings)
- Output quality: [Rate 1-10]

QUALITY VS. COST TRADE-OFF:
[Did the token diet hurt output quality? By how much? Worth it?]

OPTIMIZATION TECHNIQUES USED:
1. [Technique] — Saved [X] tokens
2. [Technique] — Saved [X] tokens
3. [Technique] — Saved [X] tokens

RECOMMENDATION:
[Would you deploy the diet version to production? Why/why not?]
```

---

##  Lab Wrap-Up

### Key Takeaways:

* **Tokens = Dollars** — Every AI interaction has a real cost that scales with usage
* **Estimate Early** — Token math should inform PRDs and pricing, not be an afterthought
* **Prompt Efficiency** — Bloated prompts are silent cost killers; audit them regularly  
* **Productize Limits** — Usage constraints work best when designed as features, not hidden restrictions
* **Monitor Religiously** — You can't optimize what you don't measure; track tokens per user/feature

### The "Token-Conscious PM" Mindset:

Before: "Let's add AI to this feature!"  
After: "Let's add AI to this feature. It'll use ~2K tokens per call, cost $0.06 per user action, so at 10K daily active users we're looking at $18K/month. We can afford that if we charge $29/month. Let's build it."

**That's the difference between AI experiments and AI businesses.**

---

## 📖 Quick Reference: Token Cost Estimation

### Simple Formula:
```
Monthly Cost = (Users × Actions per User × Tokens per Action × Price per Token)
```

### Example Calculation:
```
Users: 5,000
Actions per user per day: 10
Days per month: 30
Tokens per action: 1,000 (avg)
Price per 1K tokens: $0.03 (GPT-4 input)

Calculation:
5,000 × 10 × 30 × 1,000 = 1.5 billion tokens/month
1.5B ÷ 1,000 = 1.5M (thousands of tokens)
1.5M × $0.03 = $45,000/month
```

### Model Pricing Reference (Approximate):
| Model | Input (per 1K tokens) | Output (per 1K tokens) | Use Case |
|-------|----------------------|------------------------|----------|
| GPT-4 | $0.03 | $0.06 | Complex reasoning |
| GPT-3.5 Turbo | $0.0015 | $0.002 | Simple tasks |
| Claude 3 Opus | $0.015 | $0.075 | Long context |
| Claude 3 Sonnet | $0.003 | $0.015 | Balanced |

### Token Estimation Rules:
- **English text:** 1 word ≈ 1.33 tokens
- **Code:** 1 line ≈ 5-15 tokens (varies by language)
- **Structured data (JSON):** Higher token density than prose
- **Non-English:** 1 word ≈ 2-4 tokens (character-based languages)

---

**Remember:** In AI-powered products, **token efficiency is product efficiency**. Master this, and you'll build sustainable AI features instead of expensive experiments.
