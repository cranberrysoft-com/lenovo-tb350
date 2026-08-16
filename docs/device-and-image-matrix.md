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
