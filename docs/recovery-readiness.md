# Recovery readiness checklist

Complete this checklist before unlocking the bootloader or changing any partition.

## Device identity

- [ ] Photograph or record the exact model label: TB350FU or TB350XU.
- [ ] Record the region, stock build number and full build fingerprint.
- [ ] Record the tablet serial number privately; never commit it to this repository.
- [ ] Confirm whether the tablet is subject to carrier or organization management.

## Data protection

- [ ] Back up photographs, downloads and application data.
- [ ] Export authenticator recovery codes and other non-synchronised credentials.
- [ ] Remove accounts if required to avoid Factory Reset Protection complications.
- [ ] Accept that bootloader unlocking performs a factory reset.

## Stock recovery material

- [ ] Obtain the exact stock package for the model and region from Lenovo.
- [ ] Record the package filename, source URL, build number, file size and SHA-256 checksum.
- [ ] Do not upload or redistribute proprietary firmware in this repository.
- [ ] Install Lenovo Software Fix on the recovery computer.
- [ ] Confirm the tool detects the tablet before unlocking.
- [ ] Keep a tested USB data cable and a charged computer available.

## Tooling and connectivity

- [ ] Install a current Android SDK Platform-Tools release.
- [ ] Confirm both ADB and fastboot can detect the tablet in their respective modes.
- [ ] Record the commands and expected identifiers without publishing the serial number.
- [ ] Ensure the battery is comfortably above 50 percent before any write operation.

## Rollback plan

- [ ] Document how to reach bootloader, `fastbootd` and stock recovery modes.
- [ ] Identify which operation restores only the system image and which performs a full rescue.
- [ ] Keep exact stock TB350 boot-related images available, but do not flash them pre-emptively.
- [ ] Never relock the bootloader while a modified or unsigned system is installed.
- [ ] Stop after any unexpected error; do not continue a sequence merely because later commands were listed.

Passing this checklist makes experimentation more recoverable, not risk-free.
