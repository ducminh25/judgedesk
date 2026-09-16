# Changelog

Tất cả thay đổi đáng chú ý của JudgeDesk được ghi tại đây. Dự án dùng Semantic
Versioning; các bản `1.x` giữ tương thích updater và dữ liệu ứng dụng hiện có.

## [Unreleased]

## [1.5.0] - 2026-09-16

### Universal Precompiled Header C++, Tự Động Di Trú Chính Sách & Cải Tiến Updater

- **Universal Precompiled Header (`bits/stdc++.h.gch`) cho Trình biên dịch C++**:
  - Tự động phát hiện và sinh bộ nhớ đệm Precompiled Header đa năng cho mọi trình biên dịch C++ (Managed GCC, MinGW-w64, MSYS2 UCRT64, Apple Clang).
  - Áp dụng bộ băm định danh compiler siêu nhạy theo đường dẫn chuẩn hoá (canonical path), dung lượng tệp, mtime và mã hash phiên bản để tái sử dụng an toàn và làm mới cache đúng lúc.
  - Tăng tốc độ biên dịch các bài thi C++ nạp `<bits/stdc++.h>` vượt trội trên mọi nền tảng.
- **Tự động chuyển đổi Chính sách Thực thi (Toolchain Execution Policy Migration)**:
  - Tự động di trú chính sách thực thi toolchain cũ (`migrateLegacyExecutionPolicy`) sang cấu hình bảo mật chuẩn một cách minh bạch, an toàn và không gây gián đoạn cho người dùng.
- **Tinh gọn Giao diện Quản lý Trình biên dịch (`F6 Toolchain Manager`)**:
  - Loại bỏ bảng "Nâng cao" không cần thiết và danh sách phiên bản thử nghiệm phức tạp, mang lại trải nghiệm cấu hình trực quan, tập trung vào các chế độ nguồn thực thi đã chứng nhận.
- **Cải tiến Trình Cập nhật Ứng dụng & Nút Ủng hộ (In-App Updater & Donation Toolbar)**:
  - Tích hợp nút cập nhật ứng dụng nổi bật trên thanh công cụ với tiến trình tải Core ngầm và nút khởi động lại một chạm để áp dụng.
  - Luôn áp dụng bản Core nhất quán dù trước đó cài bản Full hay Core, bảo lưu 100% các tuỳ chọn người dùng và đường dẫn compiler đã thiết lập.
  - Bổ sung nút mời cà phê / donate tiện lợi và thân thiện trên thanh công cụ.
- **Bộ Nhận diện Thương hiệu & Biểu tượng Mới (Brand Identity & Icons)**:
  - Cập nhật logo và bộ biểu tượng ứng dụng mới sắc nét trên cả Windows và macOS, mang lại diện mạo hiện đại và chuyên nghiệp cho JudgeDesk.

## [1.4.46] - 2026-09-04

### Bài Tương Tác (Interactive), Chấm Điểm Phân Số & Tối Ưu Tốc Độ Khởi Động

- **Hỗ trợ bài thi tương tác (Interactive Problems & Interactor)**: Tích hợp runner chấm bài thi tương tác trên Windows qua cơ chế Win32 AppContainer dual-job runner với hai đường ống ẩn danh hai chiều (bidirectional anonymous pipes). Hỗ trợ biên dịch và thực thi Interactor viết bằng C++ (hỗ trợ `testlib.h` chuẩn Codeforces/Polygon) hoặc Python. Hỗ trợ nạp bộ testcase bài tương tác chỉ chứa file dữ liệu vào (`.inp`) mà không bắt buộc có file kết quả mẫu (`.out`).
- **Công cụ chấm điểm phân số chính xác tuyệt đối (Rational Scoring Engine)**: Triệt tiêu hoàn toàn hiện tượng sai số trôi dấu phẩy động IEEE 754 trong tính điểm và tổng điểm (hỗ trợ phân số tối giản như `1/3`, `2/3`, `0.35 * 10 = 3.5`). Tính toán và cộng dồn điểm chính xác dạng số hữu tỉ tối giản qua ước chung lớn nhất (GCD), đồng bộ định dạng điểm trên Scoreboard và xuất file Excel.
- **Tối ưu tốc độ khởi động & loại bỏ độ trễ warm-up 10–13s**:
  - Tự động sinh và nạp Precompiled Header (`bits/stdc++.h.gch`) với cờ chuẩn C++14 (`-std=c++14 -O2 -pipe`), tăng tốc độ biên dịch C++ gấp ~16.5 lần (từ ~3.6s xuống ~0.23s).
  - Cơ chế Background Judge Pre-warm chạy ngầm ngay khi khởi động ứng dụng để nạp sẵn Managed GCC, khởi tạo kênh giao tiếp và làm nóng sandbox trước khi người dùng bấm Chấm bài.
  - Thiết lập cơ chế bắt tay đồng bộ giữa phiên chấm và tiến trình pre-warm để đảm bảo không xung đột tài nguyên.
- **Cải tiến giao diện & Sửa lỗi đồng bộ cấu hình**:
  - Cải tiến cửa sổ tiến trình: giữ thanh tiến trình ở trạng thái chờ mượt mà trong giai đoạn chuẩn bị hệ thống, hiển thị phần trăm chuẩn xác từ bài thi đầu tiên.
  - Bản địa hóa toàn diện tiếng Việt cho các thông báo trạng thái chấm.
  - Sửa lỗi không lưu cờ Interactive vào `Config.cfg` và sửa lỗi không tải testcase bài tương tác trong cửa sổ Cài đặt bài tập.

## [1.4.45] - 2026-08-31

### Nâng giới hạn Bộ nhớ 1024 MB & Tối ưu Trình biên dịch

- Hỗ trợ cấu hình giới hạn bộ nhớ (Memory Limit) lên đến 1024 MB (1 GB) trên cả Windows và macOS, mặc định 256 MB theo tiêu chuẩn các kỳ thi Olympic Tin học / HSG / ICPC.
- Mở rộng chính sách tài nguyên bộ nhớ trên Windows từ 512 MB lên 1024 MB cho các tiến trình chấm và biên dịch.
- Tối ưu hóa khâu chuẩn bị tệp nguồn và đối tượng biên dịch (Compiler Staging), đảm bảo định danh tệp an toàn và xử lý sạch sẽ các ký tự đặc biệt / phần mở rộng.
- Rút gọn token UUID trong chẩn đoán hệ thống (Diagnostics) giúp tăng tốc độ kiểm tra tự động và duy trì tính ổn định cao.
- Cập nhật giao diện Cài đặt bài thi và bảng dịch đa ngôn ngữ (Tiếng Việt & Tiếng Anh).

