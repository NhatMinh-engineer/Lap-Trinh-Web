# BÀI TẬP VỀ NHÀ MÔN LẬP TRÌNH WEB

* **Họ và tên:** Nguyễn Hữu Nhật Minh  
* **Mã sinh viên:** K235480106097  
* **Trường:** Đại học Kỹ thuật Công nghiệp Thái Nguyên (TNUT)  

---

## BÀI TẬP 1: Môi trường Giả lập Linux & Hạ tầng Docker Compose

### 1. Giả lập môi trường Linux OS
* **Hệ điều hành máy chủ:** Windows 11.
* **Giải pháp giả lập Linux:** **WSL2 (Windows Subsystem for Linux)** tích hợp cùng **Docker Desktop**. Giải pháp này chạy trực tiếp trên nhân Linux của Microsoft, giúp tối ưu RAM/CPU so với việc chạy máy ảo VMware/VirtualBox.

### 2. Cài đặt các dịch vụ trên Docker Compose
Dự án sử dụng file `docker-compose.yml` để khởi chạy đồng thời 5 dịch vụ (containers):
1. **Nginx (`nginx:latest`):** Đóng vai trò Web Server và Reverse Proxy tiếp nhận các request.
2. **Node-RED (`nodered/node-red:latest`):** Môi trường lập trình kéo thả xây dựng các đường dẫn API Backend.
3. **MariaDB (`mariadb:latest`):** Cơ sở dữ liệu quan hệ lưu trữ thông tin.
4. **phpMyAdmin (`phpmyadmin:latest`):** Giao diện quản trị CSDL MariaDB (chạy tại cổng `8080`).
5. **Cloudflared (`cloudflare/cloudflared:latest`):** Dịch vụ Cloudflare Quick Tunnel mở cổng công khai website ra ngoài Internet với chứng chỉ bảo mật HTTPS.

### 3. Cấu hình Nginx 2 Website & Proxy API
Trong file `nginx/conf.d/default.conf`:
* Cấu hình 2 Virtual Hosts phân luồng 2 website (`site1` và `site2`).
* Cấu hình Reverse Proxy chuyển tiếp các request có tiền tố `/api/` về container `nodered_app:1880/api/`.

### 4. Địa chỉ truy cập Public ra Internet (Cloudflare Tunnel)
* **Trang web Demo:** `https://cloud-deals-voted-local.trycloudflare.com`
* **API Demo:** `https://cloud-deals-voted-local.trycloudflare.com/api/tacke`

---

## BÀI TẬP 2: Xây dựng API Node-RED & Gọi API bằng JavaScript (AJAX)

### 1. Thiết lập API trên Node-RED
Sử dụng các node trong luồng (Flow) Node-RED:
* **`http in`**: Lắng nghe yêu cầu `GET` tại URL `/api/tacke`.
* **`function`**: Trả về dữ liệu JSON danh sách sinh viên đúng định dạng yêu cầu:
  ```json
  {
    "ok": 1,
    "msg": "thành công",
    "dssv": [
      {"name": "Cốp", "money": 123},
      {"name": "David", "money": 456}
    ]
  }