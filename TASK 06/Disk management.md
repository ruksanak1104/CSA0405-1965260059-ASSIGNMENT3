
# Disk Management, RAID and Swap-Space Management

## 1. Aim

To study disk formatting, partitioning, booting, bad-block handling, RAID, and swap-space management.

---

## 2. Disk Formatting

Disk formatting involves preparing a storage device for storing data.

### Types of Formatting

1. **Low-Level Formatting:** Divides a disk into sectors and tracks.
2. **Partitioning:** Divides the disk into logical sections.
3. **Logical Formatting:** Creates a file system such as ext4.

---

## 3. Boot Block and Boot Process

The boot block contains the code required to start the operating system.

### Boot Process

1. Computer starts.
2. BIOS/UEFI performs hardware initialization.
3. Bootloader is loaded.
4. Bootloader loads the Linux kernel.
5. Kernel initializes the system.
6. System services start.

---

## 4. Bad-Block Handling

Bad blocks are damaged disk sectors that cannot store data reliably.

### Handling Methods

- Bad-block detection.
- Sector remapping.
- Spare sectors.
- Error correction.

---

## 5. RAID Comparison

RAID (Redundant Array of Independent Disks) combines multiple disks to improve performance, storage capacity, or reliability.

| RAID | Minimum Disks | Fault Tolerance | Usable Capacity with 6 × 4 TB | Performance |
|---|---:|---|---:|---|
| RAID 0 | 2 | None | 24 TB | High |
| RAID 1 | 2 | Mirroring | Depends on mirror layout | Good |
| RAID 5 | 3 | 1 disk failure | 20 TB | Good reads |
| RAID 10 | 4 | At least 1 disk, depending on mirror failures | 12 TB | High |

---

## 6. Recommended RAID

RAID 10 is recommended for the hospital records server because:

- Provides high read and write performance.
- Uses mirroring for redundancy.
- Can survive disk failures if both disks in the same mirror pair do not fail.
- Supports faster recovery than parity-based RAID in many situations.

**RAID is not a substitute for backups.**

---

## 7. Swap-Space Management

Swap space is disk space used as an extension of RAM.

### Advantages

- Helps when physical RAM is insufficient.
- Can be configured as a swap partition or swap file.
- A separate swap partition offers dedicated space.
- A swap file is easier to resize and manage.

### Recommendation

Use a dedicated swap area or a managed swap file based on the server's workload and memory requirements. Place it on reliable storage and monitor swap usage.

---

## 8. Linux Commands

### Display Disks and Partitions

```bash
lsblk
```

### Display Partition Information

```bash
sudo fdisk -l
```

### Display Swap Information

```bash
swapon --show
```

### Display Memory and Swap Usage

```bash
free -h
```

---

## 9. Capacity Calculation for Six 4 TB Disks

| RAID | Calculation | Usable Capacity |
|---|---|---:|
| RAID 0 | 6 × 4 TB | 24 TB |
| RAID 5 | (6 − 1) × 4 TB | 20 TB |
| RAID 10 | (6 ÷ 2) × 4 TB | 12 TB |

---

## 10. RAID 1 and RAID 5 Reliability

Assume each disk has an independent failure probability of 2% during the chosen period.

### RAID 1 with Two Disks

Data loss occurs if both disks fail.

\[
P = 0.02 \times 0.02
\]

\[
P = 0.0004 = 0.04\%
\]

### RAID 5 with Six Disks

Data loss occurs if two or more disks fail.

\[
P = 1 - [0.98^6 + 6(0.02)(0.98^5)]
\]

\[
P \approx 0.00572
\]

\[
P \approx 0.572\%
\]

These are simplified probability calculations, not real-world failure predictions. They assume independent failures and do not include rebuild risks.

---

## 11. Conclusion

RAID 10 is recommended for the hospital records server because it provides good performance and redundancy.

Regular backups and monitoring are also necessary to protect important records.