## [1.4.44] - 2026-08-29

### Hoàn thiện tuyệt đối Chấm thi Scratch trên Windows & macOS

- Khắc phục triệt để lỗi khởi tạo runtime `0xC0000142` (`STATUS_DLL_INIT_FAILED`) trên Windows Sandbox bằng cơ chế cách ly an toàn `ResourceOnly` qua Windows Job Object, kiểm soát chặt chẽ giới hạn thời gian (Time Limit), bộ nhớ (Memory Limit), số lượng tiến trình và luồng I/O.
- Tương thích hoàn chỉnh cơ chế sandbox Apple Seatbelt & Run Guardian trên macOS.
- Đưa `scratch-windows-x64` và `scratch-macos-arm64` vào bản kê phân phối chính thức có chữ ký số Ed25519 (`toolchains-manifest.json`), loại bỏ hoàn toàn hiện tượng Scratch bị chuyển về `Not available` sau khi người dùng cập nhật ứng dụng.
- Cập nhật schema chuẩn Scratch 3.0 cho chẩn đoán tự động (Diagnostics) và self-test probe.
- Đồng bộ hiển thị badge phiên bản động và xác thực chấm thi thành công 100% với các bộ đề thi Tin học trẻ thực tế.

## [1.4.43] - 2026-08-29

### Tích hợp chính thức scratch-run (VNOI) & Hoàn thiện giao diện

- Tích hợp runner chuẩn `scratch-run` của VNOI (VNOI-Admin / Lê Bảo Hiệp) xây dựng trên TurboWarp `scratch-vm`, giải quyết triệt để lỗi `TOOLCHAIN_NOT_SANDBOX_COMPATIBLE` và đảm bảo tương thích 100% với các kỳ thi Tin học trẻ.
- Bổ sung gói toolchain Scratch vào bản Full Edition tự động kích hoạt theo mặc định và hỗ trợ tải về tự động trong bản Core Edition.
- Tối ưu thanh công cụ: Loại bỏ nút Sắp xếp điểm trên Toolbar để giao diện gọn gàng (vẫn giữ nút sắp xếp cạnh tiêu đề Tổng điểm trên Scoreboard).
- Tinh chỉnh nút Updater: Hiển thị trạng thái «Đã mới nhất» ngắn gọn, nổi bật khi có bản cập nhật mới.
- Thêm Scratch vào chẩn đoán hệ thống (Diagnostics) và ghi nhận credit chính thức cho VNOI-Admin trong Trung tâm hướng dẫn.

## [1.4.42] - 2026-08-29

### Bản sửa lỗi nhanh (Hotfix) cho v1.4.4

- Sửa lỗi không thể kích hoạt hoặc áp dụng gói Scratch trong Quản lý trình biên dịch (`F6`) và hộp thoại chào mừng.
- Cải thiện độ chuyển mượt của thanh tiến trình chấm bài khi kết thúc làm ấm và bắt đầu chấm bài thi đầu tiên.

## [1.4.4] - 2026-08-28

### Tích hợp Chấm Bài thi Scratch (.sb3, .sb2, .sb)

- Hỗ trợ chấm trực tiếp các tệp bài làm Scratch (`.sb3`, `.sb2`, `.sb`) phục vụ các kỳ thi Tin học trẻ.
- Cơ chế nạp test tự động qua khối lệnh `Hỏi và đợi` (bỏ qua câu hỏi hiển thị) và xuất kết quả qua khối `Nói` hoặc `Nghĩ`.
- Máy ảo Scratch Headless chạy chế độ Turbo Mode siêu tốc (10-25ms/testcase), tiêu thụ ít tài nguyên và xử lý an toàn vòng lặp vô hạn (TLE).
- Tương thích hoàn toàn trên cả Windows và macOS với quy tắc chấm đồng nhất 100%.
- Hộp thoại hỏi kích hoạt 1 lần duy nhất khi nâng cấp lên v1.4.4, dễ dàng bật tắt lại trong Quản lý trình biên dịch (F6).

### Tối ưu Phản hồi Cửa sổ Chấm & Khởi động Nhanh

- Cửa sổ tiến trình chấm bài hiện ngay lập tức khi bấm Chấm bài (F9), loại bỏ hoàn toàn độ trễ 2-5s trước đây.
- Tự động làm ấm sandbox chạy ngầm khi mở ứng dụng và lưu cache nhị phân cho Custom Checker (`.cpp`, `.py`, `.exe`), giúp chấm testcase đầu tiên ngay tức thì mà không phải chờ đợi.

### Bổ sung Hướng dẫn Nộp bài trong Trợ giúp (F1)

- Cập nhật tài liệu hướng dẫn quy cách nộp bài và các lưu ý khi làm bài thi Scratch trong Trung tâm Trợ giúp (F1) ở cả tiếng Việt và tiếng Anh.

## [1.4.3] - 2026-08-28

### Nút Sắp xếp Điểm Động & Xuất Excel Theo Thứ tự

- Bổ sung nút chuyển đổi 3 trạng thái sắp xếp điểm trên Toolbar (Điểm cao -> Điểm thấp -> Mặc định), tự động đồng bộ khi click cột Tổng Điểm trên Bảng điểm.
- Thí sinh chưa có điểm tự động hiển thị ở cuối danh sách; hòa điểm được xếp theo tên A-Z.
- File Excel xuất ra tuân thủ chính xác thứ tự bảng điểm đang hiển thị và sắp xếp các cột bài thi chi tiết từ trái qua phải.
- Loại bỏ khung lỗi tràn ngang trên Toolbar, chuyển sang hệ thống Thông báo Lỗi nổi (App Error Toast) linh hoạt.

### Tự Kiểm Tra Trình Biên Dịch & Tối Ưu Bảng Điểm

