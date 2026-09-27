# 🤖 AWS Bedrock + Kubernetes + RAG + DevOps AI Assistant

![AWS](https://img.shields.io/badge/AWS-Bedrock-orange?logo=amazon-aws)
![Kubernetes](https://img.shields.io/badge/Kubernetes-1.30%2B-326CE5?logo=kubernetes)
![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python)
![FastAPI](https://img.shields.io/badge/FastAPI-API-009688?logo=fastapi)
![RAG](https://img.shields.io/badge/AI-RAG-purple)
![Docker](https://img.shields.io/badge/Docker-Container-blue?logo=docker)
![EKS](https://img.shields.io/badge/AWS-EKS-orange?logo=amazon-aws)
![Terraform](https://img.shields.io/badge/IaC-Terraform-7B42BC?logo=terraform)
![Status](https://img.shields.io/badge/Project-Hands--On-success)

> 🚀 A production-oriented **AI-powered Kubernetes troubleshooting assistant** using **Amazon Bedrock, RAG, Kubernetes APIs, Prometheus, Loki, FastAPI, Docker, and Amazon EKS**.

---

## 📌 Project Overview

Traditional Kubernetes troubleshooting requires engineers to manually inspect:

```text
kubectl logs
kubectl describe
kubectl get events
kubectl get pods
kubectl get services
Prometheus
Grafana
Loki
Runbooks
Documentation
```

This project combines these sources with **Amazon Bedrock + RAG** to create a DevOps AI assistant.

The assistant can analyze:

* Kubernetes resources
* Pod status
* Deployment status
* Kubernetes events
* Application logs
* Prometheus metrics
* Loki logs
* DevOps runbooks
* Architecture documentation

and provide an evidence-based troubleshooting response.

---

# 🎯 Project Goals

Build an AI assistant capable of answering questions such as:

```text
Why is my frontend pod failing?

Why is my application in CrashLoopBackOff?

Why is my service returning HTTP 503?

Why is my deployment unavailable?

Why is Redis connection failing?

What caused this Kubernetes error?

How should I troubleshoot ImagePullBackOff?

Explain this incident using our runbook.
```

---

# 🏗️ Architecture

```text
                              ┌──────────────────────┐
                              │      Developer       │
                              └──────────┬───────────┘
                                         │
                                         ▼
                              ┌──────────────────────┐
                              │ Web UI / CLI / API   │
                              └──────────┬───────────┘
                                         │
                                         ▼
                              ┌──────────────────────┐
                              │    FastAPI Backend   │
                              └──────────┬───────────┘
                                         │
                    ┌────────────────────┼────────────────────┐
                    │                    │                    │
                    ▼                    ▼                    ▼
           ┌────────────────┐   ┌────────────────┐   ┌────────────────┐
           │ Kubernetes API │   │ RAG Knowledge   │   │ Observability  │
           │                │   │ Base            │   │                │
           └───────┬────────┘   └───────┬────────┘   └───────┬────────┘
                   │                    │                    │
                   ▼                    ▼                    ▼
          ┌────────────────┐     ┌────────────┐      ┌───────────────┐
          │ Pods           │     │ S3         │      │ Prometheus    │
          │ Deployments    │     │ Runbooks   │      │ Loki          │
          │ Services       │     │ Docs       │      │ Metrics/Logs  │
          │ Events         │     └────────────┘      └───────────────┘
          └───────┬────────┘
                  │
                  └────────────────┬─────────────────────┐
                                   ▼                     │
                         ┌──────────────────────┐        │
                         │ Context Builder      │◄───────┘
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │    Amazon Bedrock    │
                         │                      │
                         │   Foundation Model   │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │      Guardrails      │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Evidence-Based AI    │
                         │ Troubleshooting      │
                         └──────────────────────┘
```

---

# 🔄 AI Troubleshooting Workflow

```text
User Question
      │
      ▼
FastAPI API
      │
      ├──────────────► Kubernetes API
      │                      │
      │                      ├── Pods
      │                      ├── Services
      │                      ├── Events
      │                      └── Deployments
      │
      ├──────────────► Prometheus
      │                      │
      │                      └── Metrics
      │
      ├──────────────► Loki
      │                      │
      │                      └── Application Logs
      │
      └──────────────► Bedrock Knowledge Base
                             │
                             ├── Runbooks
                             ├── Architecture
                             └── Troubleshooting Docs
                                      │
                                      ▼
                              Context Builder
                                      │
                                      ▼
                              Amazon Bedrock
                                      │
                                      ▼
                                AI Response
```

---

# 🧠 Why RAG?

A foundation model alone does not automatically know your:

* Internal runbooks
* Kubernetes architecture
* Application configuration
* Incident history
* Organization-specific procedures

RAG adds relevant documentation to the model context.

```text
                 User Question
                       │
                       ▼
              ┌────────────────┐
              │ Retrieve Docs  │
              └───────┬────────┘
                      │
                      ▼
             Relevant Runbooks
                      │
                      ▼
              Kubernetes State
                      │
                      ▼
                 Context
                      │
                      ▼
              Amazon Bedrock
                      │
                      ▼
                AI Response
```

---

# 🧰 Technology Stack

| Technology              | Purpose                    |
| ----------------------- | -------------------------- |
| Amazon Bedrock          | Foundation model inference |
| Bedrock Knowledge Bases | RAG                        |
| Amazon S3               | Document storage           |
| Kubernetes              | Container orchestration    |
| Amazon EKS              | Managed Kubernetes         |
| Python                  | Backend development        |
| FastAPI                 | REST API                   |
| Boto3                   | AWS SDK                    |
| Prometheus              | Metrics                    |
| Loki                    | Logs                       |
| Grafana                 | Observability              |
| Docker                  | Containerization           |
| Terraform               | Infrastructure as Code     |
| IAM                     | Security                   |
| EKS Pod Identity        | AWS workload identity      |

---

# 📁 Repository Structure

```text
aws-bedrock-k8s-devops-ai/
│
├── README.md
│
├── backend/
│   ├── Dockerfile
│   ├── requirements.txt
│   └── app/
│       ├── __init__.py
│       ├── main.py
│       ├── bedrock.py
│       ├── kubernetes.py
│       ├── rag.py
│       ├── prometheus.py
│       ├── loki.py
│       └── prompts.py
│
├── frontend/
│   ├── Dockerfile
│   └── ...
│
├── knowledge-base/
│   ├── runbooks/
│   │   ├── crashloopbackoff.md
│   │   ├── imagepullbackoff.md
│   │   ├── pod-pending.md
│   │   ├── service-503.md
│   │   ├── oomkilled.md
│   │   └── redis-errors.md
│   │
│   └── architecture/
│       ├── kubernetes.md
│       ├── networking.md
│       └── observability.md
│
├── k8s/
│   ├── namespace.yaml
│   ├── serviceaccount.yaml
│   ├── clusterrole.yaml
│   ├── clusterrolebinding.yaml
│   ├── deployment.yaml
│   ├── service.yaml
│   └── networkpolicy.yaml
│
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── iam.tf
│   └── eks.tf
│
└── scripts/
    ├── build.sh
    ├── deploy.sh
    └── test.sh
```

---

# 🚀 Getting Started

## 1. Clone Repository

```bash
git clone https://github.com/YOUR_USERNAME/aws-bedrock-k8s-devops-ai.git

cd aws-bedrock-k8s-devops-ai
```

---

# 2. Configure AWS CLI

Verify:

```bash
aws --version
```

Configure:

```bash
aws configure
```

Verify authentication:

```bash
aws sts get-caller-identity
```

Example:

```json
{
  "UserId": "...",
  "Account": "...",
  "Arn": "arn:aws:iam::123456789012:user/devops"
}
```

---

# 3. Configure AWS Region

Set your Bedrock-supported Region:

```bash
export AWS_REGION=us-east-1
```

Windows PowerShell:

```powershell
$env:AWS_REGION="us-east-1"
```

Verify:

```bash
echo $AWS_REGION
```

> Model availability varies by AWS Region. Verify that your selected model is available before running the application.

---

# 4. Check Available Bedrock Models

```bash
aws bedrock list-foundation-models \
  --region "$AWS_REGION" \
  --query 'modelSummaries[].modelId' \
  --output text
```

Example model:

```text
amazon.nova-lite-v1:0
```

Set:

```bash
export BEDROCK_MODEL_ID=amazon.nova-lite-v1:0
```

---

# 5. IAM Permissions

The application requires appropriate Bedrock permissions.

For model invocation:

```text
bedrock:InvokeModel
```

For the `Converse` API, ensure the identity is permitted to invoke the selected model.

For S3:

```text
s3:GetObject
```

For later Bedrock Knowledge Base components, additional permissions are required.

### Security principle

```text
Least Privilege
       │
       ▼
IAM Role
       │
       ├── Bedrock
       └── S3
```

Do not store AWS access keys inside:

```text
Docker images
Kubernetes Secrets
Git repositories
Python source code
.env files committed to Git
```

---

# 6. Test Bedrock Before Kubernetes

Install dependencies:

```bash
python -m pip install boto3
```

Test:

```bash
python -c "import boto3; print(boto3.__version__)"
```

Create:

```text
backend/test_bedrock.py
```

```python
import os
import boto3

region = os.getenv("AWS_REGION", "us-east-1")
model_id = os.getenv(
    "BEDROCK_MODEL_ID",
    "amazon.nova-lite-v1:0"
)

client = boto3.client(
    "bedrock-runtime",
    region_name=region
)

response = client.converse(
    modelId=model_id,
    messages=[
        {
            "role": "user",
            "content": [
                {
                    "text": "Explain Kubernetes CrashLoopBackOff in simple terms."
                }
            ]
        }
    ],
    inferenceConfig={
        "maxTokens": 500,
        "temperature": 0.2
    }
)

print(
    response["output"]["message"]["content"][0]["text"]
)
```

Run:

```bash
python backend/test_bedrock.py
```

Expected:

```text
CrashLoopBackOff means Kubernetes repeatedly starts
a container and the container keeps failing...
```

---

# 7. Kubernetes Development Environment

You can start locally with:

```text
Docker Desktop Kubernetes
```

or:

```text
kind
```

or:

```text
minikube
```

Production deployment:

```text
Amazon EKS
```

Verify:

```bash
kubectl cluster-info
```

```bash
kubectl get nodes
```

---

# 8. Create Namespace

```bash
kubectl create namespace devops-ai
```

Verify:

```bash
kubectl get namespace devops-ai
```

---

# 9. Kubernetes RBAC

The AI assistant should initially have **read-only access**.

Example:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: devops-ai-reader
rules:
  - apiGroups: [""]
    resources:
      - pods
      - services
      - events
      - configmaps
    verbs:
      - get
      - list
      - watch

  - apiGroups: ["apps"]
    resources:
      - deployments
      - replicasets
      - statefulsets
      - daemonsets
    verbs:
      - get
      - list
      - watch
```

Apply:

```bash
kubectl apply -f k8s/clusterrole.yaml
```

---

# 🔐 Security Model

Start with:

```text
AI Assistant
     │
     ▼
Read-only Kubernetes RBAC
```

Allowed:

```text
get
list
watch
```

Avoid initially:

```text
create
update
patch
delete
```

---

# 10. Kubernetes Python Client

Install:

```bash
pip install kubernetes
```

Example:

```python
from kubernetes import client, config


def load_kubernetes():

    try:
        config.load_incluster_config()

    except config.ConfigException:
        config.load_kube_config()


load_kubernetes()

core = client.CoreV1Api()
apps = client.AppsV1Api()


def get_pods(namespace):

    pods = core.list_namespaced_pod(namespace)

    return [
        {
            "name": pod.metadata.name,
            "phase": pod.status.phase,
            "node": pod.spec.node_name
        }
        for pod in pods.items
    ]


def get_deployments(namespace):

    deployments = apps.list_namespaced_deployment(namespace)

    return [
        {
            "name": deployment.metadata.name,
            "desired": deployment.spec.replicas,
            "available": deployment.status.available_replicas or 0
        }
        for deployment in deployments.items
    ]
```

---

# 11. Bedrock Service

Create:

```text
backend/app/bedrock.py
```

```python
import os
import boto3


REGION = os.getenv(
    "AWS_REGION",
    "us-east-1"
)

MODEL_ID = os.getenv(
    "BEDROCK_MODEL_ID",
    "amazon.nova-lite-v1:0"
)


client = boto3.client(
    "bedrock-runtime",
    region_name=REGION
)


def ask_bedrock(prompt):

    response = client.converse(
        modelId=MODEL_ID,

        messages=[
            {
                "role": "user",
                "content": [
                    {
                        "text": prompt
                    }
                ]
            }
        ],

        inferenceConfig={
            "maxTokens": 1000,
            "temperature": 0.2
        }
    )

    return response[
        "output"
    ][
        "message"
    ][
        "content"
    ][0]["text"]
```

---

# 12. Context Builder

The AI should receive structured evidence.

Example:

```python
def build_prompt(
    question,
    kubernetes_data,
    runbook_context,
    metrics,
    logs
):

    return f"""
You are a Kubernetes DevOps troubleshooting assistant.

Answer using only the supplied evidence.

USER QUESTION:
{question}

KUBERNETES DATA:
{kubernetes_data}

RUNBOOK CONTEXT:
{runbook_context}

PROMETHEUS DATA:
{metrics}

LOKI LOGS:
{logs}

Return:

1. Problem
2. Evidence
3. Possible Causes
4. Commands to Run
5. Recommended Next Step
6. Verification

Rules:

- Do not invent cluster state.
- Do not claim a command was executed unless its result is provided.
- Clearly distinguish evidence from hypotheses.
- Prefer safe read-only troubleshooting commands.
"""
```

---

# 13. FastAPI Application

Create:

```text
backend/app/main.py
```

```python
from fastapi import FastAPI
from pydantic import BaseModel

from .kubernetes import get_pods
from .bedrock import ask_bedrock


app = FastAPI(
    title="DevOps AI Assistant",
    version="1.0.0"
)


class Question(BaseModel):

    question: str
    namespace: str = "default"


@app.get("/health")
def health():

    return {
        "status": "healthy"
    }


@app.post("/ask")
def ask(request: Question):

    pods = get_pods(
        request.namespace
    )

    prompt = f"""
You are a Kubernetes DevOps assistant.

Namespace:
{request.namespace}

Current Pods:
{pods}

Question:
{request.question}

Return:

Problem:
Evidence:
Possible Causes:
Commands to Run:
Recommended Next Step:
"""

    answer = ask_bedrock(prompt)

    return {
        "question": request.question,
        "answer": answer,
        "kubernetes": {
            "pods": pods
        }
    }
```

---

# 14. Run FastAPI

```bash
cd backend
```

```bash
uvicorn app.main:app --reload
```

Open:

```text
http://localhost:8000/docs
```

Health check:

```bash
curl http://localhost:8000/health
```

Expected:

```json
{
  "status": "healthy"
}
```

---

# 15. Test AI Troubleshooting

```bash
curl -X POST \
  http://localhost:8000/ask \
  -H "Content-Type: application/json" \
  -d '{
    "question": "Why is my frontend pod failing?",
    "namespace": "roboshop"
  }'
```

Expected response:

```json
{
  "question": "Why is my frontend pod failing?",
  "answer": "Based on the available Kubernetes evidence...",
  "kubernetes": {
    "pods": []
  }
}
```

---

# 📚 RAG Knowledge Base

Create:

```text
knowledge-base/runbooks/
```

Recommended runbooks:

```text
crashloopbackoff.md
imagepullbackoff.md
pod-pending.md
oomkilled.md
service-503.md
redis-errors.md
mysql-errors.md
dns-errors.md
ingress-errors.md
```

---

# 16. CrashLoopBackOff Runbook

Example:

````markdown
# CrashLoopBackOff

## Symptoms

A container starts and repeatedly terminates.

## Investigation

```bash
kubectl get pod <pod> -n <namespace>

kubectl describe pod <pod> -n <namespace>

kubectl logs <pod> -n <namespace>

kubectl logs <pod> -n <namespace> --previous

kubectl get events \
  -n <namespace> \
  --sort-by=.lastTimestamp
````

## Common Causes

* Application crash
* Invalid environment variables
* Missing Secret
* Missing ConfigMap
* Failed probes
* Insufficient memory
* Dependency failure

## Verification

Check:

* Container exit code
* Application logs
* Events
* Probe configuration
* Resource limits

````

---

# 17. Upload RAG Documents

Create an S3 bucket:

```bash
aws s3 mb s3://YOUR-UNIQUE-BEDROCK-RAG-BUCKET
````

Upload:

```bash
aws s3 cp knowledge-base/ \
  s3://YOUR-UNIQUE-BEDROCK-RAG-BUCKET/ \
  --recursive
```

Verify:

```bash
aws s3 ls \
  s3://YOUR-UNIQUE-BEDROCK-RAG-BUCKET/ \
  --recursive
```

Then configure the bucket as the data source for an Amazon Bedrock Knowledge Base.

---

# 🔎 RAG Pipeline

```text
Runbooks
   │
   ▼
S3
   │
   ▼
Bedrock Knowledge Base
   │
   ▼
Embeddings
   │
   ▼
Vector Store
   │
   ▼
Retrieve Relevant Documents
   │
   ▼
Context
   │
   ▼
Amazon Bedrock
```

---

# 📊 Prometheus Integration

Prometheus provides metrics such as:

```text
CPU usage
Memory usage
Pod restarts
HTTP requests
HTTP 5xx
Latency
Container metrics
```

Example question:

```text
Which application is experiencing increased HTTP 5xx errors?
```

Pipeline:

```text
Question
   │
   ▼
Prometheus
   │
   ▼
Metrics
   │
   ▼
Context Builder
   │
   ▼
Bedrock
```

---

# 📝 Loki Integration

Loki provides application logs.

Example:

```text
Question:
Why is payment failing?

        │
        ▼
      Loki
        │
        ▼
Application Logs
        │
        ▼
Context Builder
        │
        ▼
     Bedrock
```

The assistant can correlate:

```text
Kubernetes State
       +
Metrics
       +
Logs
       +
Runbooks
       ↓
AI Analysis
```

---

# 🐳 Docker

Create:

```text
backend/Dockerfile
```

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app ./app

EXPOSE 8000

CMD [
  "uvicorn",
  "app.main:app",
  "--host",
  "0.0.0.0",
  "--port",
  "8000"
]
```

Build:

```bash
docker build \
  -t devops-ai-assistant:1.0 \
  ./backend
```

Run:

```bash
docker run \
  -p 8000:8000 \
  devops-ai-assistant:1.0
```

---

# ☸️ Deploy to Kubernetes

```bash
kubectl apply -f k8s/namespace.yaml

kubectl apply -f k8s/serviceaccount.yaml

kubectl apply -f k8s/clusterrole.yaml

kubectl apply -f k8s/clusterrolebinding.yaml

kubectl apply -f k8s/deployment.yaml

kubectl apply -f k8s/service.yaml
```

Check:

```bash
kubectl get pods -n devops-ai
```

```bash
kubectl get svc -n devops-ai
```

Logs:

```bash
kubectl logs \
  deploy/devops-ai-assistant \
  -n devops-ai
```

---

# ☁️ Amazon EKS

For production:

```text
Developer
    │
    ▼
AWS Load Balancer / API Gateway
    │
    ▼
Amazon EKS
    │
    ▼
DevOps AI Assistant
    │
    ├── Kubernetes API
    ├── Prometheus
    └── Loki
          │
          ▼
     Amazon Bedrock
```

---

# 🔐 EKS Pod Identity

Do not put AWS access keys inside the Pod.

Use:

```text
EKS Pod
   │
   ▼
Kubernetes ServiceAccount
   │
   ▼
EKS Pod Identity
   │
   ▼
IAM Role
   │
   ├── Bedrock
   └── S3
```

This allows AWS credentials to be provided through AWS identity mechanisms instead of embedding static credentials into application configuration.

---

# 🏗️ Terraform

Recommended AWS resources:

```text
VPC
EKS
IAM
EKS Pod Identity
S3
CloudWatch
```

Example:

```text
terraform/
├── main.tf
├── variables.tf
├── outputs.tf
├── iam.tf
└── eks.tf
```

Deploy:

```bash
terraform init
```

```bash
terraform plan
```

```bash
terraform apply
```

---

# 🛡️ Security Architecture

```text
                     Developer
                         │
                         ▼
                  API Authentication
                         │
                         ▼
                  FastAPI Backend
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
       Kubernetes RBAC          IAM Role
        Read Only                  │
                                   ▼
                             Amazon Bedrock
```

Security principles:

* Least privilege IAM
* Read-only Kubernetes RBAC initially
* EKS Pod Identity
* No static AWS credentials
* Encrypt sensitive data
* Protect API endpoints
* Avoid logging secrets
* Validate user input
* Limit AI context size
* Audit AWS API activity
* Apply network controls

---

# ⚠️ AI Safety Design

The AI assistant should distinguish:

```text
OBSERVED
────────
Facts returned by Kubernetes,
Prometheus, Loki, or RAG.

HYPOTHESIS
──────────
Possible causes inferred from
the available evidence.

RECOMMENDATION
──────────────
Commands or actions suggested
for human review.
```

Example:

```text
Observed:
frontend pod has restarted 8 times.

Hypothesis:
The application may be failing its
startup or liveness probe.

Recommendation:
Run:

kubectl describe pod frontend-xxx -n roboshop
kubectl logs frontend-xxx -n roboshop --previous
```

The assistant should **not** claim it ran these commands unless the execution results are actually available.

---

# 🤖 Future Controlled Remediation

Do not initially allow the model to execute arbitrary Kubernetes commands.

Progressively introduce automation:

```text
Phase 1
Read-only investigation
       │
       ▼
Phase 2
AI-generated commands
       │
       ▼
Phase 3
Human approval
       │
       ▼
Phase 4
Allowlisted actions
       │
       ▼
Phase 5
Audited automation
```

Example:

```text
AI:
Deployment frontend has 0 available replicas.

Suggested action:
kubectl rollout status deployment/frontend

Human:
Approve

Controller:
Execute approved operation

Audit:
Record user + action + result
```

---

# 🧪 Test Scenarios

Create controlled Kubernetes failures.

## Test 1 — CrashLoopBackOff

```yaml
command:
  - /bin/sh
  - -c
  - exit 1
```

Expected:

```text
CrashLoopBackOff
```

Ask:

```text
Why is this pod restarting?
```

---

## Test 2 — ImagePullBackOff

Deploy an intentionally invalid image:

```text
example.invalid/devops-ai-test:999
```

Ask:

```text
Why is my pod stuck in ImagePullBackOff?
```

---

## Test 3 — Service Failure

Create a Service whose selector does not match the Pods.

Ask:

```text
Why does my Kubernetes service have no endpoints?
```

---

## Test 4 — High Memory

Create a workload with an intentionally small memory limit.

Ask:

```text
Why was my container OOMKilled?
```

---

## Test 5 — Redis Connectivity

Create a controlled Redis connectivity problem.

Ask:

```text
Why is the application unable to connect to Redis?
```

---

# 📈 Observability

Monitor:

```text
AI request count
Bedrock latency
Model errors
Token usage
API latency
Kubernetes API errors
RAG retrieval latency
Prometheus query latency
Loki query latency
```

Architecture:

```text
                 DevOps AI Assistant
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
      Application      Bedrock      Kubernetes
        Metrics         Metrics        Metrics
          │              │              │
          └──────────────┼──────────────┘
                         ▼
                    CloudWatch
```

---

# 💰 Cost Awareness

Monitor:

```text
Bedrock model usage
Input tokens
Output tokens
Knowledge Base usage
S3 storage
EKS resources
Load Balancer
CloudWatch
Prometheus/Loki infrastructure
```

Use budgets and monitoring before running the project continuously in AWS.

For development, shut down or remove resources that are no longer needed.

---

# 🧪 End-to-End Example

Developer asks:

```text
Why is the cart application failing?
```

The assistant collects:

```text
Kubernetes
──────────
Pod status
Deployment status
Events
Service
Endpoints

Prometheus
──────────
CPU
Memory
HTTP 5xx
Restarts

Loki
────
Application errors

RAG
───
Redis troubleshooting runbook
```

Then:

```text
                 Context
                    │
                    ▼
             Amazon Bedrock
                    │
                    ▼
              AI Analysis
                    │
                    ▼
            Evidence-Based
               Response
```

Example output:

```text
Problem
-------
The cart application is reporting Redis connectivity errors.

Evidence
--------
- Cart Pods are running.
- Redis Service exists.
- Application logs report connection failures.
- Redis endpoint information should be verified.

Possible Causes
---------------
1. Incorrect Redis hostname.
2. Redis Service has no healthy endpoints.
3. Network connectivity issue.
4. Redis application failure.

Commands
--------
kubectl get svc redis -n roboshop

kubectl get endpoints redis -n roboshop

kubectl logs deploy/cart -n roboshop

Recommended Next Step
---------------------
Verify that the Redis Service has healthy endpoints and
confirm the application configuration uses the correct
Redis hostname and port.
```

---

# 🗺️ Project Roadmap

```text
                    AWS BEDROCK
                         │
                         ▼
                Model Invocation
                         │
                         ▼
                      FastAPI
                         │
                         ▼
                 Kubernetes API
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
          Prometheus              Loki
              │                     │
              └──────────┬──────────┘
                         ▼
                    RAG / KB
                         │
                         ▼
                    S3 Runbooks
                         │
                         ▼
                 Amazon Bedrock
                         │
                         ▼
                    Guardrails
                         │
                         ▼
                    AI Response
                         │
                         ▼
                    EKS Deploy
                         │
                         ▼
               Controlled Automation
```

---

# 🏆 Skills Demonstrated

This project demonstrates hands-on experience with:

### ☁️ AWS

* Amazon Bedrock
* Amazon EKS
* Amazon S3
* IAM
* EKS Pod Identity
* CloudWatch
* CloudTrail

### 🤖 Generative AI

* Foundation Models
* Prompt Engineering
* RAG
* Knowledge Bases
* Context Engineering
* Guardrails
* AI Agents
* LLM application development

### ☸️ Kubernetes

* Kubernetes API
* RBAC
* ServiceAccounts
* Deployments
* Services
* Pods
* Events
* Troubleshooting

### 📊 Observability

* Prometheus
* Loki
* Grafana
* Metrics
* Logs
* Incident analysis

### 🚀 DevOps

* Docker
* Terraform
* CI/CD
* EKS
* Infrastructure as Code
* Security
* Production architecture

---

# 🎓 Learning Path

```text
LEVEL 1
AWS Bedrock
   │
   ├── Models
   ├── Converse API
   └── Boto3
   │
   ▼
LEVEL 2
FastAPI
   │
   ▼
LEVEL 3
Kubernetes API
   │
   ├── Pods
   ├── Deployments
   ├── Services
   └── Events
   │
   ▼
LEVEL 4
RAG
   │
   ├── S3
   ├── Knowledge Base
   └── Runbooks
   │
   ▼
LEVEL 5
Observability
   │
   ├── Prometheus
   └── Loki
   │
   ▼
LEVEL 6
Security
   │
   ├── IAM
   ├── RBAC
   └── Pod Identity
   │
   ▼
LEVEL 7
EKS
   │
   ├── Docker
   ├── Terraform
   └── Load Balancer
   │
   ▼
FINAL
🤖 DevOps AI Assistant
```

---

# 📌 Production Enhancements

Future versions can add:

* Bedrock Agents
* Bedrock Guardrails
* Multi-model support
* Conversation history
* Incident correlation
* Alert summarization
* Slack integration
* Microsoft Teams integration
* GitHub integration
* Argo CD integration
* Jira integration
* Automated incident reports
* Human approval workflows
* Allowlisted Kubernetes remediation
* OpenTelemetry
* Distributed tracing
* Multi-cluster support

---

# ⭐ Final Architecture

```text
                         ┌─────────────────────┐
                         │      Developer      │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │     DevOps AI UI    │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │      FastAPI        │
                         └──────────┬──────────┘
                                    │
              ┌─────────────────────┼─────────────────────┐
              │                     │                     │
              ▼                     ▼                     ▼
        Kubernetes API         Prometheus                Loki
              │                     │                     │
              └─────────────────────┼─────────────────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │   Context Builder   │
                         └──────────┬──────────┘
                                    │
                         ┌──────────▼──────────┐
                         │ Bedrock Knowledge   │
                         │ Base / RAG          │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │    Amazon Bedrock   │
                         │   Foundation Model  │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │     Guardrails      │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │ Evidence-Based AI   │
                         │ Troubleshooting     │
                         └─────────────────────┘
```

---

# 📜 License

This project is intended for educational and portfolio purposes.

Add an appropriate license before using the repository for production or commercial distribution.

---

# 👨‍💻 Author

**Venkata Ram Vanimina**

Cloud | DevOps | Kubernetes | AWS | Terraform | Generative AI

---

# ⭐ If This Project Helps You

Consider:

```text
⭐ Star the repository
🍴 Fork the project
🐛 Open an issue
💡 Submit improvements
```

Build it. Break Kubernetes intentionally. Let the AI investigate the evidence. Then progressively add **RAG, observability, EKS, and human-approved remediation**.
