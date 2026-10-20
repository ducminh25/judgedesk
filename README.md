# JudgeDesk

JudgeDesk là phần mềm desktop chấm bài lập trình cục bộ (offline) cho kỳ thi, đội tuyển và lớp học Tin học, chạy gốc (native) trên **Windows 10/11 x64**, **macOS Apple Silicon (`arm64`, macOS 11+)** và **Linux 64-bit (`x86_64` / `amd64` — từ Ubuntu 22.04 LTS, 24.04 LTS, 26.04 LTS+ và các bản phân phối tương thích)**.

Ứng dụng hoạt động hoàn toàn độc lập với mạng Internet trong lúc chấm bài, hỗ trợ bảng điểm trực tiếp theo thời gian thực, cấu hình nhóm test (Subtasks), chấm bài thi tương tác (Interactive), chấm điểm phân số chính xác tuyệt đối, nhận bài qua thư mục mạng LAN (`F8` OSD), đóng gói kỳ thi `.judgedesk`, xuất bảng điểm Excel (`.xlsx`), giao diện Sáng/Tối cùng hai ngôn ngữ Tiếng Việt và Tiếng Anh.

JudgeDesk hỗ trợ 6 ngôn ngữ lập trình: **C++, C, Python 3, Pascal (Free Pascal), Java (OpenJDK) và Scratch (`.sb3`, `.sb2`, `.sb`)**. Mã nguồn thí sinh được thực thi trong môi trường cách ly an toàn của từng hệ điều hành (Windows AppContainer + Job Objects, macOS Seatbelt + Run Guardian, Linux Bubblewrap + `cgroup v2` + `seccomp-bpf`) và không tự ý đổi sang trình biên dịch khác khi cấu hình lỗi.

---

## Tải về và chọn bản cài đặt (từ v1.6.0)

