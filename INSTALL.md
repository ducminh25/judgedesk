# Hướng dẫn cài đặt JudgeDesk

Chỉ tải JudgeDesk từ trang [Releases chính thức](https://github.com/ducminh25/judgedesk/releases).

## Windows 10/11 x64

1. Mở release mới nhất và chọn một file:
   - `JudgeDesk_..._full_x64_en-US.msi` (khuyến nghị cho phòng thi/lớp học,
     có sẵn WebView2 và các managed toolchain đã duyệt).
   - `JudgeDesk_..._core_x64_en-US.msi` (nhỏ hơn, có thể tải toolchain sau).
2. Tải thêm `SHA256SUMS` trong cùng release.
3. Kiểm tra checksum bằng PowerShell:

   ```powershell
   Get-FileHash .\JudgeDesk_*.msi -Algorithm SHA256
   Get-Content .\SHA256SUMS
   ```

4. Đối chiếu hai giá trị rồi chạy file MSI.
5. Mở JudgeDesk và chọn cách cấu hình compiler/runtime ở màn hình đầu tiên.

MSI v1.2.0 trở lên giữ nguyên mã nâng cấp của ThemisV2. Nếu máy đã cài ThemisV2
1.1.3/1.1.4 hoặc các bản JudgeDesk cũ, bộ cài sẽ nâng cấp ứng dụng hiện có thay
vì tạo một bản cài song song. Dữ liệu và toolchain hiện có được giữ nguyên.

Nếu Windows hiển thị cảnh báo nhà phát hành, không tiếp tục nếu file không đến
từ repo này hoặc checksum không khớp.

## Cập nhật trong JudgeDesk từ v1.3.4

Khi tìm thấy phiên bản mới, bấm nút Updater rồi chọn **Core** hoặc **Full**.
JudgeDesk đánh dấu edition đang dùng và cho biết dung lượng tải/cài đặt:

- Chọn đúng edition hiện tại để cập nhật bình thường; không có thêm cảnh báo.
- Chọn edition khác để xem trước thay đổi toolchain rồi xác nhận hoặc hủy trước
  khi tải.
- Đóng JudgeDesk sau khi tải nhưng trước khi cài sẽ bỏ gói đã tải. Lần sau có
  thể tải lại; dữ liệu kỳ thi và cấu hình compiler không bị xóa.
- Updater luôn dùng release mới nhất. Không cần cài lần lượt các phiên bản đã bỏ
  qua.

Các bản trước v1.3.4 chưa hiểu hai kênh Core/Full. Hãy cài v1.3.4 thủ công một
lần từ trang Releases; từ đó các lần cập nhật tiếp theo dùng luồng trên.

## macOS Apple Silicon

JudgeDesk hỗ trợ macOS 11 trở lên trên máy Apple Silicon (dòng M).

1. Mở release mới nhất và tải `JudgeDesk_<version>_full_aarch64.dmg` (khuyến
   nghị, có sẵn các runtime managed) hoặc
   `JudgeDesk_<version>_core_aarch64.dmg` (nhỏ hơn, tải runtime sau).
2. Tải `SHA256SUMS`, sau đó kiểm tra trong Terminal:

   ```bash
   shasum -a 256 JudgeDesk_*.dmg
   cat SHA256SUMS
   ```

3. Mở DMG và kéo JudgeDesk vào thư mục Applications.
4. Mở JudgeDesk từ Applications.

Nếu Gatekeeper chặn ứng dụng, chỉ dùng **System Settings → Privacy & Security →
Open Anyway** sau khi đã xác minh checksum và nguồn tải. Không chạy các lệnh xóa
quarantine lấy từ nguồn không tin cậy.

## Compiler và runtime

JudgeDesk v1.3 hỗ trợ C++, C, Python, Pascal và Java. Trong **Quản lý Trình biên
dịch & Runtimes**, mỗi ngôn ngữ có một nguồn cố định:

- `None`: tắt ngôn ngữ đó.
- `Managed by toolchain`: dùng gói đi kèm bản Full hoặc do JudgeDesk tải về.
- `Local (path)`: dùng đường dẫn do người dùng chọn.
- `Local (auto-detect)`: dùng compiler/runtime được phát hiện nhanh trên máy.

Windows Full kèm GCC cho C/C++, CPython, Free Pascal và Temurin Java. macOS Full
kèm CPython, Free Pascal và Temurin Java; C/C++ dùng Apple Clang từ Xcode
Command Line Tools. Nguồn không khả dụng sẽ không âm thầm fallback sang nguồn
khác.

Chế độ local ít cô lập hơn chỉ hoạt động khi người dùng chủ động bật trong
Advanced và không nên là lựa chọn mặc định cho máy thi.

## Gỡ cài đặt

- Windows: **Settings → Apps → Installed apps → JudgeDesk → Uninstall**.
- macOS: xóa JudgeDesk khỏi Applications.

Dữ liệu toolchain được quản lý nằm trong thư mục dữ liệu ứng dụng của người dùng
và có thể gỡ ngay trong màn hình quản lý Tools trước khi gỡ app.
