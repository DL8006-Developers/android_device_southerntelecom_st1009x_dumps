# SmartTab ST1009X Partition Dumps

Partition dumps from the SmartTab ST1009X (SF_3GR) tablet by Southern Telecom.

## Device Information

| Property | Value |
|----------|-------|
| Brand | SmartTab / Southern Telecom |
| Model | ST1009X |
| Codename | SF_3GR |
| Also known as | Polaroid Q1010 |
| Architecture | Intel x86 (Atom) |
| Platform | Intel Sofia |
| Android Version | 6.0.1 Marshmallow |
| Kernel | 3.14.x |
| Security Patch | June 01, 2016 |

## Build Information

| Property | Value |
|----------|-------|
| Build ID | MMB29M |
| Build Display | ST1009_20160629 |
| Build Number | 01.04.009.106.01 |
| FOTA Version | 1.0.2 |
| Build Date | Thu Dec 1 09:30:44 CST 2016 |
| Build User | liangjp |
| Build Host | liangjp-OptiPlex-9020 |

## Partition Layout

| Partition | Block Device | Description |
|-----------|--------------|-------------|
| DeviceID| mmcblk0p4 | Device-Specific Identifiers (not included) |
| boot | mmcblk0p9 | Boot image (also /dev/block/by-name/ImcPartID071) |
| system | mmcblk0p14 | System partition |
| userdata | mmcblk0p17 | User data (not included) |

## eMMC Boot Partitions

The eMMC boot partitions (`mmcblk0boot0` and `mmcblk0boot1`) contain the Intel Sofia PSI (Primary Secure Image) bootloader.

### mmcblk0boot0 Structure

| Offset | Size | Content | Entropy |
|--------|------|---------|---------|
| 0x00000 | 128 KB | PSI Flash bootloader code | ~5.8 bits/byte |
| 0x20000 | 128 KB | Configuration/state data | ~1.0 bits/byte |
| 0x40000 | 3.75 MB | Empty (0xFF padding) | ~0.001 bits/byte |

### mmcblk0boot1 Structure

| Offset | Size | Content |
|--------|------|---------|
| 0x00000 | 128 KB | PSI Flash bootloader code (identical to boot0) |
| 0x20000 | 3.875 MB | Empty (zeroed) |

### Bootloader Details

- **Magic**: `liHU` at offset 0x04, `CJKT` at offset 0x3C
- **Features**:
  - PSI Flash loader with SLB (Secondary Loader Binary) support
  - M Key secure boot validation
  - Key certificate chain validation
  - IMC (Intel Mobile Communications) chipset detection
  - USB bootcore module
  - Fastboot bootloader update capability (`fb_update_bl`)

### Notes

- `mmcblk0boot1` appears to be a fallback/recovery copy containing only the core PSI code
- `mmcblk0boot0` contains the active bootloader with configuration data
- The two partitions share identical code in the first 128 KB

## Firmware Revisions

The ST1009X has multiple firmware revisions in the wild. The "X" is a retail SKU designation and does not appear in `build.prop` (both report as `ro.product.model=ST1009`).

| Property | This Dump (Older) | FOTA 3.0.0 (Newer) |
|----------|-------------------|---------------------|
| Build Display | ST1009_20160629 | ST1009_20161227 |
| Build Number | 01.04.009.106.01 | 01.04.009.123.01 |
| FOTA Version | 1.0.2 | 3.0.0 |
| Build Date | Dec 1, 2016 | Dec 27, 2016 |

Boot images are **not interchangeable** between firmware revisions despite sharing the same SPL and base platform.

### Newer Firmware (FOTA 3.0.0)

A dump of the newer FOTA 3.0.0 firmware is available on Archive.org:

https://archive.org/download/southern-telecom-smartab-st1009x-firmware/system.img

If your device shipped with or updated to FOTA 3.0.0, use that dump for recovery instead of this one.

## Notes

- Bootloader unlock uses `fastboot flashing unlock` (unusual for Marshmallow)
- Southern Telecom no longer provides firmware or kernel source
- Vulnerable to Dirty COW (CVE-2016-5195) due to old SPL
- Zygisk does not function properly on this device
- Xposed v87-sdk23 from 4PDA works (use with caution, keep ADB access ready)

## Rooting

See the [XDA thread](https://xdaforums.com/t/guide-rooting-smarttab-st1009x.4799011/) for rooting instructions.

## Why This Exists

Southern Telecom has stated they no longer have copies of the firmware. These dumps preserve the stock partitions for recovery and development purposes.

## License

These dumps are provided for preservation and development purposes only. All original firmware remains property of their respective owners.

## Reassembly

Some partitions are split due to GitHub file size limits. To reassemble:

```bash
# Reassemble mmcblk0p16.img
cat mmcblk0p16.img.part_* > mmcblk0p16.img
```

Verify integrity after reassembly with sha256sum against the checksums provided.

## Excluded Partitions

The following partitions are excluded from this dump for privacy reasons:

| Partition | Reason |
|-----------|--------|
| mmcblk0p4 | DeviceID partition - contains unique device serial numbers |
| mmcblk0p17 | User data partition |
