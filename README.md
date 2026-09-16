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

## Điểm Nổi Bật Trên Phiên Bản v1.5.0

- **Universal Precompiled Header C++ (`bits/stdc++.h.gch`)**: Tự động sinh và cache PCH cho mọi compiler C++ (Managed GCC, MinGW-w64, MSYS2 UCRT64, Apple Clang), tăng tốc độ biên dịch gấp ~16.5 lần (từ ~3.6s xuống chỉ còn ~0.23s).
- **Chấm Bài Thi Tương Tác (Interactive Problems & Interactor)**: Runner chấm tương tác độc quyền qua Win32 AppContainer dual-job runner với hai đường ống ẩn danh hai chiều (bidirectional anonymous pipes). Hỗ trợ interactor viết bằng C++ (`testlib.h`) hoặc Python.
- **Chấm Điểm Phân Số Chính Xác Tuyệt Đối (Rational Scoring Engine)**: Tính toán điểm qua phân số tối giản (GCD) triệt tiêu hoàn toàn sai số trôi số thực IEEE 754 trên bảng điểm và xuất Excel.
- **Giới Hạn Bộ Nhớ Lên Đến 1024 MB**: Hỗ trợ Memory Limit từ 8 MB đến 1024 MB (1 GB) trên cả Windows và macOS, mặc định 256 MB chuẩn Olympic / HSG / ICPC.
- **Chấm Thi Scratch Hoàn Thiện Tuyệt Đối**: Runner `scratch-run` (VNOI / TurboWarp VM) tích hợp sẵn, nạp test qua `ask and wait` và xuất kết quả qua `say`/`think`, cô lập an toàn trong Windows Job Objects / macOS Seatbelt.
- **Làm Ấm Sandbox Ngầm (Background Judge Pre-warm)**: Khởi động nền làm nóng môi trường thực thi và compiler ngay khi mở ứng dụng, loại bỏ độ trễ chấm lần đầu.
- **Cập Nhật 1-Chạm Siêu Tốc & Nút Ủng Hộ (Donation)**: Nút cập nhật trực tiếp trên thanh công cụ với tiến trình tải Core ngầm (~20MB) và khởi động lại một chạm; bổ sung nút ủng hộ / mời cà phê thân thiện.
- **Bộ Nhận Diện Thương Hiệu Mới**: Logo và bộ icon ứng dụng mới hiện đại, sắc nét trên cả Windows và macOS.
