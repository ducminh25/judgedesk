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

## Ubuntu 24.04 LTS x86_64 — Core, từ v1.5.6

Bản Linux đầu tiên dành cho máy Intel/AMD 64 bit (amd64/x86_64), dùng GCC 13 cho C++/C và CPython 3.12 cho Python. Sandbox đã được kiểm chứng với kernel GA 6.8 trong VM dùng cấu hình boot riêng của lab; boot mặc định, Desktop/Wayland, HWE và máy thật cần thêm thử nghiệm. ARM64, 32 bit và các distro khác chưa được xác nhận. Linux chưa có Full, managed toolchain, Pascal, Java, Scratch hoặc bài tương tác.

1. Tải `JudgeDesk_<version>_core_amd64.deb` và `SHA256SUMS` từ cùng release (bản đầu tiên là v1.5.6, khi được công bố). Đối chiếu kết quả `sha256sum JudgeDesk_<version>_core_amd64.deb` với dòng tương ứng trong `SHA256SUMS`.
2. Mở Terminal trong thư mục chứa file, thay `<version>` bằng số thực tế:

   ```bash
   sudo apt update
   sudo apt install ./JudgeDesk_<version>_core_amd64.deb
   sudo apt install gcc-13 g++-13 python3.12
   ```

   Cần Internet khi APT tải dependencies chưa có. Đây là APT cài file local; chưa có APT repository riêng của JudgeDesk.

3. Mở JudgeDesk từ menu ứng dụng bằng tài khoản bình thường. Chờ **Đang chuẩn bị bộ chấm** hoàn tất; lần đầu hoặc sau cập nhật app cần xác minh và tạo cache runtime. Nếu app báo không thể khởi động bộ chấm, lưu log và báo lỗi. Không dùng `sudo` để chạy app và không tắt cơ chế bảo mật của Ubuntu.
4. Trong cấu hình compiler/runtime, chọn Local: C++ `/usr/bin/g++-13`, C `/usr/bin/gcc-13`, Python `/usr/bin/python3.12`. Chạy compatibility cho từng lựa chọn và kiểm tra sandbox sẵn sàng trước khi chấm. Cấu hình không tương thích phải được sửa; app không tự đổi compiler.
5. Thử contest mẫu C++, C và Python, lưu/mở lại contest, kiểm tra verdict/điểm và xuất Excel.

Updater Linux tải Core `.deb` đã ký, hỏi quyền quản trị và giữ các đường dẫn đã lưu. Có thể nâng cấp thủ công bằng cách đóng app rồi chạy `sudo apt install ./JudgeDesk_<version>_core_amd64.deb` với file mới. Sau update, kiểm tra version, compatibility và contest. Cấu hình được giữ trong `~/.local/share/ThemisV2/` (hoặc `$XDG_DATA_HOME/ThemisV2/`). Đóng app trước khi nâng cấp compiler hệ thống bằng APT.

Nếu APT/dpkg báo lỗi, lưu thông báo và liên hệ nhóm phát triển; không xóa file lock để ép cài. Khi báo lỗi, gửi version app, `lsb_release -ds`, `uname -r`, loại desktop, version/đường dẫn compiler và testcase tái hiện đã loại dữ liệu riêng tư.

## Cập nhật phiên bản và chuyển đổi giữa Core / Full (Windows/macOS)

Ứng dụng kiểm tra phiên bản mới tự động qua nút **Cập nhật (Updater)** trên thanh công cụ:
- **Tự động cập nhật qua Updater (In-App)**: Luôn tải gói cập nhật ứng dụng cốt lõi siêu nhẹ (~20MB), giữ nguyên toàn bộ toolchain đã có sẵn mà không phải tải lại 800MB compiler qua mạng.
- **Chuyển từ Core sang Full**: Có 2 cách thuận tiện:
  1. *Cách 1 (In-App)*: Vào tab **Quản lý Toolchain (Toolchains)** và bấm **Tải về** các ngôn ngữ mong muốn (lưu tại `%LOCALAPPDATA%\ThemisV2\toolchains`).
  2. *Cách 2 (Cài đặt đè)*: Tải và chạy file `JudgeDesk_<version>_full_*.msi` mới nhất từ GitHub Releases để cài đặt trọn bộ compiler vào thư mục ứng dụng (tự động nhận diện và dọn sạch các bản sao trùng lặp trong AppData nếu có).

## Quản lý trình biên dịch và lưu trữ

Windows/macOS hỗ trợ 6 ngôn ngữ: C++, C, Python, Pascal, Java và Scratch (.sb3, .sb2, .sb). Linux dùng compiler/runtime hệ thống cho C++/C/Python theo mục Ubuntu ở trên:
- **Bản Full (Đóng gói sẵn)**: Chứa sẵn bộ compiler/runtime trong thư mục cài đặt ứng dụng (`resources/toolchains`), dùng trực tiếp (Zero-Copy), khởi động tức thì và dùng chung an toàn cho mọi tài khoản trên máy.
- **Bản Core (Tải thêm theo nhu cầu)**: Các gói tải về từ kho lưu trữ chính thức được lưu tại `%LOCALAPPDATA%\ThemisV2\toolchains` (Windows) hoặc `~/Library/Application Support/ThemisV2/toolchains` (macOS).
- **Trình biên dịch trên máy (Local)**: Ứng dụng tự động phát hiện compiler có sẵn trong `PATH` hoặc qua đường dẫn chỉ định thủ công nếu vượt qua kiểm tra an toàn.

## Gỡ cài đặt

- **Windows:** Vào **Settings → Apps → Installed apps**, tìm **JudgeDesk** và chọn **Uninstall**.
- **macOS:** Kéo ứng dụng JudgeDesk từ thư mục Applications vào Trash.
- **Ubuntu:** Đóng JudgeDesk rồi chạy `sudo apt remove judgedesk`. Contest và cấu hình cá nhân được giữ lại.

Dữ liệu toolchain và cấu hình người dùng được lưu trong thư mục `%LOCALAPPDATA%\ThemisV2` (Windows) hoặc `~/Library/Application Support/ThemisV2` (macOS). Có thể xóa trực tiếp trong mục Quản lý Toolchain trước khi gỡ ứng dụng.
