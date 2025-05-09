# Hybrid Log Classification System with Deepseek R1 LLM, BERT, NLP, and Regex

This repository provides an end-to-end, production-ready system for automatic log message classification in enterprise environments. The pipeline combines traditional NLP (Regex), BERT-based machine learning, and GenAI (Deepseek R1 LLM), ensuring high accuracy, adaptability, and scalability for both common and rare log types. The system is deployable as a FastAPI service and is designed for real-world IT operations, security monitoring, and automated incident detection.

---

## Project Architecture

![Architecture](arch.jpg)

**Workflow:**
1. **Regex Classification:**  
   - Log messages are first checked against a set of regular expressions for known, frequent patterns (e.g., "Backup completed successfully.").
   - If a match is found, the log is instantly classified (e.g., as "System Notification" or "User Action").

2. **Unknown Class Handling:**  
   - If the log does **not** match any regex pattern:
     - If there are **enough labeled training samples**, it is classified using BERT embeddings and a logistic regression model.
     - If there are **not enough samples**, it is sent to the Deepseek R1 LLM for few-shot or prompt-based classification (ideal for rare or novel log types).

---

## Key Features

- **Hybrid AI Pipeline:**  
  Combines Regex, BERT + Logistic Regression, and LLM (Deepseek R1) for robust, accurate log classification.
- **Automated Regex Discovery:**  
  Uses DBSCAN clustering to identify new log patterns and expand regex coverage.
- **Scalable and FastAPI-Ready:**  
  Batch CSV classification via REST API for easy integration into enterprise workflows.
- **Real-World Impact:**  
  Enables faster error, threat, and event detection, reducing manual log review and improving IT operations.

---

## Real-World Use Cases

- **IT Operations:**  
  Instantly categorize millions of system logs to detect errors, warnings, and notifications.
- **Security Monitoring:**  
  Automatically flag suspicious activities (e.g., unauthorized access, privilege escalation).
- **Compliance & Auditing:**  
  Ensure critical events are logged and categorized for regulatory compliance and reporting.
- **Cost Optimization:**  
  Regex and BERT handle most logs efficiently; LLM is used only for rare/complex cases.

---

## Example Log Types and Labels

| Log Message Example                                            | Classified Label         | Method Used   |
|---------------------------------------------------------------|-------------------------|---------------|
| "Backup completed successfully."                              | System Notification     | Regex         |
| "Unauthorized access to data was attempted"                   | Security Alert          | BERT          |
| "API endpoint 'getCustomerDetails' is deprecated"             | Deprecation Warning     | LLM           |
| "User User243 logged in."                                     | User Action             | Regex         |
| "Critical system unit error: unit ID Component55"             | Critical Error          | BERT          |

---

## Project Steps

1. **Data Collection & Exploration:**  
   - Aggregate and clean logs (see `synthetic_logs.csv`).
2. **Regex Pattern Discovery:**  
   - Use DBSCAN clustering (see `training.ipynb`) to find new patterns.
3. **Model Training:**  
   - Train BERT + logistic regression for embedding-based classification.
4. **Pipeline Integration:**  
   - Implement hybrid logic in `classify.py`.
5. **API Deployment:**  
   - Deploy with FastAPI (`server.py`) for batch log classification.

---

## Getting Started

1. **Install dependencies:**
    ```
    pip install -r requirements.txt
    ```
2. **Run the FastAPI server:**
    ```
    uvicorn server:app --reload
    ```
3. **Classify logs:**
   - Upload a CSV file (`source`, `log_message`) to the `/classify/` endpoint.
   - Receive a CSV with an additional `target_label` column.

---

## Tech Stack

- Python, Pandas, scikit-learn, Sentence Transformers, joblib
- Deepseek R1 LLM (via groq)
- FastAPI
- DBSCAN for pattern discovery

---

## File Structure

- `processor_regex.py` – Regex-based classifier
- `processor_bert.py` – BERT embedding + logistic regression classifier
- `processor_llm.py` – LLM-based classifier (Deepseek R1)
- `classify.py` – Orchestrates the hybrid pipeline
- `server.py` – FastAPI backend for batch CSV classification
- `training.ipynb` – Data exploration, clustering, and model training
- `synthetic_logs.csv` – Example dataset

---

## Why Hybrid AI?

- **Speed:** Regex handles the majority of logs instantly.
- **Accuracy:** BERT and LLMs handle complex and rare cases.
- **Scalability:** Adapts to new log types and large data volumes.
- **Cost Efficiency:** LLMs are used only when necessary, keeping operational costs low.

---

**Ideal for IT, DevOps, and security teams seeking robust, adaptable, and cost-efficient log analysis.**
