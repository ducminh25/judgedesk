# JudgeDesk

JudgeDesk là phần mềm desktop chấm bài lập trình cục bộ (offline) cho kỳ thi, đội tuyển và lớp học Tin học, chạy gốc (native) trên **Windows 10/11 x64**, **macOS Apple Silicon (`arm64`, macOS 11+)** và **Linux 64-bit (`x86_64` / `amd64` — từ Ubuntu 22.04 LTS, 24.04 LTS, 26.04 LTS+ và các bản phân phối tương thích)**.

Ứng dụng hoạt động hoàn toàn độc lập với mạng Internet trong lúc chấm bài, hỗ trợ bảng điểm trực tiếp theo thời gian thực, cấu hình nhóm test (Subtasks), chấm bài thi tương tác (Interactive), chấm điểm phân số chính xác tuyệt đối, nhận bài qua thư mục mạng LAN (`F8` OSD), trung tâm thông báo chính thức có chữ ký số (Notification Center), đóng gói kỳ thi `.judgedesk`, xuất bảng điểm Excel (`.xlsx`), giao diện Sáng/Tối cùng hai ngôn ngữ Tiếng Việt và Tiếng Anh.

JudgeDesk hỗ trợ 6 ngôn ngữ lập trình: **C++, C, Python 3, Pascal (Free Pascal), Java (OpenJDK) và Scratch (`.sb3`, `.sb2`, `.sb`)**. Mã nguồn thí sinh được thực thi trong môi trường cách ly an toàn của từng hệ điều hành (Windows AppContainer + Job Objects, macOS Seatbelt + Run Guardian, Linux Bubblewrap + `cgroup v2` + `seccomp-bpf`) và không tự ý đổi sang trình biên dịch khác khi cấu hình lỗi.

Liên hệ với tác giả: Hoàng Đức Minh (Mon) - 26521215 - ducmon2509@gmail.com

---

## Tải về và chọn bản cài đặt

