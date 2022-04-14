# MassOS and Free Software
This article explains what free software is, how free software is related to MassOS, and the potential issues with free software and MassOS.

**NOTE:** This is not legal advice.

# What is free software?
Simply put, free software is software which guarantees the freedom of its users. It ensures that users have the four essential freedoms, which are:

- The freedom to use the software for any purpose.
- The freedom to study the software's source code, and change it so it works the way you desire.
- The freedom to redistribute exact copies of the software.
- The freedom to redistribute your modified copies of the software.

Free software does not mean the software is free of charge. Although the majority of free software programs are also free of charge, the term "free software" explicitly refers to the users' freedom, as stated above. A program which is free of charge may not necessarily be free software. A program is considered free software if it guarantees all of the freedoms above. Programs which don't are considered non-free or proprietary.

## What about open source?
Physically, free software and open source are (almost always) referring to the same software, however they have different ethical outlooks. Open source agrees with the practical benefits of free software, such as enabling users to fix bugs without relying on the developer, however it ignores the ethical issues surrounding proprietary software.

# Free Software in MassOS
MassOS is a collection of many different pieces of software, which all come together to form a working operating system.

All the scripts and utilities found in the MassOS repositories are licensed under the GNU General Public License version 3 or later (GPLv3+).

We try to ensure that the third party software which makes up the MassOS system is free software. One way we do this is by enforcing a requirement that any software included by default in MassOS must be either dedicated to the public domain, or protected by a GPL-compatible license. At our level of commitment, we try to reject programs licensed under GPL-incompatible licenses (with a very few number of exceptions listed in the section below). Two notorious examples are cdrtools, a set of utilities for working with CD and ISO images, and FDK-AAC, a library for working with the AAC audio format. For these examples, we have substituted both of them for GPL-compatible alternatives - cdrkit and FAAD2.

If you happen to notice any other GPL-incompatible software included by default in MassOS (which **isn't** listed under the "Exceptions" section below), please let us know by [opening an issue](https://github.com/MassOS-Linux/MassOS/issues). We will try to rectify it as soon as possible, ideally by finding a GPL-compatible alternative, or by removing it entirely in the unlikely event that we have to.

# Exceptions
Although we try to ensure the MassOS system remains free, there are a very few number of exceptions to the free software requirement, which are listed here.

**NOTE:** Irrespective of these exceptions, we do **not** desire or plan to request listing/endorsement by the GNU project.

## Firmware
Many pieces of hardware, particularly modern wireless, sound and graphics cards, require proprietary firmware "blobs" in order to function. Without these "blobs", it will not be possible to operate these devices, which could severely hinder the use of the computer in some cases. To resolve this, we provide two Live ISO images for MassOS. The default one contains the non-free firmware required to operate most awkward devices, and the alternative one (with the label "-nofirmware" appended) contains no proprietary firmware. If you know your hardware doesn't require the firmware, and/or you refuse to use it, you can grab the "-nofirmware" ISO. However, please only report issues regarding hardware if you are using the image with firmware.

The core system (produced by the MassOS build system) contains no non-free firmware (this includes the rootfs tarball). Firmware is only added at the Live ISO creation stage.

## Mozilla Firefox trademark issue
**NOTE:** This issue may or may not also apply to Mozilla Thunderbird. We are currently unaware of whether it does or not.

The Firefox web browser, developed by Mozilla Foundation, is free software, released under version 2 of the Mozilla Public License (MPL). Although the software is free, Mozilla enforces a separate trademark license, which outlines conditions on how the branding of "Firefox" and its logo can/cannot be used with regards to the four freedoms defined by the Free Software Foundation.

The license states that binaries provided by Mozilla (which are the ones used in MassOS), as well as any other binaries produced from the unmodified Firefox sources, can be freely redistributed under the name "Firefox", and use the official Firefox logo. However, the license requires that binaries produced from modified source code must contain a different name and logo.

Although this theoretically makes it awkward to exercise the fourth freedom (the freedom to redistribute modified copies), we don't consider the issue major enough to justify excluding Mozilla Firefox from MassOS, despite the fact that we have strong requirements surrounding freedom regarding the software which can be included in MassOS.

The small minority of programmers who want to modify Firefox will most likely be OK with having the name of their end product changed (in fact, they'd probably desire a clear distinction from the official Mozilla Firefox). In practice, the freedom to redistribute Firefox is not taken away, you just have to abide by a minor condition if you wish to distribute your modified binaries. Furthermore, the build system for Firefox won't even produce binaries with the official branding unless you explicitly tell it to by passing the flag `--enable-official-branding` to the build system. If you leave this flag excluded like it would be by default, the build system will produce a binary with a generic name and logo, which is fully redistributable. Therefore, people who modify Firefox don't require any extra effort to ensure they comply with the condition, assuming they don't pass the aforementioned flag.

For these reasons, we don't consider Mozilla Firefox incompatible with the requirements for software included in MassOS by default.

## Proprietary software in Flatpak/Flathub repository
FLathub, the default repository for Flatpak, does include some non-free programs which are installable through the `flatpak` command-line utility and display in the graphical software center, however this software is clearly marked as such, and none of it is installed by default, nor maintained by MassOS developers, and therefore we don't feel the need to remove it.

## Blobs in the Linux Kernel
Apparently, modern versions of the Linux Kernel (i.e. the versions used in MassOS releases), contain a very small amount of non-free firmware blobs as described above, which are separate to the firmware blobs included in the default MassOS Live ISO image. To us, this is a minor issue, and if this firmware was not included, hardware depending on it would cease to be operable.

The GNU project have created a fork of Linux, called Linux-Libre, which has this firmware removed. At the time of writing, it is not in our plans to use this for the kernel in MassOS instead of the standard Linux kernel, however we may consider it in the future, if we are able to confirm that the firmware included in the standard MassOS ISO works with Linux-Libre as it does with standard Linux, for the people who do have hardware dependent on it and who aren't bothered about using the non-free firmware.

# Final notes
MassOS is closer to freedom than many distributions. Although there are exceptions, which are kept due to what we think is best for the end user, MassOS is still far closer to freedom than the majority of distributions, due to our policies against non-free software in the main system packages and the fact that we do our best to use alternatives to non-free software where possible, as mentioned above.

If you want a distro containing nothing but 100% free software, with no mention of or association with any sort of proprietary software, try out [Dragora](http://dragora.org/en/index.html) or [Trisquel](https://trisquel.info). But don't blame us if you (for example) have an Intel wireless card and therefore have no working Wi-Fi :).
