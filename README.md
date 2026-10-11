# JudgeDesk

JudgeDesk là ứng dụng desktop chấm bài lập trình cục bộ phục vụ các kỳ thi và lớp học, chạy trên Windows x64, macOS Apple Silicon (ARM64) và, từ v1.5.6, Ubuntu 24.04 LTS x86_64 (Intel/AMD 64 bit). Ứng dụng hỗ trợ bảng điểm trực tiếp, cấu hình nhóm test, giao diện sáng/tối cùng hai ngôn ngữ giao diện tiếng Việt và tiếng Anh. Linux có bản Core cho C++, C và Python; cần Internet để APT tải các thư viện và compiler chưa có khi cài đặt.

Repository này cung cấp các bản cài đặt chính thức, mã kiểm tra SHA-256, manifest cập nhật, managed toolchain và tài liệu hướng dẫn sử dụng. Mã nguồn nội bộ không được phân phối tại repository này.

## Tải xuống và Cài đặt

- **[Releases](https://github.com/ducminh25/judgedesk/releases)**: Tải bộ cài đặt `.msi` (Windows), `.dmg` (macOS) hoặc Core `.deb` (Ubuntu x86_64, từ v1.5.6 khi được công bố). Chọn file thuộc đúng release và hệ điều hành.
- **[INSTALL.md](INSTALL.md)**: Hướng dẫn cài đặt, xác minh mã băm SHA-256 và luồng cập nhật Core/Full.
- **[TOOLCHAINS.md](TOOLCHAINS.md)**: Danh mục trình biên dịch/runtime (GCC 14.2, CPython 3.12, Free Pascal 3.2.2, Temurin OpenJDK 21, Scratch Headless VM) và cờ biên dịch chuẩn.
- **[CHANGELOG.md](CHANGELOG.md)**: Lịch sử thay đổi chi tiết theo từng phiên bản.
- **[SUPPORT.md](SUPPORT.md)**: Hướng dẫn báo lỗi và gửi phản hồi kỹ thuật.
- **[SECURITY.md](SECURITY.md)**: Chính sách bảo mật và báo cáo lỗ hổng an toàn.

## Bản phân phối Core và Full

Bản **Full** tích hợp sẵn toàn bộ trình biên dịch và runtime do ứng dụng quản lý (GCC, CPython, Free Pascal, Eclipse Temurin OpenJDK, Scratch Headless VM). Trên Windows, gói cài đặt chứa cả WebView2 runtime offline. Trên macOS, bản Full đi kèm Python, FPC, Java và Scratch runtime, riêng C/C++ sử dụng Apple Clang từ Xcode Command Line Tools. Đây là bản phù hợp cho phòng thi, máy giáo viên và môi trường không có kết nối mạng.

Bản **Core** chỉ gồm phần mềm chấm thi. Trên Windows/macOS, người dùng có thể tải thêm các gói compiler đã ký từ giao diện hoặc chọn compiler có sẵn trên máy sau khi ứng dụng kiểm tra độ tương thích. Linux đợt đầu dùng GCC 13 và CPython 3.12 từ Ubuntu qua các đường dẫn đã cấu hình; chưa có bản Full hoặc managed toolchain Linux.

Core/Full dùng chung định danh ứng dụng và vị trí lưu trữ dữ liệu trên từng hệ điều hành. Updater giữ cấu hình và đường dẫn compiler đã chọn; Linux tải bản Core `.deb` đã ký và hỏi quyền quản trị để cài đặt. Người dùng cũng có thể nâng cấp bằng APT với file `.deb` mới.

## Cài JudgeDesk trên Ubuntu

Phạm vi ban đầu: **Ubuntu 24.04 LTS amd64/x86_64**, máy Intel/AMD 64 bit, **GCC 13** cho C++/C và **CPython 3.12** cho Python. Sandbox đã được kiểm chứng với kernel GA 6.8 trong VM dùng cấu hình boot riêng của lab; boot mặc định, Ubuntu Desktop/Wayland, kernel HWE và máy thật cần thêm thử nghiệm. ARM64, máy 32 bit và các distro khác chưa được xác nhận. Không cần tắt cơ chế bảo mật của Ubuntu để cài app.

1. Tải `JudgeDesk_1.5.6_core_amd64.deb` và `SHA256SUMS` từ release v1.5.6 khi các file được công bố. Nếu dùng phiên bản mới hơn, thay số version trong các lệnh dưới đây. Với candidate thử nghiệm, dùng file và checksum do nhóm phát triển cung cấp.
2. Mở Terminal trong thư mục tải về, chạy `sha256sum JudgeDesk_1.5.6_core_amd64.deb` và đối chiếu với dòng tương ứng trong `SHA256SUMS`. Chỉ cài khi hash khớp.
3. Cài app và compiler/runtime:

   ```bash
   sudo apt update
   sudo apt install ./JudgeDesk_1.5.6_core_amd64.deb
   sudo apt install gcc-13 g++-13 python3.12
   ```

   APT sẽ tải dependencies cần thiết từ Ubuntu. `./` cho biết đây là file local; hiện chưa có APT repository riêng của JudgeDesk. Khi `sudo` hỏi mật khẩu, Terminal không hiện ký tự đang gõ.

4. Mở **JudgeDesk** từ menu ứng dụng bằng tài khoản bình thường. Chờ màn hình **Đang chuẩn bị bộ chấm** hoàn tất; lần đầu hoặc sau cập nhật có thể lâu hơn do tạo cache runtime. Trong cấu hình compiler/runtime, chọn **Local** và đặt C++ là `/usr/bin/g++-13`, C là `/usr/bin/gcc-13`, Python là `/usr/bin/python3.12`. Chạy kiểm tra compatibility cho cả ba; chỉ chấm khi compiler và sandbox đều sẵn sàng. Không mở app bằng `sudo`.
5. Thử tạo/mở/lưu contest, chấm bài mẫu bằng ba ngôn ngữ, kiểm tra verdict/điểm và xuất Excel. Pascal, Java, Scratch và bài tương tác chưa được hỗ trợ trên Linux. Gửi phiên bản app, `lsb_release -ds`, `uname -r`, loại desktop và testcase tái hiện khi báo lỗi.

Để nâng cấp thủ công, đóng app rồi chạy `sudo apt install ./JudgeDesk_<version>_core_amd64.deb` với file mới. Sau khi mở lại, kiểm tra version, ba đường dẫn đã lưu và compatibility. Cấu hình nằm trong `~/.local/share/ThemisV2/` hoặc `$XDG_DATA_HOME/ThemisV2/`; bộ cài/updater không sửa hoặc xóa thư mục này. Không nâng cấp compiler hệ thống trong khi đang chấm. Xem thêm [INSTALL.md](INSTALL.md).

## Ngôn ngữ được hỗ trợ

Windows/macOS hỗ trợ sáu ngôn ngữ dưới đây. Linux đợt đầu hỗ trợ C++, C và Python theo phiên bản Ubuntu nêu trên.

- **C++ (C++14):** GCC 14.2.0 (Windows) / Apple Clang (macOS) / GCC 13 (Ubuntu).
- **C (C11):** GCC 14.2.0 (Windows) / Apple Clang (macOS) / GCC 13 (Ubuntu).
- **Python:** CPython 3.12.13 (Windows/macOS) / CPython 3.12 (Ubuntu).
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
