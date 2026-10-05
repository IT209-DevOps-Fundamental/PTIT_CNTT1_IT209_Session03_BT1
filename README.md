# Bài 1: Thay đổi cổng kết nối SSH (SSH Port Hardening)

## Mục tiêu
Thay đổi cổng dịch vụ SSH mặc định từ 22 sang 2222 trên máy chủ ảo nhằm hạn chế tối đa các cuộc quét cổng tự động của botnet trên mạng internet.
Thực hiện cấu hình tường lửa UFW trước khi khởi động lại dịch vụ để tránh nguy cơ bị khóa quyền truy cập (Lockout).

## Các bước thực hiện

**Lưu ý:** Chỉ thực hiện thao tác bằng tài khoản người dùng thường `devops` có quyền `sudo`, không sử dụng tài khoản `root` trực tiếp.

### Bước 1: Thay đổi cổng kết nối SSH
Sử dụng trình soạn thảo (ví dụ `nano`) để chỉnh sửa tệp tin cấu hình SSH:
```bash
sudo nano /etc/ssh/sshd_config
```
Tìm dòng `#Port 22` hoặc `Port 22`, bỏ dấu `#` (nếu có) và sửa lại thành:
```
Port 2222
```
Lưu và thoát khỏi trình soạn thảo (trong nano: `Ctrl+O` -> `Enter` để lưu, `Ctrl+X` để thoát).

### Bước 2: Cho phép cổng mới qua tường lửa UFW
Sử dụng tường lửa UFW trong hệ điều hành để mở cổng `2222/tcp` trước khi khởi động lại dịch vụ SSH daemon:
```bash
sudo ufw allow 2222/tcp
```

### Bước 3: Khởi động lại dịch vụ SSH
Khởi động lại dịch vụ để áp dụng các thay đổi cấu hình:
```bash
sudo systemctl restart ssh
```
*(hoặc `sudo systemctl restart sshd` tùy thuộc vào hệ thống)*

### Bước 4: Kiểm tra kết nối
**Lưu ý quan trọng:** Không tắt cửa sổ Terminal hiện tại đang cấu hình (để đề phòng lỗi cấu hình có thể khôi phục lại). Mở một cửa sổ Terminal mới trên máy cá nhân và chạy các lệnh kiểm tra sau:

1. **Kiểm tra kết nối qua cổng mới (Kết quả mong đợi: Kết nối thành công bằng SSH Key)**
   ```bash
   ssh -p 2222 devops@<IP_ADDRESS_DROPLET>
   ```

2. **Kiểm tra kết nối qua cổng mặc định cũ (Kết quả mong đợi: Kết nối bị chặn hoàn toàn, báo lỗi Connection refused hoặc Timeout)**
   ```bash
   ssh -p 22 devops@<IP_ADDRESS_DROPLET>
   ```

---

## Báo cáo thực hành (Học viên điền vào đây)

### Log Terminal chứng minh

*(Học viên dán các đoạn log terminal chứng minh việc thực hiện các thao tác và kết quả kiểm tra vào phần bên dưới)*

```bash
# 1. Log cấu hình ufw và khởi động lại dịch vụ ssh:


# 2. Log kiểm tra kết nối thành công qua cổng 2222:


# 3. Log kiểm tra kết nối thất bại qua cổng 22:


```
