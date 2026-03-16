# Day 1 Bonus Lab: The "AI-Driven Feature" Pressure Test

**← [Back to Course Overview](../README.md)**

**Duration:** 30 Minutes
**Tools:** ChatGPT or Microsoft Copilot (required); Python IDE or Jupyter Notebook (optional — see no-code path below)

## Lab Objective
To transition from a "Business Idea" to a "Data Strategy" by engineering features, identifying model risks, and simulating real-world data drift.

---

## Part 1: The Raw Data

Below is a simplified stream of Visa transaction data. You'll use this in all three steps.

```python
# Raw Transaction Data for Analysis
transactions = [
    {"tx_id": 1, "amount": 54.20, "time": "14:22", "mcc": "5411", "zip": "94103", "is_fraud": 0},
    {"tx_id": 2, "amount": 1200.00, "time": "03:15", "mcc": "5732", "zip": "10001", "is_fraud": 1},
    {"tx_id": 3, "amount": 15.00, "time": "09:45", "mcc": "5812", "zip": "60601", "is_fraud": 0},
    {"tx_id": 4, "amount": 850.00, "time": "02:10", "mcc": "5944", "zip": "90210", "is_fraud": 1},
    {"tx_id": 5, "amount": 65.00, "time": "18:30", "mcc": "5411", "zip": "94103", "is_fraud": 0}
]
```

> **Note:** A larger version of this dataset is available in [`data/data.json`](./data/data.json) if you want to explore beyond these five rows.

---

## Step 1: Feature Engineering (10 Mins)

**Goal:** Transform raw data into predictive signals that an AI model can actually use.

### The Task

Add two new "engineered features" to the transaction data:

- `is_night_time`: `1` if the transaction occurred between 11 PM and 5 AM, else `0`
- `is_high_value`: `1` if the amount is greater than $500, else `0`

### Two Paths — Pick One

**Path A: Python (if you have a notebook or IDE ready)**

Use ChatGPT or Copilot to write the function, then run it:

> *"I have a list of Python dictionaries called `transactions`. Write a function that iterates through them and adds two binary features: `is_night_time` (based on the 'time' string, flag 11 PM–5 AM) and `is_high_value` (if 'amount' > 500). Show the updated list."*

Copy the generated code into your Python environment and run it. Verify the output: transactions 2 and 4 should have `is_night_time: 1` and `is_high_value: 1`.

**Path B: No-Code (ChatGPT or Copilot only)**

Paste this prompt directly into ChatGPT or Copilot:

> *"I have this transaction data: [paste the transactions block above]. Manually apply these two rules to each row and show me the updated table: (1) `is_night_time = 1` if time is between 23:00–05:00, else 0; (2) `is_high_value = 1` if amount > 500, else 0."*

The AI will produce the annotated table. Verify it looks right — this is your working dataset for the rest of the lab.

### The PM "Why"

Why transform raw timestamps into a binary `is_night_time` flag instead of feeding the model the raw time string?

*(Hint: ML models learn from patterns in numbers, not raw strings. A binary flag makes the "unusual hours" signal explicit and consistent, reducing the compute and training data the model needs to discover it on its own. This is the core idea behind feature engineering: you do cognitive work upfront so the model doesn't have to.)*

---

## Step 2: Governance & Bias Check (10 Mins)

**Goal:** Identify hidden risks in your data layer before they reach production.

### The Task

Analyze the `zip` (ZIP code) column in the data.

**Open ChatGPT or Copilot and ask:**

> *"I am a Product Manager at a payments company. We are using 'ZIP Code' as a feature in our fraud detection model. What are the specific ethical, regulatory, and bias risks associated with this? What could 'ZIP Code' be a proxy for?"*

### The Pivot

Your Legal/Compliance team asks you to remove ZIP code to prevent discriminatory outcomes. Propose an alternative geographic feature that captures genuine transaction-location risk without targeting specific neighborhoods.

Before you answer, consider: would your proposed alternative still encode neighborhood-level income or demographic data indirectly? Think about what a regulator or fair-lending attorney would ask when they audit your model. Many "alternatives" to ZIP code turn out to be equally problematic proxies.

### The Deliverable

Complete this table (you can fill it in yourself or use AI to draft it, then revise):

```
BIAS AUDIT SUMMARY
==================

| Column  | What It Measures     | What It's Actually a Proxy For | Risk Level |
|---------|----------------------|-------------------------------|------------|
| zip     | Transaction location | [your answer]                 | [H/M/L]    |
| [alt 1] | [what it measures]   | [proxy concern]               | [H/M/L]    |
| [alt 2] | [what it measures]   | [proxy concern]               | [H/M/L]    |

MY RECOMMENDATION:
Replace ZIP code with: [your choice]
Rationale: [1-2 sentences — why this is better AND what residual risk remains]
```

### The PM "Why"

Why can't you just let your data science team handle bias risk?

*(Hint: Bias in a fraud model is a product decision, not just a technical one. If your model disproportionately flags transactions from certain ZIP codes, the legal and reputational liability lands on the product team. PMs who understand proxy variables can catch these risks in the design phase — before any code is written.)*

---

## Step 3: Simulating Data Drift (10 Mins)

**Goal:** Anticipate how your "perfect" model will break in the real world.

### The Scenario

It's December 20th. Legitimate holiday shopping is peaking. Average transaction amounts are 3x higher than usual. Your `is_high_value` feature — trained on typical spending patterns — suddenly fires constantly on normal purchases.

**Glossary:** A **False Positive** in fraud detection means the model flagged a transaction as fraud when it was actually legitimate. A high **False Positive Rate** means lots of real customers are having their transactions blocked or reviewed unnecessarily — a major UX and trust problem.

### The Task

Use AI to predict the impact. Paste this prompt into ChatGPT or Copilot:

> *"In my fraud detection model, I created a feature called `is_high_value` for transactions > $500. During a holiday shopping surge where legitimate spending triples, what happens to the False Positive Rate of my model? How should I explain 'Data Drift' to non-technical business stakeholders?"*

### The Deliverable

**1. Slack message to your VP of Product (2 sentences max):**

Write a message explaining why fraud alerts are spiking and what the team is doing about it. Be honest but not alarming. No jargon.

```
DRAFT SLACK MESSAGE
===================

@[VP Name] — [Your 2-sentence message here]
```

**2. Short answer:**

What's the long-term fix? (Not "wait for the holidays to end" — what would you add to the model specification to prevent this from happening next year?)

---

## Lab Wrap-Up

### What You Did:
- **Feature Engineering** — Transformed raw timestamps and amounts into binary signals an ML model can learn from
- **Proxy Variable Identification** — Spotted a bias risk hiding in a geographic field and proposed alternatives
- **Data Drift Planning** — Predicted a real-world failure mode and prepared stakeholder communication

### The Underlying Pattern:

All three steps follow the same PM instinct: *look one level deeper than the obvious.*

- Raw data → What signal is hiding here?
- Convenient feature → What does this actually represent?
- Working model → What happens when the world changes?

Engineers build the features. PMs ask these questions before engineers start building.

### Checklist Before Any AI Feature Ships:

- [ ] Have I identified what raw data becomes model input?
- [ ] Have I audited each input feature for proxy variable risk?
- [ ] Have I simulated at least one "world changes" scenario that could break the model?
- [ ] Can I explain data drift to a non-technical stakeholder in two sentences?
