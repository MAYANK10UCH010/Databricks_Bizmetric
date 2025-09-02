# Question  
Can we practice Auto ML over Databricks Free Edition. Suggest the best source and Strategy for preparing for Databricks certifications.  
Starting with GenAI Associate, Machine learning Associate and Data Engineer Associate followed by Professional Certification. If they can be ranked based on Level of difficult do that for me as well.  

---

# Answer  

Great set of questions 👍 — you’re thinking about this the right way. Let’s break it into two parts: **(1) AutoML on Databricks Free Edition** and **(2) Strategy for preparing for Databricks certifications (ranked by difficulty).**  

---

## 1️⃣ AutoML on Databricks Free Edition
- **Databricks AutoML** (the built-in low-code ML feature) is available only on **paid tiers (Standard/Premium/Enterprise)**, **not** on the Free Community Edition.  
- On **Free Edition**, you can still:
  - Use **MLlib (Spark ML)** pipelines manually for classification/regression.
  - Build AutoML-like workflows by integrating **Hyperopt**, **MLflow**, or open-source **FLAML/Auto-sklearn** inside Databricks.
  - Train small models on structured datasets, log experiments with **MLflow**, and simulate hyperparameter sweeps.
- ✅ Strategy: If your main goal is **learning**, Free Edition is enough. For hands-on **cert exam prep**, you don’t need AutoML; Spark ML, MLflow, and feature engineering practice are more important.  
- If you want **true Databricks AutoML practice**, you’ll need a **trial (14-day)** or **work/school Databricks account**.

---

# Direct Links 

# Hands-on Resources for Databricks Certification Preparation  

## ❓ Question  
Give me some links to try hands on below given topics  

---

## ✅ Answer  

Here are **direct links** where you can practice hands-on for the topics in the 3-month study plan. They are grouped by certification: **GenAI Associate, Data Engineer Associate, and Machine Learning Associate**.  

---

## 🔹 GenAI Associate (Weeks 1–2)  
**Hands-on notebooks & resources**  
- [Databricks Academy – Generative AI Fundamentals](https://customer-academy.databricks.com/learn/course/external/view/elearning/328/generative-ai-fundamentals) (official free course).  
- [Hugging Face course](https://huggingface.co/course/chapter1) (free, practical LLM hands-on).  
- [FAISS for Vector Search](https://github.com/facebookresearch/faiss) → includes example notebooks.  
- [MLflow for LLMs (Databricks Blog Example)](https://www.databricks.com/blog/mlflow-llms-manage-and-deploy-large-language-models-mlflow) → shows how to log and serve LLMs.  

---

## 🔹 Data Engineer Associate (Weeks 3–6)  
**Hands-on Spark & Delta**  
- [Databricks Academy – Data Engineer Associate Learning Path](https://customer-academy.databricks.com/learn/learning_plan/view/27/data-engineer-learning-plan-associate) (official).  
- [Databricks Sample Datasets Repo](https://github.com/databricks/Spark-The-Definitive-Guide/tree/master/data) → small datasets for CE.  
- [Delta Lake Quickstart (Databricks docs)](https://docs.databricks.com/en/delta/delta-quickstart.html) → with Python & SQL examples.  
- [Delta Lake GitHub Quickstart](https://github.com/delta-io/delta/tree/master/examples) → end-to-end examples you can adapt to CE.  
- [Apache Spark SQL Guide](https://spark.apache.org/docs/latest/sql-programming-guide.html) → to practice advanced queries.  

---

## 🔹 Machine Learning Associate (Weeks 7–12)  
**Hands-on MLflow & Spark ML**  
- [Databricks Academy – Machine Learning Associate Learning Path](https://customer-academy.databricks.com/learn/learning_plan/view/36/machine-learning-associate-learning-plan) (official).  
- [MLflow Quickstart Tutorial](https://mlflow.org/docs/latest/quickstart.html) → run in CE to log metrics & models.  
- [Spark MLlib Guide](https://spark.apache.org/docs/latest/ml-guide.html) → pipelines, transformers, estimators.  
- [Feature Store Concepts (Databricks docs)](https://docs.databricks.com/en/machine-learning/feature-store/index.html) → can simulate in CE using Delta tables.  
- [Sample ML Notebooks for Databricks](https://github.com/databricks/databricks-example-notebooks) → covers MLflow, Spark ML, Delta integration.  

---

## 🔹 Practice Exams & Extra Prep  
- [Databricks Practice Exams – Community GitHub](https://github.com/databricks-academy) → has open practice quizzes.  
- [Exam Study Guide – Official](https://www.databricks.com/learn/certification) → each certification page has a study guide PDF.  
- [Kaggle Datasets](https://www.kaggle.com/datasets) → to pull small datasets into Databricks CE for practice.  

---
