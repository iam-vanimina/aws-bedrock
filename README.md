# aws-bedrock
# aws official :
``
https://docs.aws.amazon.com/bedrock/latest/userguide/getting-started-api-ex-python.html?utm_source
```
# 🚀 AWS Bedrock — Step-by-Step Hands-On Learning

![AWS](https://img.shields.io/badge/AWS-Bedrock-orange?logo=amazon-aws)
![AI](https://img.shields.io/badge/Generative-AI-blue)
![Python](https://img.shields.io/badge/Python-3.x-yellow?logo=python)
![Terraform](https://img.shields.io/badge/IaC-Terraform-purple?logo=terraform)
![Status](https://img.shields.io/badge/Lab-Hands--On-success)

> A practical, production-oriented guide to learning **Amazon Bedrock**, from basic foundation-model invocation to RAG, Agents, Guardrails, monitoring, and production architecture.

---

# 📚 What You Will Learn

This repository builds AWS Bedrock knowledge step by step.

```text
AWS Bedrock
     │
     ├── 01. AWS Account & IAM
     ├── 02. Bedrock Model Access
     ├── 03. Bedrock Console
     ├── 04. AWS CLI
     ├── 05. Python + Boto3
     ├── 06. Prompt Engineering
     ├── 07. RAG
     │      └── Knowledge Bases
     ├── 08. Agents
     ├── 09. Guardrails
     ├── 10. Monitoring
     └── 11. Production Architecture
```

---

# 🎯 Prerequisites

You should have basic knowledge of:

* AWS IAM
* AWS CLI
* Python
* JSON
* Linux
* Git/GitHub
* REST APIs
* Docker basics

Optional:

* Terraform
* Kubernetes
* CloudWatch
* OpenSearch

---

# 🏗️ Lab Architecture

```text
                         ┌─────────────────────┐
                         │       User          │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │   Application/API   │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │   Amazon Bedrock    │
                         │                     │
                         │ Foundation Models   │
                         └───────┬─────┬───────┘
                                 │     │
                    ┌────────────┘     └────────────┐
                    ▼                               ▼
             ┌──────────────┐                ┌──────────────┐
             │ Knowledge    │                │  Guardrails  │
             │ Bases / RAG  │                │              │
             └──────┬───────┘                └──────────────┘
                    │
                    ▼
             ┌──────────────┐
             │ S3 Documents │
             └──────────────┘

                    │
                    ▼
             ┌──────────────┐
             │ CloudWatch   │
             │ Monitoring   │
             └──────────────┘
```

---

# 1️⃣ Step 1 — Configure AWS CLI

Check AWS CLI:

```bash
aws --version
```

Configure credentials:

```bash
aws configure
```

Enter:

```text
AWS Access Key ID:
AWS Secret Access Key:
Default region name:
Default output format:
```

Example:

```text
ap-south-1
json
```

Verify:

```bash
aws sts get-caller-identity
```

Expected:

```json
{
    "UserId": "...",
    "Account": "...",
    "Arn": "..."
}
```

---

# 2️⃣ Step 2 — Choose a Bedrock Region

Bedrock model availability differs by AWS Region.

Check the AWS documentation for the current model/region availability before starting a lab.

For this repository, you can use a Bedrock-supported region such as:

```bash
export AWS_REGION=us-east-1
```

Verify:

```bash
echo $AWS_REGION
```

---

# 3️⃣ Step 3 — IAM Permissions

Your IAM identity needs permission to invoke Bedrock models.

For the examples in this repository, the important permission is:

```text
bedrock:InvokeModel
```

For streaming:

```text
bedrock:InvokeModelWithResponseStream
```

For listing models:

```text
bedrock:ListFoundationModels
```

AWS documents `bedrock:InvokeModel` as the required permission for the `Converse` operation.

For later Knowledge Bases, Agents, S3, CloudWatch, and other labs, additional permissions will be required.

> ⚠️ Do not permanently use `AdministratorAccess` for a production application.

---

# 4️⃣ Step 4 — Open Amazon Bedrock

Open the AWS Console and navigate to:

```text
Amazon Bedrock
```

Explore:

```text
Foundation Models
Model Catalog
Playgrounds
Knowledge Bases
Agents
Guardrails
Evaluation
Monitoring
```

The exact console layout can change over time.

---

# 5️⃣ Step 5 — Explore Foundation Models

List available models:

```bash
aws bedrock list-foundation-models \
  --region "$AWS_REGION"
