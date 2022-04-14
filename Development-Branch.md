# MassOS Development Branch
This article is all about MassOS development and the development branch of MassOS.
# What is the development branch?
In short, the development branch is a branch of the MassOS repository which contains the changes for the next version of MassOS which is currently in development. All new features, updates and contributions go to the development branch. When the next version of MassOS is released, the development branch changes get merged into the main (stable) branch.
# Testing out the development branch
We don't recommend using a development branch build as your daily driver, however we welcome you to test it out on a spare computer or in a virtual machine.

At specific milestones in the development process (the time between the start of development and the release of the next version), we create development builds of MassOS for testing purposes. Those builds are stored on [this Google Drive folder](https://go.thesonicmaster.net/qk9). If you just want to test a development branch build and can't (or don't want to) build MassOS yourself, these are the ones you can use.

**NOTE:** Only the rootfs tarballs are released for development builds. If you want to create an ISO image, you'll need to do it manually using the scripts in the [livecd-installer](https://github.com/MassOS-Linux/livecd-installer) repository.

The development builds provided are probably out of date some of the time, since changes are pushed to the development branch quite frequently, and we are unable to create a new build for each commit. Therefore, we recommend [Building MassOS](https://github.com/MassOS-Linux/MassOS/wiki/Building-MassOS) yourself if you want the latest changes. Since the build system is a big part of MassOS development, by building it yourself, you can also open issues to report any failures which occur, which will help us massively.
# Upgrading to/from a development build
If you are running a stable version of MassOS and want to upgrade it to the development branch, you can use the upgrade utility and pass the development rootfs tarball as an argument, like this:
```
sudo ./massos-upgrade massos-development-rootfs-x86_64.tar.xz
```
If you used the scripts in the [livecd-installer](https://github.com/MassOS-Linux/livecd-installer) repository to create an ISO image, you can update using it by using the **Refresh** functionality of the installer. See [Upgrading MassOS]([livecd-installer](https://github.com/MassOS-Linux/MassOS/wiki/Upgrading-MassOS) for more information.

If you are running a development branch build and want to upgrade to the next stable release, you can follow the [Upgrading MassOS](https://github.com/MassOS-Linux/MassOS/wiki/Upgrading-MassOS) guide as normal. **Be aware that if the current stable version if older than your development build, it will be a downgrade.**
