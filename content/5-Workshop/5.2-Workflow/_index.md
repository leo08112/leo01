---
title: "Understanding the Workflow of the Bilingual Translation Project"
date: 2026-09-07
weight: 2
chapter: false
pre: " <b> 5.2. </b> "

---

# 5.2. Understanding the Workflow of the Bilingual Translation Project

In this chapter, we will explore the real-time workflow of our bilingual translation project utilizing AWS services.
![WORK-FLOW](/content/pictuer/workflow.jpg)
---

### Part 1: Web Client & Audio Streaming

* This section outlines the process of building the Web Client and capturing audio from the user's microphone. Audio is processed into small data segments (chunks) and continuously transmitted to the Backend to facilitate real-time Speech-to-Text processing.

* Microphone => Web Browser => Audio Capture => Audio Chunks => WebSocket => Backend

* The browser uses Web APIs to request microphone access permissions.

When the user clicks the "Start Recording" button, the application requests permission to use the microphone.

### Part 2: WebSocket & Streaming Speech-to-Text

* This section implements a real-time communication channel between the Web Client and the Backend, while integrating the Streaming Speech-to-Text service to convert audio into text as the user speaks.

* The system uses WebSockets to stream audio chunks, enabling continuous data transmission while the user speaks and allowing the receipt of processing results without creating a new HTTP request for each chunk. Each chunk is subdivided to minimize latency when displaying results on the screen.

### Part 3: Integrating Streaming Speech-to-Text (AWS Transcribe) and AWS Translate

* After deploying the system to an EC2 instance, the Backend runs on Amazon EC2 and serves as the central hub for system data coordination. The core objective is to enable the backend to call the AWS Transcribe and AWS Translate APIs **within the same environment (region)**, thereby reducing system latency.

(EC2 image + API call code)

*   Audio is transmitted by the backend to Amazon Transcribe Streaming to convert speech into text.
*   Unlike recording an entire sentence before uploading, Streaming Transcription allows the system to process audio while the user is speaking (making it well-suited for WebSockets).
*   Once a transcript segment suitable for translation is received, the backend sends the text to Amazon Translate.

### Part 4: Displaying the translation stream

*   Consolidating conversation data: After receiving the translation result, the backend generates a message containing both the original and translated content.
![Main interface](/PHAMTHO-AWS/picture1/translate-live.jpg)

*   This information facilitates the display on the user interface, allowing users to identify the speaker, the source language, the original spoken content, and the translated content.
*   This serves as the foundation for the system to display two languages ​​within a single conversation session.