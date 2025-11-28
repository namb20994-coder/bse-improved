<p align="center">
  <img src="https://git.bnamm.org/namm/BSE-Improved/raw/branch/main/assets/detectionlist.png" width="800" />
</p>

<p align="center">
  <strong>This document lists all the detections observed in BShield for Android. The information is accurate as of November 28th, 2025. If you discover additional detections, feel free to report them in the Issues tab.</strong>
</p>

<p align="Center">
  <a href="docs/DETECTION.md">🇬🇧 English</a> |
  <a href="docs/DETECTION.vi.md">🇻🇳 Tiếng Việt</a>
</p>

> [!CAUTION]
> **This project is for educational purposes only. The intention is to highlight the weaknesses of current security solutions and to encourage the development of better, more reliable alternatives. Use this information responsibly. Do NOT use this for malicious intent. I am not responsible for the actions taken by users of this module or project.**


**Table of contents:**

These are BShield code detections, check them if you encounter error codes in a BShield-powered app:
- Code 1:
  - [Modified app detection](#modified-app-detection-code-1)
- Code 2:
  - [Detected virtual machine](#detected-virtual-machine-privacy-space-code-2-8)
- Code 3:
  - [Package name detection](#package-name-detection-code-3-7)
- Code 4:
  - [Debugging app](#debugging-app-code-4)
- Code 5:
  - [Detected dangerous system properties](#system-properties-code-5)
  - [Found injection/maps detection](#maps-detection-code-5)
  - [Enforcing status](#enforcing-status-code-5)
  - [Custom launcher detected (using launcher module)](#leaks-from-custom-launchers-code-5)
  - [JNI hook detected](#leaks-from-custom-launchers-code-5)
  - [Bootloader unlocked](#unconfirmed-bootloader-check-syscall-check-code-5-6)
  - [KSU/AP image loop detected](#unconfirmed-ksu-ap-module-image-loop-detection-code-5)
- Code 6:
  - [Bootloader unlocked](#unconfirmed-bootloader-check-syscall-check-code-5-6)
- Code 7:
  - [Detected suspicious app](#package-name-detection-code-3-7)
- Code 8:
  - [Detected privacy space/app cloning](#detected-virtual-machine-privacy-space-code-2-8)
- Code 10:
  - [ADB debugging is enabled](#adb-debugging-developer-mode-detection-code-10-11)
- Code 11: 
  - [Developer Mode is enabled](#adb-debugging-developer-mode-detection-code-10-11)


## Modified app detection (Code 1)

This error occurs when you install unsigned app or modified app.

**Solution:** Remove the modified, unsigned app from your system and install from Google Play.

## Detected virtual machine/privacy space (Code 2/8)

This error occurs when you install the app in the virtual machine/privacy space.

**Solution:** Don't install the app in the virtual machine/privacy space.

## Package name detection (Code 3/7)

Another classic detection used by many applications, BShield checks the installed app list to identify apps commonly associated with root access.

Below is the list of apps that BShield currently detects (there may be more; these are only the ones confirmed through testing. Feel free to request updates in the Issues tab):

```txt
com.rifsxd.ksunext
me.bmax.apatch
me.weishu.kernelsu
com.topjohnwu.magisk
com.drdisagree.iconify
(and more, maybe LSPosed module)
```

**Solution:**
You can use a combination such as:

- [ReLSPosed](https://github.com/ThePedroo/ReLSPosed)
- [HMA-OSS](https://github.com/frknkrc44/HMA-OSS)

to hide these apps.

## Debugging app (Code 4)
This error only occurs when using Google’s debug tools. It won’t appear in the production version of the app. If you encounter it, please contact the app developers.

## System properties (Code 5)

BShield also detects certain Android system properties. Some known examples include:

- `init.svc.adb_root`
- `service.adb.root`

**Solution:**  
These properties can be hidden easily by overriding them, for example:

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

**Note:** These properties will reset on reboot.

## Maps detection (Code 5)

BShield can also detect whether the memory maps contain traces of **LineageOS** or injection-related entries (such as Kernel Injection, just found recently).  

You can verify this using the **Native Detector** tool ([download](https://dl.reveny.me/)). 

For example, it may report "Injection Detection" or "LineageOS Detected (14)".  
Alternatively, you can check manually with:

```sh
cat /proc/self/maps | grep "framework-res.jar"
cat /proc/self/maps | grep "lineage"
```

**Bypassing maps detection:**

Hiding these entries is difficult. To avoid LineageOS traces, you may need to modify your AOSP/Pixel-based custom ROM or kernel.

**Here is some solution:**
- If your kernel supports KernelSU + SuSFS (with SUS_MAP enabled), you can add the leaked map paths to the SuSFS map list. 
- If you're using font module, it may also leak map entries. Remove it or add its paths to SUS_MAP as mentioned above.
- You can also try Pedro's TreatWheel module to hide maps, but its effectiveness is limited and it requires a ReZygisk build to operate.
- If you’re using a custom kernel and encountering “injection detection” in Native Detector, please switch to a different kernel as soon as possible. (Alternatively, you can rebuild your kernel if you have the skills.)

**A note about /system/framework/framework-res.apk maps detection. (Kernel injection)**

<img src="https://git.bnamm.org/namm/BSE-Improved/raw/branch/main/assets/photo_2025-11-24_16-50-21.jpg" width="200" align="right">

You may notice that in the <b>Native Detector</b> tool, it shows <b>Found Injection</b>, and the results look something like the image on the right.


This happens because your custom kernel likely contains the LineageOS file-hiding patch in task_mmu.c. See: [reference commit (MoonWake@bea4fe4)](https://github.com/RainyXeon/moonwake_kernel_xiaomi_ruby/commit/bea4fe4ecfa41edb52f26ce9254a16643dda57ea).

The purpose of this LineageOS file-hiding patch is to replace real LineageOS file paths with `framework-res.apk`.

Basically, if a file mapped by a VMA contains lineage in its filename, then `/proc/<pid>/map_files/<start>-<end>` will point to `framework-res.apk` instead of the actual file. This prevents tools like MagiskDetector, root-checkers, app integrity scanners, MDM systems, etc., from detecting LineageOS files in memory maps.

The main idea for this LineageOS file hiding commit is to replace real file paths from LineageOS with `framework-res.apk`. Basically, if the file mapped by a VMA contains `lineage` in its filename, Then `/proc/<pid>/map_files/<start>-<end>` will point to `framework-res.apk`, not the real file. Tools like MagiskDetector, root-checkers, app integrity scanners, MDM systems, etc. cannot see LineageOS files in memory maps.

However, this hiding mechanism is outdated and unintentionally triggers **Found Injection** in **Native Detector**, because the fake VMA header is still detectable. This happens because the patch only replaces the file path, not the entire VMA metadata before that path (which is likely why BShield is able to detect it, in my opinion).

If you are a custom kernel developer, you can revert the commit that contains the LineageOS file-hiding code mentioned above. If you are a user, there is nothing you can do unless you replace the kernel or ask the developer to do so.

## Enforcing status (Code 5)

This is a common detection used by many applications. It is strongly recommended **not** to use a custom ROM with **permissive SELinux**, as it is considered insecure by modern standards.

If your ROM is running with **permissive SELinux**, certain attacks may be possible. BShield requires **SELinux** to be set to **Enforced** to function properly.

**Solution:**
- Set SELinux to **Enforcing**
```sh
setenforce 1
```
- Use a kernel or ROM with **Enforcing SELinux**

## Leaks from custom launchers (Code 5)

BShield can detect many custom launcher modules, possibly through mounts, memory maps, or other indicators. 

**Solution:**  
The simplest approach is to remove custom launchers and use the default system launcher. Alternatively, using standard app launchers typically does not trigger detection.

## [UNCONFIRMED] JNI hook detection (Code 5)

In some releases of VNeID, BShield was able to detect if the app was being hooked. This issue may have been resolved in newer versions of **ReZygisk CI** and **ZygiskNext**.

**Solution:**  
If you are still experiencing this detection, check your ReZygisk or ZygiskNext version.

## [UNCONFIRMED] Bootloader check, `syscall` check (Code 5/6)

In recent versions of VNeID (CA-E005 error), the app behaves strangely, such as kicking the user out after already logging in. The detection response also appears slower than usual.

It is currently unclear what BShield is detecting here. 

**Solution:**  
A temporary workaround is to add the package name (`com.vnid`) to the **TrickyStore** `target.txt` file.  
Open the Tricky Addon WebUI, select VNeID, press **Save**, and you’re done!

## [UNCONFIRMED] KSU/AP module image loop detection (Code 5)

In the recent reports from [@Hzzmonet](t.me/HzzMonet), BShield also detect if the KSU/AP module image proc loop. This because in older KSU/AP, it use OverlayFS to operate, which cause detection.

You can verify this using Native Detector.

For example, it may report "KSU/AP loop" or something similar like that.

**Solution:**
- If you're in original or older KernelSU, please use Pedro's TreatWheel module to hide those.
- If you're in KernelSU-Next, please disable the Use OverlayFS switch in settings tab. You have to backup your module before operate.

## ADB debugging/Developer Mode detection (Code 10/11)
This error occurs when you use Developer Mode or ADB debugging in your device.

**Solutions:**
You can use a combination such as:
- [ReLSPosed](https://github.com/ThePedroo/ReLSPosed)
- [ImNotADeveloper](https://github.com/notyour777/ImNotADeveloper)

to hide Developer Mode, ADB debug mode.

Or don't enable Developer Mode/ADB debugging when you don't use it.
