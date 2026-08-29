# Cài đặt JudgeDesk

Tải các bản phát hành chính thức tại [GitHub Releases](https://github.com/ducminh25/judgedesk/releases).

## Windows 10/11 x64

1. Tải tệp cài đặt phù hợp từ trang release:
   - `JudgeDesk_<version>_full_x64_en-US.msi`: Khuyến nghị cho phòng máy và lớp học. Tích hợp sẵn WebView2 Runtime và toàn bộ compiler/runtime quản lý.
   - `JudgeDesk_<version>_core_x64_en-US.msi`: Gói rút gọn, cho phép tải toolchain sau qua giao diện.
2. Tải tệp `SHA256SUMS` trong cùng mục release.
3. Mở PowerShell trong thư mục chứa file và kiểm tra mã băm:

```powershell
Get-FileHash .\JudgeDesk_*.msi -Algorithm SHA256
Get-Content .\SHA256SUMS
```

4. Chạy tệp `.msi` khi hai chuỗi SHA-256 khớp nhau.
5. Mở ứng dụng và kiểm tra trạng thái compiler ở màn hình ban đầu.

Gói cài đặt tự động nâng cấp nếu trên máy đã có phiên bản JudgeDesk trước đó, giữ nguyên dữ liệu kỳ thi và cấu hình toolchain hiện tại.

## macOS Apple Silicon (M1/M2/M3/M4)

JudgeDesk hỗ trợ macOS 11 trở lên trên kiến trúc Apple Silicon (ARM64).

1. Tải bản cài đặt từ release:
   - `JudgeDesk_<version>_full_aarch64.dmg`: Khuyến nghị, có sẵn runtime Python, Pascal và Java.
   - `JudgeDesk_<version>_core_aarch64.dmg`: Gói rút gọn.
2. Tải `SHA256SUMS` và xác minh qua Terminal:

```bash
shasum -a 256 JudgeDesk_*.dmg
cat SHA256SUMS
```

3. Mở tệp `.dmg` và kéo biểu tượng JudgeDesk vào thư mục Applications.
4. Mở ứng dụng từ Launchpad hoặc Applications.

Trường hợp Gatekeeper hiển thị cảnh báo ứng dụng chưa xác minh danh tính lập trình viên, vào **System Settings → Privacy & Security**, tìm thông báo về JudgeDesk và chọn **Open Anyway**.

## Cập nhật phiên bản và chuyển đổi giữa Core / Full

Ứng dụng kiểm tra phiên bản mới tự động qua nút **Cập nhật (Updater)** trên thanh công cụ:
- **Tự động cập nhật qua Updater (In-App)**: Luôn tải gói cập nhật ứng dụng cốt lõi siêu nhẹ (~20MB), giữ nguyên toàn bộ toolchain đã có sẵn mà không phải tải lại 800MB compiler qua mạng.
- **Chuyển từ Core sang Full**: Có 2 cách thuận tiện:
  1. *Cách 1 (In-App)*: Vào tab **Quản lý Toolchain (Toolchains)** và bấm **Tải về** các ngôn ngữ mong muốn (lưu tại `%LOCALAPPDATA%\ThemisV2\toolchains`).
  2. *Cách 2 (Cài đặt đè)*: Tải và chạy file `JudgeDesk_<version>_full_*.msi` mới nhất từ GitHub Releases để cài đặt trọn bộ compiler vào thư mục ứng dụng (tự động nhận diện và dọn sạch các bản sao trùng lặp trong AppData nếu có).

## Quản lý trình biên dịch và lưu trữ

Hệ thống hỗ trợ 6 ngôn ngữ: C++, C, Python, Pascal, Java và Scratch (.sb3, .sb2, .sb):
- **Bản Full (Đóng gói sẵn)**: Chứa sẵn bộ compiler/runtime trong thư mục cài đặt ứng dụng (`resources/toolchains`), dùng trực tiếp (Zero-Copy), khởi động tức thì và dùng chung an toàn cho mọi tài khoản trên máy.
- **Bản Core (Tải thêm theo nhu cầu)**: Các gói tải về từ kho lưu trữ chính thức được lưu tại `%LOCALAPPDATA%\ThemisV2\toolchains` (Windows) hoặc `~/Library/Application Support/ThemisV2/toolchains` (macOS).
- **Trình biên dịch trên máy (Local)**: Ứng dụng tự động phát hiện compiler có sẵn trong `PATH` hoặc qua đường dẫn chỉ định thủ công nếu vượt qua kiểm tra an toàn.

## Gỡ cài đặt

- **Windows:** Vào **Settings → Apps → Installed apps**, tìm **JudgeDesk** và chọn **Uninstall**.
- **macOS:** Kéo ứng dụng JudgeDesk từ thư mục Applications vào Trash.

Dữ liệu toolchain và cấu hình người dùng được lưu trong thư mục `%LOCALAPPDATA%\ThemisV2` (Windows) hoặc `~/Library/Application Support/ThemisV2` (macOS). Có thể xóa trực tiếp trong mục Quản lý Toolchain trước khi gỡ ứng dụng.
