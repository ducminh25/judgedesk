# Quản lý Toolchain trong JudgeDesk

JudgeDesk có hai hình thức phân phối toolchain:

Gói **Full** chứa sẵn toàn bộ trình biên dịch và runtime cần thiết cho hệ điều hành, sẵn sàng chấm offline ngay sau khi cài đặt.

Gói **Core** chỉ gồm nhân chấm thi, cho phép tải các gói compiler đã ký từ GitHub Release hoặc cấu hình compiler có sẵn trên máy.

## Danh mục compiler và runtime

| Ngôn ngữ | Đuôi tệp | Windows x64 | macOS Apple Silicon | Linux x86_64 (`amd64` — Ubuntu 22.04 LTS+) |
| :--- | :--- | :--- | :--- | :--- |
| **C++** | `.cpp` | GCC 14.2.0 (Managed) / Local | Apple Clang (Hệ thống / Xcode Tools) | xPack GCC 14.2.0-2 (Managed — Bản Full) / GCC (`g++`) hoặc Clang (`clang++`) hệ thống |
| **C** | `.c` | GCC 14.2.0 (Managed) / Local | Apple Clang (Hệ thống / Xcode Tools) | xPack GCC 14.2.0-2 (Managed — Bản Full) / GCC (`gcc`) hoặc Clang (`clang`) hệ thống |
| **Python** | `.py` | CPython 3.12.13 (Managed) / Local | CPython 3.12.13 (Managed) / Local | CPython 3.12.13 (Managed — Bản Full) / CPython 3.x (`python3`) hệ thống |
| **Pascal / FPC** | `.pas`, `.pp` | Free Pascal 3.2.2 (Managed) / Local | Free Pascal 3.2.2 (Managed) / Local | Free Pascal 3.2.2 (Managed — Bản Full) / Free Pascal (`fpc` / `ppcx64`) hệ thống |
| **Java** | `.java` | Eclipse Temurin OpenJDK 21.0.12+8 (Managed) / Local | Eclipse Temurin OpenJDK 21.0.12+8 (Managed) / Local | Eclipse Temurin OpenJDK 21.0.12+8 (Managed — Bản Full) / OpenJDK (`javac` / `java` 17/21+) hệ thống |
| **Scratch** | `.sb3`, `.sb2`, `.sb` | TurboWarp VM / scratch-run 0.1.7 (Managed) | TurboWarp VM / scratch-run 0.1.7 (Managed) | TurboWarp VM / scratch-run 0.1.7 (Managed — tích hợp sẵn cả bản Full & Core) |

Phiên bản hiện tại hỗ trợ Windows 10/11 x64, macOS Apple Silicon (`arm64`) và Linux 64-bit (`x86_64` / `amd64`, chuẩn `GLIBC >= 2.34`, từ Ubuntu 22.04 LTS trở lên).

## Thành phần tích hợp trong bản Full và cơ chế trên Linux

Bản Windows Full gồm các gói:
1. `gcc-windows-x64` (dùng chung cho C++ và C)
2. `python-windows-x64`
3. `fpc-windows-x64`
4. `temurin21-windows-x64`
5. `scratch-windows-x64`

Bản macOS Full gồm các gói:
1. `python-macos-arm64`
2. `fpc-macos-arm64`
3. `temurin21-macos-arm64`
4. `scratch-macos-arm64`

Trên macOS, việc biên dịch C và C++ sử dụng Apple Clang từ Xcode Command Line Tools.

Bản **Linux Full (`_full_amd64.deb`, `-full-1.x86_64.rpm`, `_full_amd64.AppImage` — từ v1.6.1)** gồm các gói:
1. `gcc-linux-x64` (xPack GCC 14.2.0-2 dùng chung cho C++ và C, kèm binutils & sysroot)
2. `python-linux-x64` (CPython 3.12.13)
3. `fpc-linux-x64` (Free Pascal 3.2.2)
4. `temurin21-linux-x64` (Eclipse Temurin OpenJDK 21.0.12+8)
5. `scratch-linux-x64` (`scratch-run` 0.1.7)

