# 🤖 Dani Ai Engineer | Universal Automated Data Intake & AI Response System

An end-to-end Agentic AI workflow engineered with **n8n**, **Google Forms**, **Google Sheets**, **Groq (Llama 3.3)**, and **Gmail**. 

This system acts as a universal automation pipeline for **Student Registrations, Customer Support, Event Sign-ups, Job Applications, and Lead Management**. It completely eliminates manual data entry, logs structured data into Google Sheets, and sends personalized AI-generated emails in real time.

---

## 📌 How The Universal Workflow Works (End-to-End Flow)

1. **User Form Submission:** 
   A user submits a Google Form (e.g., Student Registration, Support Query, or Event Registration) with details like **Name**, **Email**, and **Details/Queries**.
2. **Automated Sheet Logging:** 
   Google Forms automatically pushes the user data into the attached Google Sheet.
3. **n8n Google Sheets Trigger:** 
   n8n monitors the spreadsheet and triggers instantly whenever a new row is appended.
4. **Duplicate Prevention Filter:** 
   A Filter node verifies whether the Ai Reply column is empty, ensuring no duplicate emails are dispatched for already-processed records.
5. **Contextual AI Generation:** 
   The AI model processes the user's information and generates a tailored, context-specific response following strict prompt rules.
6. **Automated Email Dispatch:** 
   The Gmail node dynamically formats and sends a clean response directly to the user's email address.
7. **Audit & Log Update:** 
   n8n updates the exact row in Google Sheets with the AI-generated reply for record-keeping and tracking.

---

## 🏗️ Workflow Architecture

[ Google Form ]
       │
       ▼
[ Google Sheets ] ➔ [ Sheets Trigger ] ➔ [ Filter (Is Empty) ] ➔ [ AI Model ] ➔ [ Gmail Node ] ➔ [ Update Sheet Row ]

💡 Versatile Use Cases

   Educational Institutions: Student onboarding, admission inquiries, and course registrations.

   Human Resources: Job application intake, candidate confirmations, and interview scheduling logs.

   Customer Support: Automated feedback management, ticket acknowledgment, and inquiry routing.

   Event Management: Webinar registrations, ticket confirmations, and participant tracking.

📋 Prerequisites

   Automation Engine: Local or Cloud n8n Instance.

   Form & Storage: Google Form linked to a Google Sheet.

   Accounts & Credentials: Google Sheets OAuth2, Gmail OAuth2, and AI API Key (Groq / OpenAI / Gemini).


   🔧 Node Mappings Reference
AI Node Prompt:
Write ONLY the body of a polite 1 to 2 sentence email responding to {{ $('Google Sheets Trigger').item.json.Name }} regarding their submission: "{{ $('Google Sheets Trigger').item.json.Feedback || $('Google Sheets Trigger').item.json.Details }}"

CRITICAL RULES:
- Do NOT include any greetings (No "Dear", No "Hi", No "Hello").
- Do NOT include any sign-offs or names at the end (No "Warm regards", No "[Your Name]", No signature).
- Output ONLY the 1-2 sentence core thank-you/acknowledgment message text.

Gmail Message Body (Clean Parsing):
Hi {{ $('Google Sheets Trigger').item.json.Name }},

{{ ($json.text || $json.output \vert{}\vert{}$json.message?.content || '').replace(/^Dear\s+[^,\n]+,?\s*/i, '').replace(/Warm regards,?\s*[?Your Name]?.*/is, '').trim() }}

Best regards,
Ai Engineer (M.Danish)

👨‍💻 Developed & Maintained By

Dani Ai Engineer

Full-Stack Web Developer & AI Automation Engineer
