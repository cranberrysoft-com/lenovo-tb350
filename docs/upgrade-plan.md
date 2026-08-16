# TB350 open-source Android upgrade plan

Status: initial research plan, 16 August 2026. This is not yet an installation guide.

## Objective

Replace the Lenovo Android userspace with a community-built, open-source-oriented Android system while preserving a tested path back to the exact stock firmware.

The practical first target is a vanilla LineageOS 22.2 / Android 15 GSI. A LineageOS 23.2 / Android 16 GSI is a later experiment, not the initial permanent installation.

## Verified baseline

The TB350FU and TB350XU are Lenovo Tab P11 (2nd Gen) variants built on the MediaTek Helio G99 platform. Lenovo's PSREF states that they launched with Android 12L, would receive two major OS upgrades through Android 14 and would receive three years of security upgrades. Lenovo's Upgrade Matrix lists security maintenance ending on 30 October 2025 for the TB350FU and 30 December 2025 for the TB350XU.

No maintained, device-native LineageOS build for the TB350 has been verified. Available LineageOS 22.2 and 23.2 images are generic, unofficial Treble GSIs rather than official support for this tablet.

## What a GSI replaces

A GSI replaces most of the Android system partition, including Lenovo's launcher and system applications. It does not replace all device software. The following remain device-specific:

- bootloader and verified-boot implementation;
- Linux kernel and device tree;
- vendor hardware abstraction layers;
- modem, Wi-Fi, Bluetooth and other firmware;
- proprietary drivers and calibration data.

The achievable result is therefore "Lenovo-free Android userspace," not a completely Lenovo-free firmware stack.

## Proposed stages

### 0. Establish recovery first

- Identify the exact model: TB350FU or TB350XU.
- Obtain the matching stock recovery package from an official Lenovo source.
- Record its filename, build identifier and checksum without committing the package.
- Confirm that Lenovo Software Fix recognizes the tablet.
- Back up user data and any authentication or recovery material kept only on the tablet.

Do not unlock or flash until the [recovery readiness checklist](recovery-readiness.md) is complete.

### 1. Record the stock configuration

Capture the stock build fingerprint, Android version, kernel version, first API level, VNDK version, bootloader state, slot layout and available dynamic partitions. Use Treble Info to independently record the required GSI architecture and whether the device requires a regular VNDK or VNDK-Lite image.

These results will determine the exact candidate image. Do not infer the image type from another Lenovo tablet.

### 2. Unlock the bootloader

Document a TB350-specific unlock procedure and verify it on both variants where they differ. Unlocking is expected to factory-reset the tablet and weaken verified-boot protections.

After unlocking, boot the stock system once and re-enable USB debugging before continuing.

### 3. Test with DSU where supported

Use Dynamic System Updates to boot the selected GSI without replacing the stock system partition, if the TB350 implementation accepts the image. DSU lowers the risk to the installed system but does not make an incompatible image safe.

Validate at minimum:

- Wi-Fi and internet access;
- LTE, mobile data and location on TB350XU;
- Bluetooth pairing and Bluetooth audio;
- speakers, microphones and headphone output;
- front and rear cameras;
- GPS and assisted location;
- suspend, resume and automatic rotation;
- USB data, charging and fast charging;
- keyboard, stylus and external storage;
- encryption, screen lock and reboot stability.

Collect logs for failures rather than applying fixes from a different device.

### 4. Evaluate a permanent GSI installation

Only proceed when recovery is proven and the DSU result is acceptable. The initial permanent experiment should change the minimum necessary set of partitions, preferably the logical system image through `fastbootd` while retaining the stock TB350 boot and vendor components.

Create a separate, reviewed installation document containing exact commands, expected output and rollback points before performing this stage.

### 5. Consider Android 16

Android 16 testing follows a successful Android 15 baseline. Research recorded on 16 August 2026 indicates that the option is viable in principle, though nothing has been proven on a project device:

