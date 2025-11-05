# 🚗 Vehicle Data MLOps Project

An **end-to-end Machine Learning Operations (MLOps)** project demonstrating a complete production-grade pipeline — from data ingestion and validation to model training, evaluation, deployment, and CI/CD automation using **AWS, Docker, GitHub Actions, MongoDB Atlas, and EC2**.

---

## 🧱 Project Overview

This project is designed to showcase how to build, train, deploy, and automate a machine learning system following MLOps best practices.  
It includes every essential stage of an ML pipeline — from **data ingestion and feature engineering** to **cloud integration (AWS)** and **continuous deployment** using **GitHub Actions**.

---

## ⚙️ Tech Stack

| Category | Tools / Technologies |
|-----------|----------------------|
| **Language** | Python 3.10 |
| **Environment** | Conda Virtual Environment |
| **Database** | MongoDB Atlas |
| **Cloud** | AWS (S3, EC2, IAM, ECR) |
| **Version Control** | Git, GitHub |
| **Automation** | GitHub Actions |
| **Containerization** | Docker |
| **Core Components** | Data Ingestion, Validation, Transformation, Model Trainer, Model Evaluation, Model Pusher |
| **Logging & Monitoring** | Custom Python Logging System |

---

## 🧩 Project Setup

### 1️⃣ Project Initialization
```bash
# Run template generator
python template.py
````

### 2️⃣ Local Package Setup

Write setup configurations:

* **setup.py**
* **pyproject.toml**

📄 Refer to `crashcourse.txt` for more details on `setup.py` and `pyproject.toml`.

---

### 3️⃣ Create and Activate Virtual Environment

```bash
conda create -n vehicle python=3.10 -y
conda activate vehicle
```

Then install all dependencies:

```bash
pip install -r requirements.txt
pip list   # verify installed packages
```

---

## 🍃 MongoDB Atlas Setup

<details>
<summary>📦 Click to Expand MongoDB Setup Steps</summary>

1. Sign up on [MongoDB Atlas](https://www.mongodb.com/cloud/atlas).
2. Create a new project → provide a name → click *Next* and *Create*.
3. On “Create a cluster” → choose **M0 (Free Tier)** → *Create Deployment*.
4. Set up **DB user** credentials.
5. In *Network Access*, add IP: `0.0.0.0/0` (access from anywhere).
6. Copy the **Connection String** for Python (Driver ≥ 3.6).
7. Add your **dataset** inside the `notebook/` folder.
8. Create `mongoDB_demo.ipynb` and push data to MongoDB.
9. Verify your dataset inside MongoDB Atlas → Database → Collections.

</details>

---

## 🧾 Logging, Exception Handling, and Notebooks

1. Implement **logger.py** and test via `demo.py`.
2. Implement **exception.py** and test via `demo.py`.
3. Add **EDA** and **Feature Engineering** notebooks.

---

## 🧮 Data Ingestion

<details>
<summary>🧠 Expand Data Ingestion Steps</summary>

1. Define constants in `constants/__init__.py`.
2. Create connection functions in `configuration/mongo_db_connections.py`.
3. In `data_access/`, implement `proj1_data` to connect to MongoDB and return data as DataFrame.
4. Define classes in:

   * `entity/config_entity.py` → `DataIngestionConfig`
   * `entity/artifact_entity.py` → `DataIngestionArtifact`
5. Implement component logic in `components/data_ingestion.py`.
6. Integrate it into **training pipeline** and run `demo.py`.

### 🌍 MongoDB Connection Setup

```bash
# Bash
export MONGODB_URL="mongodb+srv://<username>:<password>@cluster.mongodb.net/"
echo $MONGODB_URL

