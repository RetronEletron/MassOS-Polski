# Contributing
We welcome contributions of all types to MassOS. This document explains some of the ways you can contribute to MassOS.
# Ways you can contribute
- Porting MassOS to other architectures (e.g. i386, ARM).
- Porting large software to MassOS which people might want, such as alternative desktop environments.
- Finding and reporting [issues](https://github.com/MassOS-Linux/MassOS/issues).
- Even better: Fixing issues that people have reported.
- Writing documentation for the MassOS wiki.
- Translating MassOS and the documentation into your native language.

# Guidelines for contributions
Please try to follow these guidelines when contributing to MassOS:
## General guidelines
- Only send modify and send pull requests to the **development branch**. We won't accept pull requests to the main branch.

## Adding software to MassOS
These requirements apply to both feature requests and pull requests:

- As stated in the [license](https://github.com/MassOS-Linux/MassOS/blob/main/LICENSE), we will only add software to MassOS that is either in the public domain, or under a GPL-compatible license. We will not accept non-free programs or programs licensed under GPL-incompatible licenses such as the CDDL.
- As an exception to the above, some non-free firmware will be accepted. Read [MassOS and Free Software](https://github.com/MassOS-Linux/MassOS/wiki/MassOS-and-Free-Software) for more information.
- If you modify the build system to add software, please read the "Build system" section below.

## Build system
Modifying the build system is the best way to add extra packages to MassOS. The build system is made up of the following files:

- `source-urls`: Contains the source tarball URLs.
- `retrieve-sources.sh`: Downloads all the source tarballs in `source-urls`.
- `stage1.sh`: Builds the set of temporary bootstrap tools, used to build the full MassOS system later. This should only be modified in extremely rare cases.
- `stage2.sh`: Sets up a chroot environment, calls `build-system.sh`, and creates the final rootfs tarball.
- `build-system.sh`: Builds the full MassOS system in a chroot environment. This is the main script that will be modified when adding extra packages.
- `patches`: A folder containing various patches for packages.

Please follow these guidelines when modifying/adding to the build system:

- Keep the source URLs in alphabetical order according to the file name in `source-urls`.
- Don't use Sourceforge download URLs; try and find a mirror instead, or ask us nicely to put the source tarball on archive.massos.org.
- Don't put patches in `source-urls`; put them in the `patches` directory instead.
- Put any external systemd units required for packages in the directory `utils/systemd-units`. They will be automatically copied to the system directory at build time.
- In `build-system.sh`, build packages after any dependencies they require, and before any other package they can serve as an optional dependency for. If the other package can use your package as an optional dependency but doesn't by default, consider modifying the configuration/build flags of the other package so it can use it.
- MassOS uses a unified filesystem structure (`/bin`, `/lib` and `/sbin` are symlinked to their user counterparts). Therefore, the installation prefix for packages can always be `/usr`.
- Some distros merge `sbin` with `bin` and `libexec` with `lib`. MassOS **does not**; `/usr/bin`, `/usr/sbin`, `/usr/lib` and `/usr/libexec` are all separate directories.
- Building packages twice to workaround circular dependencies (even optional ones) is OK.
- Always install any license files the package has. They are typically named "COPYING", "LICENSE", or variations of that. Install them after building with a command such as `install -t /usr/share/licenses/<package-name> -Dm644 COPYING`.
- Prefer Meson and CMake over Autotools if a package supports either build system.

## Documentation
You can contribute to this wiki of documentation, by improving existing pages or adding extra pages, however please follow these guidelines:

- Use good English - no grammatical errors are expected.
- If editing existing pages, you can add enhancements/improvements, but don't change the main content.
- If adding new pages, make sure they are about their own topic and not simply rephrasing what is already explained in other pages.