- Thêm tính năng Chẩn đoán / Tự kiểm tra (Self-Test Probe) trong Quản lý bộ công cụ cho 5 ngôn ngữ thi đấu (C++, C, Python, Pascal, Java) trực tiếp trong Native Sandbox Runner để đo đạc thời gian biên dịch, thực thi và bộ nhớ tiêu thụ.
- Tối ưu hóa bảng băm O(1) cho bảng điểm, giúp tra cứu và sắp xếp tức thì trên các kỳ thi có hàng trăm thí sinh và bài nộp.
- Tách biệt hoàn toàn tầng Modals Layer và tối ưu hóa bộ mã nguồn ứng dụng.

## [1.4.2] - 2026-08-27

### Tương thích Custom Checker Đa Định dạng & Tự động Nạp Header

- Hỗ trợ toàn diện Custom Checker định dạng `.exe`, `.cpp` và `.py` trong thư mục test bài tập trên Windows; tự động ưu tiên file `.exe` lên đầu danh sách chọn.
- Tự động nạp toàn bộ header cục bộ trong thư mục bài tập (`testlib.h`, header Themis, v.v.) khi biên dịch C++ checker trên cả Windows và macOS, chấm dứt hoàn toàn lỗi CE do thiếu header.
- Trên macOS: Tự động loại trừ các binary Windows `.exe` không tương thích, ưu tiên các checker mã nguồn `.cpp` và `.py`.

### Hoàn thiện Nhận diện Compiler Themis & Policy Tương thích

- Khắc phục lỗi hiển thị sai phiên bản compiler Themis; hiển thị chính xác phiên bản thực tế của toàn bộ công cụ Themis phát hiện được (GCC 9.2.0, FPC 3.2.2, Python 3.12.4, Java 1.7.0).
- Quét toàn diện trên mọi ổ đĩa chuẩn (`C:\Themis`, `D:\Themis`, `E:\Themis`, `Program Files (x86)\Themis`), phát hiện đầy đủ mọi toolchain mà không bị ngắt sớm do các compiler có sẵn trên `PATH`.
- Mở rộng chính sách sandbox và cơ chế fallback Job Object an toàn cho các binary legacy 32-bit (x86 WOW64) của Themis MinGW GCC / FPC / Python, xóa bỏ hoàn toàn lỗi "Không tương thích với policy máy chấm an toàn hiện tại".

## [1.4.1] - 2026-08-25

### Tự động nhận diện & So sánh Bộ công cụ Themis

- Tự động phát hiện bộ công cụ Themis (GCC 9.2, FPC 3.0/3.2, Python, Java) khi mở ứng dụng lần đầu và hiển thị bảng so sánh trực quan với Chuẩn Olympic 64-bit.
- Hỗ trợ áp dụng toàn bộ công cụ của Themis hoặc tinh chỉnh từng ngôn ngữ trong Cài đặt Bộ công cụ.

### Cải tiến Giao diện Cấu hình Bài thi

- Căn giữa cửa sổ chỉnh sửa điểm (POINTS), thời gian chạy và nhóm test để thuận tiện thao tác, tránh bị tràn nút Lưu trên các màn hình kích thước nhỏ.

## [1.4.0] - 2026-08-25

### Sửa lỗi Tương tác Kỳ thi .judgedesk

- Khắc phục triệt để lỗi không thao tác được bảng điểm khi mở tệp .judgedesk bằng cách bấm đúp từ File Explorer (Windows) và Finder (macOS).
- Cho phép tích chọn thí sinh, lọc bài tập, chấm bài, xoá điểm và cấu hình bài thi mượt mà sau khi mở file mà không bị ghi đè dữ liệu gốc.
- Tự động phát hiện thay đổi và hỗ trợ lưu đè trực tiếp (Ctrl+S / ⌘S) hoặc lưu mới (Ctrl+Shift+S / ⌘Shift+S) vào chính file .judgedesk đang mở.

### Trải nghiệm Cập nhật 1-Click Siêu tốc

- Đơn giản hóa luồng cập nhật: Bấm 1 click vào nút cập nhật trên thanh công cụ để tự động tải ngay gói Core siêu nhẹ (~20 MB) trong nền mà không cần qua hộp thoại lựa chọn.
- Bảo lưu 100% các bộ toolchain, cấu hình và dữ liệu người dùng đã có trong máy sau mỗi lần cập nhật.

### Tối ưu hóa Trình chấm, Bảo mật & Đa nền tảng

- Tối ưu hóa khởi động Native Sandbox Self-Test: Nâng thời gian chờ lên 20s và tái sử dụng probe.exe, giải quyết triệt để hiện tượng quét chậm của Windows Defender trên các máy Windows 10/11.
- Chuẩn hóa quy trình biên dịch Java: Tự động trích xuất và staging theo tên public class thực tế, đảm bảo các bài nộp Java hoạt động chuẩn xác trên cả macOS và Windows.
- Đồng nhất kết quả chia cho 0 trên macOS: Bổ sung bẫy SIGTRAP cho Apple Clang trên CPU ARM64, trả về lỗi chạy Runtime Error (0 điểm) chính xác 100% như trên x86 Windows.
- Tự động khử trùng lặp Toolchain (Deduplication): Bản Full khởi động tức thì (< 1s) với cơ chế Zero-Copy và tự động dọn dẹp các thư mục duplicate trong AppData giúp giải phóng ~800 MB ổ cứng.

## [1.3.82] - 2026-08-22

### Sửa triệt để Xuất Excel & Tối ưu Bảng điểm

- **Sửa dứt điểm lỗi xuất Excel bảng điểm trống**: Nâng cấp bộ phân giải selection linh hoạt (case-insensitive) và cơ chế tự động fallback thông minh; đảm bảo xuất đầy đủ 100% các cột bài tập, điểm số từng bài, tổng điểm và chi tiết testcase trong mọi tình huống tick chọn.
- **Run Metadata môi trường hoàn chỉnh**: Tự động tổng hợp và ghi nhận đầy đủ thông số môi trường hệ thống trong file Excel kể cả khi mở từ kỳ thi đã lưu hoặc khi thông số host đang nạp ngầm.

## [1.3.8] - 2026-08-22

### Trải nghiệm Cập nhật & Thông tin Phát hành

