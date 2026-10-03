# AI-Driven Guest Post Agency Automation

> **Replaces weeks of manual Off-Page SEO outreach with a full-stack autonomous n8n AI agent. Crawls prospect domains, classifies niches via Gemini LLM, drafts customized pitches, and balances multi-account Gmail delivery.**

## 📌 Overview

This project is a real-world **Off-Page SEO and Guest Post Outreach Automation System** built with **n8n, Google Gemini, Google Sheets, Gmail, and automated web research**.

It was developed to solve a practical business problem: a Guest Post / Off-Page SEO manager repeatedly performing the same prospect research, qualification, contact discovery, proposal writing, email sending, and tracking tasks.

Instead of automating only one step, this workflow automates the complete prospect-processing pipeline while filtering out websites that should never reach the outreach stage.

---

# 🎯 Real-World Problem

Guest post outreach is not simply about sending emails.

Before a single outreach email can be sent, an SEO professional may have to:

- Search Google for relevant websites
- Open and analyze prospect websites
- Look for guest posting opportunities
- Check whether the website should be considered
- Find contact information
- Extract email addresses
- Check existing records for duplicates
- Identify the website's niche
- Write a personalized outreach proposal
- Prepare a relevant subject line
- Send the email
- Record the outreach status
- Repeat the entire process for the next prospect

When hundreds of search results are involved, most of the work becomes repetitive.

More importantly, **not every website discovered through search is a valid outreach prospect**.

Some websites should be excluded, some do not offer guest posting opportunities, and some do not expose a usable email address.

Manually investigating all of these possibilities consumes significant operational time even though many prospects will ultimately be rejected.

---

# 💡 The Solution

This project converts that repetitive manual process into an automated **n8n prospect qualification and outreach pipeline**.

The workflow does not blindly send an email to every website it discovers.

Instead, every prospect moves through multiple research, extraction, filtering, and qualification stages.

```text
Keyword File
     ↓
Search for Potential Websites
     ↓
Process Prospect
     ↓
Website / Page Extraction
     ↓
Website Analysis & Qualification
     ↓
 ┌───────────────────────────────┐
 │        Prospect Result        │
 └───────────────────────────────┘
          ↓
 ┌────────┼──────────┬───────────────┐
 ↓        ↓          ↓               ↓
Not      Guest      Email        Qualified
Allowed  Post Not   Not Found    Prospect
         Found                         ↓
 ↓        ↓          ↓             AI Analysis
Skip     Skip       Skip              ↓
                               Proposal Generation
                                      ↓
                               Database Update
                                      ↓
                                15-sec Wait
                                      ↓
                              Gmail Selection
                                      ↓
                                Send Email
                                      ↓
                              Update Tracking
                                      ↓
                                Next Prospect
```

This filtering architecture is important because **search result ≠ outreach email**.

Only prospects that successfully pass the required workflow conditions continue toward proposal generation and email delivery.

---

# 🔍 Intelligent Prospect Filtering

One of the most important parts of the automation is deciding which prospects should continue through the workflow.

The system contains separate handling paths for different outcomes.

### 🚫 Not Allowed Website

If a website falls into the workflow's excluded/not-allowed conditions, it does not continue through the normal outreach pipeline.

### ❌ Guest Post Not Found

A website may be valid and accessible but still not provide the guest posting opportunity the workflow is looking for.

That prospect is handled separately and the workflow continues with the next item.

### 📭 Email Not Found

A website may appear suitable for outreach, but the automation may fail to discover a usable email address.

Instead of attempting to send an email, the prospect is recorded/handled through the **Email Not Found** path and processing continues.

### ✅ Qualified / Email Found

Only a prospect that satisfies the required conditions and provides the necessary outreach information continues to the AI and proposal-generation stages.

This means the system spends the email-sending stage only on prospects that successfully reach that part of the pipeline.

---

# 🤖 AI-Powered Website Classification

Qualified prospect information is passed to a **Google Gemini Chat Model** through an n8n AI Agent.

A Structured Output Parser helps return information in a predictable format.

The AI stage assists with information such as:

- Brand identification
- Website understanding
- Niche/category classification
- Structured prospect information

The resulting information is then passed to the proposal-generation stage.

---

# ✍️ Personalized Proposal Generation

Once a qualified prospect reaches the outreach stage, the workflow automatically generates its proposal.

The Proposal Generator uses prospect information such as:

- Website/domain
- Brand name
- Website niche/category

Multiple proposal templates and subject-line variations are available rather than relying on one identical email format for every prospect.

The result includes:

```text
Qualified Prospect
        ↓
Gemini Analysis
        ↓
Brand + Category
        ↓
Proposal Generator
       ↙   ↘
  Subject   Personalized
   Line       Proposal
```

