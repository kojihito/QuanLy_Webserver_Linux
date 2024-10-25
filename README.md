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
  - **1. Cài đặt module ssl**
  - **2. Tạo thư mục lưu trữ website**
  - **3. Tạo chứng chỉ SSL tự ký**
  - **4. Cấu hình Virtual host cho HTTPS**
  - **5. Kiểm tra**

## I. Hướng dẫn chuẩn bị
### 1. Hoàn thành DNS server
- Để có thể thực hiện quản lý web server, trước tiên ta cần có DNS hoàn chỉnh

![image](https://github.com/user-attachments/assets/3b76edde-7958-4251-985f-7a3683c9a50e)


- Để có trang 2 trang web mang tên miền là sgu.edu.vn và vnlab.net, DNS của 2 domain này đều phải trỏ về IP của máy web server là 192.168.1.1


### 2. Cài đặt Apache server

- Để cài đặt Apache, cần đảm bảo máy có kết nối mạng:
```yum install httpd```

## II. Cấu hình HTTP
### 1. Tạo thư mục chứa website
- Tạo 2 thư mục là ```vnlab.net``` và ```sgu.edu.vn``` ở folder var/www/html
```
mkdir -p /var/www/html/vnlab.net
mkdir -p /var/www/html/sgu.edu.vn
```
### 2. Gán quyền
- Cấp quyền cho trang web hoạt động
```
chown -R apache:apache /var/www/html/vnlab.net
chown -R apache:apache /var/www/html/sgu.edu.vn
```
- Cấp quyền chmod 755 để xem được website
```
chmod 755 /var/www/
chmod 755 /var/www/html
```

### 3. Cài đặt virtual host
- Cài đặt virtual host để cấu hình từng trang web với tên miền đã tạo
- Vào đường dẫn ```/etc/httpd/conf/httpd.conf```
- Thêm đoạn code bên dưới vào cuối file
```
# Supplemental configuration 
# 
# Load config files in the "/etc/httpd/conf.d" directory, if any. 
IncludeOptional conf.d/*.conf 
NameVirtualHost *:80
```

- Tạo các virtual host cho websites tương ứng. Lưu ý rằng, các file virtual host phải có dạng *.conf. Ngoài ra, tại mỗi website ta nên tạo 1 virtual host cho dễ quản lý.
- Cho tên miền ```vnlab.net``` có thể tạo file ```/etc/httpd/conf.d/vnlab.net.conf```

```
<VirtualHost *:80> 
    ServerAdmin webmaster@vnlab.net    
     DocumentRoot /var/www/html/vnlab.net     #đây là đường dẫn đến thư mục chứa nội dung website 
     ServerName vnlab.net    # đây là domain của website 
     ServerAlias vnlab.net   # đây là Alias của website 
</VirtualHost>
```
- Tương tự cho ```sgu.edu.vn``` tạo file ```/etc/httpd/conf.d/sgu.edu.vn.conf```
```
<VirtualHost *:80> 
    ServerAdmin webmaster@sgu.edu.vn 
     DocumentRoot /var/www/html/sgu.edu.vn     #đây là đường dẫn đến thư mục chứa nội dung website 
     ServerName sgu.edu.vn    # đây là domain của website 
     ServerAlias sgu.edu.vn   # đây là Alias của website 
</VirtualHost> 
```

### 4. Tạo file index.html
- Vào thư mục chứa các website tạo file ```index.html```
- Đối với ```vnlab.net```
```
cd /var/www/html/vnlab.net
gedit index.html
```
- Làm tương tự với ```sgu.edu.vn```
```
cd /var/www/html/sgu.edu.vn
gedit index.html
```
- Nhập nội dung bất kỳ cho file index.html và save
![image](https://github.com/user-attachments/assets/045b6b46-6cf8-476a-b5a1-0563e7b52410)

- Khi hoàn thành, gán quyền cho file ```index.html``` với câu lệnh
```
chmod +x /var/www/html/vnlab.net/index.html
chmod +x /var/www/html/sgu.edu.vn/index.html
```

### 5. Khởi động apache
- Sau đó, khởi động Apache
```
systemctl enable httpd 
systemctl start httpd
systemctl restart httpd
```

### 6. Kiểm tra website
- Cuối cùng, xem website từ một máy client hoặc xem trực tiếp từ trình duyệt web Firefox có sẵn trên CentOS

![image](https://github.com/user-attachments/assets/4b8dc4d7-ab47-4a65-aa29-e3530158370d)
![image](https://github.com/user-attachments/assets/2e0caf34-9345-44d4-8597-8ebd9899dcec)

## III. Cấu hình HTTPS
**Để đảm bảo, nên cấu hình HTTPS trên một máy ảo khác, đồng thời cài đặt và khởi động lại Apache**
### 1. Cài đặt module ssl
- Mở cổng 80 và 443 trên tường lửa
```
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```
- Cài đặt ```mod_ssl```
```
sudo yum install mod_ssl -y
```

### 2. Tạo thư mục lưu trữ website
- Tạo thư mục DocumentRoot cho website ```sgu.edu.vn```
```
sudo mkdir -p /var/www/sgu.edu.vn
```
- Cấp quyền cho thư mục
```
sudo chown -R apache:apache /var/www/sgu.edu.vn
```
### 3. Tạo chứng chỉ SSL tự ký
- Tạo một chứng chỉ tự ký:
```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/pki/tls/private/sgu.key -out /etc/pki/tls/certs/sgu.crt
```
- Sau đó tự điền các thông tin theo yêu cầu

### 4. Cấu hình Virtual Host cho HTTPS
- Tạo file cấu hình cho website tại ```/etc/httpd/conf.d/sgu.edu.vn.conf```
```
<VirtualHost *:443>
    ServerName sgu.edu.vn
    DocumentRoot /var/www/sgu.edu.vn

    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/sgu.crt
    SSLCertificateKeyFile /etc/pki/tls/private/sgu.key

    <Directory /var/www/sgu.edu.vn>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog /var/log/httpd/sgu.edu.vn-error.log
    CustomLog /var/log/httpd/sgu.edu.vn-access.log combined
</VirtualHost>

## Chuyển hướng http sang https
<VirtualHost *:80>
    ServerName sgu.edu.vn
    Redirect permanent / https://sgu.edu.vn/
</VirtualHost>
```

### 5. Kiểm tra
- Nhập ```sudo apachectl configtest``` nếu hệ thống trả về ```Syntax OK``` thì tiếp tục, nếu không, kiểm tra lại các file cấu hình
- Sau đó, khởi động lại Apache:
```
sudo systemctl restart httpd
```

- Cuối cùng, mở trang web từ trình duyệt firefox có sẵn hoặc từ máy client, nếu ra trang web như bên dưới là đã hoàn thành

![2024-10-25_160435](https://github.com/user-attachments/assets/9ccc80e1-c150-4287-8c77-67742ea17b12)