- **Tự động hiển thị "Có gì mới trong JudgeDesk"**: Sau khi cập nhật lên phiên bản mới, ứng dụng tự động hiển thị cửa sổ giới thiệu các tính năng mới và cải tiến một lần duy nhất; tự động ghi nhớ để không làm phiền ở các lần mở tiếp theo.
- **Tra cứu Nhật ký thay đổi (Changelog)**: Bổ sung mục tra cứu toàn bộ lịch sử phát hành song ngữ (Tiếng Việt & Tiếng Anh) trực tiếp từ **Trung tâm Trợ giúp (`F1`)**.

### Sửa lỗi Xuất Excel & Run Metadata

- Sửa triệt để lỗi xuất Excel bảng điểm trống và thông báo "Run Metadata: Unavailable" khi xuất từ contest đã lưu hoặc sau khi chấm trực tuyến OSD.
- Hỗ trợ so khớp tên thí sinh và bài thi không phân biệt hoa thường (`case-insensitive`), bảo toàn định dạng hiển thị.
- Tự động tổng hợp dữ liệu môi trường thực tế (`hostStatus`) làm fallback khi chưa có metadata lượt chạy.

### Mở kỳ thi & Kéo thả (Drag & Drop)

- Hỗ trợ mở file kỳ thi `.judgedesk` trực tiếp khi bấm đúp từ File Explorer (Windows) và Finder (macOS), tự động nạp toàn bộ đề bài, thí sinh và bảng điểm.
- Hỗ trợ chuyển giao mở file tức thì khi ứng dụng đang mở sẵn (single-instance IPC activation qua socket).
- Thêm hiệu ứng overlay kéo thả file `.judgedesk` toàn màn hình và nâng cấp vùng thả thư mục trong hộp thoại chọn Đề thi / Thí sinh.

## [1.3.7] - 2026-08-21

### Chấm bài trực tuyến (Online Judge / OSD)

- Tự động quét và nhận diện bài nộp mới theo thời gian thực từ thư mục nộp bài chung (OSD Shared Folder).
- Hàng đợi chấm bài thông minh, cập nhật tiến độ, điểm số và chi tiết chấm tức thì trên bảng điểm.
- Bổ sung nút trạng thái OSD và bảng điều khiển cấu hình tiện lợi trên thanh công cụ.

### Sửa lỗi xuất Excel & Bảng điểm

- Sửa triệt để lỗi xuất Excel bảng điểm trống và thông báo "Run Metadata: Unavailable" khi chấm bài trực tuyến hoặc nạp kỳ thi đã lưu.
- Hỗ trợ so khớp tên thí sinh và bài thi không phân biệt hoa thường (`case-insensitive`), bảo toàn định dạng hiển thị.
- Hỗ trợ xuất file Excel đầy đủ Run Metadata môi trường ngay cả khi không có hàng/cột nào được tích chọn.

### Mở kỳ thi & Kéo thả (Drag & Drop)

- Hỗ trợ mở kỳ thi trực tiếp khi bấm đúp file `.judgedesk` trên cả Windows và macOS, tự động nạp đề bài, thí sinh, cấu hình và kết quả chấm.
- Cải thiện trải nghiệm trực quan khi kéo thả file kỳ thi `.judgedesk` với overlay toàn màn hình, và hiệu ứng nhận diện thư mục rõ ràng trong hộp thoại nạp Đề thi / Thí sinh.

## [1.3.6] - 2026-08-20

### Hiệu năng chấm bài & Tối ưu hóa chuyển Testcase

- **Khôi phục cơ chế Submission-level Artifact Caching**: File thực thi của thí sinh và custom checker được nạp vào bộ nhớ đệm an toàn của Sandbox (`artifacts/`) đúng 1 lần duy nhất ngay sau khi biên dịch và tái sử dụng tức thì trên toàn bộ các testcase.
- **Triệt tiêu độ trễ giữa các testcase**: Loại bỏ hoàn toàn thao tác sao chép (`fs.copyFileSync`) và xóa file `.exe` lặp đi lặp lại ở từng testcase, giải phóng 100% gánh nặng I/O đĩa NTFS và ngăn chặn triệt để hiện tượng Windows Defender liên tục chặn quét lại cùng một file thực thi qua mỗi test.
- **Tối ưu hóa máy tính đời cũ**: Giảm tải hoàn toàn tắc nghẽn đọc/ghi trên các phòng máy dùng ổ cứng cơ (HDD) và CPU thế hệ cũ, mang lại trải nghiệm chấm nhảy testcase tức thì (< 1ms).

## [1.3.5] - 2026-08-19

### Phím tắt & Trải nghiệm điều hướng bàn phím

- Bổ sung hệ thống phím tắt đồng bộ với Themis truyền thống (`F1` - `F12`, `Ctrl+N/O/S`, `Ctrl+Shift+S`, `Ctrl+A/D/I`, `Ctrl+Shift+A/D/I`).
- Tự động nhận diện và chuyển đổi phím bổ trợ trên macOS (`Ctrl` → `Command ⌘`, `Ctrl+Shift` → `Shift+Command ⇧⌘`).
- Hỗ trợ điều hướng bảng điểm hoàn toàn không cần chuột: Dùng các phím mũi tên (`↑ ↓ ← →`) di chuyển ô chọn với đường viền tiêu điểm rõ ràng, phím `Space` để bật/tắt ô checkbox, phím `Enter` để mở cửa sổ xem chi tiết kết quả testcase.
- Cơ chế bảo vệ thông minh tránh xung đột phím tắt: Giữ nguyên hành vi bôi đen toàn bộ văn bản của `Ctrl+A` khi con trỏ đang ở ô nhập liệu (`<input>`, `<textarea>`); khóa thao tác bảng điểm ngầm khi đang mở modal.
- Hỗ trợ phím `Esc` (Escape) trên toàn bộ các modal, dialog, cửa sổ cấu hình và xem log để thoát nhanh về màn hình chính.

### Trình biên dịch & Ổn định môi trường chấm

