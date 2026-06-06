# 🤖 AI Job Application Pipeline — n8n Workflow

> Fill one form → 4 AI agents run automatically → personalized cover letter + application email generated → sent to HR → logged to Google Sheets tracker.

---

## 📌 Overview

This is a fully automated job application pipeline built with **n8n** and **OpenAI GPT-4o-mini**. You fill in a form once with your CV, job description, and company details — and the pipeline handles everything else: researching the company, analyzing your resume, writing a cover letter, drafting a professional email, sending it, and logging the application to a tracker.

---

## ✨ Features

- 🏢 **Company Intelligence** — scrapes and analyzes the company website automatically
- 📊 **Resume Analysis** — scores your CV against the job description (suitability score out of 10)
- 📝 **Cover Letter Generation** — writes a personalized, role-specific cover letter
- 📧 **Application Email** — drafts a concise professional email with your name, phone, and email
- 📤 **Auto Send or Manual Review** — choose to send automatically or review first
- 📋 **Google Sheets Tracker** — logs every application with date, score, follow-up date, and notes
- 📬 **Digest Email** — sends you a full summary of everything the agents produced

---

## 🔁 Workflow Architecture

```
[Form Trigger]
      │
      ├──→ [Agent 1a: Scrape Company Website]
      │         └──→ [Code: Clean HTML]
      │                   └──→ [Agent 1b: Analyze Company] ──┐
      │                                                       ├──→ [Agent 3: Cover Letter Writer]
      └──→ [Agent 2: Resume Analyzer] ────────────────────────┘
                                                                        │
                                                              [Agent 4a: Write Email]
                                                                        │
                                                         [Wait / Merge All 4 Agents]
                                                                        │
                                                       [Compile All Agent Outputs]
                                                                        │
                                                              [Agent 4b: Send Email]
                                                                        │
                                                       [Send Full Digest to Yourself]
                                                                        │
                                                       [Log to Applications Tracker]
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| [n8n](https://n8n.io) | Workflow automation platform |
| OpenAI GPT-4o-mini | All 4 AI agents |
| Gmail (OAuth2) | Sending emails |
| Google Sheets (OAuth2) | Application tracker |
| HTTP Request node | Website scraping |
| JavaScript Code node | HTML cleaning + data compilation |

---

## 📋 Prerequisites

- n8n account (cloud or self-hosted)
- OpenAI API key (or n8n free credits)
- Gmail account connected via OAuth2
- Google Sheet with these columns:

```
Date Applied | Job Title | Company | HR Email | Suitability Score | Email Sent? | Status | Follow Up Date | Notes
```

---

## 🚀 Setup Instructions

### 1. Import the Workflow
- Download `My_workflow_3_FIXED.json`
- In n8n: **Workflows → Import from file**

### 2. Connect Credentials
Go to **Settings → Credentials** and connect:
- ✅ OpenAI API — add your API key
- ✅ Gmail OAuth2 — authorize with your Google account
- ✅ Google Sheets OAuth2 — authorize with your Google account

### 3. Set Up Google Sheet
Create a new Google Sheet with exactly these column headers in Row 1:
```
Date Applied | Job Title | Company | HR Email | Suitability Score | Email Sent? | Status | Follow Up Date | Notes
```
Copy the sheet URL and paste it into the **Log to Applications Tracker** node.

### 4. Update Your Email
In the **Agent 4b — Send Application Email** node, update the CC/digest email to your own address.

### 5. Activate the Workflow
Click **Publish** → **Active** to enable the form trigger.

---

## 📝 How to Use

1. Open the form URL (found in the Trigger node)
2. Fill in:
   - Job Title & Company Name
   - Company Website URL
   - Hiring Manager Name & Email
   - Full Job Description (paste from job posting)
   - Your CV as plain text (paste from your CV)
   - Choose: Send automatically or review first
3. Submit — the pipeline runs in ~30–60 seconds
4. Check your email for the full digest
5. Check your Google Sheet — new row added automatically

---

## 📧 Email Format

The application email sent to HR includes:

```
[Professional 100-150 word email body]

--- COVER LETTER ---
[Personalized 250-350 word cover letter]

--- CV / RESUME ---
[Your full CV text]
```

---

## 🗂️ Google Sheets Tracker Output

| Column | Source |
|--------|--------|
| Date Applied | Auto — today's date |
| Job Title | Form input |
| Company | Form input |
| HR Email | Form input |
| Suitability Score | AI extracted (e.g. 7/10) |
| Email Sent? | Yes / No |
| Status | "Applied" (default) |
| Follow Up Date | Auto — 7 days from today |
| Notes | First 500 chars of resume feedback |

---

## ⚠️ Known Limitations

- **Gmail only** — sending uses your personal Gmail OAuth, so only works from your account
- **Text CV only** — CV must be pasted as plain text (no PDF upload)
- **n8n free credits** — limited to GPT-4o-mini and 2,500 executions/month on free tier
- **Website scraping** — some sites block HTTP scraping; the pipeline gracefully handles this with a 3,000 character text limit

---

## 🔧 Customization

**Change the AI model:** Open any agent node → change Model from `gpt-4o-mini` to `gpt-4.1-mini` or your preferred model.

**Change follow-up days:** In the Compile node, find `followUpDate.setDate(followUpDate.getDate() + 7)` and change `7` to your preferred number.

**Add more CV fields to the form:** Edit the Trigger node → add fields like LinkedIn URL, Portfolio, etc. — then reference them in the agent prompts.

---

## 📁 File Structure

```
├── My_workflow_3_FIXED.json    # Main n8n workflow file
└── README.md                   # This file
```

---

## 🙏 Credits

Built with [n8n](https://n8n.io) — the open-source workflow automation tool.
AI powered by [OpenAI](https://openai.com) GPT-4o-mini.

---

## 📄 License

MIT — free to use, modify, and share.