Tải bộ cài chính thức tại [GitHub Releases](https://github.com/ducminh25/judgedesk/releases).

### Bảng tổng hợp các loại tệp cài đặt theo hệ điều hành

| Hệ điều hành | Phiên bản hỗ trợ | Định dạng & Tên tệp cài đặt | Khuyến nghị & Đặc điểm |
| :--- | :--- | :--- | :--- |
| **Windows x64** | Windows 10, Windows 11 (64-bit) | `JudgeDesk_<version>_full_x64_en-US.msi`<br/>`JudgeDesk_<version>_core_x64_en-US.msi` | **Full** *(Khuyên dùng cho phòng thi)*: Có sẵn WebView2 Runtime offline và toàn bộ compiler/runtime (GCC 14, Python 3.12, Free Pascal, OpenJDK 21, Scratch VM).<br/>**Core**: Bản rút gọn, tự nhận diện compiler trên máy hoặc tải thêm gói ký số trong app. |
| **macOS Apple Silicon** | macOS 11 Big Sur trở lên (`arm64` — M1/M2/M3/M4) | `JudgeDesk_<version>_full_aarch64.dmg`<br/>`JudgeDesk_<version>_core_aarch64.dmg` | **Full** *(Khuyên dùng)*: Đóng gói sẵn Python 3.12, Free Pascal, OpenJDK 21 và Scratch VM; C/C++ dùng Apple Clang (`xcode-select --install`).<br/>**Core**: Bản rút gọn dùng compiler hệ thống hoặc tải gói managed. |
| **Linux x86_64 (`amd64`) — Ubuntu / Debian / Mint** | **Ubuntu 22.04 LTS, 24.04 LTS, 26.04 LTS+**<br/>Debian 12+, Linux Mint 21+, Pop!_OS 22.04+, Zorin OS 17+ *(GLIBC $\ge$ 2.34)* | `JudgeDesk_<version>_amd64.deb` *(Ưu tiên khuyên dùng)* | Cài đặt qua `sudo apt install ./JudgeDesk_<version>_amd64.deb`. Tự động kéo các gói phụ thuộc (`bubblewrap`, `zenity`, `g++`, `python3`, GTK3/WebKit2GTK) tương thích cả chuẩn thư viện cũ và `t64`. |
| **Linux x86_64 (`amd64`) — Fedora / RHEL / openSUSE** | Fedora 38+, RHEL / AlmaLinux / Rocky 9+, openSUSE *(GLIBC $\ge$ 2.34)* | `JudgeDesk-<version>-1.x86_64.rpm` | Cài đặt qua `sudo dnf install ./JudgeDesk-<version>-1.x86_64.rpm` (hoặc `zypper` / `rpm`). |
| **Linux x86_64 (`amd64`) — Portable / Distro khác** | Hầu hết các bản phân phối Linux 64-bit có `GLIBC >= 2.34` | `JudgeDesk_<version>_amd64.AppImage` | Tệp thực thi độc lập (`chmod +x` rồi chạy trực tiếp), không cần quyền root để cài đặt ứng dụng. |

### Bản Full, Bản Core và Cơ chế Cập nhật Tự động (In-App Updater)

- **Windows & macOS**:
  - **Bản Full** tích hợp sẵn các gói trình biên dịch và runtime do JudgeDesk quản lý, phù hợp cho phòng máy, kỳ thi offline và máy giáo viên muốn cài một lần là chấm được ngay.
  - **Bản Core** chỉ gồm ứng dụng chấm thi và Scratch Headless VM, dành cho máy đã cài sẵn compiler hoặc muốn tải từng ngôn ngữ theo nhu cầu trong mục **Trình biên dịch & Runtime (`F6`)**.
- **Linux (`x86_64` / `amd64`)**:
  - Hỗ trợ từ **Ubuntu 22.04 LTS** trở lên (bao gồm **Ubuntu 22.04 LTS**, **Ubuntu 24.04 LTS**, **Ubuntu 26.04 LTS+** và các distro có `GLIBC >= 2.34`).
  - Khi cài gói `.deb` bằng `apt`, hệ thống tự động cài đặt `bubblewrap`, `zenity`, `g++`, `python3`. JudgeDesk tự động quét và nhận diện mọi phiên bản `g++`/`gcc`/`clang++`, `python3`, `fpc`, `javac` có trên máy ở chế độ `Local (auto-detect)` mà không cần chọn đường dẫn thủ công.
  - Hỗ trợ đầy đủ cả **6 ngôn ngữ** (C++, C, Python, Pascal, Java, Scratch), **Bài thi tương tác (Interactive)** và **Universal Precompiled Header (`bits/stdc++.h.gch`)**.
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
     sudo apt install ./JudgeDesk_<version>_amd64.deb
     ```
     *(Tùy chọn: cài thêm `sudo apt install fpc default-jdk` nếu kỳ thi có chấm Pascal hoặc Java).*

   - **Gói `.rpm` (Fedora 38+, RHEL / AlmaLinux / Rocky 9+, openSUSE):**
     ```bash
     sudo dnf install ./JudgeDesk-<version>-1.x86_64.rpm
     sudo dnf install bubblewrap zenity gcc-c++ python3
     ```

   - **Gói `.AppImage` (Portable cho mọi distro Linux `x86_64`):**
     ```bash
     chmod +x ./JudgeDesk_<version>_amd64.AppImage
     ./JudgeDesk_<version>_amd64.AppImage
     ```

3. Mở **JudgeDesk** từ menu ứng dụng (hoặc gõ `themis-v2` trong Terminal). Ứng dụng sẽ tự động nhận diện trình biên dịch hệ thống và kiểm tra môi trường Sandbox.

Xem hướng dẫn chi tiết, cách kiểm tra trạng thái compiler và gỡ cài đặt trong [INSTALL.md](INSTALL.md).

---

## Sử dụng cơ bản

1. Mở **JudgeDesk**, kiểm tra trạng thái môi trường và trình biên dịch (`F6`).
2. Nạp thư mục đề bài (**Load Problems** — `F2`) và thư mục bài làm thí sinh (**Load Contestants** — `F3`).
3. Nhấp chuột phải vào cột bài tập để thiết lập chế độ vào/ra (Standard I/O hoặc File I/O), giới hạn thời gian/bộ nhớ (tới 1024 MB), thang điểm/subtasks, cờ biên dịch và trình chấm (Checker / Interactor).
4. Chọn thí sinh và bài cần chấm, sau đó bấm **Chấm bài** (`F9`) để chấm trực tiếp với tiến độ thời gian thực, hoặc bật **Chấm trực tuyến qua LAN (`F8` OSD)**.
5. Nhấp đúp vào từng ô điểm để xem chi tiết từng testcase, lưu toàn bộ kỳ thi thành tệp `.judgedesk` (`Ctrl+S` / `⌘S`) hoặc xuất bảng điểm ra tệp Excel `.xlsx` (`F12`).

Trong ứng dụng, nhấn `F1` bất kỳ lúc nào để mở **Trung tâm hướng dẫn offline**.

---

## Tài liệu khác & Mã nguồn

- [Hướng dẫn cài đặt chi tiết cho Windows, macOS và Linux](INSTALL.md)
- [Quản lý compiler và runtime](TOOLCHAINS.md)
- [Hỗ trợ và báo lỗi](SUPPORT.md)
- [Chính sách bảo mật](SECURITY.md)
- [Lịch sử thay đổi (Changelog)](CHANGELOG.md)
- **Kho mã nguồn công khai (Source Repository):** [`ducminh25/judgedesk-source`](https://github.com/ducminh25/judgedesk-source)

---

## Ủng hộ dự án (Donate)

JudgeDesk được phát triển và duy trì hoàn toàn miễn phí nhằm phục vụ cộng đồng giáo viên và học sinh dạy - học lập trình. Nếu ứng dụng giúp ích cho công việc giảng dạy, chấm thi của thầy/cô và các bạn, mọi người có thể ủng hộ tác giả một ly cà phê để tiếp thêm động lực duy trì và phát triển các tính năng mới:

<p align="center">
  <img src="https://img.vietqr.io/image/MB-0835385259-compact.svg" alt="Mã QR Ủng hộ tác giả Hoàng Đức Minh" width="320" />
</p>

- **Ngân hàng:** MB
- **Chủ tài khoản:** HOANG DUC MINH
- **Số tài khoản:** `0835385259`

Trân trọng cảm ơn sự đồng hành và đóng góp của quý thầy cô và các bạn!