Cơ chế vận hành trên **Linux x86_64 (`.deb`, `.rpm`, `.AppImage`)**:
- **Với Bản Linux Full**: Toàn bộ 5 gói compiler/runtime cho 6 ngôn ngữ được đóng gói kèm chỉ mục nội dung `SHA-256` từng tệp (`toolchain-content-index`). Khi khởi chạy lần đầu, ứng dụng xác minh toàn vẹn từng tệp và tự động nhập vào bộ nhớ đệm cục bộ mà không cần kết nối mạng. Trong Bubblewrap sandbox, hệ thống tự động ánh xạ đường dẫn ngắn (`/jd-tools`, `/jd-work`) để Free Pascal biên dịch ổn định với đường dẫn thư mục dài, đồng thời thiết lập `LANG=C.UTF-8` cho Java runtime.
- **Với Bản Linux Core**: Khi cài đặt bằng gói `JudgeDesk_<version>_amd64.deb` qua `sudo apt install ./JudgeDesk_<version>_amd64.deb`, trình quản lý gói `apt` tự động cài đặt `g++`, `python3`, `bubblewrap`, `zenity` theo kho phần mềm chuẩn của bản Ubuntu/Debian đang chạy (ví dụ GCC 11 + Python 3.10 trên Ubuntu 22.04 LTS; GCC 13 + Python 3.12 trên Ubuntu 24.04 LTS; GCC 14+ trên Ubuntu 26.04 LTS+). JudgeDesk tự động phát hiện và kích hoạt các trình biên dịch hệ thống ở chế độ `Local (auto-detect)` ngay khi mở ứng dụng mà không khoá cứng số hiệu phiên bản hay đường dẫn.
- Máy ảo chấm **Scratch (`.sb3`, `.sb2`, `.sb`)** được nhúng sẵn trực tiếp trong ứng dụng trên cả 3 hệ điều hành (cả bản Full và bản Core).

## Cờ biên dịch và quy ước thực thi mặc định

Thiết lập biên dịch mặc định giữ tính tương thích với Themis và chuẩn Olympic/VNOJ:

- **C++ (C++14 mặc định):** `-pipe -O2 -s -lm -x c++` (Windows bổ sung `-static` và stack reserve `66060288` byte; Linux liên kết động an toàn với `/usr` và `/lib*` chỉ đọc trong Bubblewrap sandbox).
- **C (C11 mặc định):** `-pipe -O2 -s -lm -x c` (Windows bổ sung `-static` và stack reserve `66060288` byte).
- **Pascal:** `-O2 -XS -Sg -Cs66060288`.
- **Java:** Biên dịch qua `javac` vào thư mục đầu ra riêng để thu thập tệp `.class`.
- **Python:** Chạy trực tiếp qua trình thông dịch Python được chỉ định.
- **Scratch:** Chạy trực tiếp qua máy ảo Scratch Headless. Dữ liệu vào được cấp qua khối `ask and wait`, dữ liệu ra thu thập từ khối `say` (kèm ký tự xuống dòng) và `think` (không kèm ký tự xuống dòng). Chế độ Turbo Mode được bật mặc định để tối ưu tốc độ.

## Universal Precompiled Header C++ (`bits/stdc++.h.gch`)

Từ phiên bản v1.5.0+, JudgeDesk tự động khởi tạo và tái sử dụng bộ nhớ đệm Precompiled Header (PCH) đa năng cho mọi trình biên dịch C++ trên Windows, macOS và Linux (Managed GCC, MinGW-w64, MSYS2 UCRT64, Apple Clang, Linux GCC/Clang):
- **Cơ chế băm nhận diện**: Tạo mã băm 16-hex dựa trên đường dẫn tệp thực thi chuẩn hoá, kích thước tệp, thời gian sửa đổi (`mtimeMs`) và chuỗi đầu ra phiên bản (`compiler -v` / `--version`).
- **Tăng tốc biên dịch vượt trội**: Giảm thời gian biên dịch các bài thi C++ nạp `<bits/stdc++.h>` từ ~3.6s xuống chỉ còn ~0.23s (tăng tốc gấp ~16.5 lần).
- **Làm ấm ngầm (Background Judge Pre-warm)**: Tự động nạp sẵn PCH và khởi tạo môi trường sandbox ngay khi mở ứng dụng, sẵn sàng chấm tức thì mà không có độ trễ ở lần chấm đầu tiên.

