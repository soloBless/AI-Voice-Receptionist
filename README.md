# 🍽️ Voice Reservation Assistant  
**AI-Powered Restaurant Booking System (VAPI + n8n + Supabase + RAG)**  

The **Voice Reservation Assistant** is a fully automated restaurant reservation management system powered by **VAPI**, orchestrated via **n8n**, and backed by **Supabase**.  

It enables guests to make reservations, check availability, modify or cancel bookings, and ask general questions — all through a natural conversational voice interface.  
For non-reservation questions, the system uses a **RAG (Retrieval-Augmented Generation)** pipeline connected to restaurant documentation stored in Google Drive.

<img width="1561" height="726" alt="Vapi + RAG" src="https://github.com/user-attachments/assets/198fad8d-9cb7-4e06-918b-cc6be08098d2" />
<img width="524" height="790" alt="vapi 3" src="https://github.com/user-attachments/assets/c93950bd-cf13-402e-9ea6-e4fa99c01e40" />
<img width="798" height="835" alt="vapi 1" src="https://github.com/user-attachments/assets/43e0cebe-dfcc-4485-b2bd-87e078fb7271" />




---

## 🌐 Overview
This system combines **AI voice automation**, **real-time reservation APIs**, and **on-demand document retrieval** to create a seamless experience for both restaurant guests and staff.

The workflow handles:

- Reservation creation  
- Reservation modifications  
- Reservation cancellations  
- Real-time table availability checks  
- Retrieval of specific booking details  
- General inquiry responses using RAG  
- Restaurant policy & menu information via document ingestion  
- Voice-friendly JSON output for VAPI  

All workflows run in **n8n**, where VAPI calls the appropriate tool endpoints depending on the guest's intent.

---

## ✨ Key Features

### 🎙️ Voice Automation with VAPI
- Natural, conversational reservation handling  
- Strict tool calling for structured outputs  
- Clean JSON responses for real-time voice feedback  

### 🛎️ Smart Reservation Management
- Create, modify, or cancel reservations  
- Check availability based on date, time, and party size  
- Auto-response if the restaurant is fully booked  

### 📄 Dynamic RAG System
- Uses Google Drive uploads + Supabase Vector Store  
- Automatically reindexes menu, policies, or restaurant documents  
- AI answers guest questions conversationally  
- Perfect for FAQs: menu, pricing, children policies, dress code, etc.

### 🗂️ Supabase-Powered Backend
- API for reservations  
- Unique reservation IDs  
- Automatic table assignment  
- Tracking guest details and reservation metadata  

