<p align="center">
  <img src="https://git.bnamm.org/namm/BSE-Improved/raw/branch/main/assets/detectionlist.png" width="800" />
</p>

<p align="center">
  <strong>Tài liệu này liệt kê tất cả các phát hiện được quan sát thấy trong BShield cho Android. Thông tin được cập nhật chính xác tính đến ngày 28 tháng 11 năm 2025. Nếu bạn phát hiện thêm phát hiện nào khác, vui lòng báo cáo trong tab "Issues".</strong>
</p>

<p align="Center">
  <a href="DETECTION.md">🇬🇧 English</a> |
  <a href="DETECTION.vi.md">🇻🇳 Tiếng Việt</a>
</p>

> [!CAUTION]
> **Dự án này chỉ phục vụ cho mục đích học tập. Mục tiêu là làm rõ các điểm yếu trong những giải pháp bảo mật hiện tại và khuyến khích phát triển các giải pháp tốt hơn, đáng tin cậy hơn. Hãy sử dụng thông tin này một cách có trách nhiệm. Tuyệt đối không sử dụng với mục đích gây hại. Tôi không chịu trách nhiệm cho bất kỳ hành động nào mà người dùng thực hiện từ dự án này.**

**Table of contents:**

**Mục lục:**

Đây là các mã phát hiện của BShield, hãy kiểm tra nếu bạn gặp mã lỗi trong ứng dụng được BShield hỗ trợ:
- Mã 1:
  - [Phát hiện ứng dụng đã sửa đổi](#phát-hiện-ứng-dụng-đã-sửa-đổi-mã-1)
- Mã 2:
  - [Phát hiện máy ảo](#đã-phát-hiện-máy-ảo-không-gian-riêng-tư-mã-2-8)
- Mã 3:
  - [Phát hiện tên gói](#phát-hiện-tên-gói-mã-3-7
- Mã 4:
  - [Gỡ lỗi ứng dụng](#gỡ-lỗi-ứng-dụng-mã-4)
- Mã 5:
  - [Phát hiện thuộc tính hệ thống nguy hiểm](#thuộc-tính-hệ-thống-mã-5)
  - [Phát hiện injection/maps](#phát-hiện-maps-và-kernel-injection-mã-5)
  - [Trạng thái Enforcing](#trạng-thái-enforcing-mã-5)
  - [Phát hiện trình khởi chạy tùy chỉnh (sử dụng module)](#leak-injection-từ-các-trình-khởi-chạy-tùy-chỉnh-mã-5)
  - [Phát hiện JNI hook](#chưa-xác-nhận-phát-hiện-hook-jni-mã-5)
  - [Đã mở khóa bootloader](#chưa-xác-nhận-kiểm-tra-trạng-thái-bootloader-kiểm-tra-syscall-mã-5-6)
  - [Đã phát hiện hình ảnh KSU/AP bị "proc loop"]#chưa-xác-nhận-phát-hiện-vòng-lặp-hình-ảnh-mô-đun-ksu-ap-mã-5)
- Mã 6:
  - [Đã mở khóa bootloader](#chưa-xác-nhận-kiểm-tra-trạng-thái-bootloader-kiểm-tra-syscall-mã-5-6)
- Mã 7:
  - [Phát hiện ứng dụng đáng ngờ](#phát-hiện-tên-gói-mã-3-7)
- Mã 8:
  - [Phát hiện privacy space/nhân bản ứng dụng](#đã-phát-hiện-máy-ảo-không-gian-riêng-tư-mã-2-8)
- Mã 10:
  - [Đã bật gỡ lỗi ADB](#phát-hiện-gỡ-lỗi-adb-chế-độ-nhà-phát-triển-mã-10-11)
- Mã 11:
  - [Chế độ nhà phát triển đã được bật](#phát-hiện-gỡ-lỗi-adb-chế-độ-nhà-phát-triển-mã-10-11)

## Phát hiện ứng dụng đã sửa đổi (Mã 1)

Lỗi này xảy ra khi bạn cài đặt ứng dụng chưa được ký hoặc ứng dụng đã được sửa đổi.

**Giải pháp:** Gỡ bỏ ứng dụng đã được sửa đổi, chưa được ký khỏi hệ thống và cài đặt từ Google Play.

## Đã phát hiện máy ảo/không gian riêng tư (Mã 2/8)

Lỗi này xảy ra khi bạn cài đặt ứng dụng trong máy ảo/không gian riêng tư.

**Giải pháp:** Không cài đặt ứng dụng trong máy ảo/không gian riêng tư.

## Phát hiện tên gói (Mã 3/7)

Một phát hiện cổ điển khác được nhiều ứng dụng sử dụng, BShield kiểm tra danh sách ứng dụng đã cài đặt để xác định các ứng dụng thường được liên kết với quyền root.

Dưới đây là danh sách các ứng dụng mà BShield hiện đang phát hiện (có thể còn nhiều hơn nữa; đây chỉ là những ứng dụng được xác nhận thông qua thử nghiệm. Vui lòng yêu cầu cập nhật trong tab "Issues"):

```txt
com.rifsxd.ksunext
me.bmax.apatch
me.weishu.kernelsu
com.topjohnwu.magisk
com.drdisagree.iconify
(và nhiều hơn nữa, có thể là mô-đun LSPosed)
```

**Giải pháp:**
Bạn có thể sử dụng kết hợp các lệnh sau:

- [ReLSPosed](https://github.com/ThePedroo/ReLSPosed)
- [HMA-OSS](https://github.com/frknkrc44/HMA-OSS)

để ẩn các ứng dụng này.

## Gỡ lỗi ứng dụng (Mã 4)
Lỗi này chỉ xảy ra khi sử dụng công cụ gỡ lỗi của Google. Lỗi này sẽ không xuất hiện trong phiên bản chính thức của ứng dụng. Nếu bạn gặp phải lỗi này, vui lòng liên hệ với nhà phát triển ứng dụng.

## Thuộc tính hệ thống (Mã 5)

BShield cũng phát hiện một số thuộc tính hệ thống Android. Một số ví dụ đã biết bao gồm:

- `init.svc.adb_root`
- `service.adb.root`

**Giải pháp:**
Các thuộc tính này có thể dễ dàng bị ẩn đi bằng cách ghi đè chúng, ví dụ:

```sh
resetprop -n -p init.svc.adb_root ""
resetprop -n -p service.adb.root ""

# của RainyXeon và Jan
resetprop init.svc.adb_root đã dừng
resetprop init.svc.adbd đã dừng
resetprop persist.sys.usb.config mtp
resetprop ro.adb.secure 1
resetprop ro.secure 1
resetprop ro.debuggable 0
resetprop service.adb.root 0
```

**Lưu ý:** Các thuộc tính này sẽ được đặt lại khi khởi động lại.

## Phát hiện "maps" và "kernel injection" (Mã 5)

BShield cũng có thể phát hiện xem memory map có chứa dấu vết của **LineageOS** hoặc các mục liên quan đến injection (chẳng hạn như Kernel Injection, vừa được tìm thấy gần đây).

Bạn có thể xác minh điều này bằng công cụ **Native Detector** ([tải xuống](https://dl.reveny.me/)).

Ví dụ: nó có thể báo cáo "Injection Detection" hoặc "LineageOS Detected (14)".

Ngoài ra, bạn có thể kiểm tra thủ công bằng lệnh:

```sh
cat /proc/self/maps | grep "framework-res.jar"
cat /proc/self/maps | grep "lineage"
```

**Bypass maps detection:**

Việc ẩn các mục này rất khó. Để tránh dấu vết của LineageOS, bạn có thể cần phải sửa đổi ROM hoặc kernel tùy chỉnh dựa trên AOSP/Pixel của mình.

**Đây là một số giải pháp:**
- Nếu kernel của bạn hỗ trợ KernelSU + SuSFS (với SUS_MAP được bật), bạn có thể thêm các đường dẫn bị "leak injection" vào SuSFS.
- Nếu bạn đang sử dụng mô-đun phông chữ, nó cũng có thể làm "leak injection". Hãy xóa nó hoặc thêm các đường dẫn của nó vào SUS_MAP như đã đề cập, nhưng hiệu quả của nó bị hạn chế và cần ReZygisk để hoạt động.
- Nếu bạn đang sử dụng kernel tùy chỉnh và gặp phải lỗi "Kernel Injection" trong Native Detector, vui lòng chuyển sang một kernel khác càng sớm càng tốt. (Ngoài ra, bạn có thể dựng lại kernel của mình nếu bạn có kỹ năng.)

**Lưu ý về phát hiện maps /system/framework/framework-res.apk. (kernel injection)**

<img src="https://git.bnamm.org/namm/BSE-Improved/raw/branch/main/assets/photo_2025-11-24_16-50-21.jpg" width="200" align="right">

Bạn có thể nhận thấy rằng trong công cụ <b>Native Detector</b>, nó hiển thị <b>Đã tìm thấy Injection</b> và kết quả trông giống như hình ảnh bên phải.

Điều này xảy ra vì kernel tùy chỉnh của bạn có thể chứa bản vá ẩn tệp LineageOS trong task_mmu.c. Xem: [commit (MoonWake@bea4fe4)](https://github.com/RainyXeon/moonwake_kernel_xiaomi_ruby/commit/bea4fe4ecfa41edb52f26ce9254a16643dda57ea).

Mục đích của bản vá ẩn tệp LineageOS này là thay thế các đường dẫn tệp LineageOS thực bằng `framework-res.apk`.

Về mặt kỹ thuật, nếu một tệp được mapped bởi VMA chứa "lineage" trong tên tệp của nó, thì `/proc/<pid>/map_files/<start>-<end>` sẽ trỏ đến `framework-res.apk` thay vì tệp thực tế. Điều này ngăn các công cụ như MagiskDetector, root-checkers, trình quét toàn vẹn ứng dụng, hệ thống MDM, v.v., phát hiện các tệp LineageOS trong bản đồ bộ nhớ.

Ý tưởng chính cho việc ẩn tệp LineageOS này là thay thế các đường dẫn tệp thực từ LineageOS bằng `framework-res.apk`. Về cơ bản, nếu tệp được mapped bởi VMA chứa `lineage` trong tên tệp của nó, thì `/proc/<pid>/map_files/<start>-<end>` sẽ trỏ đến `framework-res.apk`, chứ không phải tệp thực tế. Các công cụ như MagiskDetector, root-checkers, trình quét toàn vẹn ứng dụng, hệ thống MDM, v.v. không thể thấy các tệp LineageOS trong bản đồ bộ nhớ.

Tuy nhiên, cơ chế ẩn này đã lỗi thời và vô tình kích hoạt **Found Injection** trong **Native Detector**, vì header VMA giả vẫn có thể bị phát hiện. Điều này xảy ra vì bản vá chỉ thay thế đường dẫn tệp, chứ không phải toàn bộ VMA metadata trước đường dẫn đó (theo tôi, đó có thể là lý do tại sao BShield có thể phát hiện ra nó).

Nếu bạn là nhà phát triển kernel, bạn có thể revert commit có chứa mã ẩn tệp LineageOS đã đề cập ở trên. Nếu bạn là người dùng, bạn không thể làm gì khác ngoài việc thay thế kernel hoặc yêu cầu nhà phát triển thực hiện việc này.

## Trạng thái Enforcing (Mã 5)

Đây là một phát hiện phổ biến được nhiều ứng dụng sử dụng. Chúng tôi khuyến nghị **không** sử dụng ROM tùy chỉnh với **permissive**, vì nó được coi là không an toàn theo tiêu chuẩn hiện đại.

Nếu ROM của bạn đang chạy với **permissive**, một số cuộc tấn công có thể xảy ra. BShield yêu cầu **SELinux** được đặt thành **Enforcing** để hoạt động bình thường.

**Giải pháp:**
- Đặt SELinux thành **Enforcing**
```sh
setenforce 1
```
- Sử dụng kernel hoặc ROM với **Enforcing SELinux**
- Use a kernel or ROM with **Enforcing SELinux**

## Leak injection từ các trình khởi chạy tùy chỉnh (Mã 5)

BShield có thể phát hiện nhiều mô-đun trình khởi chạy tùy chỉnh, có thể thông qua mount, bản đồ bộ nhớ hoặc các chỉ báo khác.

**Giải pháp:**
Cách đơn giản nhất là xóa các trình khởi chạy tùy chỉnh và sử dụng trình khởi chạy hệ thống mặc định. Ngoài ra, việc sử dụng các trình khởi chạy ứng dụng tiêu chuẩn thường không kích hoạt phát hiện.

## [CHƯA XÁC NHẬN] Phát hiện hook JNI (Mã 5)

Trong một số bản phát hành VNeID, BShield có thể phát hiện xem ứng dụng có đang bị hook hay không. Sự cố này có thể đã được khắc phục trong các phiên bản mới hơn của **ReZygisk CI** và **ZygiskNext**.

**Giải pháp:**
Nếu bạn vẫn gặp phải tình trạng phát hiện này, hãy kiểm tra phiên bản ReZygisk hoặc ZygiskNext của bạn.

## [CHƯA XÁC NHẬN] Kiểm tra trạng thái Bootloader, kiểm tra `syscall` (Mã 5/6)

Trong các phiên bản gần đây của VNeID (lỗi CA-E005), ứng dụng hoạt động bất thường, chẳng hạn như đá người dùng ra ngoài mặc dù đã đăng nhập. Phản hồi phát hiện cũng chậm hơn bình thường.

Hiện tại, chưa rõ BShield đang phát hiện điều gì.

**Giải pháp:**
Một giải pháp tạm thời là thêm tên gói (`com.vnid`) vào tệp `target.txt` của **TrickyStore**.

Mở Tricky Addon WebUI, chọn VNeID, nhấn **Lưu**, và bạn đã hoàn tất!

## [CHƯA XÁC NHẬN] Phát hiện vòng lặp hình ảnh mô-đun KSU/AP (Mã 5)

Trong các báo cáo gần đây từ [@Hzzmonet](t.me/HzzMonet), BShield cũng phát hiện xem quá trình xử lý hình ảnh mô-đun KSU/AP có bị lặp lại hay không. Điều này là do nó sử dụng OverlayFS để hoạt động, dẫn đến việc phát hiện.

Bạn có thể xác minh điều này bằng Native Detector.

Ví dụ: nó có thể báo "KSU/AP image proc loop" hoặc tương tự.

**Giải pháp:**
- Nếu bạn đang sử dụng KernelSU gốc hoặc cũ hơn, vui lòng sử dụng mô-đun TreatWheel của Pedro để ẩn những lỗi này.
- Nếu bạn đang sử dụng KernelSU-Next, vui lòng tắt công tắc "Sử dụng OverlayFS" trong tab cài đặt. Bạn phải sao lưu mô-đun trước khi tắt.

## Phát hiện gỡ lỗi ADB/Chế độ nhà phát triển (Mã 10/11)
Lỗi này xảy ra khi bạn sử dụng Chế độ nhà phát triển hoặc gỡ lỗi ADB trên thiết bị của mình.

**Giải pháp:**
Bạn có thể sử dụng kết hợp các lệnh sau:
- [ReLSPosed](https://github.com/ThePedroo/ReLSPosed)
- [ImNotADeveloper](https://github.com/notyour777/ImNotADeveloper)

để ẩn Chế độ nhà phát triển, chế độ gỡ lỗi ADB.

Hoặc không bật Chế độ nhà phát triển/gỡ lỗi ADB khi bạn không sử dụng.
