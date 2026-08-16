# Lenovo TB350 Open Source Upgrade Project

Community research and documentation for running a more open Android system on the Lenovo Tab P11 (2nd Gen):

- `TB350FU`: Wi-Fi model
- `TB350XU`: LTE model

## Current direction

The first candidate is a vanilla LineageOS 22.2 / Android 15 Generic System Image (GSI). Android 16 GSIs remain an experimental second step until networking and the rest of the hardware have been validated on the TB350 itself.

A GSI can replace Lenovo's Android system and applications, but it continues to use the tablet's Lenovo-provided bootloader, kernel, vendor interface, firmware and proprietary hardware components. This project does not currently provide a fully device-native ROM.

## Documentation

- [Upgrade plan](docs/upgrade-plan.md)
- [Device and image matrix](docs/device-and-image-matrix.md)
- [Recovery readiness checklist](docs/recovery-readiness.md)

## Safety and redistribution

Unlocking the bootloader erases user data. Flashing an incompatible image can make the tablet unbootable. Never flash bootloader, boot, recovery or firmware images made for another Lenovo model.

This repository does not distribute Lenovo firmware or other proprietary binaries. Link to official sources instead and record cryptographic checksums for locally obtained recovery files.