- Loại bỏ cơ chế băm file SHA-256 nhị phân trước khi chấm, chấm trực tiếp file thực thi trong thư mục tạm được bảo vệ của sandbox. Giải quyết triệt để hiện tượng cảnh báo nhầm (false-positive) từ Windows Defender mà vẫn đảm bảo 100% độ an toàn và cô lập tiến trình.
- Nâng cấp cơ chế tự động nhận diện compiler cục bộ (MSYS2 UCRT64, MinGW64, CodeBlocks MinGW, Themis GCC) ngay lần đầu khởi động, tự động bảo lưu và ghi nhớ đường dẫn qua các lần cập nhật ứng dụng.

### Tài liệu & Hướng dẫn sử dụng

- Đại tu toàn diện nội dung Trung tâm Hướng dẫn (`HelpCenter`), chuẩn hóa thuật ngữ sư phạm, diễn đạt tự nhiên, rõ ràng cho giáo viên và học sinh.
- Bổ sung chuyên mục tra cứu phím tắt chi tiết trong Hướng dẫn sử dụng (`F1`) và hiển thị gợi ý phím tắt trực tiếp trên tooltip của các nút thanh công cụ.

## [1.3.4] - 2026-08-13

### Cập nhật Core và Full

- Khi có phiên bản mới, nút Updater mở hai lựa chọn Core và Full, đánh dấu bản
  đang dùng và hiển thị dung lượng tải về/dung lượng sau cài đặt từ chính
  artefact release.
- Cập nhật cùng edition bắt đầu tải ngay sau khi chọn. Khi đổi Core ↔ Full,
  JudgeDesk giải thích trước ảnh hưởng tới toolchain và yêu cầu xác nhận trước
  khi tải; người dùng không phải gỡ ứng dụng hoặc tự tìm đúng file cài đặt.
- Core → Core và Full → Full giữ lựa chọn compiler/runtime tùy chỉnh. Full mới
  thay các toolchain tích hợp bằng package của release mới; Core không tự đổi
  đường dẫn compiler người dùng đã chọn.
- Gói cập nhật đã tải nhưng chưa cài chỉ được giữ trong bộ nhớ của tiến trình và
  được giải phóng khi hủy, chọn lại hoặc đóng JudgeDesk. Updater luôn đọc
  `latest.json`, nên máy bỏ qua một hay nhiều bản vẫn nhận phiên bản mới nhất.

### Ủng hộ dự án

- Thêm nút cốc cà phê giữa Updater và Công cụ. JudgeDesk luôn hỏi xác nhận trước
  khi mở trang QR ủng hộ bằng trình duyệt mặc định.

## [1.3.3] - 2026-08-13

### Hiệu năng và độ phản hồi

- Giảm mạnh thời gian khởi động Native protection trên Windows khi cache
  toolchain lớn đã có đúng ACL: JudgeDesk kiểm tra policy trước và bỏ qua lượt
  áp quyền đệ quy dư thừa; mọi sai lệch vẫn được sửa theo đường fail-closed cũ.
- Mỗi phiên chấm chỉ quét source thí sinh/test một lần và lưu sẵn cấu hình trọng
  số, điểm tối đa, time limit để dùng lại. Đây không phải tính trước điểm kết
  quả: điểm vẫn chỉ có sau khi bài chạy và checker trả verdict như trước.
- Bảng điểm dùng chỉ mục kết quả, memo hóa từng hàng và gom các progress event
  dày vào frame giao diện mới nhất, giúp cuộn/chấm kỳ thi lớn ổn định hơn.
- Nén/mở kỳ thi `.judgedesk`, tạo workbook Excel và các thao tác toolchain nặng
  được chuyển khỏi luồng giao diện; polling toolchain chậm không còn chồng lặp.

### Xuất Excel

- Nút **Xuất file Excel** sáng khi bảng điểm có ít nhất một thí sinh hoặc một
  bài, kể cả trước khi chấm; các ô và tổng điểm chưa có kết quả là ô trống thật.
- Trên Windows có Microsoft Excel, JudgeDesk mở một workbook `.xlsx` mới chưa
  lưu để `Ctrl+S` hoặc đóng workbook đi qua Save As. Máy không có Excel và
  macOS vẫn giao chính file `.xlsx` cho ứng dụng do hệ điều hành chọn.
- Chặn thao tác xuất trùng trong lúc worker đang tạo file và thay giới hạn cũ
  bằng một giới hạn an toàn 256 MiB cho toàn payload, phù hợp hơn với kỳ thi lớn.

### Nền tảng build

- Khóa release vào Node.js 24.19.0/npm 11.17.0, Rust 1.97.1 và dependency Tauri
  v2 đã cập nhật, đồng thời làm mới các GitHub Action đã pin SHA.
- Không thay đổi hệ điều hành hỗ trợ: Windows 10/11 x64 và macOS 11+ trên Apple
  Silicon; application identifier, dữ liệu người dùng và updater key được giữ nguyên.

## [1.3.2] - 2026-08-10

### Toolchain và trải nghiệm cài đặt

- Full tự chọn lại mọi package managed đi kèm đúng một lần khi cài mới hoặc khi
  thay Core bằng Full; sau bootstrap, lựa chọn thủ công của người dùng tiếp tục
  được tôn trọng. Full → Core tự chuyển nguồn managed không còn tồn tại về tự
  phát hiện local.
- Discovery C/C++/Python chạy nền mỗi lần mở app, ưu tiên đúng thứ tự `PATH`,
  đọc version/vị trí rồi mới thử các đường dẫn chuẩn có giới hạn. API dùng
  snapshot ngay lập tức và không chặn giao diện để launch compiler/runtime.
- Bỏ Verify/Freeze khỏi luồng chính của các profile local nhanh. Strict
  compatibility probe vẫn là mặc định; công tắc Advanced resource-only phải do
  người dùng chủ động bật và vẫn giữ TLE/MLE/process-tree cleanup, nhưng không
  cô lập filesystem/network.
- Full macOS tiếp tục dùng Apple Clang + macOS SDK từ Xcode Command Line Tools
  hoặc Xcode cho C/C++; Python, FPC và Temurin ARM64 là các package đi kèm.
- Smoke gate Apple Silicon chờ strict compatibility probe hoàn tất trong thời
  hạn hữu hạn thay vì báo lỗi ngay khi trạng thái còn đang được kiểm tra.

### Cấu hình bài và hướng dẫn