- Google publishes official Android 16 GSIs, with stable builds through Android 16 QPR2. Google's stated baseline for any GSI is a fully Treble-compliant device that launched with Android 9 (API level 28) or later; the TB350 launched with Android 12L and meets that criterion. Running a GSI newer than the installed OS additionally requires the vendor image to be fully VNDK-compliant, which must be read from the device in stage 1.
- Two community build lines track LineageOS 23 / Android 16 and were still being updated in mid-2026: the TrebleDroid-based `lineage_treble` releases (23.2 QPR2, June 2026, whose release notes describe bundled VNDK support from Android 9 through Android 16) and the `MisterZtr/LineageOS_gsi` releases (23.2, 24 May 2026, published in vanilla and GApps variants with ext4 and EROFS filesystem images). Andy Yan's widely used GSI line offered LineageOS 22 / Android 15 builds but no LineageOS 23 build as of 16 August 2026.
- The MediaTek BPF `arraymap` bug that breaks BPF-based networking under newer Android versions is documented against 4.14 and 4.19 vendor kernels, and the public patcher documents testing only on those versions. A community report places the TB350 stock kernel at `5.10.209-android12`, which if confirmed would make that specific bug unlikely to apply; see the community-reported data in [device-and-image-matrix.md](device-and-image-matrix.md). If networking fails under Android 16, prove the cause from TB350 logs rather than assuming the BPF problem.

Results on other MediaTek devices in 2026 are mixed. A March 2026 XDA report describes a Helio G99 phone (Infinix NOTE 30 Pro) on a `5.10.209-android12` kernel — the same kernel line reported for the TB350 — showing a permanent black screen under a TrebleDroid-based Android 16 GSI, traced by the reporter to a mismatch between the GSI display driver and the vendor device tree, while a Helio G85 phone in the same thread ran an Android 16 GSI with working telephony and Wi-Fi. Those results do not transfer to the TB350 in either direction; each image must be validated on the tablet itself.

Never flash a patched boot image made for the older TB-J616X or for any other model.

## Decision gates

The project should not recommend a permanent installation until all of the following are true:

- the exact GSI type is confirmed from the device;
- stock recovery has been tested or otherwise credibly demonstrated;
- Wi-Fi works reliably during the evaluation;
- the XU variant has a documented LTE result;
- suspend and charging work without serious regressions;
- the rollback procedure has been reviewed by someone other than its author.

## Primary references

- [Lenovo Tab P11 (2nd Gen) specifications](https://psref.lenovo.com/syspool/Sys/PDF/Lenovo_Tablets/Tab_P11_2nd_Gen/Tab_P11_2nd_Gen_Spec.PDF)
- [Lenovo Android Upgrade Matrix](https://support.lenovo.com/us/en/solutions/ht501098)
- [Android Generic System Image requirements](https://developer.android.com/topic/generic-system-image)
- [Google GSI release notes](https://developer.android.com/topic/generic-system-image/releases)
- [Android Dynamic System Updates](https://source.android.com/docs/core/ota/dynamic-system-updates)
- [TrebleDroid GSI list](https://github.com/TrebleDroid/treble_experimentations/wiki/Generic-System-Image-%28GSI%29-list)
- [Unofficial LineageOS 23.2 Treble GSI releases](https://github.com/Doze-off/lineage_treble/releases)
- [Andy Yan's LineageOS GSI builds](https://sourceforge.net/projects/andyyan-gsi/)
- [MisterZtr LineageOS GSI releases](https://github.com/MisterZtr/LineageOS_gsi/releases)
- [MediaTek BPF kernel patcher](https://github.com/R0rt1z2/mtk-bpf-patcher)
- [XDA thread: TB350FU unlock and GSI installation report, 21 November 2024](https://xdaforums.com/t/tb350fu-lenovo-p11-2nd-gen-how-to-unlock-install-gsi-root.4704177/)
- [XDA thread: TB350 Android 16 custom-kernel request, 26 July 2025](https://xdaforums.com/t/closed-looking-for-developer-to-build-custom-kernel-for-android-16-gsi-on-lenovo-tab-p11-2nd-gen-tb350fu-tb350xu.4751935/)
- [XDA thread: Android 16 GSI reports on MediaTek devices, March 2026](https://xdaforums.com/t/shared-eol-treble-gsi-a16-a-b-graphiteos.4780675/page-2)
