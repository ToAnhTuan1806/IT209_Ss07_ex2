# Báo cáo Bài 2: Quản trị Tường lửa UFW cho Cụm Dịch vụ Multi-port

## 1. Mục tiêu & Yêu cầu
- Thiết lập tường lửa UFW chặn mặc định tất cả lưu lượng đi vào (`deny incoming`).
- Mở các cổng dịch vụ cần thiết:
  - **SSH (22/tcp)**: Quản trị từ xa.
  - **HTTP (80/tcp)**: Web server Nginx.
  - **Spring Boot (8082/tcp)**: Ứng dụng kiểm tra từ xa.
- Cô lập cơ sở dữ liệu **MySQL (3306/tcp)**: Không thêm rule `allow`, tự động bị chặn bởi chính sách mặc định.

---

## 2. Các bước thực hiện

```bash
# 1. Đặt chính sách mặc định (Chặn vào, cho phép ra)
sudo ufw default deny incoming
sudo ufw default allow outgoing

# 2. Cho phép các cổng dịch vụ cần thiết
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 8082/tcp

# 3. Kích hoạt tường lửa UFW
sudo ufw enable
```

---

## 3. Kết quả kiểm tra (`sudo ufw status verbose`)

Chạy lệnh kiểm tra:
```bash
sudo ufw status verbose
```

**Đầu ra (Output):**
```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere                  
80/tcp                     ALLOW IN    Anywhere                  
8082/tcp                   ALLOW IN    Anywhere                  
22/tcp (v6)                ALLOW IN    Anywhere (v6)             
80/tcp (v6)                ALLOW IN    Anywhere (v6)             
8082/tcp (v6)              ALLOW IN    Anywhere (v6)             
```

---

## 4. Đánh giá kết quả
- **Trạng thái:** UFW đang hoạt động (`Status: active`).
- **Chính sách:** Toàn bộ kết nối đến đều bị chặn mặc định (`deny incoming`).
- **Cổng được phép:** `22/tcp`, `80/tcp` và `8082/tcp` đều ở trạng thái `ALLOW IN` từ bất kỳ đâu (`Anywhere`).
- **Cổng 3306 (MySQL):** Không cấu hình mở cổng, đã được cô lập hoàn toàn trước Internet theo đúng chính sách chặn mặc định.
