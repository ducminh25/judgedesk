# JudgeDesk

JudgeDesk là ứng dụng desktop chấm bài lập trình cục bộ phục vụ các kỳ thi và lớp học, chạy trên Windows x64 và macOS Apple Silicon (ARM64). Ứng dụng hoạt động offline, hỗ trợ bảng điểm trực tiếp, cấu hình nhóm test, giao diện sáng/tối cùng hai ngôn ngữ giao diện tiếng Việt và tiếng Anh.

Repository này cung cấp các bản cài đặt chính thức, mã kiểm tra SHA-256, manifest cập nhật, managed toolchain và tài liệu hướng dẫn sử dụng. Mã nguồn nội bộ không được phân phối tại repository này.

## Tải xuống và Cài đặt

- **[Releases](https://github.com/ducminh25/judgedesk/releases)**: Tải bộ cài đặt `.msi` (Windows) hoặc `.dmg` (macOS).
- **[INSTALL.md](INSTALL.md)**: Hướng dẫn cài đặt, xác minh mã băm SHA-256 và luồng cập nhật Core/Full.
- **[TOOLCHAINS.md](TOOLCHAINS.md)**: Danh mục trình biên dịch/runtime (GCC 14.2, CPython 3.12, Free Pascal 3.2.2, Temurin OpenJDK 21, Scratch Headless VM) và cờ biên dịch chuẩn.
- **[CHANGELOG.md](CHANGELOG.md)**: Lịch sử thay đổi chi tiết theo từng phiên bản.
- **[SUPPORT.md](SUPPORT.md)**: Hướng dẫn báo lỗi và gửi phản hồi kỹ thuật.
- **[SECURITY.md](SECURITY.md)**: Chính sách bảo mật và báo cáo lỗ hổng an toàn.

## Bản phân phối Core và Full

Bản **Full** tích hợp sẵn toàn bộ trình biên dịch và runtime do ứng dụng quản lý (GCC, CPython, Free Pascal, Eclipse Temurin OpenJDK, Scratch Headless VM). Trên Windows, gói cài đặt chứa cả WebView2 runtime offline. Trên macOS, bản Full đi kèm Python, FPC, Java và Scratch runtime, riêng C/C++ sử dụng Apple Clang từ Xcode Command Line Tools. Đây là bản phù hợp cho phòng thi, máy giáo viên và môi trường không có kết nối mạng.

Bản **Core** chỉ gồm phần mềm chấm thi. Người dùng có thể tải thêm các gói compiler đã ký từ giao diện hoặc chọn compiler có sẵn trên máy sau khi ứng dụng kiểm tra độ tương thích.

Cả hai bản dùng chung định danh ứng dụng và vị trí lưu trữ dữ liệu. Khi có bản cập nhật mới, ứng dụng hỗ trợ cập nhật 1-click siêu tốc (~20MB) trong nền mà không làm mất dữ liệu hay toolchain hiện có.

## Hỗ trợ 6 ngôn ngữ thi đấu

- **C++ (C++14):** GCC 14.2.0 (Windows) / Apple Clang (macOS).
- **C (C11):** GCC 14.2.0 (Windows) / Apple Clang (macOS).
- **Python:** CPython 3.12.13.
- **Pascal:** Free Pascal 3.2.2.
- **Java:** Eclipse Temurin OpenJDK 21.0.12+8.
- **Scratch:** TurboWarp VM / scratch-run 0.1.7 (`.sb3`, `.sb2`, `.sb`).
