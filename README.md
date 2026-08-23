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

## Partition Layout

| Partition | Block Device | Description |
|-----------|--------------|-------------|
| DeviceID| mmcblk0p4 | Device-Specific Identifiers (not included) |
| boot | mmcblk0p9 | Boot image (also /dev/block/by-name/ImcPartID071) |
| system | mmcblk0p14 | System partition |
| userdata | mmcblk0p17 | User data (not included) |

## Notes

- Bootloader unlock uses `fastboot flashing unlock` (unusual for Marshmallow)
- Southern Telecom no longer provides firmware or kernel source
- Vulnerable to Dirty COW (CVE-2016-5195) due to old SPL
- Zygisk does not function properly on this device
- Xposed causes unrecoverable bootloops

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