- Thêm cấu hình biên dịch riêng từng bài trong `Config.cfg`: `Tương thích
  Themis` (mặc định), `Hiện đại (tham khảo IOI/ICPC 2025)`, `ICPC World Finals`
  và `Tùy chỉnh`. Flags được lưu dưới dạng argv snapshot; các override source,
  output, plugin, toolchain root và linker nguy hiểm bị chặn.
- Thêm Help Center offline song ngữ, tìm kiếm, phím F1, nút Hướng dẫn bên trái
  updater và phần About/chẩn đoán sản phẩm trong cùng một cửa sổ.
- Audit khả năng portable được ghi lại riêng; v1.3.2 vẫn chỉ hỗ trợ installer
  Windows và disk image macOS, không phát hành ZIP/SFX portable.

## [1.3.0] - 2026-08-01

### Chấm bài đa ngôn ngữ

- Bổ sung registry có version cho C++, C, Python, Pascal và Java; mỗi ngôn ngữ
  công khai rõ trạng thái `Không tìm thấy`, `Chỉ dùng để dịch máy chấm` hoặc
  `Sẵn sàng chấm` và không fallback âm thầm khi nguồn đã chọn không khả dụng.
- Thêm lựa chọn nguồn cố định `None`, `Managed by toolchain`, `Local (path)` và
  `Local (auto-detect)`. Auto-detect nhanh chỉ tự chạy trong lần thiết lập đầu;
  các lần sau người dùng chủ động bấm quét lại.
- C và C++ giữ bộ cờ thi đấu tương thích Themis theo từng nền tảng; Python chạy
  qua managed interpreter khi chấm submission; Pascal dùng FPC và Java dùng
  Temurin JDK đã ghim phiên bản.
- Custom checker C++ và Python đi qua cùng native secure executor, giới hạn tài
  nguyên và cleanup contract như submission; syntax-check và execution của
  checker Python được khóa vào cùng managed runtime đã xác minh.

### Core, Full và managed toolchain

- Phát hành riêng Core và Full cho Windows x64 và macOS Apple Silicon. Full
  tích hợp toàn bộ managed package được duyệt; Core tải đúng các gói đã ký từ
  public repository khi cần.
- Thêm manifest v2 ký Ed25519, SHA-256 bắt buộc, metadata package nghiêm ngặt,
  chống path traversal/symlink escape và probe thật trước khi gói được đánh dấu
  sẵn sàng.
- Windows Full kèm GCC 14.2, Python 3.12.13, FPC 3.2.2, Temurin 21 và WebView2
  offline. macOS Full kèm Python/FPC/Temurin ARM64; C/C++ dùng Apple Clang đã
  kiểm tra nhanh từ Xcode Command Line Tools.
- Tối ưu clone cache compiler/runtime và snapshot nội bộ để giảm overhead mỗi
  lượt chấm mà vẫn giữ workspace, quyền thực thi và package identity tách biệt.

### Native protection và vòng đời tiến trình

- Hoàn thiện single-instance: mở JudgeDesk lần hai sẽ chuyển focus về cửa sổ
  đang chạy thay vì tạo thêm backend hoặc lượt chấm chồng chéo.
- Siết giám sát backend, process group và run guardian trên macOS; fail closed
  khi không thiết lập được containment, cleanup hoặc generation guard.
- Mở rộng secure execution cho C, Pascal và Java; chuẩn hóa timeout khởi động
  compiler lạnh, chẩn đoán lỗi guardian và cleanup sau hủy/chấm xong.
- Sửa các trường hợp FPC/macOS cần đúng SDK, linker script và utility hệ thống;
  giảm false positive Defender từ binary Pascal mà không nới sandbox.

### Giao diện và vận hành

- Thêm màn hình quản lý compiler/runtime và thiết lập lần đầu bằng tiếng
  Việt/tiếng Anh, với thao tác cài, áp dụng, gỡ managed package hoặc chọn đường
  dẫn local rõ ràng.
- Nút kiểm tra/tải bản cập nhật trên toolbar có chữ hiển thị trực tiếp, vùng bấm
  lớn hơn, tiến độ tải, trạng thái lỗi và thao tác thử lại dễ nhận biết.
- Sửa discovery snapshot bị cũ sau khi cài Full, đường dẫn resource Windows và
  giá trị mặc định để năm ngôn ngữ sẵn sàng ngay sau cài đặt hợp lệ.

### Repository, phát hành và kiểm thử

- Chuyển mã nguồn sang repository private `ducminh25/judgedesk-source`; giữ
  `ducminh25/judgedesk` làm repository công khai cho bộ cài, hướng dẫn, hỗ trợ,
  updater manifest và managed toolchain.
- Workflow toolchain v2 phát hành chéo bằng credential riêng; release pipeline
  tạo Core/Full có tên artifact riêng và chỉ dùng Full làm updater chuẩn.
- Bổ sung gate thật cho package, C/Pascal/Java/Python, custom checker, resource
  limit, single-instance, macOS process supervisor và packaged Full app.

## [1.2.7] - 2026-07-27

### Ổn định Python được quản lý trên macOS

- Tôn trọng đúng nguồn Python đang được chọn: System/Custom Python chỉ phục vụ
  trusted checker, còn submission chỉ báo sẵn sàng khi Managed Python thực sự
  được chọn và có thể chạy trong sandbox.
- Đưa toàn bộ managed Python package vào executor cache tự chứa, viết lại các
  symbolic link nội bộ sau khi copy và xử lý đúng alias đường dẫn tạm
  `/var`/`/private/var` trên macOS.
- Siết kiểm tra containment theo đường dẫn canonical: chấp nhận symlink hợp lệ
  nằm trong toolchain nhưng từ chối link thoát khỏi thư mục cài đặt/cache.
- Thêm release smoke test cài Managed Python thật, import runtime vào native
  executor và chạy compile-and-run submission Python trên Apple Silicon.

### Reload và phát hiện test

- Reload giữ nguyên trạng thái đã tick/bỏ tick của thí sinh và problem hiện có;
  mục mới phát hiện được chọn mặc định, mục đã bị xóa được loại khỏi trạng thái.
