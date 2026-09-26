---
title: "Proposal"
date: 2026-09-06
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Real-time Bidirectional Translation Web Application on AWS

## 1. Project Summary

This project involves building a **web application that supports real-time bilingual conversation translation**, enabling two users speaking different languages ​​to communicate within the same conversation session.

The system utilizes **Streaming Speech-to-Text (STT)** to convert speech into text and **Machine Translation** to translate the content into the other user's language. Both the **original and translated text** are displayed almost simultaneously on the web interface.

The architecture prioritizes **low latency**, using WebSockets to maintain a real-time connection between the client and the backend.

---

## 2. Problem Statement

Conventional conversation translation systems often face the following issues:

* High latency due to sequential, sentence-by-sentence processing.
* Poor support for continuous data transmission from the microphone.
* Users must wait for a full sentence to be completed before receiving results.
* Difficulty synchronizing content between the two participants in the conversation.

### Proposed Solution

A system that processes data via streaming:

**Microphone → Web Client → WebSocket → Backend → Streaming STT → Translation → Original + Translated Text → Web Client**

Participants join the same conversation session and can view content from both sides.

---

## 3. System Architecture

### Flow 1 – Audio Streaming

Users grant microphone permissions and speak directly into the browser.

Audio is divided into small chunks and transmitted continuously to the backend via **WebSocket**.

### Flow 2 – Speech-to-Text

The backend receives the audio stream and forwards it to the **Streaming Speech-to-Text** service.

Transcript results are returned continuously, rather than waiting for the user to finish speaking completely. ### Flow 3 – Translation

Once received, the transcript is sent to the Translation service to be converted into the target language.

Example:

**User A:** "Hello, how are you?"
**Original:** Hello, how are you?
**Translated:** Xin chào, bạn khỏe không?

### Flow 4 – Shared Conversation

The backend sends conversation session results via WebSocket.

Both User A and User B receive:

*   Original content.
*   Translated content.
*   Speaker information.

---

## 4. AWS Services Used

| AWS Service                 | Role                                     |
| --------------------------- | ---------------------------------------- |
| **Amazon S3**               | Hosting frontend/static files            |
| **API Gateway / WebSocket** | Real-time connection between client and backend |
| **AWS Lambda**              | Handling appropriate API/backend logic   |
| **Amazon Transcribe**       | Streaming Speech-to-Text                 |
| **Amazon Translate**        | Text translation                         |
| **Amazon DynamoDB**         | Storing conversation/session data if needed |
| **Amazon CloudWatch**       | Logging and monitoring                   |
| **IAM**                     | AWS access permission management         |

The backend can run on Lambda for suitable tasks, but **the entire system should not be deployed on Lambda**, especially when maintaining long-running streaming connections or handling continuous real-time processing.

---

## 5. Main Operational Flow

```text
User A
│
│ Microphone
▼
Web Client
│
│ WebSocket
▼
Backend
│
▼
Streaming STT
│
▼
Transcript
│
▼
Translation
│
├──────────────┐
▼              ▼
Original       Translated
│              │
└──────┬───────┘
▼
Shared Conversation
▲
│
User B
```

---

## 6. Implementation Phases

### Phase 1 – Design

*   Define the conversation flow. * Design the WebSocket architecture.
* Determine the approach for handling audio streaming.
* Define latency requirements.

### Phase 2 – MVP Development

* Build the web interface.
* Capture microphone audio.
* Transmit audio via WebSocket.
* Integrate Streaming STT.
* Display the transcript.

### Phase 3 – Translation Integration

* Integrate Amazon Translate.
* Display both original and translated text.
* Implement a conversation session for User A and User B.

### Phase 4 – AWS Deployment

* Deploy the frontend to S3.
* Deploy the backend to AWS.
* Configure IAM.
* Set up CloudWatch logging.
* Test real-time connectivity.

### Phase 5 – Benchmarking & Optimization

Measure:

* **Speech-to-Text latency**
* **Translation latency**
* **End-to-end latency**
* WebSocket stability.
* Capability to handle continuous audio chunks.

Subsequently, optimize chunk size, network communication, and streaming processing.

---

## 6. Expected Outcomes

The completed system is capable of:

* **Real-time, two-way** translation.
* Allowing two users to participate in a single conversation session.
* Simultaneously displaying **Original Text + Translated Text**.
* Utilizing Streaming STT instead of processing audio only after recording is complete.
* Delivering low latency and being deployable on AWS.
* Enabling latency measurement and evaluation via benchmarking.

### Key Objective

> **Microphone → Streaming STT → Translation → Shared Conversation with low latency.**