```

List only model IDs:

```bash
aws bedrock list-foundation-models \
  --region "$AWS_REGION" \
  --query 'modelSummaries[].modelId' \
  --output text
```

You can use this command to confirm which models are available in your selected Region.

Model availability and API compatibility vary by model and Region.

---

# 6️⃣ Step 6 — Test a Model in the Playground

Go to:

```text
Amazon Bedrock
        ↓
Playgrounds
        ↓
Chat
```

Select an available model.

Example prompt:

```text
Explain Kubernetes deployments to a beginner.
```

Try another:

```text
You are a senior DevOps engineer.

Explain how Kubernetes rolling updates work.

Include:
1. Deployment
2. ReplicaSet
3. Pods
4. Rollout
5. Rollback
```

Observe:

* Response quality
* Latency
* Token usage
* Temperature
* Maximum output tokens

---

# 7️⃣ Step 7 — Working Model Invocation Using AWS CLI

For new applications, AWS recommends the `bedrock-runtime` endpoint. The `Converse` API provides a common interface for supported models.

First verify that Amazon Nova Lite is available in your Region:

```bash
aws bedrock list-foundation-models \
  --region "$AWS_REGION" \
  --query "modelSummaries[?modelId=='amazon.nova-lite-v1:0'].{Model:modelId,Status:modelLifecycle.status}" \
  --output table
```

Then invoke it:

```bash
aws bedrock-runtime converse \
  --region "$AWS_REGION" \
  --model-id amazon.nova-lite-v1:0 \
  --messages '[{"role":"user","content":[{"text":"Explain AWS Bedrock in one paragraph."}]}]' \
  --inference-config '{"maxTokens":512,"temperature":0.5,"topP":0.9}'
```

AWS provides this same `Converse` pattern in its current CLI examples.

Expected response structure:

```json
{
  "output": {
    "message": {
      "role": "assistant",
      "content": [
        {
          "text": "Amazon Bedrock is..."
        }
      ]
    }
  }
}
```

### Save the response

```bash
aws bedrock-runtime converse \
  --region "$AWS_REGION" \
  --model-id amazon.nova-lite-v1:0 \
  --messages '[{"role":"user","content":[{"text":"Explain AWS Bedrock in one paragraph."}]}]' \
  --inference-config '{"maxTokens":512,"temperature":0.5,"topP":0.9}' \
  > response.json
```

Inspect:

```bash
cat response.json
```

---

# 8️⃣ Step 8 — Working Python + Boto3 Example

Install Boto3:

```bash
python -m pip install boto3
```

Verify:

```bash
python -c "import boto3; print(boto3.__version__)"
```

Create:

```text
04-python-boto3/invoke_model.py
```

Use this complete example:

```python
import boto3
from botocore.exceptions import ClientError

REGION = "us-east-1"
MODEL_ID = "amazon.nova-lite-v1:0"

client = boto3.client(
    "bedrock-runtime",
    region_name=REGION,
)

user_message = """
You are a senior DevOps engineer.

Explain AWS Bedrock to a beginner.

Include:
1. What Bedrock is
2. What a foundation model is
3. What RAG means
4. One real-world DevOps use case
"""

messages = [
    {
        "role": "user",
        "content": [
            {
                "text": user_message
            }
        ],
    }
]

try:
    response = client.converse(
        modelId=MODEL_ID,
        messages=messages,
        inferenceConfig={
            "maxTokens": 512,
            "temperature": 0.5,
            "topP": 0.9,
        },
    )

    response_text = response["output"]["message"]["content"][0]["text"]

    print("\n===== MODEL RESPONSE =====\n")
    print(response_text)

    print("\n===== USAGE =====\n")
    print(response.get("usage"))

