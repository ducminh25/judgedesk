# Hỗ trợ và Báo lỗi

Trước khi tạo issue báo lỗi:

1. Kiểm tra ứng dụng đã ở phiên bản mới nhất.
2. Thử nghiệm lại lỗi với một thư mục bài thi mẫu đơn giản.
3. Loại bỏ thông tin cá nhân của thí sinh và đề thi chưa công bố khỏi nội dung gửi kèm.

Thông tin cần cung cấp khi báo lỗi:
- Phiên bản JudgeDesk đang sử dụng và định dạng gói cài đặt (`.msi`, `.dmg`, `.deb`, `.rpm`, hoặc `.AppImage`).
- Hệ điều hành và kiến trúc phần cứng:
  - Windows 10/11 x64
  - macOS Apple Silicon (`arm64`)
  - Linux `x86_64` (kèm kết quả `lsb_release -ds` / `/etc/os-release`, `uname -r`, và môi trường màn hình X11/Wayland)
- Thông tin chẩn đoán từ nút **Sao chép chẩn đoán** trong cửa sổ `F1` (**Hướng dẫn & thông tin sản phẩm**).
- Thứ tự các bước thực hiện để tạo lại lỗi.
- Kết quả mong muốn và kết quả thực tế nhận được.
- Ảnh chụp màn hình hoặc đoạn log liên quan trong `backend.log` (`%LOCALAPPDATA%\ThemisV2\logs\backend.log` trên Windows, `~/Library/Application Support/ThemisV2/logs/backend.log` trên macOS, hoặc `~/.local/share/ThemisV2/logs/backend.log` trên Linux — đã lọc bỏ dữ liệu nhạy cảm).

Gửi báo lỗi kỹ thuật tại [GitHub Issues](https://github.com/ducminh25/judgedesk/issues). Với các vấn đề liên quan đến lỗ hổng bảo mật, thực hiện theo [SECURITY.md](SECURITY.md).
