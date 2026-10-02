# AI-Driven Guest Post Agency Automation

> **Replaces weeks of manual Off-Page SEO outreach with a full-stack autonomous n8n AI agent. Crawls prospect domains, classifies niches via Gemini LLM, drafts customized pitches, and balances multi-account Gmail delivery.**

## 📌 Overview

This project is a real-world **Off-Page SEO and Guest Post Outreach Automation System** built with **n8n, AI, Google Sheets, Gemini, and Gmail**.

The goal was not simply to automate one task.

The goal was to automate an entire repetitive workflow that an Off-Page SEO / Guest Post manager would otherwise have to perform manually — from finding potential websites to researching them, identifying guest post opportunities, extracting contact information, checking duplicates, generating personalized outreach proposals, sending emails, and tracking the outreach status.

The workflow transforms a time-consuming manual process into an automated, repeatable pipeline.

---

## 🎯 The Real-World Problem

Guest Post outreach involves a large number of repetitive tasks.

A typical manual process requires an SEO manager to:

- Search for websites related to a target keyword or niche
- Open and inspect potential websites
- Find relevant guest post pages
- Extract website and contact information
- Identify email addresses
- Check whether the website has already been contacted
- Check whether the same email has already been used
- Research the website and understand its niche
- Categorize the website
- Write a suitable outreach proposal
- Create different email subject lines
- Send outreach emails individually
- Wait between emails
- Manage multiple sender accounts
- Record which account sent each email
- Update outreach status
- Record the sending date
- Move to the next prospect

When performed manually across hundreds of prospects, these activities become highly repetitive and time-consuming.

### The Core Problem

**The SEO manager was spending valuable time on repetitive operational work instead of focusing on higher-value SEO and outreach decisions.**

This project was built to remove those repetitive steps from the process.

---

# 💡 The Automation Solution

The solution is an end-to-end **AI-powered n8n automation pipeline**.

Instead of manually processing every prospect, the system takes a keyword file as input and processes prospects through a structured workflow.

### Automated Pipeline

```text
Keyword Input
     ↓
Website Discovery
     ↓
Prospect Processing
     ↓
Website Crawling / Page Extraction
     ↓
Guest Post Opportunity Detection
     ↓
Contact & Email Extraction
     ↓
Duplicate Protection
     ↓
AI Website / Niche Analysis
     ↓
AI Classification
     ↓
Personalized Proposal Generation
     ↓
Google Sheets Database Update
     ↓
Controlled Wait
     ↓
Gmail Account Rotation
     ↓
Email Delivery
     ↓
Outreach Status Update
     ↓
Next Prospect
```

---

# 🤖 What the AI Agent Does

The workflow uses a **Google Gemini Chat Model** connected to an n8n **AI Agent** with a structured output parser.

The AI layer helps analyze the collected website information and generate structured information used for outreach.

The workflow then passes the AI-generated information into the proposal generation stage.

### AI Processing Includes

- Website / brand analysis
- Niche or category classification
- Brand name extraction
- Structured AI output
- Information used for personalized outreach
- Proposal generation

---

# ✉️ Personalized Outreach Generation

Instead of sending the same generic message to every website, the workflow generates proposals using multiple templates.

The proposal generator dynamically uses information such as:

- Brand name
- Website/domain
- Website category/niche

It also generates multiple subject-line variations, including collaboration and guest-post related subjects.

This helps make the outreach process more structured and less dependent on manually writing every email.

---

# 🔎 Website & Prospect Research

The workflow processes potential websites and extracts relevant information from both homepage and specific pages.

The automation includes processing stages for:

- Homepage extraction
- Specific page extraction
- Page field extraction
- Website filtering
- Guest post opportunity detection
- Contact information extraction
- Social profile information
- Website analysis

Websites that do not meet the workflow's conditions can be routed away from the main outreach process.

---

# 🛡️ Duplicate Protection

One of the important problems in outreach is accidentally contacting the same prospect more than once.

This workflow includes a dedicated duplicate protection stage based on **website + email** information.

A unique identifier is generated for the prospect and used to help prevent duplicate outreach.

```text
Website + Email
       ↓
Unique ID
       ↓
Duplicate Check
       ↓
Already Contacted?
   ↙          ↘
 YES           NO
 ↓             ↓
Skip        Continue
             ↓
          AI Analysis
```

The workflow also stores a **Unique ID** in the Google Sheets database for tracking and matching.

---

# ⏱️ Controlled Outreach

The system does not immediately send every generated email one after another.

A dedicated **Wait2** node introduces a **15-minute wait** before the email-account selection and sending stage.

This creates a controlled outreach flow instead of processing every prospect as an immediate email action.

Additional wait stages are also present during website processing.

---

# 📧 Multi-Account Gmail Rotation

The workflow supports multiple Gmail sender accounts.

A dedicated account-selection stage maintains a global counter and alternates between the configured sender accounts.

