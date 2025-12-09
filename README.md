# 🍽️ Voice Reservation Assistant  
**AI-Powered Restaurant Booking System (VAPI + n8n + Supabase + RAG)**  

The **Voice Reservation Assistant** is a fully automated restaurant reservation management system powered by **VAPI**, orchestrated via **n8n**, and backed by **Supabase**.  

It enables guests to make reservations, check availability, modify or cancel bookings, and ask general questions — all through a natural conversational voice interface.  
For non-reservation questions, the system uses a **RAG (Retrieval-Augmented Generation)** pipeline connected to restaurant documentation stored in Google Drive.

<img width="1561" height="726" alt="Vapi + RAG" src="https://github.com/user-attachments/assets/198fad8d-9cb7-4e06-918b-cc6be08098d2" />


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
