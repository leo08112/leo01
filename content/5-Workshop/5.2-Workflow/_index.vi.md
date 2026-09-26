---
title: "Tìm hiểu về Work-Flow của project dịch song ngữ"
date: 2026-09-07
weight: 2
chapter: false
pre: " <b> 5.2. </b> "

---

# 5.2. Tìm hiểu về Work-Flow của project dịch song ngữ

Trong chương này, chúng ta sẽ tìm hiểu WorkFlow của Project dịch song ngữ theo thời gian thực của mình khi sử dụng các dịch vụ của AWS
![Giao diện chính](/PHAMTHO-AWS/picture1/workflow.jpg)
---

### Phần 1: Web Client & Audio Streaming

* Mục này trình bày quá trình xây dựng Web Client và thu âm thanh từ microphone của người dùng. Audio được xử lý thành các đoạn dữ liệu nhỏ và truyền liên tục đến Backend để phục vụ quá trình Speech-to-Text theo thời gian thực.
 
* Microphone =>Web Browser =>Audio Capture =>Audio Chunks => WebSocket => Backend

* Trình duyệt sử dụng Web APIs để yêu cầu quyền truy cập microphone.

Khi người dùng nhấn nút Start Recording, ứng dụng yêu cầu quyền sử dụng microphone

### Phần 2: WebSocket & Streaming Speech-to-Text

* Mục này triển khai kênh giao tiếp real-time giữa Web Client và Backend, đồng thời tích hợp dịch vụ Streaming Speech-to-Text để chuyển âm thanh thành văn bản trong quá trình người dùng đang nói.

* Hệ thống sẽ sử dụng Websocket để stream các audio chunk và cần truyền dữ liệu liên tục trong khi người dùng đang nói và nhận kết quả xử lý mà không phải tạo một HTTP request mới cho mỗi audio chunk. Mỗi chunk được chia nhỏ thành từng đoạn để tối ưu cho việc giảm độ trễ khi trả kết quả về màn hình

### Phần 3: Tích hợp Streaming Speech-to-Text (AWS Transcribe) và AWS translate

* Sau khi đã deploy hệ thống lên EC2 instance ,Backend được triển khai trên Amazon EC2 và đóng vai trò trung tâm điều phối dữ liệu của hệ thống. Cốt lõi là để back-end có thể gọi API của AWS Transcribe và AWS Translate **trong cùng 1 môi trường (khu vực)** từ đó giảm latency của hệ thống

(ảnh ec2 + code gọi api)

* Audio được Backend truyền tới Amazon Transcribe Streaming để chuyển đổi giọng nói thành văn bản.
* Khác với việc ghi âm toàn bộ câu rồi mới upload, Streaming Transcription cho phép hệ thống xử lý audio trong khi người dùng đang nói. (Phù hợp với Websocket)
* Sau khi nhận được đoạn transcript phù hợp để dịch, Backend gửi văn bản tới Amazon Translate.

### Phần 4: Hiện thị luồng dịch thuật

* Kết hợp dữ liệu hội thoại. Sau khi nhận được kết quả dịch, Backend tạo một message chứa cả nội dung gốc và nội dung đã dịch.
![Giao diện chính](/PHAMTHO-AWS/picture1/translate-live.jpg)

* Thông tin này giúp hiển thị trên giao diện , giúp người dùng biết: Ai là người nói, Ngôn ngữ gốc , Nội dung người dùng nói , Nội dung đã dịch.
* Đây là cơ sở để hệ thống hiển thị hai ngôn ngữ trong cùng một phiên hội thoại.

