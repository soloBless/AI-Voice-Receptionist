🍽️ Voice Reservation Assistant

AI-Powered Restaurant Booking System (VAPI + n8n + Supabase + RAG)

The Voice Reservation Assistant is a fully automated restaurant reservation management system powered by VAPI, orchestrated via n8n, and backed by Supabase.

It enables guests to make reservations, check availability, modify or cancel bookings, and ask general questions — all through a natural conversational voice interface.
For non-reservation questions, the system uses a RAG (Retrieval-Augmented Generation) pipeline connected to restaurant documentation stored in Google Drive.

<img width="1561" height="726" alt="Vapi + RAG" src="https://github.com/user-attachments/assets/f75ec900-36ee-4dc6-b5ef-2d8acfa07d32" />


🌐 Overview

This system combines AI voice automation, real-time reservation APIs, and on-demand document retrieval to create a seamless experience for both restaurant guests and staff.

The workflow handles:

Reservation creation

Reservation modifications

Reservation cancellations

Real-time table availability checks

Retrieval of specific booking details

General inquiry responses using RAG

Restaurant policy & menu information via document ingestion

Voice-friendly JSON output for VAPI

All workflows run in n8n, where VAPI calls the appropriate tool endpoints depending on the guest's intent.

✨ Key Features
🎙️ Voice Automation with VAPI

Natural, conversational reservation handling

Strict tool calling for structured outputs

Clean JSON responses for real-time voice feedback

🛎️ Smart Reservation Management

Create, modify, or cancel reservations

Check availability based on date, time, and party size

Auto-response if the restaurant is fully booked

📄 Dynamic RAG System

Uses Google Drive uploads + Supabase Vector Store

Automatically reindexes menu, policies, or restaurant documents

AI answers guest questions conversationally

Perfect for FAQs: menu, pricing, children policies, dress code, etc.

🗂️ Supabase-Powered Backend

API for reservations

Unique reservation IDs

Automatic table assignment

Tracking guest details and reservation metadata

📞 Real-Time Responses

All VAPI tool calls return structured JSON like:

{"results":[{"toolCallId":"xyz","result":"Your table is confirmed for 7pm"}]}

##Architecture

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

##Workflow Breakdown

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