Tải bộ cài chính thức phiên bản mới nhất tại [GitHub Releases](https://github.com/ducminh25/judgedesk/releases/latest).

### Bảng tổng hợp các loại tệp cài đặt theo hệ điều hành

| Hệ điều hành | Phiên bản hỗ trợ | Định dạng & Tên tệp cài đặt | Mô tả |
| :--- | :--- | :--- | :--- |
| **Windows x64** | Windows 10, Windows 11 (64-bit) | `JudgeDesk_<version>_full_x64_en-US.msi`<br/>`JudgeDesk_<version>_core_x64_en-US.msi` | **Full** *(Khuyên dùng cho phòng thi)*: Có sẵn WebView2 Runtime offline và toàn bộ compiler/runtime (GCC 14, Python 3.12, Free Pascal, OpenJDK 21, Scratch VM).<br/>**Core**: Bản rút gọn, tự nhận diện compiler trên máy hoặc tải thêm gói ký số trong app. |
| **macOS Apple Silicon** | macOS 11 Big Sur trở lên (`arm64` — M1/M2/M3/M4) | `JudgeDesk_<version>_full_aarch64.dmg`<br/>`JudgeDesk_<version>_core_aarch64.dmg` | **Full** *(Khuyên dùng)*: Đóng gói sẵn Python 3.12, Free Pascal, OpenJDK 21 và Scratch VM; C/C++ dùng Apple Clang (`xcode-select --install`).<br/>**Core**: Bản rút gọn dùng compiler hệ thống hoặc tải gói managed. |
| **Linux x86_64 (`amd64`) — Ubuntu / Debian / Mint** | **Ubuntu 22.04 LTS, 24.04 LTS, 26.04 LTS+**<br/>Debian 12+, Linux Mint 21+, Pop!_OS 22.04+, Zorin OS 17+ *(GLIBC $\ge$ 2.34)* | `JudgeDesk_<version>_full_amd64.deb`<br/>`JudgeDesk_<version>_amd64.deb` *(Core)* | **Full** *(Khuyên dùng)*: Đóng gói sẵn toàn bộ 6 ngôn ngữ offline (xPack GCC 14.2, Python 3.12, Free Pascal 3.2.2, Temurin JDK 21, Scratch 0.1.7).<br/>**Core**: Tự động kéo `bubblewrap`, `zenity`, `g++`, `python3` qua `apt` và nhận diện mọi compiler trên máy kèm Scratch tích hợp sẵn. |
| **Linux x86_64 (`amd64`) — Fedora / RHEL / openSUSE** | Fedora 38+, RHEL / AlmaLinux / Rocky 9+, openSUSE *(GLIBC $\ge$ 2.34)* | `JudgeDesk-<version>-full-1.x86_64.rpm`<br/>`JudgeDesk-<version>-1.x86_64.rpm` *(Core)* | **Full** *(Khuyên dùng)*: Đóng gói sẵn toàn bộ 6 ngôn ngữ offline.<br/>**Core**: Tự động nhận diện `g++`/`clang++`, `python3`, `fpc`, `javac` trên máy kèm Scratch tích hợp sẵn. |
| **Linux x86_64 (`amd64`) — Portable / Distro khác** | Hầu hết các bản phân phối Linux 64-bit có `GLIBC >= 2.34` (kèm `libfuse2` + `bubblewrap`) | `JudgeDesk_<version>_full_amd64.AppImage`<br/>`JudgeDesk_<version>_amd64.AppImage` *(Core)* | Tệp thực thi độc lập (`chmod +x` rồi chạy trực tiếp), hỗ trợ cả bản **Full** (đủ 6 ngôn ngữ offline) và **Core**, không cần quyền root để cài đặt ứng dụng. |

### Bản Full, Bản Core, Trung Tâm Thông Báo và Cơ chế Cập nhật Tự động

- **Bản Full (trên cả Windows, macOS & Linux)**:
  - Tích hợp sẵn các gói trình biên dịch và runtime do JudgeDesk quản lý, phù hợp cho phòng máy, kỳ thi offline và máy giáo viên muốn cài một lần là chấm được ngay mà không cần kết nối mạng.
  - Từ **v1.6.1**, **Linux (`x86_64`)** đã có đầy đủ **Bản Full** cho cả 3 định dạng (`.deb`, `.rpm`, `.AppImage`) đóng gói sẵn toàn bộ 6 ngôn ngữ: xPack GCC 14.2.0-2 (C/C++ kèm binutils & sysroot), CPython 3.12.13, Free Pascal 3.2.2, Eclipse Temurin JDK 21.0.12+8 và `scratch-run` 0.1.7, kèm xác minh chỉ mục `SHA-256` từng tệp và tự động nhập vào cache khi khởi chạy lần đầu.
- **Bản Core (trên cả Windows, macOS & Linux)**:
  - Chỉ gồm ứng dụng chấm thi và Scratch Headless VM tích hợp sẵn, dành cho máy đã cài sẵn compiler hoặc muốn tải từng ngôn ngữ theo nhu cầu trong mục **Trình biên dịch & Runtime (`F6`)**.
  - Trên Linux, khi cài gói `.deb` bằng `apt`, hệ thống tự động cài đặt `bubblewrap`, `zenity`, `g++`, `python3`. JudgeDesk cũng tự động quét và nhận diện mọi phiên bản `g++`/`gcc`/`clang++`, `python3`, `fpc`, `javac` có trên máy ở chế độ `Local (auto-detect)` mà không cần chọn đường dẫn thủ công.
- **Trung tâm Thông báo Admin có chữ ký số (`Notification Center`)**:
  - Biểu tượng chuông thông báo trên thanh công cụ (kèm số lượng tin chưa đọc) giúp nhận các bản tin phát hành, hướng dẫn kỹ thuật và thông báo quan trọng chính thức từ ban quản trị.
  - Hỗ trợ đọc bài viết định dạng editorial song ngữ (Tiếng Việt & Tiếng Anh), xác thực chữ ký số **Ed25519** chống giả mạo và lưu đệm an toàn để xem lại offline mọi lúc mà không ảnh hưởng tới quá trình chấm thi.
- **Cập nhật 1-Click giữ nguyên định dạng gói cài đặt**:
  - Khi có phiên bản mới, nút **Cập nhật** trên thanh công cụ tải bản cập nhật nhẹ trong nền và giữ nguyên 100% cấu hình cùng dữ liệu kỳ thi.
  - Trên Linux, trình cập nhật được tách kênh riêng theo đúng định dạng bạn đã cài đặt ban đầu (`deb` cập nhật qua `.deb`, `rpm` cập nhật qua `.rpm`, `AppImage` cập nhật qua `.AppImage`), tuyệt đối không tải nhầm định dạng gói của bản phân phối khác.

---

## Cài đặt nhanh

Trước khi mở gói cài đặt, nên tải kèm tệp `SHA256SUMS` trong cùng bản release để đối chiếu mã băm SHA-256.

### 1. Windows 10/11 x64

1. Tải `JudgeDesk_<version>_full_x64_en-US.msi` (hoặc bản `core`) và `SHA256SUMS`.
2. Kiểm tra mã băm trong PowerShell:
   ```powershell
   Get-FileHash .\JudgeDesk_*.msi -Algorithm SHA256
   Get-Content .\SHA256SUMS
   ```
3. Chạy tệp `.msi` để cài đặt hoặc nâng cấp trực tiếp bản cũ.

### 2. macOS Apple Silicon (M1/M2/M3/M4 — macOS 11+)

1. Tải `JudgeDesk_<version>_full_aarch64.dmg` (hoặc bản `core`) và `SHA256SUMS`.
2. Kiểm tra mã băm trong Terminal:
   ```bash
   shasum -a 256 JudgeDesk_*.dmg
   cat SHA256SUMS
   ```
3. Mở tệp `.dmg`, kéo `JudgeDesk.app` vào thư mục `Applications`.
4. Trước lần mở đầu tiên, gỡ thuộc tính cách ly (quarantine) của macOS:
   ```bash
   xattr -dr com.apple.quarantine /Applications/JudgeDesk.app
   ```
5. Nếu muốn chấm bài C/C++ trên macOS, cài đặt Xcode Command Line Tools:
   ```bash
   xcode-select --install
   ```

### 3. Linux x86_64 (`amd64` — Ubuntu 22.04 LTS, 24.04 LTS, 26.04 LTS+ & Distro khác)

1. Kiểm tra mã băm SHA-256 trong thư mục chứa tệp tải về:
   ```bash
   sha256sum --ignore-missing -c SHA256SUMS
   ```
2. Cài đặt theo định dạng tệp bạn đã chọn:

   - **Gói `.deb` (Ubuntu 22.04 LTS / 24.04 LTS / 26.04 LTS+, Debian 12+, Linux Mint 21+, Pop!_OS — Khuyên dùng):**
     ```bash
     sudo apt update
     sudo apt install ./JudgeDesk_<version>_full_amd64.deb   # hoặc ./JudgeDesk_<version>_amd64.deb (bản Core)
     ```
     *(Nếu dùng bản Core và kỳ thi có chấm Pascal hoặc Java trên máy chưa có sẵn, cài thêm `sudo apt install fpc default-jdk`; bản Full đã đóng gói sẵn cả 6 ngôn ngữ).*

   - **Gói `.rpm` (Fedora 38+, RHEL / AlmaLinux / Rocky 9+, openSUSE):**
     ```bash
     sudo dnf install ./JudgeDesk-<version>-full-1.x86_64.rpm   # hoặc ./JudgeDesk-<version>-1.x86_64.rpm (bản Core)
     sudo dnf install bubblewrap zenity
     ```

   - **Gói `.AppImage` (Portable cho mọi distro Linux `x86_64`):**
     ```bash
     chmod +x ./JudgeDesk_<version>_full_amd64.AppImage   # hoặc ./JudgeDesk_<version>_amd64.AppImage (bản Core)
     ./JudgeDesk_<version>_full_amd64.AppImage
     ```

3. Mở **JudgeDesk** từ menu ứng dụng (hoặc gõ `themis-v2` trong Terminal). Ứng dụng sẽ tự động kích hoạt các gói ngôn ngữ (bản Full) hoặc nhận diện trình biên dịch hệ thống (bản Core) và kiểm tra môi trường Sandbox.

Xem hướng dẫn chi tiết, cách kiểm tra trạng thái compiler và gỡ cài đặt trong [INSTALL.md](INSTALL.md).

---

## Sử dụng cơ bản

1. Mở **JudgeDesk**, kiểm tra trạng thái môi trường và trình biên dịch (`F6`).
2. Nạp thư mục đề bài (**Load Problems** — `F2`) và thư mục bài làm thí sinh (**Load Contestants** — `F3`).
3. Nhấp chuột phải vào cột bài tập để thiết lập chế độ vào/ra (Standard I/O hoặc File I/O), giới hạn thời gian/bộ nhớ (tới 1024 MB), thang điểm/subtasks, cờ biên dịch và trình chấm (Checker / Interactor).
4. Chọn thí sinh và bài cần chấm, sau đó bấm **Chấm bài** (`F9`) để chấm trực tiếp với tiến độ thời gian thực, hoặc bật **Chấm trực tuyến qua LAN (`F8` OSD)**.
5. Nhấp đúp vào từng ô điểm để xem chi tiết từng testcase, lưu toàn bộ kỳ thi thành tệp `.judgedesk` (`Ctrl+S` / `⌘S`) hoặc xuất bảng điểm ra tệp Excel `.xlsx` (`F12`).

Trong ứng dụng, nhấn `F1` bất kỳ lúc nào để mở **Trung tâm hướng dẫn offline**, hoặc bấm vào **biểu tượng chuông thông báo** trên thanh công cụ để xem các thông báo và hướng dẫn mới nhất từ ban quản trị.

---

## Tài liệu khác

- [Hướng dẫn cài đặt chi tiết cho Windows, macOS và Linux](INSTALL.md)
- [Quản lý compiler và runtime](TOOLCHAINS.md)
- [Hỗ trợ và báo lỗi](SUPPORT.md)
- [Chính sách bảo mật](SECURITY.md)
- [Lịch sử thay đổi (Changelog)](CHANGELOG.md)

---

## Ủng hộ dự án (Donate)

JudgeDesk được phát triển và duy trì **hoàn toàn miễn phí** nhằm phục vụ cộng đồng giáo viên và học sinh dạy - học lập trình. Nếu ứng dụng giúp ích cho công việc giảng dạy, chấm thi của thầy/cô và các bạn, mọi người có thể ủng hộ tác giả một ly cà phê để tiếp thêm động lực duy trì và phát triển các tính năng mới:

<p align="center">
  <img src="https://img.vietqr.io/image/MB-0835385259-compact.svg?amount=&addInfo=JudgeDesk%20Donation" alt="Mã QR Ủng hộ tác giả Hoàng Đức Minh" width="320" />
</p>

- **Ngân hàng:** MB
- **Chủ tài khoản:** HOANG DUC MINH
- **Số tài khoản:** `0835385259`

Trân trọng cảm ơn sự đồng hành và đóng góp của quý thầy cô và các bạn!
