# Hướng Dẫn Cài Đặt MinIO Trên Ubuntu

MinIO là một hệ thống lưu trữ object tương thích với S3, phù hợp với các giải pháp lưu trữ hiện đại.

---

## Bước 1: Tải và Cài Đặt MinIO

```bash
# Tải MinIO bản phát hành ngày 2025-02-28
wget https://dl.min.io/server/minio/release/linux-amd64/archive/minio.RELEASE.2025-02-28T09-55-16Z

# Đổi tên dễ dùng
mv minio.RELEASE.2025-02-28T09-55-16Z minio

# Cấp quyền thực thi
chmod +x minio

# Di chuyển vào thư mục thực thi
sudo mv minio /usr/local/bin/
```

## Bước 2: Tạo Thư Mục Dữ Liệu và Người Dùng MinIO

# Tạo thư mục dữ liệu
mkdir -p ~/minio/data

# Tạo user hệ thống (không login được)
sudo useradd -r minioadmin -s /sbin/nologin

## Bước 3: Thiết Lập Biến Môi Trường
Thêm các dòng sau vào cuối file ~/.bashrc:

export MINIO_ROOT_USER="minioadmin"
export MINIO_ROOT_PASSWORD="minioadmin"
Sau đó chạy:

source ~/.bashrc


## Bước 4: Chạy Thử MinIO Thủ Công
minio server ~/minio/data --console-address ":9001"
Mở trình duyệt và truy cập: http://localhost:9001

## Bước 5: Cài Đặt MinIO Chạy Dưới Dạng Dịch Vụ Systemd
### 5.1. Tạo file service
sudo nano /etc/systemd/system/minio.service
Nội dung file:
``` [Unit]
Description=MinIO Storage Server
After=network.target

[Service]
User=ubuntu22
Group=ubuntu22
ExecStart=/usr/local/bin/minio server /home/ubuntu22/minio/data --console-address ":9001"
Restart=always
EnvironmentFile=-/etc/default/minio

[Install]
WantedBy=multi-user.target
⚠️ Đảm bảo bạn thay ubuntu22 bằng tên người dùng hiện tại nếu cần.
```
### 5.2. Kích hoạt dịch vụ

# Tải lại cấu hình systemd
sudo systemctl daemon-reload

# Bật dịch vụ và khởi chạy ngay
sudo systemctl enable --now minio

# Kiểm tra trạng thái
systemctl status minio
Bước 6: Kiểm Tra Truy Cập Giao Diện Quản Lý
Truy cập trình duyệt với địa chỉ:

cpp
Copy
Edit
http://<IP-của-bạn>:9001
