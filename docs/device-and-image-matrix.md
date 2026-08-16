# Device and image matrix

## In-scope hardware

| Model | Connectivity | Platform | Project status |
| --- | --- | --- | --- |
| TB350FU | Wi-Fi | MediaTek Helio G99 | Primary test target |
| TB350XU | Wi-Fi and LTE | MediaTek Helio G99 | Primary test target; cellular testing required |

## Explicitly incompatible examples

| Model | Device | Platform | Why it cannot supply TB350 boot images |
| --- | --- | --- | --- |
| TB-J616F/X | Tab P11 Plus | MediaTek Helio G90T | Different SoC, kernel, device tree and partition contents |
| TB336FU/ZU | Tab K11 Gen 2 | MediaTek Dimensity 6300 | Different generation, SoC and firmware architecture |

A similar brand name or the presence of a MediaTek processor does not make boot, LK, preloader, recovery, vendor or modem images interchangeable.

## Candidate system-image classes

| Candidate | Android base | Google components | Initial position |
| --- | --- | --- | --- |
| LineageOS 22.2 vanilla GSI | Android 15 | None bundled | Recommended first evaluation |
| LineageOS 22.2 microG GSI | Android 15 | Open-source compatibility layer | Alternative after vanilla baseline |
| LineageOS 22.2 GApps GSI | Android 15 | Google services bundled | Outside the Google-free goal |
| LineageOS 23.2 vanilla GSI | Android 16 | None bundled | Experimental second stage |
| Google AOSP GSI | Android 16 | Depends on variant | Developer reference, not a daily-use recommendation |

"Vanilla" describes a GSI without bundled Google applications. It does not mean that the image is a device-specific TB350 build.

LineageOS 23 / Android 16 GSIs are maintained by more than one builder. The TrebleDroid-based `lineage_treble` releases (updated June 2026) and the `MisterZtr/LineageOS_gsi` releases (updated 24 May 2026) both publish vanilla and GApps variants; MisterZtr additionally splits each into ext4 and EROFS filesystem images. Only the vanilla variants fit the Google-free goal, and the correct variant for the TB350 has not been established.

## Community-reported TB350 data

The following claims come from XDA forum posts by TB350 owners, linked from the primary references in [upgrade-plan.md](upgrade-plan.md). They are consistent with a GSI being possible on this tablet, but none has been verified on a project device; treat them as leads rather than recorded facts.

| Claim | Source and date |
| --- | --- |
| `TB350FU` uses an A/B slot layout with a dynamic `super` partition | XDA installation thread, 21 November 2024 |
| The bootloader unlocks through `fastboot` with a timed volume-key confirmation and performs a factory reset | XDA installation thread, 21 November 2024 |
| A LineageOS 21 / Android 14 GSI ran acceptably on a `TB350FU` | XDA installation thread, 21 November 2024 |
| The stock kernel is `5.10.209-android12`, a Generic Kernel Image era branch | XDA custom-kernel request, 26 July 2025 |
| An Android 16 GSI has been installed on a TB350, with no published hardware validation | XDA custom-kernel request, 26 July 2025 |

These reports do not shorten the list below; every value must still be read from the tablet in hand.

## Selection data still required

Before attaching a specific download to an installation guide, record the following from a real TB350:

- architecture reported by Treble Info;
- A/B or A-only layout;
- regular VNDK or VNDK-Lite requirement;
- stock VNDK version and first API level;
- dynamic-partition and `fastbootd` behavior;
- available logical-system space;
- Android Verified Boot behavior after unlocking.

Download names and hashes belong in a versioned test record. Do not use an unversioned "latest" link in a reproducible installation procedure.
