This document describes how to build a kernel and test it with OpenEmbedded(OE) Linux root filesystem. Target board is HiKey960.

Authored by: Guodong Xu <guodong.xu@linaro.org>

### Step 1: Know the Kernel source repository, and Make
Kernel source code for HiKey960 (and other types of HiKey boards) is kept in this repository:
https://github.com/96boards-hikey/linux/

There are several branches. Here I explain hikey960-upstream-rebase. https://github.com/96boards-hikey/linux/commits/hikey960-upstream-rebase

hikey960-upstream-rebase is tracking the latest mainline -rc releases. As of this writing, it is rebased to v4.14-rc5.

Linaro Landing Team uses this branch to prepare patchsets for upstreaming. Theoretically, all out-of-tree patches in this branch should be upstreamed to Linus' kernel mainline.

Note:
* Yes, I do forced-update to hikey960-upstream-rebase
* For history versions, I tagged them. Please find in https://github.com/96boards-hikey/linux/releases, with release names in format: working-hikey960-upstream-rebase-[version]-[date]. Eg. working-hikey960-upstream-rebase-v4.14-rc5-2017-10-18

To make a kernel for OE,
```bash
make defconfig
make Image
make hisilicon/hi3660-hikey960.dtb
```

### Step 2: Download OpenEmbedded Snapshot Builds
Linaro makes this OpenEmbedded build: http://snapshots.linaro.org/reference-platform/embedded/morty/hikey960/

You can download the latest from this link.

Eg. If you choose build #99, you can download these files from rpb folder:
boot-0.0+AUTOINC+ba45819943-ea12986b87-r0-hikey960-20171013132920-99.uefi.img
rpb-console-image-lava-hikey960-20171013132920-99.rootfs.img.gz

Note:
* Snapshot builds are not thoroughly tested. So, expect errors sometimes. In that case, choose another build #.
* By default, this snapshot build uses hikey960-upstream-rebase branch kernel. But it's not guranteed that it will automatically re-generate a new build when hikey960-upstream-rebase is forced-updated.

### Step 3: Download ATF/UEFI bootloader
Linaro makes this ATF/UEFI build: https://builds.96boards.org/snapshots/reference-platform/components/uefi-staging/

Find version number -> hikey960 -> release, then download it.

### Step 4: Flash ATF/UEFI to HiKey960
Here is how to flash ATF/UEFI in recovery mode. https://github.com/96boards-hikey/tools-images-hikey960/blob/master/recovery-flash-uefi.sh

Download it. Modify it so it suits your environment. Then, run it. 
```bash
recovery-flash-uefi.sh
```

Note:
* If you don't understand what recovery mode means, read https://github.com/96boards-hikey/tools-images-hikey960/blob/master/README.md


### Step 5: Flash boot and root filesystem
Follow the examples in Step 2, what you need to do is:

```bash
gunzip rpb-console-image-lava-hikey960-20171013132920-99.rootfs.img.gz
fastboot flash boot boot-0.0+AUTOINC+ba45819943-ea12986b87-r0-hikey960-20171013132920-99.uefi.img
fastboot flash system rpb-console-image-lava-hikey960-20171013132920-99.rootfs.img
```

### Step 6: Reboot
Set the board into normal mode. Then reboot.

You can login using 'linaro' account. And you can switch to super user with `su`.
