# This markdown is for noting down the learning materials from tryhackme [mbrandgptanalysis](https://tryhackme.com/r/room/mbrandgptanalysis) room.

## Boot Process

The boot process wakes up the whole system. It starts by initializing the system's hardware components, loading the opreating system into memory, and finally allowing the user to interact with the system.

The overall boot process of a systemn has multiple steps

Step 1. Power-on the system
Step 2. Post check
Step 3. Loacte bootable device
Step 4. Read MBR/GPT
Step 5. Load bootloader
Step 6. Load OS

BIOS (Basic Input/Output System) and UEFI (Unified Extensible Firmware Interface) are responsible for verifying whether all the hardware components work properly. A system can either use BIOS or UEFI firmware. The difference between them lies in their capabilities and features.

* **BIOS** has been used for decades and is still used in some hardware. It runs in the basic 16-bit mode and supports only up to 2 terabytes of disks. The most important thing to note is that BIOS supports the MBR partitioning scheme.
* **UEFI** came as a replacement for BIOS, offering 32-bit and 64-bit modes with up to 9 zettabytes of disks. UEFI offers a secure boot feature to ensure integrity during the system boot process. It also offers redundancy, allowing us to recover from the baqckup even if the boot code is corrupted. UEFI used a GPT partitioning scheme, unlike the MBR partitioning scheme used by BIOS.

## MBR partitioning scheme

The Master Boot Record (MBR) takes up 512 bytes of space at the very first sector of the disk. Now, we know that it starts from the very first sector. We can easily analyze the MBR code by starting from the first line, but how do we know where this MBR code ends? The answer to this question is straitghtfor ward. Every two digits coupled in hexadecimal represents 1 byte, and once the first 512 bytes of the disk completes, the MBR has been ended. So, in the hecadecimal editor we are using, 16 bytes are present in each row, meaning that first 32 rows of the disk would be the whole MBR. Another way to spot the end is by looking at the MBR signature. The MBR signature is represented by 55 AA, which marks the end of the MBR code.