- Chỉ nhận thư mục test có đủ file input `.inp/.in` và expected output
  `.out/.ans`. Các thư mục phụ như `__pycache__`, thư mục rỗng hoặc thiếu một
  phía không còn xuất hiện trên giao diện hay bị cộng nhầm vào tổng trọng số.
- Giao diện cấu hình problem và judge engine dùng chung một quy tắc discovery,
  giữ kết quả nhất quán trên Windows và macOS.

### Kiểm thử

- Bổ sung regression tests cho selection khi Reload, managed Python cache,
  archive symlink/containment và testcase discovery đa nền tảng.
- CI xác nhận typecheck, toàn bộ server tests, frontend/backend build và Tauri
  shell trên Windows 2022 và macOS 15.

## [1.2.6] - 2026-07-26

### Hợp nhất Repository & Phát hành Công khai (Public Release)

- Chuyển mã nguồn chính thức sang repository công khai `ducminh25/judgedesk`.
- Nhập toàn bộ tài liệu người dùng (`INSTALL.md`, `SECURITY.md`, `SUPPORT.md`, `TOOLCHAINS.md`), mẫu báo cáo lỗi và workflow đóng gói toolchain từ kho cũ.
- Cập nhật đường dẫn tra cứu bản cập nhật ứng dụng (`latest.json`), URL tải về managed toolchain (`toolchains-v1`), và các liên kết hỗ trợ về kho lưu trữ mới `ducminh25/judgedesk`.

### Sửa lỗi chấm bài Python trên macOS

- Khắc phục lỗi chọn nhầm System Python gây lỗi `TOOLCHAIN_NOT_SANDBOX_COMPATIBLE` (CE toàn bộ) khi chấm bài Python bằng Managed Toolchain trong sandbox macOS.
- Sửa lỗi xác định cache managed Python trên macOS (`bin/python3` symlink), đồng thời bổ sung kiểm tra `realpath` ngăn chặn directory traversal.
- Bổ sung quy tắc `process-exec` cho thư mục `toolchain_root` trong cấu hình Seatbelt sandbox, cho phép thi hành trình thông dịch Python trong managed runtime mà không bị từ chối quyền.
- Bảo toàn tuyệt đối các cơ chế bảo mật: chặn mạng, chặn giới hạn tiến trình/luồng (forkbomb), từ chối thực thi cmd/shell và cách ly tệp trong workspace.

## [1.2.4] - 2026-07-23

### Sửa lỗi macOS

- Truyền macOS SDK sysroot vào bước auto-detect Apple Clang, sửa trường hợp
  compiler tồn tại nhưng probe bị `ld: library 'c++' not found` trên Xcode
  17/macOS 26.
- Packaged DMG smoke test giờ bắt buộc backend sidecar thực sự nhận diện được
  Apple Clang; build sẽ fail nếu System Status còn báo G++ `Not detected`.
- Đóng gói compatibility header `bits/stdc++.h` cho Apple Clang/libc++, sửa CE
  của các bài thi C++ viết theo thói quen GCC mà không cần nới quyền sandbox.
- Dùng giới hạn process cấp kernel và hostile regression test để chặn fork bomb
  trên Mac chip M; release gate sẽ fail nếu containment này bị bỏ trong các bản
  cập nhật sau.
- Chèn trap cho phép chia số nguyên với mẫu số thực sự bằng 0 khi compile bằng
  Apple Clang. Chỉ test phát sinh lỗi nhận `RE`, nhất quán với GCC/x86 trên
  Windows; checker, phép chia số thực và các test khác không bị thay đổi.
- Làm rõ bước cài DMG một lần để đổi bundle cũ `ThemisV2.app` thành
  `JudgeDesk.app`; updater in-place vẫn giữ nguyên identifier và dữ liệu.

## [1.2.3] - 2026-07-22

### Sửa lỗi macOS

- Khôi phục auto-detect Apple Clang trên Xcode 17/macOS 26. JudgeDesk không còn
  đánh dấu compiler hợp lệ là `Not detected` khi SDK không cung cấp `libm` độc
  lập.
- Bỏ `-lm` khỏi probe và lệnh compile C++ trên macOS vì các math symbol được
  cung cấp qua `libSystem`; sửa toàn bộ bài C++ bị CE `library 'm' not found`.
- Bỏ cờ strip `-s` đã obsolete trên linker mới của Apple. Windows và Linux vẫn
  giữ nguyên các compiler flag hiện tại.

## [1.2.2] - 2026-07-22

### Sửa lỗi macOS

- Sửa toàn bộ bài C++ bị CE `TOOLCHAIN_NOT_SANDBOX_COMPATIBLE` trên các máy Mac
  mà `xcrun --find clang++` trả về shim `/usr/bin/clang++`. JudgeDesk giờ lấy
  binary Apple Clang thật từ Developer root do `xcode-select` chỉ định.
- Không còn báo Homebrew GCC/LLVM là compiler tương thích với Native protection;
  sandbox vẫn fail closed và chỉ cấp quyền cho Apple Clang từ Xcode hoặc Command
  Line Tools.
- New contest, Open contest và đóng ứng dụng không còn hỏi lưu một contest rỗng
  khi scoreboard chưa có thí sinh hoặc problem.

## [1.2.1] - 2026-07-22

### Native protection và tính đúng đắn

- Thêm native secure executor mặc định: Windows dùng LPAC/AppContainer kết hợp
  Job Object; macOS dùng sandbox profile theo từng run, process group và
  resource limits. Không còn fallback sang đường chạy native không bảo vệ.
- Chặn network, giới hạn cây process/thread, output, workspace write, wall time
  và memory; thêm startup self-test và fail closed khi runner không sẵn sàng.
- Chuẩn hóa verdict TLE/RE/OLE và số liệu time/memory giữa backend và giao diện.
  TLE chỉ hiện một dòng, ẩn exit code nội bộ và hiển thị đúng time limit.
- Sửa giới hạn môi trường compile gây `environment exceeds protocol limits`,
  tăng độ ổn định của compiler watchdog và cache artifact.
- Checker tùy chỉnh portable nhận source C++ hoặc Python; bỏ chạy checker `.exe`
  để tránh sai khác và rủi ro giữa Windows/macOS.

### Desktop và quy trình chấm

