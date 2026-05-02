# 🚀 n8n Beginner Projects — Build Your Own AI-Powered Support System

> Inspired by the **Nebula AI Support Bot** — a workflow that takes a web form submission, sends an instant AI reply to the website, and emails a detailed AI-generated response using a knowledge base.

These 5 projects follow the **same architecture pattern**:
> `Web Form → Webhook → AI Agent → Instant Reply + Async Action (Email / Sheet / Notification)`

Each project uses a **Lovable.dev frontend** + **n8n backend**.

---

## 🧠 The Core Pattern You're Replicating

```mermaid
flowchart LR
    A[Web Form\nLovable UI] -->|POST| B[n8n Webhook]
    B --> C[Quick Reply Agent\nInstant response to website]
    B --> D[Deep AI Agent\nReads knowledge base]
    C --> E[Website shows\nconfirmation message]
    D --> F[Gmail sends\ndetailed email reply]
```

Every project below follows this exact shape. Only the **domain changes**.

---

## 📦 Project 1 — College FAQ Bot

**Use Case:** A student submits a question about admissions, fees, hostel, or syllabus. They get an instant reply on screen + a detailed email answer pulled from a Google Doc FAQ.

**Tools:** Lovable (UI) · n8n Webhook · OpenAI · Google Docs Tool · Gmail

```mermaid
flowchart TD
    A["🎓 Student fills form\n(Name, Email, Question)"] -->|POST to Webhook| B[n8n Webhook]
    B --> C["⚡ Quick Reply Agent\ngpt-3.5-turbo\nSystem: You are a college helpdesk bot"]
    B --> D["🤖 Deep Answer Agent\ngpt-4\nReads Google Doc: college_faq.docx"]
    C -->|Instant text| E["✅ Website shows:\n'Your query is received!\nCheck your email shortly.'"]
    D --> F["📧 Gmail Node\nSends detailed answer\nto student email"]

    style A fill:#1e3a5f,color:#fff
    style E fill:#1a472a,color:#fff
    style F fill:#4a235a,color:#fff
```

**What beginners learn:**
- Parallel branches in n8n
- Google Docs as a knowledge base
- Structured output parser for email subject + body

---

## 📦 Project 2 — Restaurant Menu Query Bot

**Use Case:** A customer visits a restaurant's website, types a query like "Do you have vegan options?" or "What's today's special?". They get an instant reply + a detailed email with recommendations.

**Tools:** Lovable (UI) · n8n Webhook · OpenAI · Google Sheets Tool · Gmail

```mermaid
flowchart TD
    A["🍽️ Customer fills form\n(Email, Food Query)"] -->|POST| B[n8n Webhook]
    B --> C["⚡ Quick Reply Agent\nSystem: You are a friendly restaurant assistant"]
    B --> D["🤖 Menu Agent\nReads Google Sheet: menu_2026.xlsx\nMatches query to items"]
    C -->|Instant reply| E["✅ Website: 'Great question!\nWe'll email you the details.'"]
    D --> F["📧 Gmail\nSends menu details,\nprice, allergen info"]

    style A fill:#3d1f00,color:#fff
    style E fill:#1a3d00,color:#fff
    style F fill:#3d003d,color:#fff
```

**What beginners learn:**
- Google Sheets as a dynamic menu database
- How to prompt an agent to "search" structured data
- Real-world customer-facing automation

---

## 📦 Project 3 — Event Registration Confirmation Bot

**Use Case:** A user registers for a workshop or webinar via a form. They get an instant "You're registered!" message on screen + a detailed confirmation email with schedule, zoom link, and prep material.

**Tools:** Lovable (UI) · n8n Webhook · OpenAI · Google Sheets (registrations log) · Gmail

```mermaid
flowchart TD
    A["📋 User submits registration\n(Name, Email, Event chosen)"] -->|POST| B[n8n Webhook]
    B --> C["⚡ Quick Reply Agent\nConfirm registration on screen"]
    B --> D["📝 Google Sheets Node\nAppend row: Name, Email, Event, Timestamp"]
    B --> E["🤖 Email Draft Agent\nGenerates personalized\nconfirmation email with event details"]
    C --> F["✅ Website: 'You're in!\nCheck your inbox for details.'"]
    D --> G["📊 Sheet updated\n(organizer can track registrations)"]
    E --> H["📧 Gmail\nSends event schedule,\nZoom link, prep tips"]

    style A fill:#0d2137,color:#fff
    style F fill:#1a3a1a,color:#fff
    style G fill:#2a1a0d,color:#fff
    style H fill:#2a0d2a,color:#fff
```

**What beginners learn:**
- Three parallel branches (not just two!)
- Google Sheets as a live registration log
- Combining data storage + AI email generation

