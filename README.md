# 🚀 Deploying Flask on AWS with Docker, ECR, and EKS

This project demonstrates how to deploy a **Flask monitoring application** on **AWS** using:

- 🐳 **Docker** for containerization  
- 📦 **Amazon ECR** for container image storage  
- ☸️ **Amazon EKS** for Kubernetes orchestration  

> The repository includes all source code, configuration, and automation scripts required for local testing, containerization, and deployment.

---

## 📂 Project Structure

```
.
├── app.py                  # Flask app entrypoint
├── requirements.txt        # Python dependencies
├── Dockerfile              # Containerization config
├── eks.py                  # EKS deployment using Kubernetes Python client
└── README.md               # You're here!
```

---

## 📋 Prerequisites

Make sure the following are installed/configured:

- ✅ AWS account with programmatic access
- ✅ AWS CLI configured (`aws configure`)
- ✅ Python 3 & pip
- ✅ Docker
- ✅ kubectl & IAM access to create EKS resources
- ✅ A code editor (like VS Code)

---

## 🔧 Run Flask App Locally

```bash
git clone <your-repo-url>
cd <repo-directory>

# Install dependencies
pip3 install -r requirements.txt

# Run Flask app
python3 app.py
```

Visit `http://localhost:5000` in your browser to verify the app is working.

---

## 🐳 Dockerize the App

### Build Docker Image

```bash
docker build -t my-flask-app .
```

### Run Docker Container

```bash
docker run -p 5000:5000 my-flask-app
```

Test at: `http://localhost:5000`

---

## ☁️ Push Docker Image to Amazon ECR

### Create ECR Repository (Python Script)

```python
# run this snippet using boto3 (optional utility script)
import boto3

ecr = boto3.client('ecr')
repo_name = "my_monitoring_app_image"

response = ecr.create_repository(repositoryName=repo_name)
print(response['repository']['repositoryUri'])
```

### Authenticate & Push

```bash
# Authenticate Docker with ECR
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account_id>.dkr.ecr.<region>.amazonaws.com

# Tag & Push
docker tag my-flask-app <ecr_repo_uri>:latest
docker push <ecr_repo_uri>:latest
```

---

## ☸️ Deploy to EKS

### 1. Create EKS Cluster

Create via AWS Console or with `eksctl`. Ensure the cluster is active and `kubectl` is configured to use it.

### 2. Configure IAM Role & Security Group

- IAM Role: Attach `AmazonEKSServiceRolePolicy`
- Security Group: Allow inbound traffic on port `5000`

### 3. Create Node Group

Add a managed node group from the AWS Console or using `eksctl`.

---

### 4. Deploy with `eks.py`

Use the provided `eks.py` to create a Kubernetes deployment and service:

```bash
python3 eks.py
```

Make sure to update the image URI inside `eks.py`:

```python
image="ACCOUNT_ID.dkr.ecr.REGION.amazonaws.com/my_monitoring_app_image:latest"
```

---

### 5. Verify & Access the App

```bash
kubectl get deployments
kubectl get services
kubectl get pods
```

Port-forward the service to localhost:

```bash
kubectl port-forward service/my-flask-service 5000:5000
```

Then access: `http://localhost:5000`

---

## ✅ Summary

You have successfully:

- ✅ Built and containerized a Flask application
- ✅ Stored the image in Amazon ECR
- ✅ Deployed it to EKS using Kubernetes
- ✅ Exposed the app using Kubernetes service and `port-forward`

---

## 🙌 Author

**Het Patel**  
- AWS Certified | Terraform Certified | Azure Fundamentals  
- 📧 hetpatel7131@gmail.com  
- 🌐 [Medium](https://medium.com/@hetpatel7131) | [LinkedIn](https://www.linkedin.com/in/hetpatel7131/)

---