```text
              Prospect Ready
                    ↓
              15-Minute Wait
                    ↓
          Gmail Account Selector
              ↙            ↘
       Account 1          Account 2
           ↓                  ↓
       Gmail Send          Gmail Send
              ↘            ↙
             Tracking
```

This allows the workflow to distribute outreach across configured sender accounts rather than relying on a single sender.

The workflow then routes the selected account through a Switch node to the corresponding Gmail sending node.

> **Important:** The public GitHub version should use placeholder Gmail accounts and credentials. Real email addresses, OAuth credentials, Google Sheet IDs, tokens, and other private identifiers must not be committed to the repository.

---

# 📊 Google Sheets as Outreach Database

Google Sheets is used as the central tracking database.

The workflow stores information such as:

- Website
- Brand Name
- Email
- Contact links
- Guest post information
- Social profiles
- Category
- Unique ID
- Proposal
- Status
- Sent Date
- Sender Email

After an email is sent, the workflow updates the corresponding record using the Unique ID.

The post-send tracking stage records the status, sending date, Unique ID and sender email.

---

# 🔄 End-to-End Workflow

## 1. Keyword Input

The process begins with a form where a keyword file can be uploaded.

The uploaded file is then extracted and passed into the search stage.

## 2. Website Discovery

The workflow searches for potential websites based on the provided keyword data.

## 3. Prospect Processing

Each discovered prospect is processed individually through the workflow loop.

## 4. Website Analysis

The automation extracts relevant website and page information.

## 5. Guest Post Qualification

The workflow checks whether a suitable guest post opportunity exists.

If a suitable opportunity cannot be identified, the prospect is routed to a separate handling path.

## 6. Contact Discovery

The workflow attempts to identify relevant contact information and email addresses.

If an email cannot be found, the prospect is handled separately instead of continuing to the outreach stage.

## 7. Duplicate Protection

The system checks the prospect against existing records before continuing.

## 8. AI Analysis

The qualified prospect is passed to the Gemini-powered AI Agent.

## 9. Proposal Generation

A personalized proposal and subject line are generated.

## 10. Database Update

The prospect and generated outreach information are stored/updated in Google Sheets.

## 11. Controlled Wait

The workflow waits 15 minutes before proceeding to email account selection.

## 12. Sender Account Selection

The workflow selects the next configured Gmail account.

## 13. Email Sending

The proposal is sent through the selected Gmail account.

## 14. Outreach Tracking

The Google Sheets record is updated with sending information.

## 15. Continue Processing

The workflow returns to the processing loop and continues with the next prospect.

---

# 🧩 Key Automation Features

| Feature | Purpose |
|---|---|
| Keyword File Input | Start outreach from a keyword list |
| Website Discovery | Find potential guest post prospects |
| Website Crawling | Collect relevant website information |
| Guest Post Detection | Identify potential guest posting opportunities |
| Contact Extraction | Find relevant contact information |
| Duplicate Protection | Reduce repeated outreach |
| Unique ID | Track each prospect |
| Gemini AI Agent | Analyze and classify prospects |
| Structured Output | Keep AI results organized |
| Proposal Generator | Create outreach proposals |
| Subject Variations | Generate different outreach subjects |
| Google Sheets | Maintain outreach database |
| 15-Minute Wait | Control outreach timing |
| Gmail Rotation | Alternate configured sender accounts |
| Email Sending | Execute actual outreach |
| Post-Send Tracking | Record sender and sending status |
| Loop Processing | Continue automatically with next prospects |

---

# 🔧 Technology Stack

- **n8n** — Workflow orchestration and automation
- **Google Gemini** — AI-powered analysis and classification
- **Google Sheets** — Prospect database and tracking
- **Gmail** — Automated outreach delivery
- **HTTP Requests / Web Data Extraction** — Website discovery and processing
- **n8n Code Nodes** — Data transformation, IDs, proposal generation and workflow logic
- **Structured Output Parser** — Structured AI responses

---

# 📈 Before vs After

## Before — Manual Process

```text
Search Websites
     ↓
Open Website
     ↓
Find Guest Post Page
     ↓
Find Email
     ↓
Research Website
     ↓
Check Previous Outreach
     ↓
Write Proposal
     ↓
Write Subject
     ↓
Send Email
     ↓
Wait
     ↓
Select Sender Account
     ↓
Update Spreadsheet
     ↓
Repeat
```

Every prospect requires repeated manual actions.

---

## After — Automated Process

```text
Upload Keywords
       ↓
        n8n
       ↓
Website Discovery
       ↓
Research & Extraction
       ↓
Qualification
       ↓
Duplicate Protection
       ↓
Gemini AI
       ↓
Proposal Generation
       ↓
Google Sheets
       ↓
15-Minute Control
       ↓
Gmail Rotation
       ↓
Email Sending
       ↓
Automatic Tracking
       ↓
Next Prospect
```

The repetitive operational workload is moved from manual execution into an automated workflow.

---

# 💼 Real-World Business Value

This automation was designed around a practical business problem:

> **How can a Guest Post / Off-Page SEO manager process a large volume of outreach prospects without manually repeating the same research, writing, sending and tracking tasks for every prospect?**

