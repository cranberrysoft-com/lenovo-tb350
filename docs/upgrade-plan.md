# TB350 open-source Android upgrade plan

Status: initial research plan, 16 August 2026. This is not yet an installation guide.

## Objective

Replace the Lenovo Android userspace with a community-built, open-source-oriented Android system while preserving a tested path back to the exact stock firmware.

The practical first target is a vanilla LineageOS 22.2 / Android 15 GSI. A LineageOS 23.2 / Android 16 GSI is a later experiment, not the initial permanent installation.

## Verified baseline

The TB350FU and TB350XU are Lenovo Tab P11 (2nd Gen) variants built on the MediaTek Helio G99 platform. They launched with Android 12L. Lenovo's documented upgrade lifecycle ends at Android 14, and its published security-maintenance dates ended in 2025.

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

Android 16 testing follows a successful Android 15 baseline. If networking fails, first prove whether the failure is the MediaTek BPF problem on the TB350 kernel. The public MediaTek BPF patcher documents testing only on 4.14 and 4.19 kernels; that does not establish compatibility with the TB350.

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
- [Lenovo Android Upgrade Matrix](https://lenovomobilesupport.lenovo.com/us/en/products/phones/k-series/k12/solutions/ht501098-android-upgrade-matrix)
- [Android Generic System Image requirements](https://developer.android.com/topic/generic-system-image)
- [Android Dynamic System Updates](https://source.android.com/docs/core/ota/dynamic-system-updates)
- [TrebleDroid GSI list](https://github.com/TrebleDroid/treble_experimentations/wiki/Generic-System-Image-%28GSI%29-list)
- [Unofficial LineageOS 23.2 Treble GSI releases](https://github.com/Doze-off/lineage_treble/releases)
- [MediaTek BPF kernel patcher](https://github.com/R0rt1z2/mtk-bpf-patcher)
