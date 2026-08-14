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

## macOS Apple Silicon

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

## Cập nhật phiên bản từ v1.3.4

Ứng dụng kiểm tra phiên bản mới tự động qua nút Updater trên thanh công cụ.

Khi có bản mới, ứng dụng hiển thị bảng so sánh giữa hai bản Core và Full:
- Bản đang chạy được gắn nhãn nhận diện kèm dung lượng tải về cụ thể.
- Cập nhật cùng phân phối (Core lên Core, Full lên Full) sẽ tải và cài đặt ngay, giữ nguyên các tùy chỉnh đường dẫn compiler.
- Chuyển đổi giữa Core và Full hiển thị thông báo xác nhận thay đổi cấu hình toolchain trước khi tải.

## Quản lý trình biên dịch và runtime

Hệ thống hỗ trợ C++, C, Python, Pascal và Java. Mỗi ngôn ngữ được thiết lập nguồn thực thi độc lập:

- **Gói quản lý (Managed):** Tích hợp sẵn trong bản Full hoặc tải về từ kho lưu trữ chính thức đối với bản Core. Các gói chạy trong sandbox cách ly cao.
- **Trình biên dịch trên máy (Local):** Ứng dụng tự động phát hiện compiler có sẵn trong `PATH` hoặc qua đường dẫn chỉ định thủ công. Nguồn local chỉ được kích hoạt khi vượt qua bài kiểm tra tương thích.

## Gỡ cài đặt

- **Windows:** Vào **Settings → Apps → Installed apps**, tìm **JudgeDesk** và chọn **Uninstall**.
- **macOS:** Kéo ứng dụng JudgeDesk từ thư mục Applications vào Trash.

Dữ liệu toolchain và cấu hình người dùng được lưu trong thư mục `%LOCALAPPDATA%\ThemisV2` (Windows) hoặc `~/Library/Application Support/ThemisV2` (macOS). Có thể xóa trực tiếp trong mục Quản lý Toolchain trước khi gỡ ứng dụng.
