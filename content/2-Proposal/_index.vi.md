---
title: "Bản đề xuất"
date: 2026-09-06
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Real-time Bidirectional Translation Web trên AWS

## 1. Tóm tắt đề tài

Đề tài xây dựng một **web application hỗ trợ phiên dịch hội thoại song ngữ theo thời gian thực**, cho phép hai người sử dụng hai ngôn ngữ khác nhau giao tiếp trong cùng một phiên trò chuyện.

Hệ thống sử dụng **Streaming Speech-to-Text (STT)** để chuyển giọng nói thành văn bản và **Machine Translation** để dịch nội dung sang ngôn ngữ của người còn lại. Kết quả gồm **văn bản gốc và văn bản đã dịch** được hiển thị gần như đồng thời trên giao diện web.

Kiến trúc hướng tới **độ trễ thấp**, sử dụng WebSocket để duy trì kết nối real-time giữa client và backend.

---

## 2. Vấn đề cần giải quyết

Các hệ thống dịch hội thoại thông thường có thể gặp:

* Độ trễ cao do xử lý tuần tự từng câu.
* Không hỗ trợ tốt việc truyền dữ liệu liên tục từ microphone.
* Người dùng phải chờ hoàn thành toàn bộ câu mới nhận được kết quả.
* Khó đồng bộ nội dung của hai người trong cùng một cuộc hội thoại.

### Giải pháp đề xuất

Hệ thống xử lý dữ liệu theo streaming:

**Microphone → Web Client → WebSocket → Backend → Streaming STT → Translation → Original + Translated Text → Web Client**

Mỗi người tham gia cùng một conversation session và có thể nhìn thấy nội dung của cả hai phía.

---

## 3. Kiến trúc hệ thống

### Flow 1 – Audio Streaming

Người dùng cấp quyền microphone và nói trực tiếp trên trình duyệt.

Audio được chia thành các chunk nhỏ và gửi liên tục thông qua **WebSocket** tới backend.

### Flow 2 – Speech-to-Text

Backend nhận audio streaming và gửi tới dịch vụ **Streaming Speech-to-Text**.

Kết quả transcript được trả về liên tục thay vì chờ người dùng nói xong hoàn toàn.

### Flow 3 – Translation

Transcript sau khi nhận được sẽ được gửi tới dịch vụ Translation để chuyển sang ngôn ngữ đích.

Ví dụ:

**User A:** "Hello, how are you?"
**Original:** Hello, how are you?
**Translated:** Xin chào, bạn khỏe không?

### Flow 4 – Shared Conversation

Backend gửi kết quả về conversation session thông qua WebSocket.

Cả User A và User B đều nhận được:

* Nội dung gốc.
* Nội dung đã dịch.
* Thông tin người nói.

---

## 4. AWS Services sử dụng

| AWS Service                 | Vai trò                                  |
| --------------------------- | ---------------------------------------- |
| **Amazon S3**               | Host frontend/static files               |
| **API Gateway / WebSocket** | Kết nối real-time giữa client và backend |
| **AWS Lambda**              | Xử lý các API/backend logic phù hợp      |
| **Amazon Transcribe**       | Streaming Speech-to-Text                 |
| **Amazon Translate**        | Dịch văn bản                             |
| **Amazon DynamoDB**         | Lưu conversation/session nếu cần         |
| **Amazon CloudWatch**       | Logging và monitoring                    |
| **IAM**                     | Quản lý quyền truy cập AWS               |

Backend có thể chạy trên Lambda cho các tác vụ phù hợp, nhưng **không nên đưa toàn bộ hệ thống vào Lambda**, đặc biệt khi cần duy trì các kết nối streaming dài hoặc xử lý real-time liên tục.

---

## 5. Luồng hoạt động chính

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

## 6. Các giai đoạn triển khai

### Giai đoạn 1 – Thiết kế

* Xác định conversation flow.
* Thiết kế WebSocket architecture.
* Xác định cách xử lý audio streaming.
* Xác định yêu cầu về latency.

### Giai đoạn 2 – Xây dựng MVP

* Xây dựng giao diện web.
* Thu âm microphone.
* Gửi audio qua WebSocket.
* Kết nối Streaming STT.
* Hiển thị transcript.

### Giai đoạn 3 – Tích hợp Translation

* Tích hợp Amazon Translate.
* Hiển thị original text và translated text.
* Xây dựng conversation session cho User A và User B.

### Giai đoạn 4 – AWS Deployment

* Deploy frontend lên S3.
* Deploy backend lên AWS.
* Cấu hình IAM.
* Thiết lập CloudWatch logging.
* Kiểm thử kết nối real-time.

### Giai đoạn 5 – Benchmark & Optimization

Đo:

* **Speech-to-Text latency**
* **Translation latency**
* **End-to-end latency**
* Độ ổn định WebSocket.
* Khả năng xử lý nhiều chunk audio liên tục.

Sau đó tối ưu chunk size, network communication và cách xử lý streaming.

---

## 6. Kết quả kỳ vọng

Hệ thống hoàn thành có khả năng:

* Phiên dịch **hai chiều theo thời gian thực**.
* Hai người dùng cùng tham gia một conversation session.
* Hiển thị đồng thời **Original Text + Translated Text**.
* Sử dụng Streaming STT thay vì xử lý audio sau khi ghi âm hoàn toàn.
* Có độ trễ thấp và có thể triển khai trên AWS.
* Có thể đo lường và đánh giá latency thông qua benchmark.

### Mục tiêu chính

> **Microphone → Streaming STT → Translation → Shared Conversation với độ trễ thấp.**
