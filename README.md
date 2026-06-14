# 🩺 Kidney Disease Classification using Deep Learning

A deep learning web application that classifies kidney CT scan images into four categories — **Cyst**, **Normal**, **Stone**, and **Tumor** — using a fine-tuned VGG16 model. The project follows an end-to-end MLOps pipeline with **MLflow** for experiment tracking and **DVC** for pipeline orchestration.

---

## 📌 Project Overview

| Feature | Details |
|---|---|
| **Task** | Multi-class image classification |
| **Classes** | Cyst · Normal · Stone · Tumor |
| **Base Model** | VGG16 (ImageNet weights, fine-tuned) |
| **Input Size** | 224 × 224 × 3 |
| **Backend** | Flask (REST API) |
| **Tracking** | MLflow + DagsHub |
| **Pipeline** | DVC |

---

## 🗂️ Project Structure

```
Kedney-Disease-Classification/
│
├── src/cnnClassifier/
│   ├── components/         # Data ingestion, model prep, training, evaluation
│   ├── config/             # Configuration manager
│   ├── constants/          # Path constants
│   ├── entity/             # Dataclass config entities
│   ├── pipeline/           # Stage-wise pipeline scripts + prediction
│   └── utils/              # Common utility functions
│
├── config/config.yaml      # Main configuration file
├── params.yaml             # Hyperparameter definitions
├── dvc.yaml                # DVC pipeline stages
├── main.py                 # Runs the full training pipeline
├── app.py                  # Flask web application
├── requirements.txt
└── Dockerfile
```

---

## ⚙️ Workflow

Each stage follows this update order:

1. `config/config.yaml`
2. `params.yaml`
3. Entity classes
4. Configuration manager
5. Components
6. Pipeline stages
7. `main.py`
8. `dvc.yaml`
9. `app.py`

---

## 🚀 How to Run

### Step 1 — Clone the Repository

```bash
git clone https://github.com/tuhin1522/Kedney-Disease-Classification
cd Kedney-Disease-Classification
```

### Step 2 — Create a Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate
```

### Step 3 — Install Requirements

```bash
pip install -r requirements.txt
```

### Step 4 — Run the Training Pipeline

```bash
python3 main.py
```

### Step 5 — Start the Web Application

```bash
python3 app.py
```

Then open your browser at: **http://localhost:8000**

---

## 🧪 Model Hyperparameters

| Parameter | Value |
|---|---|
| Image Size | 224 × 224 × 3 |
| Batch Size | 16 |
| Epochs | 1 |
| Classes | 4 |
| Base Weights | ImageNet |
| Learning Rate | 0.01 |
| Augmentation | True |

---

## 📊 MLflow — Experiment Tracking

Track experiments locally or remotely via DagsHub.

#### Run the MLflow UI locally:
```bash
mlflow ui
```

#### Set up DagsHub remote tracking:
```bash
export MLFLOW_TRACKING_URI=https://dagshub.com/<your-username>/Kedney-Disease-Classification.mlflow
export MLFLOW_TRACKING_USERNAME=<your-dagshub-username>
export MLFLOW_TRACKING_PASSWORD=<your-dagshub-token>
```

> 🔗 [MLflow Documentation](https://mlflow.org/docs/latest/index.html) &nbsp;|&nbsp; [DagsHub](https://dagshub.com/)

---

## 🔁 DVC — Pipeline Orchestration

DVC manages the 4-stage reproducible pipeline:

| Stage | Script |
|---|---|
| Data Ingestion | `stage_01_data_ingestion.py` |
| Prepare Base Model | `stage_02_prepare_base_model.py` |
| Model Training | `stage_03_model_training.py` |
| Model Evaluation | `stage_04_model_evaluation.py` |

```bash
# Initialize DVC
dvc init

# Reproduce the full pipeline
dvc repro

# Visualize the DAG
dvc dag
```

---

## 🌐 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | Serves the web UI |
| `GET/POST` | `/train` | Triggers model training |
| `POST` | `/predict` | Returns prediction for uploaded image |

#### Prediction Request Format (`/predict`):
```json
{
  "image": "<base64-encoded-image-string>"
}
```

#### Response:
```json
[{ "image": "Cyst" }]
```

---

## 🐳 Docker

```bash
# Build the image
docker build -t kidney-disease-classifier .

# Run the container
docker run -p 8000:8000 kidney-disease-classifier
```

---

## ☁️ AWS Deployment (CI/CD with GitHub Actions)

### 1. Create an IAM User with the following policies:
- `AmazonEC2ContainerRegistryFullAccess`
- `AmazonEC2FullAccess`

### 2. Create an ECR Repository
Save your ECR URI, e.g.:
```
<account-id>.dkr.ecr.<region>.amazonaws.com/kidney-classifier
```

### 3. Launch an EC2 Instance (Ubuntu) and install Docker:
```bash
sudo apt-get update -y && sudo apt-get upgrade -y
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
```

### 4. Configure EC2 as a GitHub Self-Hosted Runner:
Go to `Settings > Actions > Runners > New self-hosted runner` and follow the instructions.

### 5. Add GitHub Secrets:

| Secret | Value |
|---|---|
| `AWS_ACCESS_KEY_ID` | Your IAM access key |
| `AWS_SECRET_ACCESS_KEY` | Your IAM secret key |
| `AWS_REGION` | e.g. `us-east-1` |
| `AWS_ECR_LOGIN_URI` | Your ECR URI |
| `ECR_REPOSITORY_NAME` | e.g. `kidney-classifier` |

---

## 📦 Tech Stack

- **Deep Learning**: TensorFlow / Keras (VGG16)
- **Web Framework**: Flask, Flask-CORS
- **Experiment Tracking**: MLflow, DagsHub
- **Pipeline Orchestration**: DVC
- **Data Download**: gdown
- **Containerization**: Docker
- **CI/CD**: GitHub Actions + AWS EC2 + ECR

---

## 👤 Author

**Md Tuhin Molla** — [GitHub](https://github.com/tuhin1522)
