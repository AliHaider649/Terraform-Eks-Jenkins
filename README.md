# Terraform EKS Cluster Deployment with Jenkins CI/CD

## 🚀 Project Overview
This project provisions a **fully managed EKS (Elastic Kubernetes Service)** cluster using **Terraform** and automates the deployment pipeline using **Jenkins**.

### ✅ Key Features
- EKS cluster created using Terraform (latest stable version).
- Managed node groups using `t2.medium` instances.
- Remote S3 backend for Terraform state management.
- Jenkins CI/CD pipeline integrated for automation.
- Follows infrastructure-as-code best practices.

---

## ⚙️ Project Structure
```
terraform-eks-project/
├── main.tf
├── variables.tf
├── outputs.tf
├── backend.tf
├── provider.tf
└── jenkinsfile
└── README.md


    
```

---

## 🧱 Prerequisites
Ensure you have the following installed on your local machine:

- **AWS CLI** – Configured with your credentials (`aws configure`)
- **Terraform** – Version >= 1.5
- **kubectl** – To interact with the EKS cluster
- **Jenkins** – Installed locally or on an EC2 instance
- **Git** – For source control

---

## ☁️ Terraform Setup

### 1. Initialize Terraform
```bash
terraform init
```

### 2. Validate and Plan
```bash
terraform validate
terraform plan
```

### 3. Apply the Configuration
```bash
terraform apply -auto-approve
```

### 4. Configure `kubectl`
```bash
aws eks update-kubeconfig --region us-east-1 --name alihaider-eks-cluster
kubectl get nodes
```

---

## 🔧 Jenkins Setup (Windows)

### 1. Install Jenkins
Download and install Jenkins from: [https://www.jenkins.io/download](https://www.jenkins.io/download)

### 2. Start Jenkins Service
```bash
net start jenkins
```

Access Jenkins in your browser: `http://localhost:8080`

### 3. Install Required Plugins
- Terraform
- AWS Credentials
- Git
- Pipeline
- Kubernetes CLI

### 4. Configure Credentials
In Jenkins:
- Go to **Manage Jenkins → Credentials → Global → Add Credentials**
- Add your **AWS Access Key ID** and **Secret Key**

### 5. Create Jenkins Pipeline Job
- Go to **New Item → Pipeline**
- Name it `terraform-eks-pipeline`
- Under **Pipeline Definition**, choose “Pipeline script from SCM”
- Add your Git repository URL

---

## 🧩 Jenkinsfile Example

```groovy
pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"
        CLUSTER_NAME = "alihaider-eks-cluster"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo/terraform-eks-project.git'
            }
        }
        stage('Init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Validate') {
            steps {
                sh 'terraform validate'
            }
        }
        stage('Plan') {
            steps {
                sh 'terraform plan -out=tfplan'
            }
        }
        stage('Apply') {
            steps {
                sh 'terraform apply -auto-approve tfplan'
            }
        }
        stage('Verify Cluster') {
            steps {
                sh 'aws eks update-kubeconfig --region us-east-1 --name ${CLUSTER_NAME}'
                sh 'kubectl get nodes'
            }
        }
    }
    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
```

---

## 🧹 Cleanup Resources
When done, destroy all resources to avoid costs:
```bash
terraform destroy -auto-approve
```

---

## 🧠 Author
**Ali Haider**  
DevOps Engineer | AWS | Terraform | Jenkins | Kubernetes  
📧 [alihaid649@example.com] 

---

## 🏁 Summary
This project demonstrates production-grade infrastructure automation using Terraform and Jenkins, providing reusable IaC templates for EKS management and CI/CD deployment pipelines.
