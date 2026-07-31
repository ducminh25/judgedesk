# Managed toolchains

JudgeDesk Full đóng gói sẵn các compiler/runtime được quản lý. JudgeDesk Core có
thể tải đúng các gói tương tự từ release
[`toolchains-v2`](https://github.com/ducminh25/judgedesk/releases/tag/toolchains-v2).

| Gói | Nền tảng | Phiên bản | Phục vụ |
| --- | --- | --- | --- |
| GCC/MinGW-w64 | Windows x64 | 14.2.0 | C++, C |
| CPython | Windows x64 | 3.12.13 | Python |
| Free Pascal | Windows x64 | 3.2.2 | Pascal (`.pas`, `.pp`) |
| Temurin JDK | Windows x64 | 21.0.12+8 | Java |
| CPython | macOS Apple Silicon | 3.12.13 | Python |
| Free Pascal | macOS Apple Silicon | 3.2.2 | Pascal (`.pas`, `.pp`) |
| Temurin JDK | macOS Apple Silicon | 21.0.12+8 | Java |

Trên macOS, C/C++ dùng Apple Clang từ Xcode Command Line Tools và lớp tương
thích của JudgeDesk; không có gói GCC macOS trong catalog.

## Kiểm tra tính toàn vẹn

`toolchains-manifest.json` v2 được ký và chứa URL, nền tảng, kiến trúc, kích
thước, SHA-256, entrypoint và các file bắt buộc của từng gói. JudgeDesk xác minh
chữ ký catalog, checksum archive và cấu trúc tối thiểu trước khi áp dụng. Lần mở
đầu chỉ chạy phát hiện/probe nhanh; ứng dụng không bắt giáo viên chờ một bộ
certification nhiều phút cho từng ngôn ngữ.

## Nguồn và giấy phép

- GCC: [GNU Compiler Collection](https://gcc.gnu.org/), được đóng gói từ bản dựng [WinLibs](https://github.com/brechtsanders/winlibs_mingw). Gói phát hành bao gồm `COPYING3`, `COPYING.RUNTIME` và `SOURCE.txt`.
- Python: [CPython](https://www.python.org/) từ [python-build-standalone](https://github.com/astral-sh/python-build-standalone).
- Pascal: [Free Pascal](https://www.freepascal.org/).
- Java: [Eclipse Temurin](https://adoptium.net/).

Repository này không thay đổi giấy phép của thành phần bên thứ ba. Mỗi archive
phát hành kèm file giấy phép, thông báo bên thứ ba, thông tin nguồn và SBOM để
đối chiếu.