except ClientError as error:
    print(f"AWS error: {error}")
    raise
```

Run:

```bash
python 04-python-boto3/invoke_model.py
```

Expected:

```text
===== MODEL RESPONSE =====

Amazon Bedrock is an AWS service that provides access
to foundation models through APIs...

===== USAGE =====

{
    'inputTokens': ...,
    'outputTokens': ...,
    'totalTokens': ...
}
```

This follows AWS's current Boto3 `Converse` example pattern.

---

# 9️⃣ Step 9 — Understand the Code

The important part is:

```python
client = boto3.client(
    "bedrock-runtime",
    region_name=REGION,
)
```

This creates the Bedrock Runtime client.

Then:

```python
response = client.converse(
    modelId=MODEL_ID,
    messages=messages,
)
```

sends the conversation to the model.

The model ID is:

```python
MODEL_ID = "amazon.nova-lite-v1:0"
```

The message is:

```python
messages = [
    {
        "role": "user",
        "content": [
            {
                "text": "Hello Bedrock"
            }
        ],
    }
]
```

The generated text is extracted using:

```python
response["output"]["message"]["content"][0]["text"]
```

---

# 🔟 Step 10 — Make It Interactive

Create:

```text
04-python-boto3/chat.py
```

```python
import boto3

REGION = "us-east-1"
MODEL_ID = "amazon.nova-lite-v1:0"

client = boto3.client(
    "bedrock-runtime",
    region_name=REGION,
)

while True:
    question = input("\nYou: ")

    if question.lower() in {"exit", "quit"}:
        break

    response = client.converse(
        modelId=MODEL_ID,
        messages=[
            {
                "role": "user",
                "content": [
                    {
                        "text": question
                    }
                ],
            }
        ],
        inferenceConfig={
            "maxTokens": 512,
            "temperature": 0.5,
        },
    )

    answer = response["output"]["message"]["content"][0]["text"]

    print("\nBedrock:")
    print(answer)
```

Run:

```bash
python 04-python-boto3/chat.py
```

Example:

```text
You: What is Kubernetes?

Bedrock:
Kubernetes is an open-source container orchestration platform...
```

Exit:

```text
You: exit
```

---

# 1️⃣1️⃣ Step 11 — InvokeModel vs Converse

Bedrock provides both APIs.

```text
                  Bedrock Runtime
                        │
             ┌──────────┴──────────┐
             │                     │
         Converse              InvokeModel
             │                     │
       Common interface       Model-specific
       across supported        request format
           models
```

### Converse

Use when you want a consistent interface across models that support it.

```python
response = client.converse(
    modelId=MODEL_ID,
    messages=messages,
)
```

### InvokeModel

Use when you need direct access to a model's native request/response format or when the model/API combination requires it.

```python
response = client.invoke_model(
    modelId=MODEL_ID,
    body=...,
    contentType="application/json",
    accept="application/json",
)
```

AWS notes that not every model supports `Converse`, while all models support `InvokeModel`.

---

# 1️⃣2️⃣ Step 12 — Prompt Engineering

Experiment with different prompts.

### Basic

```text
Explain Docker.
```

### Role-based

```text
You are a senior DevOps engineer.

Explain Docker networking.
```

### Structured

```text
Explain Docker networking.

Return:

Definition:
Architecture:
Network types:
Commands:
Production example:
Troubleshooting:
```

### Few-shot

```text
Convert the following incident into a structured report.

Example:

Input:
Pod is restarting.

Output:
Issue:
Possible Cause:
Commands:
Resolution:

Input:
ImagePullBackOff.

Output:
```

Compare the results.

---

# 1️⃣3️⃣ Step 13 — Streaming Responses

For interactive applications, streaming responses can improve perceived responsiveness.

Boto3 provides:

```python
client.converse_stream(...)
```

Conceptually:

```text
User
 │
 ▼
Application
 │
 ▼
