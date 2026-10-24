# Cài đặt JudgeDesk

Tải các bản phát hành chính thức tại [GitHub Releases](https://github.com/ducminh25/judgedesk/releases).

## Tổng quan nền tảng và tệp cài đặt (v1.6.1)

| Hệ điều hành | Phiên bản hỗ trợ | Tệp cài đặt khuyên dùng (Bản Full) | Tệp thay thế (Bản Core / Portable) |
| :--- | :--- | :--- | :--- |
| **Windows x64** | Windows 10, Windows 11 (64-bit) | `JudgeDesk_<version>_full_x64_en-US.msi` | `JudgeDesk_<version>_core_x64_en-US.msi` |
| **macOS Apple Silicon** | macOS 11 Big Sur trở lên (`arm64` — M1/M2/M3/M4) | `JudgeDesk_<version>_full_aarch64.dmg` | `JudgeDesk_<version>_core_aarch64.dmg` |
| **Linux x86_64 (`amd64`) — Ubuntu / Debian / Mint** | **Ubuntu 22.04 LTS, 24.04 LTS, 26.04 LTS+**<br/>Debian 12+, Linux Mint 21+, Pop!_OS 22.04+, Zorin OS 17+ *(GLIBC $\ge$ 2.34)* | `JudgeDesk_<version>_full_amd64.deb` *(Ưu tiên)* | `JudgeDesk_<version>_amd64.deb` *(Core)*<br/>`JudgeDesk_<version>_full_amd64.AppImage`<br/>`JudgeDesk_<version>_amd64.AppImage` *(Core)* |
| **Linux x86_64 (`amd64`) — Fedora / RHEL / openSUSE** | Fedora 38+, RHEL / AlmaLinux / Rocky 9+, openSUSE *(GLIBC $\ge$ 2.34)* | `JudgeDesk-<version>-full-1.x86_64.rpm` | `JudgeDesk-<version>-1.x86_64.rpm` *(Core)*<br/>`JudgeDesk_<version>_full_amd64.AppImage`<br/>`JudgeDesk_<version>_amd64.AppImage` *(Core)* |
| **Linux x86_64 (`amd64`) — Arch / Distro khác** | Các bản phân phối Linux 64-bit có `GLIBC >= 2.34` | `JudgeDesk_<version>_full_amd64.AppImage` | `JudgeDesk_<version>_amd64.AppImage` *(Core)* |

---

## 1. Windows 10/11 x64

1. Tải tệp cài đặt phù hợp từ trang release:
   - `JudgeDesk_<version>_full_x64_en-US.msi`: Khuyến nghị cho phòng máy và lớp học. Tích hợp sẵn WebView2 Runtime và toàn bộ compiler/runtime quản lý (GCC 14, CPython 3.12, Free Pascal, OpenJDK 21, Scratch Headless VM).
   - `JudgeDesk_<version>_core_x64_en-US.msi`: Gói rút gọn, cho phép dùng compiler có sẵn trên máy hoặc tải toolchain sau qua giao diện.
2. Tải tệp `SHA256SUMS` trong cùng mục release.
3. Mở PowerShell trong thư mục chứa file và kiểm tra mã băm:

```powershell
Get-FileHash .\JudgeDesk_*.msi -Algorithm SHA256
Get-Content .\SHA256SUMS
```

4. Chạy tệp `.msi` khi hai chuỗi SHA-256 khớp nhau.
5. Mở ứng dụng và kiểm tra trạng thái compiler ở màn hình ban đầu.

Gói cài đặt tự động nâng cấp tại chỗ nếu trên máy đã có phiên bản JudgeDesk trước đó, giữ nguyên dữ liệu kỳ thi và cấu hình toolchain hiện tại.

---

## 2. macOS Apple Silicon (M1/M2/M3/M4)

JudgeDesk hỗ trợ macOS 11 trở lên trên kiến trúc Apple Silicon (`arm64`).

1. Tải bản cài đặt từ release:
   - `JudgeDesk_<version>_full_aarch64.dmg`: Khuyến nghị, có sẵn runtime Python, Pascal, Java và Scratch. Riêng C/C++ sử dụng Apple Clang từ Xcode Command Line Tools (`xcode-select --install`).
   - `JudgeDesk_<version>_core_aarch64.dmg`: Gói rút gọn.
2. Tải `SHA256SUMS` và xác minh qua Terminal:

```bash
shasum -a 256 JudgeDesk_*.dmg
cat SHA256SUMS
```

3. Mở tệp `.dmg` và kéo biểu tượng `JudgeDesk.app` vào thư mục `Applications`.
4. Trước lần mở đầu tiên, gỡ thuộc tính cách ly (quarantine) của tệp tải về qua Terminal:

```bash
xattr -dr com.apple.quarantine /Applications/JudgeDesk.app
```

5. Mở ứng dụng từ Launchpad hoặc Applications. Trường hợp Gatekeeper hiển thị cảnh báo, vào **System Settings → Privacy & Security**, tìm thông báo về JudgeDesk và chọn **Open Anyway**.

---

## 3. Linux x86_64 (`amd64`) — Ubuntu 22.04 LTS, 24.04 LTS, 26.04 LTS+ & Các Bản Phân Phối Khác

Các tệp thực thi Linux của JudgeDesk được biên dịch đạt chuẩn tương thích ngược **ELF `GLIBC <= 2.34`** và khai báo phụ thuộc thư viện linh hoạt (`libgtk-3-0t64 | libgtk-3-0`, `libwebkit2gtk-4.1-0`, `libayatana-appindicator3-1 | libappindicator3-1`). Nhờ đó, bộ cài hoạt động xuyên suốt từ **Ubuntu 22.04 LTS (Jammy)**, **Ubuntu 24.04 LTS (Noble)** đến **Ubuntu 26.04 LTS+** và các bản phân phối nhân Linux 64-bit hiện đại mà không bị khoá cứng vào một phiên bản hệ điều hành duy nhất.

Từ phiên bản **v1.6.1**, Linux hỗ trợ đầy đủ cả **Bản Full** và **Bản Core** trên cả 3 định dạng gói (`.deb`, `.rpm`, `.AppImage`):
- **Bản Full (`_full_amd64.deb`, `-full-1.x86_64.rpm`, `_full_amd64.AppImage`)**: Đóng gói sẵn toàn bộ 6 ngôn ngữ lập trình offline ngay trong bộ cài gồm xPack GCC 14.2.0-2 (C/C++ kèm binutils & sysroot), CPython 3.12.13, Free Pascal 3.2.2, Eclipse Temurin JDK 21.0.12+8 và `scratch-run` 0.1.7. Khi khởi chạy lần đầu, ứng dụng tự động xác minh chỉ mục `SHA-256` từng tệp và kích hoạt trọn bộ compiler mà không cần kết nối mạng.
- **Bản Core (`_amd64.deb`, `-1.x86_64.rpm`, `_amd64.AppImage`)**: Bản rút gọn nhẹ, tích hợp sẵn máy ảo chấm Scratch (`scratch-run` 0.1.7) và tự động phát hiện mọi trình biên dịch có sẵn trên hệ thống (`g++`/`gcc`/`clang++`, `python3`, `fpc`, `javac`).

### 3.1. Xác minh tệp tải về (SHA-256)

Tải gói cài đặt phù hợp cùng tệp `SHA256SUMS` (hoặc `SHA256SUMS-linux`) về cùng một thư mục, sau đó kiểm tra trong Terminal:

```bash
sha256sum --ignore-missing -c SHA256SUMS
```

Chỉ tiếp tục cài đặt khi tệp tải về báo `OK`.

### 3.2. Cách cài đặt theo từng loại tệp

#### A. Gói `.deb` — Khuyên dùng cho Ubuntu 22.04 LTS / 24.04 LTS / 26.04 LTS+, Debian 12+, Linux Mint 21+, Pop!_OS

Tệp:
- Bản Full: `JudgeDesk_<version>_full_amd64.deb` *(Khuyên dùng cho phòng thi / offline)*
- Bản Core: `JudgeDesk_<version>_amd64.deb`

Chạy lệnh sau trong thư mục chứa tệp `.deb` (bắt buộc có dấu `./` ở đầu tên tệp để `apt` tự động tải các gói phụ thuộc):

```bash
sudo apt update
sudo apt install ./JudgeDesk_<version>_full_amd64.deb   # hoặc ./JudgeDesk_<version>_amd64.deb (bản Core)
```

- `apt` sẽ tự động cài đặt luôn `bubblewrap` (sandbox cô lập bài nộp), `zenity` (hộp thoại chọn thư mục gốc), `xdg-utils`, `g++`, `python3` và các thư viện giao diện GTK/WebKit2GTK phù hợp với phiên bản Ubuntu của bạn.
- Nếu bạn cài **Bản Core** và kỳ thi có chấm **Pascal** hoặc **Java** trên máy chưa có sẵn compiler, có thể cài thêm qua `apt` (bản **Full** đã đóng gói sẵn cả 6 ngôn ngữ nên không cần bước này):
  ```bash
  sudo apt install fpc default-jdk
  ```

#### B. Gói `.rpm` — Dành cho Fedora 38+, RHEL / AlmaLinux / Rocky Linux 9+, openSUSE

Tệp:
- Bản Full: `JudgeDesk-<version>-full-1.x86_64.rpm` *(Khuyên dùng)*
- Bản Core: `JudgeDesk-<version>-1.x86_64.rpm`

```bash
# Trên Fedora / RHEL / AlmaLinux / Rocky Linux:
sudo dnf install ./JudgeDesk-<version>-full-1.x86_64.rpm   # hoặc ./JudgeDesk-<version>-1.x86_64.rpm (bản Core)
sudo dnf install bubblewrap zenity

# Tùy chọn nếu dùng bản Core và muốn cài compiler hệ thống:
sudo dnf install gcc-c++ python3 fpc java-21-openjdk-devel
```

*(Trên openSUSE, sử dụng `sudo zypper install ./JudgeDesk-<version>-full-1.x86_64.rpm`).*

#### C. Gói `.AppImage` — Tệp chạy độc lập (Portable) cho mọi distro Linux `x86_64`

Tệp:
- Bản Full: `JudgeDesk_<version>_full_amd64.AppImage` *(Đóng gói sẵn toàn bộ 6 ngôn ngữ offline)*
- Bản Core: `JudgeDesk_<version>_amd64.AppImage`

1. Đảm bảo máy đã có `bubblewrap`, `zenity` và thư viện FUSE 2 để chạy AppImage (cùng compiler hệ thống nếu dùng bản Core):
   - Trên Ubuntu 22.04: `sudo apt install libfuse2 bubblewrap zenity`
   - Trên Ubuntu 24.04+: `sudo apt install libfuse2t64 bubblewrap zenity`
2. Cấp quyền thực thi và mở trực tiếp ứng dụng:
   ```bash
   chmod +x ./JudgeDesk_<version>_full_amd64.AppImage   # hoặc ./JudgeDesk_<version>_amd64.AppImage
   ./JudgeDesk_<version>_full_amd64.AppImage
   ```

### 3.3. Quản lý và tự động nhận diện trình biên dịch trên Linux

- **Với Bản Full**: Toàn bộ 6 ngôn ngữ (GCC 14.2.0-2, CPython 3.12.13, Free Pascal 3.2.2, Temurin JDK 21, Scratch 0.1.7) được xác minh `SHA-256` từng tệp và nhập tự động vào bộ nhớ đệm ngay lần đầu khởi chạy ở chế độ **Managed**. Sandbox Linux tự động ánh xạ đường dẫn ngắn (`/jd-tools`, `/jd-work`) giúp Free Pascal biên dịch ổn định ngay cả khi thư mục máy chủ có đường dẫn dài.
- **Với Bản Core (hoặc khi dùng compiler hệ thống)**: Bạn **không cần** chọn tay đường dẫn `/usr/bin/g++-13` hay `/usr/bin/python3.12`. Ngay khi mở ứng dụng, JudgeDesk tự động quét và nhận diện mọi phiên bản `g++` / `gcc` / `clang++` (GCC 11, 12, 13, 14...), `python3` (3.10, 3.11, 3.12, 3.13...), `fpc` và `javac`/`java` có trong `PATH` hoặc thư mục chuẩn của hệ thống ở chế độ **Local (auto-detect)**.
- Bạn có thể bấm phím `F6` (**Trình biên dịch & Runtime**) để kiểm tra trạng thái **Sẵn sàng chấm bài (Submission ready)** của từng ngôn ngữ, chuyển đổi giữa gói `Managed` và `Local`, hoặc bấm **Quét lại (Rescan)** nếu vừa cài thêm compiler mới.
- **Scratch (`.sb3`, `.sb2`, `.sb`)**, **Bài thi tương tác (Interactive)** và **Universal Precompiled Header (`bits/stdc++.h.gch`)** đều hoạt động sẵn trên Linux.

> **Lưu ý về Sandbox trên một số bản Linux tuỳ biến:** JudgeDesk sử dụng `bubblewrap` (`bwrap`) kết hợp `cgroup v2` và `seccomp-bpf` để cách ly bài làm thí sinh. Trên Ubuntu 22.04 LTS và 24.04 LTS chuẩn, chính sách AppArmor cho `/usr/bin/bwrap` đã được cấu hình sẵn. Nếu hệ thống của bạn khoá cứng user namespaces đối với `bwrap` (mục **Native Execution Sandbox** báo chưa sẵn sàng), hãy kiểm tra gói `bubblewrap` và cấu hình AppArmor `/etc/apparmor.d/bwrap` của bản phân phối, tuyệt đối không tắt toàn bộ cơ chế bảo mật hạt nhân của hệ điều hành.

---

## Cập nhật phiên bản và chuyển đổi giữa Core / Full

Ứng dụng kiểm tra phiên bản mới tự động qua nút **Cập nhật (Updater)** trên thanh công cụ:
- **Tự động cập nhật qua Updater (In-App)**:
  - Trên **Windows** và **macOS**: Tải gói cập nhật ứng dụng cốt lõi siêu nhẹ (~20MB), giữ nguyên toàn bộ toolchain đã có sẵn mà không phải tải lại hàng trăm MB compiler qua mạng.
  - Trên **Linux (`.deb` / `.rpm` / `.AppImage`)**: Trình cập nhật được cách ly theo đúng định dạng cài đặt ban đầu (`linux-x86_64-deb-core`/`full`, `linux-x86_64-rpm-core`/`full`, `linux-x86_64-appimage-core`/`full`). Máy cài bằng `.deb` sẽ luôn nhận bản cập nhật `.deb`, máy cài `.rpm` luôn nhận `.rpm`, và `.AppImage` luôn cập nhật `.AppImage` — tránh hoàn toàn xung đột định dạng gói giữa các bản phân phối Linux.
- **Chuyển từ Core sang Full (Windows, macOS & Linux)**: Có 2 cách thuận tiện:
  1. *Cách 1 (In-App trên Windows/macOS)*: Vào tab **Quản lý Toolchain (`F6`)** và bấm **Tải về** các ngôn ngữ mong muốn.
  2. *Cách 2 (Cài đặt đè trên Windows, macOS & Linux)*: Tải và chạy file `JudgeDesk_<version>_full_*` (`.msi` trên Windows, `.dmg` trên macOS, hoặc `_full_amd64.deb` / `-full-1.x86_64.rpm` / `_full_amd64.AppImage` trên Linux) mới nhất từ GitHub Releases để cài đặt trọn bộ compiler vào máy.

## Quản lý trình biên dịch và lưu trữ

Hệ thống hỗ trợ 6 ngôn ngữ: **C++, C, Python, Pascal, Java và Scratch (`.sb3`, `.sb2`, `.sb`)**:
- **Bản Full (Windows / macOS / Linux)**: Chứa sẵn bộ compiler/runtime trong gói cài đặt ứng dụng (`resources/toolchains`), xác thực toàn vẹn SHA-256 và sẵn sàng chấm offline ngay từ lần khởi chạy đầu tiên.
- **Bản Core / Tải thêm theo nhu cầu**: Các gói tải về hoặc được nhập vào kho lưu trữ của người dùng được lưu tại:
  - Windows: `%LOCALAPPDATA%\ThemisV2\toolchains`
  - macOS: `~/Library/Application Support/ThemisV2\toolchains`
  - Linux: `~/.local/share/ThemisV2/toolchains`
- **Trình biên dịch trên máy (Local)**: Ứng dụng tự động phát hiện compiler có sẵn trong `PATH` hoặc qua đường dẫn chỉ định thủ công sau khi vượt qua bước kiểm tra tương thích với Sandbox.

## Gỡ cài đặt

- **Windows:** Vào **Settings → Apps → Installed apps**, tìm **JudgeDesk** và chọn **Uninstall**.
- **macOS:** Kéo ứng dụng `JudgeDesk.app` từ thư mục `Applications` vào Trash.
- **Linux (`.deb` — Ubuntu / Debian / Mint):**
  ```bash
  sudo apt remove judgedesk
  ```
- **Linux (`.rpm` — Fedora / RHEL / openSUSE):**
  ```bash
  sudo dnf remove JudgeDesk
  ```
- **Linux (`.AppImage`):** Xóa trực tiếp tệp `JudgeDesk_<version>_amd64.AppImage`.

Dữ liệu cấu hình và bộ nhớ đệm của người dùng được lưu tại:
- Windows: `%LOCALAPPDATA%\ThemisV2`
- macOS: `~/Library/Application Support/ThemisV2`
- Linux: `~/.local/share/ThemisV2`

Thư mục này được giữ nguyên khi nâng cấp hoặc gỡ ứng dụng để tránh mất cấu hình. Nếu muốn xóa toàn bộ dấu vết dữ liệu cục bộ sau khi gỡ ứng dụng, bạn có thể xóa thủ công thư mục tương ứng ở trên.
