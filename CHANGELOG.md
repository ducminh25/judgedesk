# JudgeDesk v1.3.0

Phát hành ngày 2026-08-01 cho Windows 10/11 x64 và macOS Apple Silicon.

## Ngôn ngữ chấm bài

- Hỗ trợ chính thức C++, C, Python, Pascal và Java cho submission; custom
  checker nhận source C++ hoặc Python.
- Mỗi ngôn ngữ hiển thị rõ `Không tìm thấy`, `Chỉ dùng để dịch máy chấm` hoặc
  `Sẵn sàng chấm`; nguồn không khả dụng không fallback âm thầm.
- Có thể chọn `None`, managed toolchain, đường dẫn local hoặc auto-detect nhanh.
  Auto-detect tự chạy trong lần thiết lập đầu và có thể quét lại thủ công.

## Core và Full

- Có Core và Full cho cả Windows x64 và macOS Apple Silicon.
- Full tích hợp toàn bộ managed package được duyệt; Core tải đúng các gói đã ký
  từ repository này khi cần.
- Windows Full gồm GCC 14.2, Python 3.12.13, FPC 3.2.2, Temurin 21 và WebView2
  offline.
- macOS Full gồm Python/FPC/Temurin ARM64; C/C++ sử dụng Apple Clang đã kiểm tra
  từ Xcode Command Line Tools.

## Bảo mật và độ tin cậy

- Compiler, submission và custom checker chạy qua native secure executor, giới
  hạn tiến trình, thời gian, bộ nhớ, output, mạng và phạm vi filesystem.
- Hoàn thiện process supervisor/run guardian trên macOS và single-instance trên
  Windows: mở lần hai sẽ focus cửa sổ JudgeDesk đang chạy.
- Managed package dùng manifest v2 ký Ed25519, SHA-256 bắt buộc và kiểm tra cấu
  trúc trước khi áp dụng.
- Sửa đường dẫn resource/khởi tạo Full khiến compiler không được nhận diện sau
  cài đặt, cùng các lỗi FPC/macOS và timeout compiler lạnh đã tái hiện.

## Giao diện và phát hành

- Màn hình quản lý compiler/runtime và thiết lập ban đầu có tiếng Việt/tiếng
  Anh, hướng tới giáo viên không cần tự cấu hình PATH.
- Nút kiểm tra/tải cập nhật lớn hơn, có chữ, tiến độ và thao tác thử lại rõ ràng.
- Mã nguồn chuyển sang repository private; repository này tiếp tục là nguồn
  chính thức duy nhất cho bộ cài, checksum, toolchain, tài liệu và hỗ trợ.

## Lưu ý nâng cấp

- Người dùng v1.2.7 nhận bộ cài v1.3.0 mới trực tiếp; không cần chờ auto-update.
- Core và Full dùng chung application identity nên không cài song song.
- Bản macOS hiện không notarize bằng Apple Developer ID; hãy xác minh checksum
  và chỉ dùng **Open Anyway** cho artifact tải từ release chính thức này.