- Ngăn WebView tự reload khi Alt+Tab/mất focus và tắt file watcher trong chế độ
  manual desktop test để không restart sidecar ngoài ý muốn.
- Backend chờ startup có retry; đóng app, `Ctrl+C`, End task hoặc Force Quit sẽ
  dọn executor/backend theo đúng parent PID, không kill nhầm phiên khác.
- Nút Judge chỉ bật khi secure runner sẵn sàng và có ít nhất một thí sinh cùng
  một problem được chọn; chọn lại tất cả sau khi bỏ chọn hoạt động bình thường.
- Thứ tự batch judging giữ đúng thứ tự đang hiển thị trên scoreboard.
- Save problem configuration cảnh báo nếu đã có điểm và xóa toàn bộ điểm/log của
  problem sau khi người dùng xác nhận.
- Reload/New contest không tự kích hoạt workspace cũ; đường dẫn lần chọn gần nhất
  chỉ được dùng để prefill hộp chọn thư mục.
- Đồng bộ trạng thái compiler giữa System Status và Compilers & Runtimes Manager.

### Windows và macOS

- Sửa nhận diện executor đã được bundle trong desktop dev và installer.
- macOS dùng `wait4`/`ru_maxrss` cho peak memory và phân loại signal/resource
  limit; hỗ trợ đầy đủ Apple Clang/Xcode SDK trong compiler sandbox; tên bundle
  hiển thị JudgeDesk thay vì ThemisV2.
- CodeMagic Apple Silicon chạy executor unit/security tests và compile-run C++
  thật, xác minh kiến trúc, chữ ký ad-hoc, entitlements, DMG và updater tarball
  trước khi xuất artifact.

### Kiểm thử

- Thêm test executor Rust, security fixtures, boundary TLE/memory lặp 20 lần,
  lifecycle/orphan checks, compiler containment và benchmark JSON/Markdown.
- Đối chiếu 1.060 test với Themis: điểm cấp problem khớp sau quy đổi thang điểm;
  JudgeDesk cho kết quả ổn định hơn trên các test sát ngưỡng thời gian.

## [1.2.0] - 2026-07-19

### Nổi bật

- Đổi tên sản phẩm và thiết kế lại giao diện thành **JudgeDesk**, theo hướng
  desktop workbench gọn hơn trên Windows và macOS.
- Thêm file kỳ thi `.judgedesk`: lưu problems, source thí sinh, cấu hình, lựa
  chọn và toàn bộ kết quả chấm trong một file ZIP portable duy nhất.
- Scoreboard mới tự fit theo cửa sổ lớn, giữ kích thước điều khiển khi cửa sổ
  hẹp, không chọn text trong cell, không hiện dấu ba chấm và hỗ trợ menu chuột
  phải/secondary click cùng nút menu dọc.

### Thêm mới

- Toolbar rút gọn với New/Open/Save contest, Load problems, Load contestants,
  Reload, Judge, Export Excel, Delete selected scores và Update.
- Menu theo cell, problem và contestant cho cấu hình, xóa điểm, chấm lại và xem
  chi tiết; thao tác xóa luôn có bước xác nhận.
- Progress khi lưu contest và cảnh báo Save / Don't save / Cancel khi New,
  Open hoặc đóng app với dữ liệu chưa lưu.
- Chi tiết chấm hiển thị đúng `score / max score` và số test AC hoàn toàn.

### An toàn và tính đúng đắn

- Checker tích hợp và `testlib.h` không còn được cài dưới dạng source có thể sửa.
  App nhúng bản canonical vào binary và tạo một thư mục preview tạm mới mỗi lần
  người dùng chọn View source; bản preview luôn kèm `testlib.h`.
- Bản checker dùng để compile được khôi phục từ dữ liệu nhúng, hoàn toàn tách
  khỏi file preview mà người dùng có thể chỉnh sửa.
- Khóa các thao tác thay đổi contest, cấu hình, điểm và export trong lúc batch
  grading hoặc save contest đang chạy để tránh xung đột trạng thái.
- Bỏ Global time limit; mỗi test dùng time limit trong cấu hình problem/Polygon,
  mặc định 1 giây khi không có cấu hình.
- Sửa tổng điểm Polygon, score tối đa và thống kê test pass trong cửa sổ chi tiết.

### Sửa lỗi

- Sửa đóng app bằng nút X trên Windows và nút đóng cửa sổ trên macOS, bao gồm
  contest trắng, Don't save và sau khi tạo New Contest.
- Ngăn sidecar debug cũ gây lỗi `Access is denied` khó hiểu khi chạy lại app.
- Cải thiện tốc độ tải trạng thái hệ thống ban đầu và vòng đời sidecar.
- Giới hạn tên checker/cột dài để không kéo layout vượt khỏi cửa sổ.

### Nâng cấp

- Giữ nguyên identifier `com.themisv2.judge`, public updater key và endpoint.
  Máy đang chạy `1.1.3` hoặc `1.1.4` có thể cập nhật trực tiếp lên `1.2.0`.
- Giữ nguyên Windows MSI UpgradeCode của ThemisV2 để JudgeDesk thay thế bản cũ
  thay vì cài song song.
- macOS hỗ trợ Apple Silicon, macOS 11 trở lên. Windows hỗ trợ Windows 10/11 x64.

[1.2.0]: https://github.com/ducminh25/judgedesk/releases/tag/v1.2.0
[1.2.1]: https://github.com/ducminh25/judgedesk/releases/tag/v1.2.1
[1.2.2]: https://github.com/ducminh25/judgedesk/releases/tag/v1.2.2
[1.2.3]: https://github.com/ducminh25/judgedesk/releases/tag/v1.2.3
[1.2.4]: https://github.com/ducminh25/judgedesk/releases/tag/v1.2.4
[1.2.6]: https://github.com/ducminh25/judgedesk/releases/tag/v1.2.6
[1.2.7]: https://github.com/ducminh25/judgedesk/releases/tag/v1.2.7
[1.3.0]: https://github.com/ducminh25/judgedesk/releases/tag/v1.3.0
[1.3.2]: https://github.com/ducminh25/judgedesk/releases/tag/v1.3.2
[1.3.3]: https://github.com/ducminh25/judgedesk/releases/tag/v1.3.3