The solution creates a repeatable system that can:

- Reduce repetitive manual research
- Reduce repetitive data-entry work
- Automate prospect qualification
- Automate duplicate checking
- Automate AI-based classification
- Automate proposal drafting
- Automate email delivery
- Automate sender-account selection
- Automate outreach tracking
- Allow the SEO manager to focus more on strategy and decision-making

---

# 🏗️ Architecture

```text
                 ┌───────────────────┐
                 │   Keyword Input   │
                 └─────────┬─────────┘
                           ↓
                 ┌───────────────────┐
                 │ Website Discovery │
                 └─────────┬─────────┘
                           ↓
                 ┌───────────────────┐
                 │ Website Extraction│
                 └─────────┬─────────┘
                           ↓
                 ┌───────────────────┐
                 │ Qualification     │
                 └─────────┬─────────┘
                           ↓
                 ┌───────────────────┐
                 │ Duplicate Check   │
                 └─────────┬─────────┘
                           ↓
                 ┌───────────────────┐
                 │ Gemini AI Agent   │
                 └─────────┬─────────┘
                           ↓
                 ┌───────────────────┐
                 │ Proposal Generator│
                 └─────────┬─────────┘
                           ↓
                 ┌───────────────────┐
                 │  Google Sheets    │
                 └─────────┬─────────┘
                           ↓
                 ┌───────────────────┐
                 │ 15-Minute Wait    │
                 └─────────┬─────────┘
                           ↓
                 ┌───────────────────┐
                 │ Gmail Account      │
                 │ Selection/Rotation │
                 └─────────┬─────────┘
                           ↓
                 ┌───────────────────┐
                 │   Gmail Sending   │
                 └─────────┬─────────┘
                           ↓
                 ┌───────────────────┐
                 │ Outreach Tracking │
                 └─────────┬─────────┘
                           ↓
                      Next Prospect
```

---

# 🔐 Security & Sanitization

The original production workflow contains configuration-specific information such as:

- OAuth credential references
- Google Sheet identifiers
- Gmail account information
- Workflow identifiers
- Private configuration values

These should **not** be exposed in a public repository.

The GitHub version should therefore contain a sanitized workflow with placeholders.

Example:

```text
YOUR_GOOGLE_SHEET_ID
YOUR_GEMINI_CREDENTIAL
YOUR_GMAIL_CREDENTIAL
YOUR_SENDER_EMAIL
YOUR_API_KEY
```

Credentials should be configured inside n8n rather than hard-coded into the workflow.

---

# 🚀 Setup

## Requirements

Before running the workflow, you will need:

1. n8n
2. Google Sheets account
3. Gmail account(s)
4. Gemini API / Google AI configuration
5. Search/API configuration used by the workflow
6. A keyword input file
7. A Google Sheet configured according to the workflow fields

## Basic Setup

```text
1. Import the sanitized workflow into n8n
2. Configure required credentials
3. Configure Google Sheets
4. Configure Gemini
5. Configure Gmail sender accounts
6. Configure required API/search credentials
7. Review the workflow settings
8. Upload keyword file
9. Execute the workflow
```

---

# ⚠️ Production Considerations

This repository is intended to demonstrate the **automation architecture and engineering approach**.

Before deploying in a production environment:

- Configure your own credentials
- Replace placeholder IDs
- Review email sending limits
- Follow applicable email and anti-spam requirements
- Respect website terms and applicable data-protection requirements
- Review prospect qualification rules
- Test the workflow with a small dataset first
- Monitor email delivery and outreach status

---

# 📂 Repository Structure

```text
ai-driven-guest-post-agency-automation/
│
├── README.md
│
├── workflow/
│   └── guest-post-outreach-sanitized.json
│
├── docs/
│   └── workflow-diagram.png
│
└── .gitignore
```

---

# 🎓 What This Project Demonstrates

This project demonstrates practical experience with:

- n8n workflow automation
- AI Agent integration
- Gemini LLM integration
- Structured AI output
- Web data extraction
- Conditional workflow logic
- Data transformation with JavaScript
- Duplicate detection
- Unique ID generation
- Google Sheets automation
- Gmail automation
- Multi-account workflow routing
- Rate-controlled processing
- Automated outreach
- End-to-end business process automation

---

# 🌐 Project Focus

**Domain:** Off-Page SEO / Guest Post Outreach

**Automation Type:** AI + Workflow Automation

**Platform:** n8n

**AI:** Google Gemini

**Database:** Google Sheets

**Communication:** Gmail

**Primary Goal:** Automate repetitive Guest Post prospecting, research, personalization, outreach and tracking tasks.

---

## ⭐ Project Philosophy

This project follows a simple automation principle:

> **Don't automate just one task — automate the repetitive process around the task.**

Instead of creating an automation that only finds websites or only sends emails, this system connects the major operational stages into one continuous workflow.

**Input → Research → Qualification → AI → Personalization → Outreach → Tracking → Next Prospect**

That is what turns an individual automation into a practical business workflow.