Bedrock Runtime
 │
 ├── Token 1
 ├── Token 2
 ├── Token 3
 ├── Token 4
 └── ...
       │
       ▼
      User
```

The Bedrock Runtime client supports both `converse` and `converse_stream`.

---

# 1️⃣4️⃣ Step 14 — Build a RAG Application

RAG = Retrieval-Augmented Generation.

Instead of asking a model only from its learned knowledge:

```text
Question
   │
   ▼
LLM
   │
   ▼
Answer
```

we add enterprise/private information:

```text
                    ┌───────────────┐
                    │   Documents   │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │ S3 / Knowledge│
                    │     Base      │
                    └───────┬───────┘
                            │
                         Retrieval
                            │
                            ▼
User ──────── Question ──► Bedrock
                            │
                            ▼
                         Response
```

---

# 1️⃣5️⃣ Step 15 — Create an S3 Bucket

Example:

```bash
aws s3 mb s3://my-bedrock-learning-documents-UNIQUE-ID
```

Upload documents:

```bash
aws s3 cp ./documents/ \
  s3://my-bedrock-learning-documents-UNIQUE-ID/ \
  --recursive
```

Verify:

```bash
aws s3 ls \
  s3://my-bedrock-learning-documents-UNIQUE-ID/
```

---

# 1️⃣6️⃣ Step 16 — Amazon Bedrock Knowledge Base

Create a Knowledge Base through the Bedrock console.

Typical architecture:

```text
Documents
    │
    ▼
   S3
    │
    ▼
Knowledge Base
    │
    ▼
Embeddings
    │
    ▼
Vector Store
    │
    ▼
Retrieval
    │
    ▼
Foundation Model
    │
    ▼
Answer
```

Test questions against your uploaded documents.

---

# 1️⃣7️⃣ Step 17 — Build a DevOps RAG Assistant

Example:

```text
User:
Why is my Kubernetes pod in CrashLoopBackOff?

             │
             ▼

      Bedrock Knowledge Base

             │
             ├── Kubernetes Runbooks
             ├── Incident Documents
             ├── Architecture Docs
             ├── Troubleshooting Guides
             └── Deployment Docs

             │
             ▼

       Foundation Model

             │
             ▼

Answer:
1. Check pod logs
2. Check previous container logs
3. Inspect events
4. Check resource limits
5. Check probes
```

---

# 1️⃣8️⃣ Step 18 — Bedrock Agents

Agents can orchestrate multi-step tasks.

Conceptually:

```text
User
 │
 ▼
Bedrock Agent
 │
 ├── Foundation Model
 │
 ├── Knowledge Base
 │
 └── Action Group
          │
          ▼
       AWS API
```

Example DevOps Agent:

```text
User
 │
 ▼
"Check why frontend is unhealthy"
 │
 ▼
Bedrock Agent
 │
 ├── Query monitoring data
 ├── Inspect Kubernetes information
 ├── Search runbooks
 └── Explain probable causes
```

For production systems, keep action permissions narrowly scoped.

---

# 1️⃣9️⃣ Step 19 — Guardrails

Guardrails can help control model interactions according to configured policies.

Possible controls include:

```text
Content filtering
Denied topics
Sensitive information handling
Word filters
Grounding-related checks
```

Architecture:

```text
User
 │
 ▼
Application
 │
 ▼
Bedrock Guardrail
 │
 ▼
Foundation Model
 │
 ▼
Response
```

Test both allowed and blocked prompts.

---

# 2️⃣0️⃣ Step 20 — Monitoring

Use AWS monitoring capabilities to observe your application.

Important areas:

```text
CloudWatch
CloudTrail
Bedrock invocation metrics
Application logs
Latency
Errors
Usage
Cost
```

Example operational flow:

```text
Application
     │
     ▼
Amazon Bedrock
     │
     ├──────────► CloudWatch
     │
     └──────────► CloudTrail
