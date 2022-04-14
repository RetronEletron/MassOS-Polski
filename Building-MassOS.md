# Building MassOS
This guide shows you how to use the scripts in this repo to compile MassOS from source.

This guide is aimed at **developers** (see below). Most users won't want to compile MassOS themselves. If you aren't a developer and want to install MassOS normally, look at [Installing MassOS](https://github.com/MassOS-Linux/MassOS/wiki/Installing-MassOS).
# VERY IMPORTANT notices for building MassOS
Please read all of these **carefully** before settling on the decision to compile MassOS:
- Building MassOS takes a **very long time**, and the speed will vary massively depending on your CPU. The fastest CPUs will take a few hours to compile MassOS, however the build could take **several days** on slower systems. While we cannot state the time every CPU will take, here is a sample of how long different tested CPUs took to compile MassOS (all are estimations):
  - Intel Core i5-4300U (2.9 GHz, 4 threads): **~13 Hours**
  - Intel Core i5-1035G4 (3.7 GHz, 8 threads): **~7 Hours**
  - Intel Xeon E-2286G (4.9 GHz, 12 threads): **~4 Hours**
- The minimum amount of RAM needed to compile MassOS is based on how many threads your CPU has. As a general guideline, you should have twice as much RAM as CPU threads. For example, if you have a CPU with 4 threads, you should have at least 8GB of RAM. Run `nproc` if you are unsure how many threads your CPU has. Swap space can be used as a RAM alternative, however the build process may be significantly slowed down if the machine has to swap.
- The second part of the build (Stage 2) must be run with **root access**, due to the fact that it makes use a chroot environment. Userspace alternatives to chroot such as PRoot are not currently supported.
- MassOS must be built from an existing installation of a GNU/Linux system. Any modern distribution with basic build tools should be supported, however we recommend building from MassOS itself for the cleanest possible build. Although the build system does involve bootstrapping for robustness, building from MassOS ensures there is absolutely no possible external influence from another distribution.

# How to build MassOS
Now you've read the above notices and are fully aware of what you're getting yourself into, you can proceed with building MassOS.
### Clone the repo
In order to build MassOS, you first have to clone the GitHub repository locally. To do so, run the following commands:
```
git clone https://github.com/TheSonicMaster/MassOS.git
cd MassOS
```
This will checkout the stable (currently released) version of MassOS. This is recommended for first time builders, however if you are a developer, you may wish to build from the **development branch**, which contains unreleased changes which are currently in development for the next version of MassOS. If (and **only if**) you want to compile a development build, checkout the development branch with the following command:
```
git checkout development
```
**NOTE:** The development branch is UNSTABLE and not guaranteed to successfully compile at all. If you're a first time builder, we recommend building the stable version first. If you do run into a build issue with the development branch, please [open an issue](https://github.com/MassOS-Linux/MassOS/issues) to report it.
### Retrieve the sources:
Before you can begin the compiling process of MassOS, you must first download the source tarballs of all the software which make up the MassOS system. Fortunately, this command will automatically download them all for you:
```
./retrieve-sources.sh
```
**NOTE:** Ensure everything downloaded successfully. If the final message says something along the lines of "Some downloads may have failed", then you MUST NOT continue until all the downloads were successful. You can re-run the retrieve-sources.sh script to retry the download of the missing files (it won't re-download anything already successfully downloaded). Sometimes a download fails because the server hosting the source tarball is down. In this case, you can often edit the offending URL in the `source-urls` file to a mirror which also hosts the same file.
### Build the temporary bootstrap system:
The first part of the build process involves building a set of temporary bootstrap tools. This has to be done on the host system since in order to compile a compiler, you need a compiler. The temporary tools build in this stage can then be used to build the full MassOS system. Build the temporary tools with the following command:
```
./stage1.sh
```
### Build the full MassOS system:
Now the temporary tools have successfully been built, you can proceed with building the full MassOS system. This must be run as root since it makes use of a chroot environment:
```
sudo ./stage2.sh
```
### Finishing up:
When the MassOS system is completely build and finished, an output tarball labelled `massos-<VERSION>-rootfs-x86_64.tar.xz` will be created. **It is highly recommended that you change ownership of the final output tarball back to the original user. You can do this with the following command:**
```
sudo chown $(whoami):$(whoami) massos-$(cat utils/massos-release)-rootfs-x86_64.tar.xz
```
If you want to create a Live CD ISO image for your rootfs tarball, see the [livecd-installer](https://github.com/MassOS-Linux/livecd-installer) repository, which contains instructions on how to do that.