Only prospects that have successfully passed the preceding qualification stages reach this process.

---

# 🛡️ Duplicate Protection

Repeatedly contacting the same website or email address is another common problem in manual outreach.

The workflow includes dedicated **website + email duplicate protection** and Unique ID handling.

```text
Website + Email
       ↓
Create / Check Unique ID
       ↓
Duplicate Protection
       ↓
Eligible Prospect
       ↓
Continue Processing
```

This reduces unnecessary repeated outreach and makes the prospect database easier to manage.

---

# 📊 Centralized Google Sheets Tracking

Google Sheets acts as the outreach database for the automation.

Depending on the prospect and workflow path, the system can maintain information such as:

- Website
- Brand Name
- Email
- Contact Links
- Guest Post Links
- Guest Post status
- Social profiles
- Category
- Unique ID
- Proposal
- Outreach Status
- Sent Date
- Sender Email

Different outcomes can therefore be handled without forcing every discovered website into the email-sending stage.

---

# ⏱️ Controlled Processing & Natural Email Gap

The email-delivery logic is designed so that emails are **not fired for every search result one after another**.

There are two reasons for this.

## 1. Prospect Processing Creates Natural Time Between Emails

After one prospect is processed, the workflow must continue processing subsequent search results.

Those results may require:

- Website/page extraction
- Data processing
- Qualification
- Guest post checking
- Contact discovery
- Duplicate checking
- AI analysis
- Proposal generation

And many results never reach email sending at all.

For example:

```text
Email Sent to Prospect A
          ↓
Process Next Search Result
          ↓
Not Allowed
          ↓
Process Next Search Result
          ↓
Guest Post Not Found
          ↓
Process Next Search Result
          ↓
Email Not Found
          ↓
Process Next Search Result
          ↓
Qualified + Email Found
          ↓
Generate Proposal
          ↓
15-second Wait
          ↓
Send Next Email
```

Therefore, the real interval between two sent emails can naturally be **longer than 15 seconds**, because the workflow performs research and qualification work between successful outreach prospects.

## 2. Additional 15-Second Wait Before Sending

For a prospect that reaches the outreach stage, the workflow contains an additional **15-second Wait node** before Gmail account selection.

```text
Proposal Generated
       ↓
Database Updated
       ↓
Wait 15 Seconds
       ↓
Select Gmail Account
       ↓
Send Email
```

The 15 seconds should therefore **not be interpreted as “one email every 15 seconds.”**

It is an additional controlled pause inside a larger sequential workflow.

The actual gap between two sent emails depends on how many prospects are processed, rejected, analyzed, or skipped between successful email-found prospects.

---

# 📧 Multi-Account Gmail Rotation

After the qualified prospect passes the wait stage, the workflow selects between configured Gmail accounts.

```text
Qualified Prospect
        ↓
Proposal Ready
        ↓
15-Second Wait
        ↓
Gmail Account Selection
       ↙           ↘
 Account 1       Account 2
     ↓               ↓
 Send Email       Send Email
       ↘           ↙
       Update Status
             ↓
       Next Prospect
```

The account-selection logic alternates the configured sender accounts so outreach does not depend on only one Gmail connection.

---

# 🔄 Complete End-to-End Workflow

## 1. Upload Keyword File

The workflow starts through the **AI Outreach Manager** form where a keyword file is provided.

## 2. Search Potential Websites

The keyword data is used to discover potential Guest Post / Off-Page SEO prospects.

## 3. Process Search Results

The workflow processes discovered websites individually.

## 4. Extract Website Data

Relevant pages and homepage information are extracted and analyzed.

## 5. Filter Invalid / Not Allowed Websites

Prospects that should not continue are excluded from the main outreach pipeline.

## 6. Detect Guest Post Opportunity

The system evaluates whether the required guest post opportunity is present.

If not, the prospect follows the **Guest Post Not Found** path.

## 7. Extract Contact Information

The automation attempts to locate relevant contact information and email addresses.

If no usable email is found, the prospect follows the **Email Not Found** path.

## 8. Protect Against Duplicates

Website/email information and Unique IDs are used to avoid unnecessary duplicate processing/outreach.

## 9. Analyze Qualified Prospect with AI

Qualified prospect information is passed to the Gemini-powered AI Agent for structured analysis and classification.

## 10. Generate Personalized Proposal

The workflow generates the outreach proposal and email subject.

## 11. Update Prospect Database

Relevant prospect and proposal information is stored/updated in Google Sheets.

## 12. Wait 15 Seconds

Before Gmail account selection, an additional 15-second pause is introduced.

## 13. Select Gmail Sender

The account-rotation logic determines which configured Gmail account should send the email.

## 14. Send Outreach Email

The personalized proposal is delivered through the selected Gmail account.

