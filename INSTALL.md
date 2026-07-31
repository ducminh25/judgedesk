# Hướng dẫn cài đặt JudgeDesk

Chỉ tải JudgeDesk từ trang [Releases chính thức](https://github.com/ducminh25/judgedesk/releases).

Nếu một release cung cấp cả hai biến thể:

- **Full**: kèm các managed toolchain được hỗ trợ để cài xong là có thể chấm.
- **Core**: bộ chấm gọn hơn; có thể tải managed toolchain sau trong JudgeDesk
  hoặc dùng compiler/runtime cục bộ phù hợp.

## Windows 10/11 x64

1. Mở release mới nhất và tải file MSI x64; giáo viên/học sinh nên chọn bản
   **Full** nếu không có nhu cầu quản trị toolchain riêng.
2. Tải thêm file checksum dành cho Windows trong cùng release.
3. Kiểm tra checksum bằng PowerShell:

   ```powershell
   Get-FileHash .\JudgeDesk_*.msi -Algorithm SHA256
   Get-Content .\SHA256SUMS-windows
   ```

4. Đối chiếu hai giá trị rồi chạy file MSI.
5. Mở JudgeDesk và kiểm tra **Quản lý Trình biên dịch & Runtimes**. Bản Full
   phải hiển thị C++, C, Python, Pascal và Java ở trạng thái sẵn sàng chấm.

MSI giữ nguyên mã nâng cấp của các bản ThemisV2/JudgeDesk 1.x trước đây, tránh
tạo hai ứng dụng song song và giữ lại dữ liệu tương thích.

Nếu Windows hiển thị cảnh báo nhà phát hành, không tiếp tục nếu file không đến
từ repo này hoặc checksum không khớp.

## macOS Apple Silicon

JudgeDesk chỉ phát hành cho máy Mac dùng Apple Silicon (dòng M).

1. Mở release mới nhất và tải file `.dmg` aarch64/Apple Silicon; chọn bản Full
   nếu release có nhiều biến thể.
2. Tải file checksum dành cho macOS, sau đó kiểm tra trong Terminal:

   ```bash
   shasum -a 256 JudgeDesk_*.dmg
   cat SHA256SUMS-macos
   ```

3. Mở DMG và kéo JudgeDesk vào thư mục Applications.
4. Mở JudgeDesk từ Applications.

Nếu Gatekeeper chặn ứng dụng, chỉ dùng **System Settings → Privacy & Security →
Open Anyway** sau khi đã xác minh checksum và nguồn tải. Không chạy các lệnh xóa
quarantine lấy từ nguồn không tin cậy.

## Compiler và runtime

Trong **Quản lý Trình biên dịch & Runtimes**, mỗi ngôn ngữ có một nguồn cố định:

- `None`: tắt ngôn ngữ đó.
- `Managed by toolchain`: dùng gói đi kèm bản Full hoặc do JudgeDesk tải về.
- `Local (path)`: dùng đường dẫn do người dùng chọn.
- `Local (auto-detect)`: dùng compiler/runtime được phát hiện nhanh trên máy.

Windows Full kèm GCC cho C/C++, CPython, Free Pascal và Temurin Java. macOS Full
kèm CPython, Free Pascal và Temurin Java; C/C++ dùng Apple Clang từ Xcode
Command Line Tools. Nguồn không khả dụng sẽ không âm thầm fallback sang nguồn
khác.

## Gỡ cài đặt

- Windows: **Settings → Apps → Installed apps → JudgeDesk → Uninstall**.
- macOS: xóa JudgeDesk khỏi Applications.

Dữ liệu toolchain được quản lý nằm trong thư mục dữ liệu ứng dụng của người dùng
và có thể gỡ ngay trong màn hình quản lý Tools trước khi gỡ app.
