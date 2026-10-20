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
