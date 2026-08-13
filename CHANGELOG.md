# JudgeDesk changelog

## v1.3.3 — 2026-08-13

### Hiệu năng và độ phản hồi

- Giảm thời gian khởi động Native protection trên Windows khi cache toolchain
  lớn đã có đúng ACL; mọi sai lệch vẫn đi qua đường sửa/fail-closed cũ.
- Mỗi phiên chấm chỉ quét source/test và cấu hình testcase một lần. Điểm kết
  quả vẫn chỉ xuất hiện sau khi bài chạy và checker trả verdict như trước.
- Bảng điểm dùng chỉ mục kết quả, memo hóa từng hàng và gom progress event theo
  frame, giúp cuộn/chấm kỳ thi lớn ổn định hơn.
- Nén/mở kỳ thi `.judgedesk`, tạo workbook Excel và thao tác toolchain nặng
  được chuyển khỏi luồng giao diện; polling chậm không chạy chồng lặp.

### Xuất Excel

- Nút **Xuất file Excel** sáng khi có ít nhất một thí sinh hoặc một bài, kể cả
  trước khi chấm; ô và tổng điểm chưa có kết quả được để trống.
- Trên Windows có Microsoft Excel, JudgeDesk mở workbook `.xlsx` mới chưa lưu
  để `Ctrl+S` hoặc đóng workbook đi qua Save As. Máy không có Excel và macOS
  giao cùng file `.xlsx` cho ứng dụng do hệ điều hành chọn.
- Chặn xuất trùng trong lúc worker đang tạo file và dùng giới hạn tổng payload
  256 MiB, phù hợp hơn với kỳ thi lớn.

### Nền tảng build

- Khóa release vào Node.js 24.19.0/npm 11.17.0, Rust 1.97.1 và dependency Tauri
  v2 theo lockfile.
- Giữ nguyên Windows 10/11 x64 và macOS 11+ Apple Silicon, application identity,
  dữ liệu người dùng và updater key.

## v1.3.2 — 2026-08-10

- Full tự chọn package managed đi kèm đúng một lần khi cài mới hoặc thay Core;
  các lựa chọn thủ công sau bootstrap tiếp tục được giữ nguyên.
- Discovery C/C++/Python chạy nền, ưu tiên `PATH` và trả snapshot nhanh để không
  chặn giao diện khi khởi động compiler/runtime.
- Thêm flags biên dịch riêng từng bài, Help Center offline song ngữ, phím F1,
  About và chẩn đoán hỗ trợ.
- Giữ installer Windows và disk image macOS; không phát hành bản portable.

## v1.3.0 — 2026-08-01

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
