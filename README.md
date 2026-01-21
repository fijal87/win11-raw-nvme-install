# ⚔️ GLOVARY RAW / GAMING PROTOCOL (VMD OFF)

**Clean Windows 11 Installation on NVMe (UEFI, no RAID, CLI/DiskPart)**  
**Status:** Production Ready  
**Scope:** Desktop PCs and laptops with NVMe SSD

---

## 🎯 PURPOSE

This document describes a **deterministic, clean Windows 11 installation procedure**
focused on:

- native NVMe operation (no VMD / no RST / no RAID),
- full control over disk partitioning,
- removal of OEM recovery and vendor bloat,
- predictable Windows updates and boot behavior,
- maximum compatibility and minimal complexity.

This is **not** a repair guide.  
This is a **fresh install protocol**.

---

## 👤 WHO THIS IS FOR

### ✅ THIS GUIDE IS FOR:
- gamers,
- power users,
- enthusiasts,
- people installing Windows from scratch,
- systems with a **single NVMe drive**,
- users who prefer reinstalling over repairing.

### ❌ THIS GUIDE IS NOT FOR:
- corporate environments,
- RAID / Intel VMD setups,
- users dependent on **“Reset this PC”**,
- OEM factory recovery workflows,
- managed BitLocker deployments.

---

## 📋 REQUIREMENTS

- USB flash drive (8–32 GB)
- Windows 11 ISO (Media Creation Tool recommended)
- PC or laptop with NVMe SSD
- UEFI firmware

**No storage drivers required.**

---

## ⚙️ BIOS / UEFI CONFIGURATION (BEFORE INSTALL)

Enter BIOS/UEFI (usually `DEL` or `F2`).

Set the following:

- **Boot Mode:** UEFI
- **CSM / Legacy:** Disabled
- **Intel VMD / RST / RAID:** Disabled
- **Secure Boot:** Disabled (temporary)

Save changes and reboot.

---

## 🖥️ WINDOWS INSTALLATION (CLI METHOD)

### 1. Boot Installer
- Boot from USB
- Language screen → press **Shift + F10**

### 2. Disk Partitioning (DiskPart)

```batch
diskpart
select disk 0
clean
convert gpt

create partition efi size=260
format quick fs=fat32 label="System"
assign letter=S

create partition msr size=16

create partition primary
format quick fs=ntfs label="Windows"
assign letter=C

exit
exit
````
Note:
This DiskPart sequence creates the complete required UEFI/GPT layout: EFI + MSR + Primary.
No additional partitions are required for Windows 11 operation.

### 3. GUI Installation

* Install now
* I don’t have a product key
* **Custom: Install Windows only**
* Select **Windows** partition
* Next

---

## 🚀 FIRST BOOT (OOBE)

* Choose region and keyboard
* Create local or Microsoft account
* Disable optional telemetry settings

---

## ⚙️ BIOS CONFIGURATION (AFTER INSTALL)

Re-enter BIOS/UEFI:

* **Secure Boot:** Enabled
  (Enroll default keys if prompted)
* **Boot Option #1:** Windows Boot Manager
* **VMD / RAID:** Remain Disabled

Save and exit.

---

## 🛠️ POST-INSTALL STEPS

* Run Windows Update
* Check Device Manager
* Install GPU drivers (NVIDIA / AMD / Intel)
* Keep default **Microsoft NVMe driver**
  (vendor NVMe driver optional)

---

## ✅ VERIFICATION

```cmd
diskpart
select disk 0
list partition
exit
```

Expected:

* EFI
* MSR
* Primary

```cmd
reagentc /info
```

Expected:

* **Windows RE: Disabled**

---

## ⚠️ IMPORTANT NOTES

* No recovery partition is used
* OEM factory restore is intentionally removed
* System recovery should be done via **reinstall or full disk image backup**

This design reduces update issues and avoids hidden OEM breakage.

---

## ☕ SUPPORT

If this guide saved you time or solved your install issues:

[https://buycoffee.to/fijalpl](https://buycoffee.to/fijalpl)

---

