<p align="center">
  <img src="https://github.com/namb20994-coder/bse-improved/blob/main/assets/detectionlist.png" width="800" />
</p>

<p align="center">
  <strong>Tài liệu này liệt kê tất cả các cơ chế phát hiện được thử nghiệm trong BShield. Thông tin được cập nhật chính xác tính đến ngày 8 tháng 2 năm 2026. Nếu bạn phát hiện thêm cơ chế nào khác, vui lòng báo cáo trong tab "Issues".</strong>
</p>

<p align="Center">
  <a href="DETECTION.md">🇬🇧 English</a> |
  <a href="DETECTION.vi.md">🇻🇳 Tiếng Việt</a>
</p>

> [!CAUTION]
> **Dự án này chỉ phục vụ cho mục đích học tập. Mục tiêu là làm rõ các điểm yếu trong những giải pháp bảo mật hiện tại và khuyến khích phát triển các giải pháp tốt hơn, đáng tin cậy hơn. Hãy sử dụng dự án này một cách có trách nhiệm. Tuyệt đối không sử dụng với mục đích trái với pháp luật. Tôi không chịu trách nhiệm cho bất kỳ hành động dại dột nào.**

**Mục lục:**

Đây là các cơ chế phát hiện của BShield, hãy đọc nếu bạn gặp mã lỗi trong ứng dụng được BShield hỗ trợ:
- Mã 1:
  - [Ứng dụng đã được chỉnh sửa trái phép](#modified-app-detection-code-1)
- Mã 2:
  - [Phát hiện ứng dụng chạy trong máy ảo](#detected-virtual-machineprivacy-space-code-2--8)
- Mã 3:
  - [Phát hiện các ứng dụng nguy hiểm](#package-name-detection-code-3--7)
- Mã 4:
  - [Phát hiện các hành vi đáng ngờ như gỡ lỗi ứng dụng trái phép](#debugging-app-code-4)
- Mã 5:
  - Hãy tham khảo phần này để biết thông tin về [phát hiện root](#root-detection-code-5).
  - [Phát hiện các thuộc tính nguy hiểm của hệ thống trong build.prop](#sensitive-system-properties)
  - [Phát hiện trong bộ nhớ có chứa dấu vết của custom ROM (maps detection)](#maps-detection)
  - [SELinux của kernel không phải là Enforcing](#enforcing-status)
  - [Rò rỉ mount/bộ nhớ khi sử dụng mô-đun launcher/font tùy chỉnh](#leaks-from-custom-launchers)
  - [Phát hiện JNI hooks](#jni-hook-detection)
  - [Trình khởi động (bootloader) đã được mở khóa](#bootloader-check-syscall-check)
  - [Phát hiện nhiều mount nghi ngờ](#suspicious-mount)
  - [Phát hiện "KSU/AP image loop"](#unconfirmed-ksuap-module-image-loop-detection)
- Code 6:
  - [Trình khởi động (bootloader) đã được mở khóa](#bootloader-is-unlocked-code-6)
- Code 7:
  - [Phát hiện các ứng dụng đáng ngờ](#package-name-detection-code-3--7)
- Code 8:
  - [Phát hiện ứng dụng chạy trong "không gian riêng tư" hoặc "nhân bản ứng dụng"](#detected-virtual-machineprivacy-space-code-2--8)
- Code 10:
  - [Gỡ lỗi ADB đang bật](#adb-debuggingdeveloper-mode-detection-code-10--11)
- Code 11: 
  - [Chế độ nhà phát triển đang bật](#adb-debuggingdeveloper-mode-detection-code-10--11)
- Code 12:
  - [Phát hiện custom ROM](#custom-rom-detection-code-12)

## Phát hiện ứng dụng đã được chỉnh sửa trái phép (Mã 1)
<p align="left">
  <img src="https://github.com/namb20994-coder/bse-improved/blob/main/assets/Screenshot%20From%202026-05-09%2010-59-55.png" width="800" />
</p>

Lỗi này xảy ra khi bạn cài đặt ứng dụng chưa được ký hoặc ứng dụng đã được sửa đổi.

**Giải pháp:** Gỡ bỏ ứng dụng đã được sửa đổi/chưa được ký khỏi hệ thống và cài đặt ứng dụng chính thức từ Google Play.

## Đã phát hiện máy ảo/không gian riêng tư (Mã 2 và 8)

<p align="left">
  <img src="https://github.com/namb20994-coder/bse-improved/blob/main/assets/Screenshot%20From%202026-05-09%2011-00-22.png" width="800" />
  <br>
  <sub>Mã 2</sub>
</p>
<p align="left">
  <img src="https://github.com/namb20994-coder/bse-improved/blob/main/assets/Screenshot%20From%202026-05-09%2011-02-18.png" width="800" />
  <br>
  <sub>Mã 8</sub>
</p>

Lỗi này xảy ra khi bạn cài đặt ứng dụng trong máy ảo/không gian riêng tư.

**Giải pháp:** Không cài đặt ứng dụng trong máy ảo/không gian riêng tư.

## Phát hiện app đáng ngờ/nguy hiểm sử dụng tên gói (Mã 3 và 7)

<p align="left">
  <img src="https://github.com/namb20994-coder/bse-improved/blob/main/assets/Screenshot%20From%202026-05-09%2011-00-44.png" width="800" />
  <br>
  <sub>Mã 3</sub>
</p>
<p align="left">
  <img src="https://github.com/namb20994-coder/bse-improved/blob/main/assets/Screenshot%20From%202026-05-09%2011-02-08.png" width="800" />
  <br>
  <sub>Mã 7</sub>
</p>

Một phát hiện cổ điển khác được nhiều ứng dụng sử dụng, BShield kiểm tra danh sách ứng dụng đã cài đặt để xác định các ứng dụng nguy hiểm sử dụng superuser.

Dưới đây là danh sách các ứng dụng mà BShield hiện đang phát hiện (có thể còn nhiều hơn nữa; đây chỉ là những ứng dụng được xác nhận thông qua thử nghiệm):

```txt
com.rifsxd.ksunext
me.bmax.apatch
me.weishu.kernelsu
com.topjohnwu.magisk
com.drdisagree.iconify
(và nhiều hơn nữa, có thể là mô-đun LSPosed)
```

**Giải pháp:**
Bạn có thể sử dụng các mô-đun sau:

- [ReLSPosed](https://github.com/ThePedroo/ReLSPosed)
- [HMA-OSS](https://github.com/frknkrc44/HMA-OSS)

để ẩn các ứng dụng này.

## Phát hiện các hành vi đáng ngờ như gỡ lỗi ứng dụng (Mã 4)

<p align="left">
  <img src="https://github.com/namb20994-coder/bse-improved/blob/main/assets/Screenshot%20From%202026-05-09%2011-01-02.png" width="800" />
</p>

Lỗi này chỉ xảy ra khi sử dụng công cụ gỡ lỗi của Google. **Lỗi này sẽ không xuất hiện trong phiên bản chính thức của ứng dụng.** Nếu bạn gặp phải lỗi này, vui lòng liên hệ với nhà phát triển ứng dụng.

## Phát hiện root (Code 5)

<p align="left">
  <img src="https://github.com/namb20994-coder/bse-improved/blob/main/assets/Screenshot%20From%202026-05-09%2011-01-17.png" width="800" />
</p>

Đây là thứ phát hiện khó ẩn nhất trong BSheld khi bạn sử dụng quyền root, nó bao gồm nhiều thứ liên quan đến quyền root và hệ thống.

Dưới đây là danh sách các cơ chế cụ thể được phát hiện:

### Phát hiện các thuộc tính hệ thống "nhạy cảm"

BShield cũng phát hiện một số thuộc tính hệ thống Android nhất định. Một số ví dụ đã biết bao gồm:

- `init.svc.adb_root`
- `service.adb.root`

**Giải pháp:** Các thuộc tính này có thể dễ dàng được ẩn đi bằng cách ghi đè chúng, ví dụ:

```sh
resetprop -n -p init.svc.adb_root ""
resetprop -n -p service.adb.root ""

# by RainyXeon and Jan
resetprop init.svc.adb_root stopped
resetprop init.svc.adbd stopped
resetprop persist.sys.usb.config mtp
resetprop ro.adb.secure 1
resetprop ro.secure 1
resetprop ro.debuggable 0
resetprop service.adb.root 0
```

**Lưu ý:** Các thuộc tính này sẽ được đặt lại về giá trị ban đầu khi khởi động lại máy.

### Phát hiện trong bộ nhớ có chứa dấu vết của custom ROM (maps detection)

BShield cũng có thể phát hiện xem bộ nhớ có chứa dấu vết của **LineageOS** hoặc các mục liên quan đến injection (chẳng hạn như Kernel Injection, vừa được phát hiện gần đây).

Bạn có thể kiểm tra bằng công cụ Native Detector: ([tải đi trời](https://dl.reveny.me/)). 

Ví dụ, nó có thể báo cáo "Injection Detection" hoặc "LineageOS Detected (14).
Ngoài ra, bạn có thể kiểm tra thủ công bằng lệnh:

```sh
cat /proc/self/maps | grep "framework-res.jar"
cat /proc/self/maps | grep "lineage"
```

**Ẩn dấu vết:**

Việc ẩn các mục này rất khó. Để tránh dấu vết của LineageOS, bạn có thể cần phải sửa 1 số thứ trong ROM dựa trên AOSP/Pixel hoặc kernel của điện thoại.

**Dưới đây là một số giải pháp:**
- Nếu kernel của bạn hỗ trợ KernelSU + SuSFS (với SUS_MAP được bật), bạn có thể thêm các đường dẫn bị "leak" vào danh sách map của SuSFS.
- Nếu bạn đang sử dụng module font, nó cũng có thể gây ra vấn đề này. Hãy xóa nó hoặc thêm các đường dẫn của nó vào SUS_MAP như đã đề cập ở trên.
- Bạn cũng có thể thử module TreatWheel của Pedro để ẩn các dấu vết này, nhưng hiệu quả của nó bị hạn chế và nó yêu cầu ReZygisk để hoạt động.

- Nếu bạn đang sử dụng kernel tùy chỉnh và gặp phải lỗi “phát hiện injection” trong Native Detector, vui lòng chuyển sang kernel khác càng sớm càng tốt. (Ngoài ra, bạn có thể biên dịch lại kernel nếu bạn có kỹ năng.)

**Lưu ý về việc phát hiện dấu vết trong /system/framework/framework-res.apk. (kernel injection)**

<img src="https://github.com/namb20994-coder/bse-improved/blob/main/assets/photo_2025-11-24_16-50-21.jpg" width="200" align="right">

Bạn có thể nhận thấy rằng trong công cụ <b>Native Detector</b>, nó hiển thị <b>Found Injection</b>, và kết quả trông giống như hình ảnh bên phải.

Điều này xảy ra vì kernel của bạn có thể chứa code ẩn tập tin LineageOS trong `task_mmu.c`. Xem: [ref commit (MoonWake@bea4fe4)](https://github.com/RainyXeon/moonwake_kernel_xiaomi_ruby/commit/bea4fe4ecfa41edb52f26ce9254a16643dda57ea).

Mục đích của code ẩn tập tin LineageOS này là thay thế đường dẫn tập tin LineageOS thực bằng `framework-res.apk`.

Về cơ bản, nếu một tập tin được map bởi VMA chứa từ "lineage" trong tên tập tin, thì `/proc/<pid>/map_files/<start>-<end>` sẽ trỏ đến `framework-res.apk` thay vì tập tin thực. Điều này ngăn các công cụ như MagiskDetector, các công cụ kiểm tra quyền root, các công cụ quét tính toàn vẹn ứng dụng, hệ thống MDM, v.v., phát hiện các tập tin LineageOS trong bản đồ bộ nhớ.

Tuy nhiên, cơ chế ẩn này đã lỗi thời và vô tình kích hoạt **Found Injection** trong **Native Detector**, vì header VMA giả vẫn có thể bị phát hiện. Điều này xảy ra vì code chỉ thay thế đường dẫn tệp, chứ không phải toàn bộ VMA metadata trước đường dẫn đó (theo tôi, đó có thể là lý do tại sao BShield có thể phát hiện ra nó).

Nếu bạn là nhà phát triển kernel, bạn có thể revert commit chứa mã ẩn tập tin LineageOS được đề cập ở trên. Nếu bạn là người dùng, bạn không thể làm gì trừ khi **bạn thay thế kernel** hoặc **yêu cầu nhà phát triển** làm điều đó.

### SELinux của kernel không phải là Enforcing

Đây là một phương pháp phát hiện phổ biến được nhiều ứng dụng sử dụng. Chúng tôi đặc biệt khuyến cáo **không** sử dụng ROM tùy chỉnh với **SELinux ở chế độ Permissive**, vì nó được coi là không an toàn theo các tiêu chuẩn hiện đại.

Nếu ROM của bạn đang chạy với **SELinux ở chế độ Permissive**, sẽ làm giảm đáng kể hàng rào bảo mật, khiến thiết bị dễ bị tấn công trước các cuộc tấn công nguy hiểm vào hệ thống Android. BShield yêu cầu **SELinux** phải được đặt thành **Enforcing** để hoạt động đúng cách.

**Giải pháp:**
- Đặt SELinux thành **Enforcing**
```sh
setenforce 1
```
- Sử dụng kernel hoặc ROM có **Enforcing SELinux**

### Rò rỉ mount/bộ nhớ khi sử dụng mô-đun launcher/font tùy chỉnh

BShield có thể phát hiện nhiều mô-đun có trình khởi chạy đã được chỉnh sửa, có thể thông qua mounts, dấu vết bộ nhớ hoặc các dấu vết khác.

**Giải pháp:** Cách đơn giản nhất là gỡ bỏ các mô-đun trình khởi chạy và sử dụng trình khởi chạy hệ thống mặc định. Ngoài ra, việc sử dụng các trình khởi chạy của hệ thống thường không gây ra hiện tượng phát hiện.

### Phát hiện hook JNI

Trong một số phiên bản VNeID, BShield có thể phát hiện xem ứng dụng có đang bị hook hay không. Vấn đề này có thể đã được khắc phục trong các phiên bản mới hơn của **ReZygisk CI** và **ZygiskNext**.

**Giải pháp:**
Nếu bạn vẫn gặp phải lỗi này, hãy nâng cấp phiên bản ReZygisk hoặc ZygiskNext của mình.

### Kiểm tra tình trạng bootloader, kiểm tra `syscall`

Các bản cập nhật VNeID gần đây đã tăng cường bảo mật thông qua BShield, đặc biệt nhắm vào bootloader và keybox, dẫn đến lỗi CA-E005.

- Lưu ý: Keybox/khóa attestation đã bị thu hồi vẫn có thể được sử dụng để vượt qua.

**Giải pháp:**
- Một giải pháp tạm thời là thêm tên gói (`com.vnid`) vào tệp `target.txt` của **JingMatrix/TEESimulator**.
- Đối với người dùng Tricky Store: Mở giao diện web của Tricky Addon, chọn VNeID, nhấn **Lưu**, và bạn đã hoàn tất!

### Phát hiện nhiều mount nghi ngờ

Từ lâu, BShield đã kiểm tra các điểm mount nghi ngờ như một cách để phát hiện quyền root. Điều này xảy ra khi bạn cài đặt một số mô-đun như thay đổi phông chữ hoặc trình khởi chạy.

Bạn có thể xác minh điều này bằng công cụ **Native Detector** ([tải xuống](https://github.com/reveny/Android-Native-Root-Detector/releases/latest)).

Ví dụ, nó có thể báo cáo "Suspicious mount".

**Giải pháp:**
- Kiểm tra tệp ZIP của mô-đun. Nếu nó sử dụng `mount --bind`, rất có thể nó sẽ kích hoạt quá trình phát hiện. Các nhà phát triển nên chuyển sang [phương pháp mới](https://kernelsu.org/guide/module.html) trong tài liệu KernelSU.

- Đảm bảo bạn đang sử dụng KernelSU (v3.0 trở lên), APatch hoặc Magisk mới nhất. Các phiên bản này xử lý mount namespaces một cách kín đáo hơn để bỏ qua quá trình phát hiện.

- Trên một số thiết bị cụ thể, ReZygisk có thể không gỡ bỏ các dấu vết đáng ngờ một cách hiệu quả. Việc nâng cấp lên phiên bản ReZygisk mới nhất thường là cần thiết để giải quyết các lỗi phát hiện này.

### [CHƯA XÁC NHẬN] Phát hiện "KSU/AP image loop"
Trong các báo cáo gần đây từ [@Hzzmonet](t.me/HzzMonet), BShield cũng phát hiện "KSU/AP image loop". Điều này là do trong các phiên bản KSU/AP cũ hơn, nó sử dụng OverlayFS để hoạt động, gây ra hiện tượng này.

Bạn có thể xác minh điều này bằng Native Detector.

Ví dụ, nó có thể báo cáo "KSU/AP loop" hoặc một cái gì đó tương tự.

**Giải pháp:**
- Nếu bạn đang sử dụng KernelSU phiên bản gốc hoặc cũ hơn, vui lòng sử dụng mô-đun TreatWheel của Pedro để ẩn chúng.
- Nếu bạn đang sử dụng KernelSU-Next, vui lòng tắt `Use OverlayFS` trong tab cài đặt. Bạn phải sao lưu mô-đun của mình trước khi thao tác.

## Bootloader đã được mở khóa (Mã 6)

<p align="left">
    <img src="https://github.com/namb20994-coder/bse-improved/blob/main/assets/Screenshot%20From%202026-05-09%2011-01-26.png" width="800" />
</p>

Lỗi này xảy ra khi bạn đã mở khóa bootloader. Hầu hết các ứng dụng BShield hiện chưa chặn lỗi này, nhưng có thể sẽ chặn trong tương lai. Nếu điều đó xảy ra, hãy sử dụng giải pháp bên dưới để khắc phục.

**Giải pháp:**
- Một giải pháp tạm thời là thêm tên gói (`com.vnid`) vào tệp `target.txt` của **JingMatrix/TEESimulator**.
- Đối với người dùng Tricky Store: Mở giao diện web của Tricky Addon, chọn VNeID, nhấn **Lưu**, và bạn đã hoàn tất!

## Phát hiện chế độ gỡ lỗi/nhà phát triển ADB (Mã 10 & 11)
Lỗi này xảy ra khi bạn sử dụng Chế độ nhà phát triển hoặc chế độ gỡ lỗi ADB trên thiết bị của mình.

<p align="left">
    <img src="https://github.com/namb20994-coder/bse-improved/blob/main/assets/Screenshot%20From%202026-05-09%2011-02-32.png" width="800" />
    <br>
    <sub>Mã 10</sub>
</p>

<p align="left">
    <img src="https://github.com/namb20994-coder/bse-improved/blob/main/assets/Screenshot%20From%202026-05-09%2011-02-40.png" width="800" />
    <br>
    <sub>Mã 11</sub>
</p>

**Giải pháp:**
Bạn có thể sử dụng kết hợp như sau:
- [ReLSPosed](https://github.com/ThePedroo/ReLSPosed)
- [ImNotADeveloper](https://github.com/notyour777/ImNotADeveloper)

để ẩn Chế độ Nhà phát triển, chế độ gỡ lỗi ADB.
Hoặc không bật Chế độ Nhà phát triển/gỡ lỗi ADB khi bạn không sử dụng.

## Phát hiện ROM tùy chỉnh (Mã 12)

<p align="left">
    <img src="https://github.com/namb20994-coder/bse-improved/blob/main/assets/1.png" width="800" />
</p>

Lỗi này xảy ra khi phát hiện ROM tùy chỉnh trên thiết bị. Hiện tại, ứng dụng [FPT Shop](https://play.google.com/store/apps/details?id=vn.frt.fptshop.app) là ứng dụng duy nhất thực hiện cơ chế này.

**Giải pháp:**
- Cách khắc phục tạm thời là thêm tên gói (`com.vnid`) vào tệp `target.txt` của **JingMatrix/TEESimulator**.
- Đối với người dùng Tricky Store: Mở giao diện web của Tricky Addon, chọn VNeID, nhấn **Lưu**, và bạn đã hoàn tất!
