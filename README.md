# 🤖 Dani Ai Engineer | End-to-End Automated Customer Feedback Response System

An end-to-end Agentic AI workflow engineered with **n8n**, **Google Forms**, **Google Sheets**, **Groq (Llama 3.3)**, and **Gmail**. 

This system automatically detects new user responses submitted via Google Forms, processes customer feedback using an AI Model, sends a personalized email reply, and logs the generated AI response back into Google Sheets.

---

## 📌 How The Complete Workflow Works (End-to-End Flow)

1. **Google Form Submission:** 
   A user opens your Google Form (e.g., Customer Feedback Form) and fills in their details such as **Name**, **Email Address**, and **Feedback**.
2. **Data Logging in Google Sheets:** 
   Google Form automatically saves the submitted entry into an attached Google Sheet (`Form Responses 1`).
3. **n8n Google Sheets Trigger:** 
   n8n checks the Google Sheet on a scheduled interval (e.g., every 1 to 5 minutes). When a new row/entry is detected, the workflow triggers automatically.
4. **Duplicate Prevention Filter:** 
   A Filter node checks whether the `Ai Reply` column for that specific row is empty. This guarantees that duplicate emails are never sent for already-processed responses.
5. **AI Response Generation (LLM):** 
   The customer's Name and Feedback are sent to the AI Model (e.g., Groq Llama 3.3). The model generates a polite, concise 1-2 sentence thank-you reply based on strict prompt engineering rules.
6. **Automated Gmail Dispatch:** 
   The Gmail node constructs a clean email with dynamic fields (`Hi [Name]`, core AI reply body, and professional signature) and sends it directly to the customer's email address.
7. **Sheet Status Update:** 
   The n8n workflow updates the exact row in Google Sheets using the dynamic `row_number`, pasting the AI-generated reply into the `Ai Reply` column for audit logging and tracking.

---

## 🏗️ Workflow Architecture

``text
[ Google Form ]
       │
       ▼
[ Google Sheets ] ➔ [ Sheets Trigger ] ➔ [ Filter (Is Empty) ] ➔ [ AI Model (Llama 3.3) ] ➔ [ Gmail Node ] ➔ [ Update Sheet Row ]

📋 Prerequisites

    Automation Engine: Local or Cloud n8n Instance.

    Form & Storage: Google Form linked to a Google Sheet.

    Accounts & Credentials:

        Google Cloud Console OAuth2 Credentials (for Google Sheets & Gmail API).

        AI API Key (Groq API, OpenAI API, or Google Gemini API Key).

⚙️ How Anyone Can Use / Customise This Workflow
1. Import Workflow into n8n

    Open your n8n Canvas.

    Click Workflow Options (...) in the top-right corner.

    Select Import from File and choose the exported workflow.json file.

2. Connect Your Google Credentials

    Open Google Sheets Trigger and Update Row nodes to link your own Google Sheets OAuth2 credential.

    Open Send a Message (Gmail) node and link your Gmail OAuth2 account.

3. Flexible AI Provider Swap (If Not Using Groq)

You are not limited to Groq! If you prefer a different AI provider:

   Using Google Gemini: Delete the Groq node, add a Google Gemini Chat Model node (or direct Gemini node), and attach your Google Gemini API Key with gemini-1.5-flash.

   Using OpenAI (ChatGPT): Delete the Groq node, add an OpenAI Chat Model node, attach your OpenAI API Key, and select gpt-4o-mini or gpt-3.5-turbo.

 Using Anthropic Claude: Attach the Anthropic Chat Model node with your Claude API Key.

🔧 Node Mappings Reference
AI Node Prompt:

Write ONLY the body of a polite 1 to 2 sentence email thanking {{ $('Google Sheets Trigger').item.json.Name }} for their feedback: "{{ $('Google Sheets Trigger').item.json.Feedback }}"

CRITICAL RULES:
- Do NOT include any greetings (No "Dear", No "Hi", No "Hello").
- Do NOT include any sign-offs or names at the end (No "Warm regards", No "[Your Name]", No signature).
- Output ONLY the 1-2 sentence core thank-you message text.

  Gmail Message Body (Clean Regex Parsing):
  Hi {{ $('Google Sheets Trigger').item.json.Name }},

{{ ($json.text || $json.output || $json.message?.content || '').replace(/^Dear\s+[^,\n]+,?\s*/i, '').replace(/Warm regards,?\s*\[?Your Name\]?.*/is, '').trim() }}

Best regards,
Ai Engineer (M.Danish)

🚀 Activation

 Make sure your Google Sheet has an empty column named Ai Reply.

   Toggle the workflow to Active (ON) in n8n.

   Submit a new test response via your Google Form to verify end-to-end execution.

👨‍💻 Developed & Maintained By

   Dani Ai Engineer

Full-Stack Web Developer & AI Automation Engineer

<FollowUp label="Want me to draft a LinkedIn post to showcase this repository?" query="Draft a professional LinkedIn post showcasing my n8n AI Automation project with GitHub link placeholders."/>
