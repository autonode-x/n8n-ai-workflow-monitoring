# 🤖 AI Workflow Monitoring System

> Production-ready **n8n Workflow Monitoring System** that automatically detects workflow failures, analyzes the root cause using AI, and instantly sends alerts through **Gmail, Slack, and Google Sheets**.

---

## 📌 Overview

Managing multiple production workflows can become difficult when failures go unnoticed.

This project continuously monitors failed n8n workflow executions and automatically generates an AI-powered incident report.

Instead of manually checking execution logs, the workflow:

* Detects workflow failures automatically
* Collects execution details
* Uses an AI Agent to analyze the error
* Identifies the probable root cause
* Classifies the severity level
* Suggests recommended fixes
* Sends notifications to Gmail and Slack
* Logs every incident in Google Sheets

---

# ✨ Features

* 🚨 Automatic Workflow Failure Detection
* 🤖 AI-Powered Root Cause Analysis
* 📧 Gmail Incident Alerts
* 💬 Slack Notifications
* 📊 Google Sheets Incident Logging
* 🧠 Error Categorization
* 🔥 Severity Classification
* 🔁 Retry Safety Detection
* 📋 Preventive Recommendations
* 📈 Production Monitoring Dashboard Ready

---

# 🛠 Tech Stack

### Automation

* n8n

### AI

* Groq
* Llama 3.3 70B
* AI Agent
* Structured Output Parser

### Integrations

* Gmail API
* Slack API
* Google Sheets API

### Data

* JSON
* Structured Output
* Incident Logs

---

# 🏗 Workflow Architecture

```text
Error Trigger
      │
      ▼
Collect Workflow Details
      │
      ▼
AI Incident Analysis
      │
      ▼
Structured JSON Output
      │
 ┌────┼────┐
 ▼    ▼    ▼
Gmail Slack Google Sheets
```

---

# 🤖 AI Analysis

The AI Agent automatically generates:

* Root Cause
* Severity
* Error Category
* Recommended Fix
* Retry Safety
* Preventive Actions
* Executive Summary

Example Output:

```json
{
  "root_cause": "Invalid API credentials",
  "severity": "High",
  "error_category": "Authentication",
  "recommended_fix": "Update expired API credentials.",
  "retry_safe": false,
  "preventive_actions": [
    "Rotate API keys regularly",
    "Monitor credential expiration"
  ],
  "summary": "Workflow failed due to authentication."
}
```

---

# 📧 Notification Channels

### Gmail

Automatically sends a detailed production incident report including:

* Workflow Name
* Failed Node
* Execution ID
* Root Cause
* Recommended Fix
* Severity
* Retry Safety

---

### Slack

Instant alert containing:

* Workflow
* Severity
* Error Category
* Root Cause
* Recommended Fix
* Execution URL

---

### Google Sheets

Stores incident history for future analysis.

Logged information includes:

* Timestamp
* Workflow Name
* Execution ID
* Failed Node
* Error Message
* Severity
* Error Category
* Root Cause
* Recommended Fix
* Retry Safe
* Status

---

# 📂 Project Structure

```text
AI-Workflow-Monitoring-System/
│
├── README.md
├── workflow.json
├── LICENSE
├── .gitignore
└── assets/
    └── workflow.png
    └── architecture.png
```

---

# 🚀 Installation

1. Clone the repository.

```bash
git clone https://github.com/autonode-x/n8n-ai-workflow-monitoring.git
```

2. Open your n8n instance.

3. Import `workflow.json`.

4. Configure credentials for:

* Groq
* Gmail
* Slack
* Google Sheets

5. Activate the workflow.

---

# 💡 Use Cases

* Production Monitoring
* Business Automation
* AI Workflow Monitoring
* Enterprise n8n Deployments
* Incident Management
* DevOps Automation
* AI Operations (AIOps)

---


# 🔮 Future Improvements

* Microsoft Teams Notifications
* Discord Alerts
* Automatic Retry Logic
* PagerDuty Integration
* Jira Ticket Creation
* Dashboard Analytics
* Multi-Environment Support
* Webhook Alerts

---

# 👨‍💻 Author

**Piyush Kumar**

AI Automation Engineer | AI Agent Developer | n8n Developer

* GitHub: https://github.com/autonode-x
* LinkedIn: https://www.linkedin.com/in/piyush-ai

---

## ⭐ Support

If you found this project useful, consider giving it a **⭐ Star** on GitHub.