## 15. Update Sending Record

After sending, the workflow updates the prospect record with information including:

- Status
- Sent Date
- Unique ID
- Sender Email

## 16. Continue to Next Prospect

The workflow returns to the loop and begins processing the next search result.

The next result may be rejected or may eventually become the next successful outreach email.

---

# 🔁 What Was Manual vs What Is Automated

| Repetitive Manual Task | Automated Solution |
|---|---|
| Search websites manually | Automated website discovery |
| Open prospect websites | Automated page processing |
| Inspect website pages | Automated extraction |
| Identify unsuitable websites | Not Allowed filtering |
| Check guest post availability | Automated qualification |
| Find contact details | Automated extraction |
| Find email addresses | Automated email discovery |
| Handle missing emails | Email Not Found branch |
| Check previous prospects | Duplicate protection |
| Understand website niche | Gemini AI classification |
| Write outreach proposal | Automated proposal generation |
| Create subject line | Automated subject generation |
| Maintain prospect database | Google Sheets automation |
| Control sending sequence | Sequential workflow + wait |
| Select sender account | Gmail account rotation |
| Send email manually | Automated Gmail delivery |
| Record sending date | Automatic tracking |
| Record sender account | Automatic tracking |
| Repeat for next website | Automated loop |

---

# 💼 Real-World Business Value

The value of this project is not simply that **“it can send emails.”**

Its real value is that it automates the repetitive work that happens **before and after** an outreach email.

Instead of an Off-Page SEO manager repeatedly performing:

**Search → Research → Filter → Extract → Check → Classify → Write → Send → Record → Repeat**

the automation performs the operational pipeline and routes each prospect according to its actual result.

The human operator can therefore spend less time performing repetitive prospect-by-prospect operations and more time on higher-level activities such as:

- SEO strategy
- Outreach strategy
- Relationship building
- Campaign decisions
- Reviewing qualified opportunities
- Improving campaign quality

---

# 🧰 Technology Stack

- **n8n** — Workflow orchestration
- **Google Gemini** — AI analysis and niche classification
- **Google Sheets** — Prospect and outreach database
- **Gmail** — Outreach email delivery
- **HTTP / Web Processing** — Prospect discovery and website extraction
- **JavaScript / n8n Code Nodes** — Workflow logic and data transformation
- **Structured Output Parser** — Structured AI responses

---

# 🔐 Security

The production workflow contains environment-specific configuration and credential references.

For a public GitHub repository, sensitive information should be replaced with placeholders such as:

```text
YOUR_GOOGLE_SHEET_ID
YOUR_GEMINI_CREDENTIAL
YOUR_GMAIL_ACCOUNT_1
YOUR_GMAIL_ACCOUNT_2
YOUR_API_KEY
```

Never commit:

- API keys
- OAuth secrets
- Access tokens
- Private credential IDs
- Production email credentials
- Other confidential configuration

---

# 📂 Repository Structure

```text
ai-driven-guest-post-agency-automation/
│
├── README.md
├── workflow/
│   └── guest-post-outreach-sanitized.json
├── docs/
│   └── workflow-diagram.png
└── .gitignore
```

---

# 🚀 Setup

1. Import the sanitized workflow JSON into n8n.
2. Configure the required search/API credentials.
3. Connect Google Gemini.
4. Configure the required Google Sheet.
5. Connect the Gmail sender accounts.
6. Replace placeholder configuration values.
7. Test the workflow with a small keyword dataset.
8. Review qualification and outreach rules before production use.
9. Activate the workflow when configuration is complete.

---

# ⚠️ Responsible Use

This project demonstrates the automation architecture used for legitimate Off-Page SEO outreach.

Users deploying it should configure appropriate prospect-selection rules, respect applicable email and data-protection requirements, follow provider sending limits, and avoid unsolicited bulk-email behavior.

---

# 🎓 What This Project Demonstrates

This real-world project demonstrates practical experience in:

- AI workflow automation
- n8n workflow development
- Multi-step business process automation
- Web research automation
- Data extraction and transformation
- Conditional workflow routing
- Prospect qualification
- Duplicate protection
- AI Agent implementation
- Google Gemini integration
- Structured AI output
- Personalized content generation
- Google Sheets automation
- Gmail automation
- Multi-account routing
- Sequential processing
- Outreach tracking

---

# ⭐ Core Automation Principle

> **Don't automate only the final task — automate the repetitive process that leads to it.**

This project does not simply automate email sending.

It automates the operational journey from:

**Keyword → Prospect Discovery → Website Research → Qualification → Contact Discovery → Duplicate Protection → AI Classification → Proposal Generation → Controlled Sending → Tracking → Next Prospect**

That is the real-world problem this automation was built to solve.