```

---

# 2️⃣1️⃣ Step 21 — Security

Production Bedrock applications should consider:

### IAM

Use least privilege.

### Encryption

Protect data using AWS encryption mechanisms.

### Network security

Evaluate private connectivity requirements for your architecture.

### Logging

Avoid accidentally logging sensitive prompts and responses.

### Secrets

Never hard-code:

```text
AWS Access Keys
API Keys
Database passwords
Tokens
```

Use appropriate AWS secret-management mechanisms.

---

# 2️⃣2️⃣ Step 22 — Cost Awareness

Generative AI costs can depend on factors such as:

```text
Model
Input tokens
Output tokens
Provisioning / inference options
Knowledge Base components
Agents
Storage
Supporting AWS services
```

Before production deployment:

```text
Estimate
   ↓
Load test
   ↓
Monitor usage
   ↓
Set budgets/alerts
   ↓
Optimize
```

---

# 🧪 Final Project

Build an:

# 🤖 AWS DevOps AI Assistant

Features:

```text
                    ┌───────────────────┐
                    │   DevOps User     │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │   Web Interface   │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │ API Gateway       │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │ Lambda / ECS      │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │ Amazon Bedrock    │
                    └─────┬─────┬───────┘
                          │     │
             ┌────────────┘     └────────────┐
             ▼                               ▼
      ┌──────────────┐                ┌──────────────┐
      │ Knowledge    │                │ Guardrails   │
      │ Base         │                │              │
      └──────┬───────┘                └──────────────┘
             │
             ▼
            S3
             │
             ▼
       Runbooks / Docs
```

The assistant should answer questions such as:

```text
Why is my Kubernetes pod failing?

How do I troubleshoot ImagePullBackOff?

Explain this Terraform error.

Generate a Kubernetes troubleshooting checklist.

Explain this CloudWatch error.

Search our internal DevOps runbook.
```

---

# 📁 Suggested Git Workflow

Create the repository:

```bash
mkdir aws-bedrock-learning

cd aws-bedrock-learning

git init

git branch -M main
```

Create README:

```bash
touch README.md
```

Add:

```bash
git add README.md
```

Commit:

```bash
git commit -m "docs: add AWS Bedrock learning guide"
```

Add GitHub remote:

```bash
git remote add origin https://github.com/YOUR_USERNAME/aws-bedrock-learning.git
```

Push:

```bash
git push -u origin main
```

---

# 🗺️ Learning Roadmap

```text
LEVEL 1
│
├── AWS CLI
├── IAM
├── Bedrock Console
└── Foundation Models
        │
        ▼
LEVEL 2
│
├── CLI Invocation
├── Python/Boto3
├── Prompt Engineering
└── Streaming
        │
        ▼
LEVEL 3
│
├── S3
├── Knowledge Bases
├── Embeddings
└── RAG
        │
        ▼
LEVEL 4
│
├── Agents
├── Action Groups
└── Guardrails
        │
        ▼
LEVEL 5
│
├── CloudWatch
├── CloudTrail
├── IAM Security
├── Cost Optimization
└── Production Architecture
        │
        ▼
FINAL PROJECT
│
└── 🤖 DevOps AI Assistant
```

---

# ⭐ Skills You Will Gain

By completing this repository, you will practice:

* Amazon Bedrock
* Generative AI
* Foundation Models
* Prompt Engineering
* Boto3
* AWS CLI
* IAM
* RAG
* Knowledge Bases
* Vector search concepts
* Bedrock Agents
* Guardrails
* S3
* Lambda
* API Gateway
* CloudWatch
* CloudTrail
* AI security
* Cost optimization
* Production AI architecture

---

## 🔥 Recommended Next Lab

After completing the basic model invocation, build this hands-on project:

**AWS Bedrock + Kubernetes + RAG + DevOps AI Assistant**

```text
Kubernetes
    │
    ├── Pod Logs
    ├── Events
    ├── Metrics
    └── Deployments
           │
           ▼
      DevOps Backend
           │
           ▼
      Amazon Bedrock
           │
           ├── Knowledge Base
           ├── RAG
           ├── Guardrails
           └── Agent
           │
           ▼
       AI Response
```

This gives you a practical bridge between **AWS Cloud/DevOps experience and Generative AI engineering**.