### 📞 Real-Time Responses
- All VAPI tool calls return structured JSON like:  
  ```json
  {"results":[{"toolCallId":"xyz","result":"Your table is confirmed for 7pm"}]}

Guest Voice Call (VAPI)
        ↓  
VAPI detects intent → triggers tool  
        ↓  
n8n Webhook receives tool call  
        ↓  
Supabase Reservations API  
        ↓  
n8n Responds with structured tool output  
        ↓  
VAPI speaks the response back to guest  
        ↓  
For general inquiries → RAG Agent → Vector Search → Answer returned

        ┌──────────────────┐
        │      Caller       │
        └─────────┬────────┘
                  │ Voice
                  ▼
        ┌──────────────────┐
        │   VAPI Voice AI   │
        └─────────┬────────┘
        JSON Out  │
                  ▼
        ┌──────────────────┐
        │       n8n        │
        │  Webhook Router  │
        └─────────┬────────┘
       Calls Tools │
                  ▼
        ┌──────────────────┐
        │     RAG Engine    │
        │ Document Lookup   │
        └───────┬──────────┘
                │
                ▼
        ┌──────────────────┐
        │ External Services │
        │  DB / API / CRM  │
        └──────────────────┘


### Workflow Breakdown
---
1️⃣ Reservation Availability Check

Triggered when VAPI calls checkAvailability:

n8n receives date, time, and party size

Queries Supabase edge function

Returns: available / fully booked

2️⃣ Create Reservation

Creates reservation in Supabase

Assigns table

Returns confirmation message + table number

Stores reservation in n8n DataTable for quick reference

3️⃣ Retrieve Reservation Detail

Matches guest name + date

Returns time, table number, and party size

4️⃣ Cancellation Handler

Validates if reservation exists

If found, deletes from Supabase

Responds with cancellation confirmation

5️⃣ Rescheduling

Finds existing reservation

Updates date/time/party size

Returns updated assignment & table

6️⃣ RAG Inquiry System

For general info questions (menus, policies, hours):

n8n indexes Google Drive documents automatically

Uses Supabase Vector Store

Google Gemini generates short, voice-appropriate responses

ai-voice-receptionist/
├── workflows/
│   └── Vapi + RAG.json      # n8n workflow (reference from user)
├── docs/
│   └── KB/                  # RAG documents
├── api/
│   ├── createReservation.js
│   ├── checkAvailability.js
│   └── catalogue.js
└── README.md


### Technology Stack

| Component                   | Purpose                                  |
| --------------------------- | ---------------------------------------- |
| **VAPI**                    | Voice agent interface                    |
| **Twilio**                  | Phone number & call routing              |
| **n8n**                     | Workflow automation and API coordination |
| **Supabase Edge Functions** | Reservation logic & table assignment     |
| **Supabase Database**       | Stores reservation records               |
| **Google Drive**            | Document repository for RAG              |
| **Cohere Embeddings**       | Semantic search embeddings               |
| **Gemini Chat Model**       | Generates answers for inquiries          |
| **Vector Store**            | RAG retrieval engine                     |


### 📡 API Endpoints (Tool Webhooks)

| Purpose                | Webhook                                 |
| ---------------------- | --------------------------------------- |
| Check Availability     | `/b20f0493-dac9-46ee-b462-7e5103597edc` |
| Create Reservation     | `/eca58f82-c9c5-44bd-b563-6ac594e7ac43` |
| Retrieve Reservation   | `/3699aaf0-61ae-46bb-849d-fa73c2026316` |
| Cancel Reservation     | `/44411751-1729-4b35-a2ad-c1e8671346f1` |
| Reschedule Reservation | `/e19f9975-4d38-45f8-97c1-9cbcbf106cc9` |
| RAG Inquiry            | `/c8108e47-ff6a-4737-9bd3-ed524e2f5f9d` |

---
### 🔧 Prerequisites

n8n instance (cloud or self-hosted)

VAPI account with active phone number

Supabase project + Edge Function deployed

Google Drive folder for restaurant documents

Cohere API key

Google Gemini API key

⚙️ Setup Guide
1. Import Workflow

Upload file:
Vapi + RAG.json

2. Configure Credentials

Supabase API

Google Drive OAuth

Cohere embeddings

Gemini model

n8n Data Tables

3. Create Google Drive Folder

Documents automatically reindexed every minute.

4. Configure VAPI Agent

Add ALL tool endpoints to your agent:

- checkAvailability

- createReservation

- getReservation

- cancelReservation

- rescheduleReservation

- generalInquiry

5. Test a Call

Try:

“I want to book a table for two tomorrow at 7pm.”

“Can you move my reservation on Saturday?”

“What is your cancellation policy?”

---

### 🛡️ Error Handling

| Issue                   | Likely Cause                 | Fix                               |
| ----------------------- | ---------------------------- | --------------------------------- |
| “Invalid JSON”          | VAPI cannot parse the result | Ensure no newlines inside strings |
| “Reservation not found” | Name/date mismatch           | Confirm exact date format         |
| RAG not returning info  | Document not embedded        | Ensure Drive file is PDF/Docx     |

---
###🔗 Useful Extensions

Add WhatsApp ordering agent

Add POS integration

Add CRM guest profile system

Add auto-reminders via SMS

---

###🙌 Acknowledgments

VAPI for voice interoperability

n8n for orchestration

Supabase for backend logic

Google Drive + Gemini for RAG

Cohere for embeddings
