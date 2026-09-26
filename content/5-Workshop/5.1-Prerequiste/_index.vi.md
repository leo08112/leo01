---
title: "Chuẩn bị Môi trường AWS"
date: 2026-09-06
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
aliases:

---

# 5.1. Chuẩn bị Môi trường AWS
Trong phần này chúng ta sẽ đi qua những công đoạn khởi tạo môi trường để cơ bản cho việc chạy project cá nhân trong quá trình thực tập tại công ty AWS Việt Nam

### Yêu cầu tiên quyết:
1.  **Tài khoản AWS (AWS Account)** :Tài khoản AWS (AWS Account) có quyền sử dụng các dịch vụ cần thiết cho GP như IAM, S3, EC2/Lambda, API Gateway, Amazon Transcribe và Amazon Translate.
2.  **Trình duyệt web** :(Google Chrome, Microsoft Edge, Firefox hoặc Safari) để truy cập AWS Management Console và kiểm thử ứng dụng Web.
3.  **Môi trường phát triển** : 
      * Visual Studio Code hoặc IDE tương đương
      * Node.js và NPM
      * Git để quản lí source code
4. Công cụ kiểm thử API: Postman hoặc cURL để gửi request và kiểm tra Backend/API.
5. Thiết bị có microphone để kiểm thử chức năng thu âm và Streaming Speech-to-Text của hệ thống.
---

### Bước 1: Đăng nhập Console và Chuyển Region
1. Truy cập [AWS Management Console](https://console.aws.amazon.com/) và đăng nhập tài khoản của bạn.
2. Trên góc trên bên phải thanh điều hướng, chọn Region **Asia Pacific (Singapore) - ap-southeast-1**.

![Chuyển Region sang Singapore](/PHAMTHO-AWS/picture1/region.jpg)

### Bước 2: Kiểm tra Region

* Sau khi chuyển sang Singapore (ap-southeast-1), tiến hành kiểm tra các AWS Service dự kiến sử dụng trong Project:

1. AWS Service	Mục đích trong GP
2. IAM	Quản lý quyền truy cập AWS
3. S3	Lưu trữ source/frontend hoặc tài nguyên tĩnh
4. EC2 / Lambda	Chạy Backend
5. API Gateway	Cung cấp API/WebSocket
6. Amazon Transcribe	Chuyển giọng nói thành văn bản
7. Amazon Translate	Dịch văn bản giữa các ngôn ngữ
8. Kết quả đạt được

### Bước 3: Thiết lập IAM & Quyền truy cập

* Tạo Các nhóm đối tượng gồm: User - Group - Role - Policy - Permission.
* Bảo mật là một thành phần xuyên suốt trong kiến trúc AWS, không nên chỉ tập trung vào việc làm cho ứng dụng hoạt động mà bỏ qua việc giới hạn quyền truy cập.

* IAM (Identity and Access Management) được sử dụng để kiểm soát người dùng, role và quyền truy cập vào các AWS Service.
![Tạo User mới trong IAM Users](/PHAMTHO-AWS/picture1/IAM-User.jpg)
* Lập các IAM Group , Phân chia các role , user cho việc truy cập từng services khác nhau
![Phân chia các role , user cho việc truy cập từng services khác nhau](/PHAMTHO-AWS/picture1/IAM-Roles.jpg)

### Bước 4: Thiết lập Network & AWS Services

* Thiết lập VPC dành riêng cho backend của Project.
![VPC cá nhân đã tạo sẵn](/PHAMTHO-AWS/picture1/VPC.jpg)

* Thiết lập Subnet và Security Group.
![Subnets được phân chia dựa theo địa chỉ IPV4](/PHAMTHO-AWS/picture1/Subnets.jpg)
* Subnets được phân chia dựa theo địa chỉ IPV4
- Public subnet được cấu hình trong khoảng 10.0.1.0/24 ( có 256 IP khả dụng)
- Tương tự với 2 Private subnet 
![Subnets được phân chia dựa theo địa chỉ IPV4](/PHAMTHO-AWS/picture1/SUBNET-MAP.jpg)
Chỉ có Public subnet có thể connect được tới Internet-Gateway

### Bước 5: Tạo S3 Bucket

1. Truy cập AWS Console, tìm kiếm S3 và chọn Create bucket.

2. Tại trang Create bucket, điền các thông tin sau:

* Bucket name: Nhập translate-2026 (hoặc tên duy nhất bất kỳ).
* AWS Region: Chọn ap-southeast-1 (Singapore).

![Subnets được phân chia dựa theo địa chỉ IPV4](/PHAMTHO-AWS/picture1/s3.jpg)
* Giữ nguyên vô hiệu hóa ACLs (khuyên dùng). Bỏ tick ô **Block all public access** nếu bạn muốn các file JSON có thể được tải về công khai. Xác nhận cảnh báo và nhấn Create bucket.
