# client-onboarding-automation-n8n
<img width="1625" height="442" alt="Client Onboarding" src="https://github.com/user-attachments/assets/0ab65488-bb8a-426e-a22e-066168adafa5" />

# 🚀 Client Onboarding Automation (Auto Welcome Email & Task Generator)

An automated n8n workflow designed to streamline client intake, leverage generative AI for task checklist creation, intelligently route data based on project categories, notify team leads across platforms, and automate client scheduling.

## 📌 Project Overview

Manual client onboarding is often slow, fragmented, and prone to administrative friction. This end-to-end automation receives client intake data in real-time, standardizes the payload, runs it through an LLM agent to construct a custom onboarding plan, routes records to team-specific storage and messaging channels, and schedules kickoff calls—all within seconds.

## ⚙️ Step-by-Step Setup Instructions

Below is the execution flow breakdown based on the workflow architecture:

### 1. **Client Data Collection** (`New Client Intake Form`)

* Sets up a Webhook endpoint (`POST`) to receive raw client form submissions (e.g., Name, Email, Project Type, Company).

### 2. **Data Normalization** (`Normalize Client Data`)

* Cleans, formats, and standardizes input parameters to guarantee downstream nodes receive structured string properties.

### 3. **Execution Delay** (`Wait`)

* Pauses workflow execution briefly to optimize rate limits and allow upstream syncs to complete.

### 4. **Personalized Checklist & Task Generation** (`Checklist & Task Generation`)

* Uses an AI Agent node backed by dual LLM fallbacks (**Groq Chat Model** as primary, **OpenRouter Chat Model** as backup) to generate a custom 5-step onboarding checklist based on the project type.

### 5. **Smart Routing & Team Distribution** (`Route by Project Type`)

* Evaluates the submitted project type and conditionally branches the payload into one of three dedicated organizational pipelines: **Strategy**, **Management**, or **IT**.

### 6. **Team-Based Record Storage** (`Log to Google Sheets`)

* Appends or updates client records in dedicated team Google Sheets (`Log to Google Sheets - Strategy`, `Management`, or `IT`).

### 7. **Internal Team Notification** (`Slack Integration`)

* Sends formatted Slack notifications (`Notify Strategy Consultant`, `Notify Management Consultant`, or `Notify IT Consultant`) containing the client's AI-generated onboarding checklist to the respective team channel.

### 8. **Client Communication** (`Send Email to Client`)

* Merges execution branches back together to auto-generate and dispatch a personalized welcome email containing the checklist directly to the client's inbox.

### 9. **Meeting Scheduling** (`Schedule Kickoff Meeting`)

* Automatically creates an automated kickoff event on Google Calendar and sends calendar invites to involved stakeholders.

### 10. **CRM Sync (Optional)** (`Sync to CRM`)

* Makes an outbound HTTP Request to sync the structured profile, generated tasks, and meeting timestamps to an external CRM (e.g., HubSpot, Salesforce, or a custom API).


## 🔑 Required Credentials

Before activating this workflow, configure the following connection credentials in your n8n instance:

| Service | Node(s) Requiring Credential | Description / Scope Needed |
| --- | --- | --- |
| **Groq API** | `Groq Chat Model` | API Key from [Groq Console](https://console.groq.com) (Primary LLM Engine). |
| **OpenRouter API** | `OpenRouter Chat Model` | API Key from [OpenRouter](https://openrouter.ai) (Fallback LLM Engine). |
| **Google Sheets OAuth2** | `Log to Google Sheets` (Strategy, Management, IT) | Google Cloud / Workspace Account with `Google Sheets API` read/write access. |
| **Slack OAuth2 / Bot** | `Notify [Team] Consultant` | Slack Bot token (`chat:write` scope) connected to target channels. |
| **Gmail OAuth2** | `Send Email to Client` | OAuth2 or App Password with send email permissions. |
| **Google Calendar** | `Schedule Kickoff Meeting` | Google Workspace calendar OAuth2 permissions for event creation. |
| **HTTP / CRM API Key** | `Sync to CRM` | Custom API Key or OAuth Bearer token for your CRM endpoint. |


## 📥 How to Import This Workflow

1. Download the sanitized `Client Onboarding Automation & Welcome Email Generator` file from this repository.
2. Open your n8n workspace $\rightarrow$ Click **Workflows** $\rightarrow$ **Import from File**.
3. Select the `.json` file.
4. Attach your credentials to the respective nodes listed in the table above.
5. Toggle the workflow status to **Active**.
