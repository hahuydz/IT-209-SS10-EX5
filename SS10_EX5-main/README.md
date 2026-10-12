# Bài 5: Tự động hóa điều phối đa dịch vụ với Docker Compose & File .env

## 1. Mục tiêu
- Đóng gói bài tập EX4 thành tệp khai báo `docker-compose.yml` theo chuẩn Infrastructure as Code (IaC).
- Quản lý hạ tầng đa container bằng **Docker Compose CLI v2** (`docker compose`).
- Bảo mật thông tin cấu hình nhạy cảm (DB name, credentials, port) bằng file `.env` và cung cấp `.env.example`.

---

## 2. Cấu trúc thư mục
```text
homework/session_10/ex5/
├── .env.example        # File mẫu cấu hình biến môi trường
├── .env                # File biến môi trường thực tế (bị ignore bởi git)
├── .gitignore          # Cấu hình bỏ qua file .env
├── Dockerfile          # Dockerfile tối giản cho backend service
├── docker-compose.yml  # File điều phối hạ tầng đa dịch vụ (IaC)
└── README.md           # Hướng dẫn chi tiết
```

---

## 3. Hướng dẫn thiết lập & thực thi

### Bước 1: Khởi tạo file môi trường
```bash
cp .env.example .env
```
*(Chỉnh sửa các tham số `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `PORT` trong file `.env` nếu cần).*

### Bước 2: Khởi chạy cụm dịch vụ
```bash
docker compose up -d
```
> **Kết quả mong đợi:** Docker Compose tự động tạo Named Volume (`pgdata`), Bridge Network (`backend-net`), build image backend và khởi động cả 2 container `db-server` và `backend-app` ở chế độ detached (`-d`).

---

## 4. Các lệnh kiểm tra (Docker Compose CLI v2)

### 4.1. Kiểm tra trạng thái các container
```bash
docker compose ps
```
**Kết quả mong đợi:** Cả hai services `database` và `backend` đều ở trạng thái `running` / `Up`.

### 4.2. Xem logs hoạt động
```bash
docker compose logs -f
```
**Kết quả mong đợi:**
- Service `database` hiển thị log PostgreSQL sẵn sàng nhận kết nối (`database system is ready to accept connections`).
- Service `backend` hiển thị thông báo khởi chạy thành công.

### 4.3. Dừng và dọn dẹp hạ tầng
```bash
# Dừng và xóa container, network
docker compose down

# (Tùy chọn) Xóa cả volume dữ liệu nếu muốn làm mới hoàn toàn
docker compose down -v
```
