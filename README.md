# 🛡️ Case Study: MedVitals AI Infrastructure Hardening & CloudTrail Forensics

This repository houses the completed engineering deliverables, forensic evidence, and source code modifications executed during the Phase 1 Security Audit of **MedVitals AI** (HealthTech Portal). 

This project forms part of my operational security portfolio for the **AI Security Fellowship**, showcasing practical defensive competencies required to secure cloud gateways and modern SaaS AI application interfaces.

<img width="1345" height="641" alt="image" src="https://github.com/user-attachments/assets/05f605b4-b650-4697-8e1b-641fcf5a0f38" />

---

## 📊 Recruiter Evaluation Grid (60-Second Scan)

*   **THE PROBLEM:** Plaintext AWS credentials hardcoded directly within the client-facing deployment scripts allowed an external threat actor (`IP: 198.51.100.45`) to compromise the infrastructure environment and exploit an over-privileged wildcard (`"Action": "*"`) cloud Identity Access Management (IAM) policy framework configuration.
*   **THE METHOD:** Conducted a comprehensive cloud footprint log forensic audit using raw AWS CloudTrail JSON access registries to isolate the exact timeline and entry vector of the breach. Remediated the source code vulnerability by deploying strict environment runtime isolation configurations.
*   **THE EVIDENCE:** 
    *   *Code Architecture Changes:* Review the historical git commit tracking diff metrics inside `app.py` highlighting the transition from vulnerable static strings to secure `os.environ.get()` parameters.
    *   *Real-Time Sandbox Validation:* Running the sandbox environment verifies the successful removal of plaintext credentials from operational memory, turning dashboard alerts from red to green.
*   **THE OUTCOME:** Successfully hardened the application architecture against automated identity-theft and credential scraping vectors, bringing the startup's backend pipeline into direct compliance alignment with **OWASP Top 10 for LLM Applications** guidelines to protect company valuation prior to institutional investment rounds.

---

## 🛠️ Step-by-Step Technical Execution Record

### 1. Incident Detection & Log Analysis
Parsed raw cloud registry transactional payloads to trace the threat actor's activity timeline. Isolated the specific unauthorized `AssumeRole` API call executed by an unverified automated script agent (`python-requests/2.28.1`) rather than the standard corporate shell:

```json
{
  "EventTime": "2026-06-30T03:02:09Z",
  "EventName": "AssumeRole",
  "SourceIPAddress": "198.51.100.45",
  "UserAgent": "python-requests/2.28.1",
  "UserIdentity": "medvitals-deploy-bot"
}
```

### 2. Code Hardening & Runtime Environment Isolation
Stripped out the hardcoded plaintext credential variables from the production application script. Created a local environment file layer (`.env`) hidden safely within the container workspace to handle live keys, and re-engineered the backend data mapping parameters to extract variables securely from system background processes:

```python
import os
from dotenv import load_dotenv

# Initialize secure environment configuration variables at runtime
load_dotenv()

# Secrets are safely drawn from system environments, leaving zero plaintext signatures in code
AWS_SECRET_ACCESS_KEY = os.environ.get("AWS_SECRET_ACCESS_KEY")
AWS_REGION = os.environ.get("AWS_REGION", "us-east-1")
DB_CONNECTION_STRING = os.environ.get("DB_CONNECTION_STRING")
```

### 3. Structural Security Recommendations
To permanently mitigate future lateral migration attacks, the wildcard permission boundaries must be revoked from the cloud service account. The following strict, resource-scoped IAM JSON policy layout has been designed as an architectural recommendation for the startup's deployment infrastructure:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": "arn:aws:s3:::medvitals-patient-data-triage/*"
    }
  ]
}
```

---

## 🚀 Workspace Sandbox Launch Instructions

Here's a reference Forensic Playbook (Medium Walkthrough): Read my step-by-step threat hunting methodology on [Medium](https://medium.com/@CyberDammy/ai-security-defense-lab-part-1-bca2fc4ba074).

