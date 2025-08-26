# Website Forms → Google Sheets + Email (Zapier Automation)

A lightweight Zapier workflow that captures **two different website form submissions**, writes each submission to **Google Sheets**, and sends a **notification email**. Forms are routed using **Paths** so each form can have its own rules, recipients, and sheet mapping.

> Built with: *Webhooks by Zapier* → *Paths by Zapier* → *Gmail* → *Google Sheets*

---

## ✨ Features

- Accepts posts from any form that can send a **webhook** (Contact Form 7, Webflow, custom code, etc.)
- **Two forms supported** via Paths (Path A and Path B) with independent conditions
- Appends a row to a **Google Sheet** for each submission
- Sends a **Gmail** notification with dynamic fields
- Simple to extend (add more paths, Slack alerts, filters, validation, etc.)

---

## 🖼️ Diagram

Then reference it here:

![Zap overview showing Webhooks trigger, split into Paths A/B, each sending Gmail and creating a Google Sheets row.](./Lovable_Automation.png)

---

## 📋 Prerequisites

- A **Zapier** account
- A **Google** account with access to Gmail and Google Sheets
- Ability to send a **webhook** (HTTP POST) from your website forms
- A target **Google Sheet** with headers (see below)

---

## 🧱 Google Sheet Structure

Create a sheet with headers that match your fields. Example:

| Timestamp | Form Name | Name | Email | Phone | Message | Source URL | Raw Payload |
| --- | --- | --- | --- | --- | --- | --- | --- |

> Add/remove columns as needed—just map them in the Zap.

---

## 🔩 How It Works (Zap Steps)

1. **Trigger – Webhooks by Zapier: _Catch Hook_**  
   - Zapier gives you a **webhook URL**.  
   - Configure both forms to **POST JSON** to this URL.

2. **Action – Paths by Zapier: _Split into Paths_**  
   - **Path A** (Form A) → define a condition, e.g.  
     `form_name` **(exactly matches)** `contact` *(or use `form_id`, `type`, etc.)*  
   - **Path B** (Form B) → define the complementary condition, e.g.  
     `form_name` **(exactly matches)** `quote`

3. **Path A → Gmail: _Send Email_**  
   - **To:** your notification address  
   - **Subject:** `New {{form_name}} submission from {{name}}`  
   - **Body:** include dynamic fields from the webhook payload.

4. **Path A → Google Sheets: _Create Spreadsheet Row_**  
   - Map webhook fields to your sheet headers.  
   - Example: `Form Name` ← `form_name`, `Name` ← `name`, `Raw Payload` ← `Zap Meta Data > Raw Body`

5. **Path B → Gmail: _Send Email_** (similar to Path A, with different recipients if desired)

6. **Path B → Google Sheets: _Create Spreadsheet Row_** (map fields for the second form)

> The screenshot in this repo shows exactly this layout: **Catch Hook → Paths → (A/B) Path Conditions → Gmail → Google Sheets**.

---

## 🚀 Setup

1. **Create/verify your Google Sheet.**  
2. **In Zapier**, create a new Zap:
   - Trigger: **Webhooks by Zapier → Catch Hook** (copy the URL)
   - Action: **Paths by Zapier → Split into Paths**
   - For each Path:
     - **Path conditions:** match on a field that distinguishes your forms (e.g., `form_name`, `form_id`, `page`)
     - **Gmail → Send Email:** set recipients/subject/body with dynamic fields
     - **Google Sheets → Create Spreadsheet Row:** connect your sheet and map fields
3. **Point your website forms** at the webhook URL (JSON POST).
4. **Publish** the Zap and test both paths.