---

## 📦 Project 4 — HR Leave Request Bot

**Use Case:** An employee submits a leave request via an internal portal. HR gets notified on Slack instantly. The employee gets a formal acknowledgment email. The request is logged in Google Sheets.

**Tools:** Lovable (UI) · n8n Webhook · OpenAI · Google Sheets · Gmail · Slack

```mermaid
flowchart TD
    A["👤 Employee fills Leave Form\n(Name, Email, Dates, Reason)"] -->|POST| B[n8n Webhook]
    B --> C["⚡ Quick Reply Agent\nInstant screen confirmation"]
    B --> D["📊 Google Sheets Node\nLog: Name, Dates, Reason, Status=Pending"]
    B --> E["🤖 Email Agent\nDrafts formal acknowledgment\nwith expected TAT"]
    B --> F["💬 Slack Node\nPings HR channel:\n'New leave request from [Name]'"]
    C --> G["✅ Website: 'Leave request submitted!\nYou'll receive a confirmation email.'"]
    E --> H["📧 Gmail → Employee\nAcknowledgment email"]

    style A fill:#1a1a3d,color:#fff
    style G fill:#1a3d1a,color:#fff
    style H fill:#3d1a1a,color:#fff
    style F fill:#2d1a3d,color:#fff
```

**What beginners learn:**
- Four parallel branches
- Slack integration (new tool!)
- Real enterprise workflow simulation

---

## 📦 Project 5 — Product Complaint Handler Bot

**Use Case:** A customer submits a product complaint on an e-commerce site. They get an instant apology + ticket number on screen. A detailed empathetic email is sent. The complaint is logged. High-severity complaints trigger a Slack alert.

**Tools:** Lovable (UI) · n8n Webhook · OpenAI · Google Sheets · Gmail · Slack · IF Node

```mermaid
flowchart TD
    A["😤 Customer submits complaint\n(Email, Product, Issue, Severity: Low/High)"] -->|POST| B[n8n Webhook]
    B --> C["⚡ Quick Reply Agent\nGenerate ticket number\nScreen: 'Complaint #1042 received!'"]
    B --> D["🤖 Complaint Email Agent\nEmpathetic reply\nResolution timeline\nNext steps"]
    B --> E["📊 Google Sheets\nLog complaint details + ticket ID"]
    D --> F["📧 Gmail → Customer\nFull resolution email"]
    B --> G["🔀 IF Node\nSeverity = High?"]
    G -->|Yes| H["💬 Slack Alert\n🚨 High severity complaint received!"]
    G -->|No| I["✅ No action\n(standard queue)"]

    style A fill:#3d0000,color:#fff
    style C fill:#1a3d00,color:#fff
    style F fill:#3d003d,color:#fff
    style H fill:#3d1a00,color:#fff
    style I fill:#1a1a1a,color:#fff
```

**What beginners learn:**
- IF Node (conditional logic — most important n8n concept!)
- Dynamic ticket ID generation
- Real-world complaint triage automation

---

## 🗺️ Choosing Your Project

| Project | Domain | New Tool Introduced | Difficulty |
|---|---|---|---|
| 1. College FAQ Bot | Education | Google Docs Tool | ⭐ Easiest |
| 2. Restaurant Menu Bot | F&B | Google Sheets Tool | ⭐ Easiest |
| 3. Event Registration Bot | Events | Sheets + 3 branches | ⭐⭐ Easy |
| 4. HR Leave Request Bot | Enterprise | Slack Node | ⭐⭐ Easy |
| 5. Product Complaint Bot | E-commerce | IF Node (conditional) | ⭐⭐⭐ Medium |

> **Recommended for first-timers:** Start with Project 1 or 2. Once comfortable, upgrade to Project 5 to learn conditional logic.

---

## 🛠️ Common Stack for All Projects

| Layer | Tool | Purpose |
|---|---|---|
| Frontend UI | Lovable.dev | Build the web form |
| Automation Backend | n8n | Orchestrate the workflow |
| AI Brain | OpenAI (gpt-3.5 / gpt-4) | Generate replies |
| Knowledge Base | Google Docs / Sheets | Store FAQs, menus, policies |
| Email Delivery | Gmail Node | Send detailed responses |
| Notifications | Slack Node | Alert teams (Projects 4 & 5) |
| Conditional Logic | IF Node | Route based on data (Project 5) |

---

## 📌 What to Submit

For each project, students should submit:

1. **Lovable UI link** (public share URL)
2. **n8n workflow JSON** (exported from n8n → Download)
3. **Screenshot** of a successful form submission + email received
4. **1-paragraph writeup**: What does your bot do? Who is it for?

---

*Built as part of the AI Product Management curriculum — IITP / MASAI cohort*