## Giới hạn tài nguyên thực thi

- **Giới hạn bộ nhớ (Memory Limit)**: Hỗ trợ cấu hình từ **8 MB đến 1024 MB (1 GB)** cho mỗi bài toán (mặc định 256 MB theo chuẩn các kỳ thi Olympic Tin học / HSG / ICPC).
- **Giới hạn thời gian (Time Limit)**: Hỗ trợ từ 100 ms đến 15000 ms với đồng hồ CPU microsecond trong Native Sandbox.
- **Cô lập an toàn**: 100% kết nối mạng bị chặn (`0-network invariant`), cách ly không gian tệp tạm thời (Windows AppContainer + Job Objects, macOS Seatbelt + Run Guardian, Linux Bubblewrap + `cgroup v2` + `seccomp-bpf`).

## Cơ chế phát hiện trình biên dịch cục bộ (Local) & Tự động di trú chính sách

Khi khởi động lần đầu, ứng dụng quét nhanh các biến môi trường `PATH` và các thư mục cài đặt tiêu chuẩn để phát hiện compiler có sẵn. Quá trình quét giới hạn trong vài giây, không duyệt toàn bộ ổ đĩa.

Mỗi ngôn ngữ có 4 trạng thái cấu hình:
- `None`: Không sử dụng.
- `Managed`: Dùng gói tải về có chữ ký của JudgeDesk.
- `Local (auto-detect)`: Dùng compiler hệ thống được tự động phát hiện.
- `Local (path)`: Dùng đường dẫn thực thi do người dùng chỉ định thủ công.

Hệ thống tự động di trú các chính sách cấu hình cũ sang chuẩn bảo mật mới (`migrateLegacyExecutionPolicy`), loại bỏ các bảng cài đặt phức tạp không cần thiết.

Trình biên dịch cục bộ phải đáp ứng các bài kiểm tra thực thi an toàn của hệ điều hành trước khi được chấp nhận chấm bài.

## Kiến trúc lưu trữ và tự động khử trùng lặp (Deduplication)

JudgeDesk sử dụng mô hình 2 tầng lưu trữ độc lập:
1. **Tầng Đóng gói sẵn (`built-in`)**: Nằm trong thư mục tài nguyên của ứng dụng (`resources/toolchains`). Bản Full sử dụng trực tiếp tầng này ở chế độ chỉ đọc (Read-Only), khởi động tức thì và dùng chung cho mọi User trên máy tính.
2. **Tầng Tải về theo người dùng (`downloaded`)**: Nằm tại `%LOCALAPPDATA%\ThemisV2\toolchains` (Windows), `~/Library/Application Support/ThemisV2/toolchains` (macOS) hoặc `~/.local/share/ThemisV2/toolchains` (Linux), dùng để lưu các compiler được tải thêm theo nhu cầu.

Khi khởi động, JudgeDesk tự động đối soát: Nếu phát hiện một gói toolchain đã có sẵn trong tầng `built-in` mà vẫn tồn tại bản sao trong thư mục `AppData` của người dùng (do phiên bản cũ hoặc từng tải trước đó), hệ thống sẽ **tự động giải phóng bản sao thừa trong `AppData`**, tránh lãng phí dung lượng ổ đĩa.

## Xác thực tính toàn vẹn và bản quyền

Mỗi gói toolchain được đóng gói kèm tệp giấy phép (License, Notices) và danh mục linh kiện phần mềm (SPDX SBOM).

Tệp manifest phiên bản 2 (`toolchains-manifest.json`) được ký số bằng thuật toán Ed25519, cố định mã gói, phiên bản, kích thước, kiến trúc CPU và mã băm SHA-256. Ứng dụng kiểm tra chữ ký và mã băm trước khi giải nén gói vào hệ thống.
