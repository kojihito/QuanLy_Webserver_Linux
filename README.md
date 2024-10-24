# Quản lý web server trên CentOS Linux

## Nội dung

- **I. Hướng dẫn chuẩn bị**
  - **1. Hoàn thành DNS server**
  - **2. Cài đặt Apache server**
- **II. Cấu hình HTTP**
  - **1. Tạo folder chứa website**
  - **2. Gán quyền**
  - **3. Cài đặt Virtual host**
  - **4. Tạo file index.html**
  - **5. Bật Apache**
  - **6. Kiểm tra website**

- **III. Cấu hình HTTPS**

## I. Hướng dẫn chuẩn bị
### 1. Hoàn thành DNS server
```bash
sudo yum update
sudo yum install httpd
```
### 2. Cài đặt Apache server
