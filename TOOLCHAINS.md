# Quản lý Toolchain trong JudgeDesk

JudgeDesk có hai hình thức phân phối toolchain:

Gói **Full** chứa sẵn toàn bộ trình biên dịch và runtime cần thiết cho hệ điều hành, sẵn sàng chấm offline ngay sau khi cài đặt.

Gói **Core** chỉ gồm nhân chấm thi, cho phép tải các gói compiler đã ký từ GitHub Release hoặc cấu hình compiler có sẵn trên máy.

## Danh mục compiler và runtime

| Ngôn ngữ | Đuôi tệp | Windows x64 | macOS Apple Silicon |
| :--- | :--- | :--- | :--- |
| **C++** | `.cpp` | GCC 14.2.0 (Managed) | Apple Clang (Hệ thống / Xcode Tools) |
| **C** | `.c` | GCC 14.2.0 (Managed) | Apple Clang (Hệ thống / Xcode Tools) |
| **Python** | `.py` | CPython 3.12.13 (Managed) | CPython 3.12.13 (Managed) |
| **Pascal / FPC** | `.pas`, `.pp` | Free Pascal 3.2.2 (Managed) | Free Pascal 3.2.2 (Managed) |
| **Java** | `.java` | Eclipse Temurin OpenJDK 21.0.12+8 (Managed) | Eclipse Temurin OpenJDK 21.0.12+8 (Managed) |

Phiên bản hiện tại không hỗ trợ macOS trên chip Intel (x86_64).

## Thành phần tích hợp trong bản Full

Bản Windows Full gồm 4 gói:
1. `gcc-windows-x64` (dùng chung cho C++ và C)
2. `python-windows-x64`
3. `fpc-windows-x64`
4. `temurin21-windows-x64`

Bản macOS Full gồm 3 gói:
1. `python-macos-arm64`
2. `fpc-macos-arm64`
3. `temurin21-macos-arm64`

Trên macOS, việc biên dịch C và C++ sử dụng Apple Clang từ Xcode Command Line Tools.

## Cờ biên dịch mặc định

Thiết lập biên dịch mặc định giữ tính tương thích với Themis:

- **C++ (C++14):** `-pipe -O2 -s -static -lm -x c++` (Windows đặt stack reserve `66060288` byte).
- **C (C11):** `-pipe -O2 -s -static -lm -x c` (Windows đặt stack reserve `66060288` byte).
- **Pascal:** `-O2 -XS -Sg -Cs66060288`.
- **Java:** Biên dịch qua `javac` vào thư mục đầu ra riêng để thu thập tệp `.class`.
- **Python:** Chạy trực tiếp qua trình thông dịch Python được chỉ định.

## Cơ chế phát hiện trình biên dịch cục bộ (Local)

Khi khởi động lần đầu, ứng dụng quét nhanh các biến môi trường `PATH` và các thư mục cài đặt tiêu chuẩn để phát hiện compiler có sẵn. Quá trình quét giới hạn trong vài giây, không duyệt toàn bộ ổ đĩa.

Mỗi ngôn ngữ có 4 trạng thái cấu hình:
- `None`: Không sử dụng.
- `Managed`: Dùng gói tải về có chữ ký của JudgeDesk.
- `Local (auto-detect)`: Dùng compiler hệ thống được tự động phát hiện.
- `Local (path)`: Dùng đường dẫn thực thi do người dùng chỉ định thủ công.

Trình biên dịch cục bộ phải đáp ứng các bài kiểm tra thực thi an toàn của hệ điều hành trước khi được chấp nhận chấm bài.

## Xác thực tính toàn vẹn và bản quyền

Mỗi gói toolchain được đóng gói kèm tệp giấy phép (License, Notices) và danh mục linh kiện phần mềm (SPDX SBOM).

Tệp manifest phiên bản 2 (`toolchains-manifest.json`) được ký số bằng thuật toán Ed25519, cố định mã gói, phiên bản, kích thước, kiến trúc CPU và mã băm SHA-256. Ứng dụng kiểm tra chữ ký và mã băm trước khi giải nén gói vào hệ thống.
