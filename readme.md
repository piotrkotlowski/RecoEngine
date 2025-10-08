# Recommendation System

This repository implements a **hybrid recommendation platform** with two components:

---

## 1. ALS-based Recommendation Engine

* Uses the **Alternating Least Squares (ALS)** algorithm for collaborative filtering implemented in PyTorch.
* **Redis** is leveraged for fast retrieval of user and item embeddings.
* The model is **retrained every 30 minutes** to adapt to new interactions and improve recommendation quality.

---

## 2. Conversational Recommendation Engine (LLM + RAG)

* Integrates **Large Language Models (LLMs)** with **Retrieval-Augmented Generation (RAG)** for context-aware, conversational recommendations.

---

## How to Run the Application

To run the entire hybrid recommendation platform, navigate to the `Prod/` directory and run the `start_services.sh` script.
This will build and launch the ALS engine, conversational chat engine, and all necessary data ingestion services automatically.

```bash
cd Prod
bash start_services.sh
```

Once the containers are up, the system will automatically initialize the ALS model training, set up Redis and Postgres for data storage, and launch the conversational recommendation API powered by the LLM + RAG pipeline.

---

## Project Structure

```
Prod/
│── als_trainer/              # ALS training pipeline
│   ├── params/               # Hyperparameters/config files
│   ├── alsEngine.py          # Core ALS training logic
│   ├── main.py               # Training entry point
│   ├── pytorchALS.py         # PyTorch ALS implementation
│
│── als_reco_engine/          # ALS-based recommendation serving
│   ├── main.py               # Serving entry point
│   ├── recommender.py        # Serving helper functions
│
│── chat_reco_engine/         # Conversational recommendation engine
│   ├── __init__.py
│   ├── chatEngine.py         # LangChain helpers
│   ├── main.py               # Chat engine entry point
│   ├── db.py                 # Postgres interaction functions
│   ├── pydanticHelper.py     # Validation schemas (Pydantic)
│
│── data_ingest/              # Source datasets
│   ├── interactions.csv      # User-item interactions
│   ├── items.csv             # Product metadata
│   ├── ProductsRAG.json      # Product catalog for RAG
│   ├── users.csv             # User metadata
│
│── docker_als_engine/        # Docker setup for ALS engine
│   ├── Dockerfile
│   ├── requirements.txt
│
│── docker_chat_engine/       # Docker setup for chat engine
│   ├── Dockerfile
│   ├── requirements.txt
│
│── docker_ingest_chroma/     # Docker setup for ChromaDB ingestion
│   ├── Dockerfile
│   ├── requirements.txt
│
│── docker_ingest_pg/         # Docker setup for Postgres ingestion
│   ├── Dockerfile
│   ├── requirements.txt
│
│── main_func/                
│   ├── main_func_als.py      # ALS example logic
│   ├── main_func_chat.py     # Chat example logic
│
│── scripts/                  # Data ingestion scripts
│   ├── ingest_chromadb.py    # Ingest data into ChromaDB
│   ├── ingest_postgres.py    # Ingest data into Postgres
│
│── .env                      # Environment variables
│── config.py                 # Global configs
│── docker-compose.yaml       # Multi-service orchestration
│── start_services.sh         # Script starting all functionalities
│
│── Modeling/                 # Offline experimentation
│   ├── Data/
│   │   ├── Cleaned/          # Processed datasets
│   │   ├── Raw/              # Raw datasets
│   │   ├── EDA/              # Exploratory analysis
│   ├── 00_EDA.ipynb
│   │
│   ├── RecoSystem/           # Experimental modules
│   ├── params/               # Modeling configs
│   ├── split/                # Train/test split utils
│   ├── als_modeling.ipynb    # ALS experiments
│   ├── als_pytorch.py        # PyTorch ALS experiments
```
