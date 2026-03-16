# Day 1 Bonus Lab: The "AI-Driven Feature" Pressure Test

**Duration:** 30 Minutes  
**Tools:** Python (Notebook or IDE), ChatGPT, and/or GitHub Copilot  

## 🎯 Lab Objective
To transition from a "Business Idea" to a "Data Strategy" by engineering features, identifying model risks, and simulating real-world data drift.

---

## 📂 Part 1: The Raw Data
Copy the following Python snippet. This represents a simplified stream of Visa transaction data.

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

## 🛠️ Step 1: Feature Engineering (10 Mins)
**Goal:** Transform raw data into predictive signals that an AI model can actually use.

1. **The Task:** Use ChatGPT or Copilot to write a Python function that adds two new "Engineered Features" to the `transactions` list provided in the setup.
2. **Required Features:**
   * `is_night_time`: `1` if the transaction occurred between 11 PM and 5 AM, else `0`.
   * `is_high_value`: `1` if the amount is greater than $500, else `0`.
3. **AI Prompt Hint:** > "I have a list of dictionaries called `transactions`. Write a Python function to iterate through them and add two binary features: `is_night_time` (based on the 'time' string) and `is_high_value` (if 'amount' > 500)."
4. **The PM "Why":** Why do we do this instead of just feeding the model raw timestamps? (Hint: Think about making patterns "obvious" to the model to reduce compute and improve accuracy).



---

##  Step 2: Governance & Bias Check (10 Mins)
**Goal:** Identify hidden risks in your data layer before they reach production.

1. **The Task:** Analyze the `zip` (ZIP code) column in the sample data.
2. **Action:** Open ChatGPT and ask the following "Audit" question:
   > "I am a Product Manager at a payments company. We are using 'ZIP Code' as a feature in our fraud detection model. What are the specific ethical, regulatory, and bias risks associated with this? What could 'ZIP Code' be a proxy for?"
3. **The Pivot:** If your Legal/Compliance team asks you to remove ZIP code to prevent bias, what *other* geographic feature could you propose that captures risk without targeting specific neighborhoods?

---

##  Step 3: Simulating Data Drift (10 Mins)
**Goal:** Anticipate how your "perfect" model will break in the real world.

1. **The Scenario:** It is now December 20th. Legitimate holiday shopping is peaking. Average transaction amounts are 3x higher than usual.
2. **The Task:** Use AI to predict the impact on your new features. Ask ChatGPT:
   > "In Step 1, I created a feature called `is_high_value` for amounts > $500. During a holiday shopping surge where legitimate spending triples, what happens to the **False Positive Rate** of my model? How should I explain this 'Data Drift' to my business stakeholders?"
3. **The Deliverable:** Write a **2-sentence Slack message** to your VP of Product explaining why fraud alerts (False Positives) are spiking and what the team is doing to address it.



---

###  Lab Wrap-Up
* **Feature Engineering:** You've moved from "Raw Data" to "Model Input."
* **Responsible AI:** You've identified a "Proxy Variable" for bias.
* **Lifecycle Management:** You've planned for "Model Decay" and stakeholder communication.