# PowerShell
$env:MONGODB_URL="mongodb+srv://<username>:<password>@cluster.mongodb.net/"
echo $env:MONGODB_URL
```

🗂 Add the `artifact` directory to `.gitignore`.

</details>

---

## 🔍 Data Validation, Transformation & Model Training

1. Complete `utils/main_utils.py` and `config/schema.yaml`.
2. Implement **Data Validation** → check schema, missing values, datatypes.
3. Implement **Data Transformation** → feature scaling, encoding, splitting.
4. Implement **Model Trainer** → model selection, training, metrics evaluation.

---

## ☁️ AWS Configuration

<details>
<summary>🔑 Expand AWS Setup Steps</summary>

1. **Login to AWS Console**

   * Region: `us-east-1`
   * Create IAM user: `firstproj` with `AdministratorAccess`
2. **Generate Access Keys**

   * Download the `.csv` credentials file
3. **Set Environment Variables**

```bash
# Bash
export AWS_ACCESS_KEY_ID="your_access_key"
export AWS_SECRET_ACCESS_KEY="your_secret_key"

# PowerShell
$env:AWS_ACCESS_KEY_ID="your_access_key"
$env:AWS_SECRET_ACCESS_KEY="your_secret_key"
```

4. Add these to `constants/__init__.py`:

```python
MODEL_EVALUATION_CHANGED_THRESHOLD_SCORE: float = 0.02
MODEL_BUCKET_NAME = "my.model.mlopsproj"
MODEL_PUSHER_S3_KEY = "model-registry"
```

5. **Create S3 Bucket**

   * Name: `my.model.mlopsproj`
   * Region: `us-east-1`
   * Uncheck: *Block all public access*
6. Add AWS utility code to:

   * `src/configuration/aws_connection.py`
   * `src/aws_storage/` (for S3 interactions)
   * `entity/s3_estimator.py` (for pull/push logic)

</details>

---

## 🤖 Model Evaluation & Model Pusher

* Implement `ModelEvaluation` and `ModelPusher` components.
* Evaluate model performance and push the model to S3 if improvement > threshold.

---

## 🧠 Prediction Pipeline and App Setup

1. Build **Prediction Pipeline** to serve predictions from trained models.
2. Setup `app.py`.
3. Add:

   * `/static`
   * `/template` directories.
4. Run app locally:

```bash
python app.py
```

---

## 🐳 CI/CD and Deployment

<details>
<summary>🚀 Expand CI-CD and Deployment Setup</summary>

### Step 1: Docker Setup

* Add `Dockerfile` and `.dockerignore`.

### Step 2: GitHub Actions Workflow

* Create `.github/workflows/aws.yaml`.

### Step 3: AWS IAM and ECR

* Create IAM user: `firstproj`
* Create ECR repo: `vehicleproj`
* Copy the ECR repository URI.

### Step 4: EC2 Setup

1. Launch Ubuntu instance (`t2.medium`, 30GB storage).
2. Allow HTTP and HTTPS traffic.
3. Connect to EC2 terminal.

### Step 5: Install Docker on EC2

```bash
sudo apt-get update -y
sudo apt-get upgrade
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
```

### Step 6: Connect GitHub Runner to EC2

* Go to **GitHub → Settings → Actions → Runners → New self-hosted runner**.
* Run the setup commands on EC2.

### Step 7: Add GitHub Secrets

```
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_DEFAULT_REGION
ECR_REPO
```

### Step 8: Enable EC2 Port

* Go to Security Groups → Edit Inbound Rules →
  Add rule: **Port 5000 → 0.0.0.0/0**

Now visit:
`http://<EC2-public-IP>:5000` → App is live 🎉

</details>

---

## 🧭 Project Structure

```
├── src/
│   ├── components/
│   ├── configuration/
│   ├── data_access/
│   ├── entity/
│   ├── aws_storage/
│   ├── utils/
│   └── pipeline/
│
├── notebook/
│   ├── mongoDB_demo.ipynb
│   ├── EDA.ipynb
│   └── Feature_Engineering.ipynb
│
├── app.py
├── setup.py
├── pyproject.toml
├── requirements.txt
├── Dockerfile
├── .dockerignore
├── .github/workflows/aws.yaml
└── README.md
```

---

## 🎯 Key Features

✅ Complete end-to-end ML pipeline
✅ MongoDB and AWS S3 integration
✅ Automated CI/CD with GitHub Actions
✅ Containerized with Docker
✅ Self-hosted EC2 runner
✅ Model versioning and cloud deployment


---

⭐ **If you like this project, give it a star!**
📌 *Contributions, issues, and feature requests are welcome*
