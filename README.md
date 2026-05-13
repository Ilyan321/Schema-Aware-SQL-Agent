# Schema-Aware SQL Agent 🛡️

A production-grade NLP-to-SQL pipeline that enables users to query relational databases using plain English. This project features a fine-tuned Llama-3-8B model protected by a deterministic Python security validation layer ("The Gatekeeper").

---

## 🚀 Project Overview

This project implements a robust, two-pillar software architecture designed to bridge the gap between non-technical users and complex data structures:

- **The Brain (Fine-Tuned LLM):**  
  A Llama-3-8B model fine-tuned using Parameter-Efficient Fine-Tuning (PEFT/LoRA). It is trained to map natural language intent to specific database schemas.

- **The Gatekeeper (Validation Layer):**  
  A deterministic security interceptor that inspects AI-generated SQL to prevent destructive actions (DROP, DELETE, etc.) before they reach the database.

---

## 🛠️ Tech Stack & Tools

- **Model:** Meta Llama-3-8B (via Unsloth)
- **Fine-Tuning:** LoRA / QLoRA (Parameter-Efficient Fine-Tuning)
- **Training Framework:** Unsloth (2x faster, 70% less memory)
- **Dataset:** Spider (Cross-domain complex SQL)
- **Environment:** Kaggle t4 x 2 GPU
- **Safety Layer:** Custom Python Validation Logic

---

## 🧠 Model Card

The fine-tuned adapters for this project are hosted on the Hugging Face Hub.

- **Model Name:** [`Ilyankhan69/schema-aware-sql-agent`](https://huggingface.co/Ilyankhan69/schema-aware-sql-agent)
- **Base Model:** `unsloth/llama-3-8b-bnb-4bit`
- **Training Data:** [Spider Text-to-SQL Dataset](https://yale-lily.github.io/spider)
- **Model Link:** [https://huggingface.co/Ilyankhan69/schema-aware-sql-agent](https://huggingface.co/Ilyankhan69/schema-aware-sql-agent)

### Training Details

The model was fine-tuned for 60 steps as a proof-of-concept. Using LoRA, only **0.17%** of the model's parameters (13.6M out of 8.04B) were trained, allowing the training to complete in under 4 minutes on a single T4 GPU while preserving the general reasoning capabilities of Llama-3.

---

## 🛡️ The Gatekeeper (Security Layer)

To make the AI output production-ready, a deterministic validation layer is incorporated. Even if the LLM "hallucinates" or receives a malicious prompt, the Gatekeeper ensures system safety.

**Key features of the Gatekeeper:**

- **Keyword Filtering:** Blocks dangerous commands like `DROP`, `DELETE`, `TRUNCATE`, `ALTER`, `INSERT`, and `UPDATE`.
- **Read-Only Enforcement:** Ensures all queries start with the `SELECT` statement.
- **Syntax Scrubbing:** Cleans and formats the SQL string before it enters the execution queue.

---

## 📊 Stress Test Results

The system was evaluated by running a 1,000-question stress test against the Spider dataset.

| Metric                     | Result |
|----------------------------|--------|
| Total Queries Processed     | 1000   |
| ✅ Approved by Gatekeeper   | 1000   |
| 🚫 Blocked by Gatekeeper    | 0      |
| **Overall Safety Accuracy** | 100%   |

> **Note:** The 100% safety rate confirms that the model reliably adheres to the training format, consistently generating safe read-only queries.

---

## 🧠 Generalization Intelligence Metrics

To validate against overfitting, the model was tested on 50 unseen, synthetic schemas (e.g., `SpaceStation_Inventory`, `Hospital_Management`) that were not part of the Spider training dataset.

**Overall Generalization Accuracy: 96.00%**

| Metric | Precision | Recall | F1-Score | Support |
| :--- | :--- | :--- | :--- | :--- |
| **Success** | 0.96 | 1.00 | 0.98 | 48 |
| **Minor Syntax Slip** | 0.00 | 0.00 | 0.00 | 2 |

**Finding:** The model demonstrated zero hallucination of training-set metadata and correctly applied structural SQL aggregation logic to novel schemas, proving it has learned underlying SQL reasoning rather than memorizing dataset samples.

---

## 📂 Project Structure

```
├── sql_agent_notebook.ipynb   # Full training and testing pipeline
├── requirements.txt           # Environment dependencies
└── README.md                  # Project documentation
```

---

## 🚀 How to Run

1. Open the `.ipynb` file in Kaggle or Google Colab.
2. Enable a GPU accelerator (T4 recommended).
3. Install dependencies:
   ```
   pip install -r requirements.txt
   ```
4. Run all notebook cells to see training, inference, and the Gatekeeper in action.

---

## ✍️ Author

Created by [Ilyankhan69](https://github.com/Ilyankhan69)
