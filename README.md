# JudgeDesk

Đây là repository công khai chính thức của JudgeDesk, ứng dụng desktop chấm bài
lập trình dành cho kỳ thi và lớp học. Repository này cung cấp bộ cài, checksum,
updater manifest, managed toolchain, hướng dẫn sử dụng và kênh hỗ trợ người
dùng. Mã nguồn ứng dụng không được phân phối trong repository này.

## Tải xuống

- [Releases](https://github.com/ducminh25/judgedesk/releases)
- [Thay đổi trong v1.3.0](CHANGELOG.md)
- [Hướng dẫn cài đặt](INSTALL.md)
- [Managed toolchains](TOOLCHAINS.md)
- [Hỗ trợ và báo lỗi](SUPPORT.md)
- [Chính sách bảo mật](SECURITY.md)

## JudgeDesk v1.3.0

JudgeDesk phát hành cho Windows 10/11 x64 và macOS Apple Silicon với hai lựa
chọn:

- **Full (khuyến nghị):** Windows cài sẵn managed toolchain cho C++, C, Python,
  Pascal và Java; macOS cài sẵn Python, Pascal và Java, đồng thời dùng Apple
  Clang từ Xcode Command Line Tools cho C/C++. Windows Full còn kèm WebView2
  offline.
- **Core:** bộ chấm gọn hơn, cho phép tải managed toolchain từ JudgeDesk sau khi
  cài hoặc dùng compiler/runtime local đã qua kiểm tra nhanh.

Trên macOS, C/C++ sử dụng Apple Clang tương thích từ Xcode Command Line Tools;
Full tích hợp Python, Free Pascal và Java dành cho Apple Silicon. Core và Full
dùng chung dữ liệu/application identity nên không cài song song; cài một bản sẽ
nâng cấp hoặc thay thế bản còn lại.

Chỉ tải artifact từ trang Releases của repository này và đối chiếu checksum
được phát hành cùng phiên bản trước khi cài đặt.
