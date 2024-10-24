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
**Để có thể thực hiện quản lý web server, trước tiên ta cần có DNS hoàn chỉnh**
![image](https://github.com/user-attachments/assets/a2168992-c737-446a-85dd-4559550da8fa)

**Để có trang 2 trang web mang tên miền là sgu.edu.vn và vnlab.net, DNS của 2 domain này đều phải trỏ về IP của máy web server là 192.168.1.1**


### 2. Cài đặt Apache server

**Cài đặt Apache**:
```yum install httpd```


