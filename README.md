### Device specific configuration to build AOSP Android 13 for Raspberry Pi 4.

#### Wiki:

- TODO

#### Issues:

- [Android](https://github.com/raspberry-vanilla/android_local_manifest/issues)
- [Linux kernel](https://github.com/raspberry-vanilla/android_kernel_manifest/issues)

### How to build:

1. Establish [Android build environment](https://source.android.com/setup/initializing) and install [repo](https://source.android.com/docs/setup/develop#installing-repo).

2. Install additional packages (TODO: possibly non-exhaustive):

```
sudo apt-get install bc coreutils dosfstools e2fsprogs fdisk kpartx mtools ninja-build
sudo snap install meson
pip3 install ply
```

3. Initialize repo:

```
repo init -u https://android.googlesource.com/platform/manifest -b android-13.0.0_r14
curl --create-dirs -L -o .repo/local_manifests/manifest_brcm_rpi4.xml -O -L https://raw.githubusercontent.com/raspberry-vanilla/android_local_manifest/android-13.0/manifest_brcm_rpi4.xml
```

You can also reduce download size by creating a shallow clone and removing unneeded projects (optional):

```
repo init -u https://android.googlesource.com/platform/manifest -b android-13.0.0_r14 --depth=1
curl --create-dirs -L -o .repo/local_manifests/remove_projects.xml -O -L https://raw.githubusercontent.com/raspberry-vanilla/android_local_manifest/android-13.0/remove_projects.xml
```

4. Sync source code:

```
repo sync
```

5. Compile:

```
. build/envsetup.sh
lunch aosp_rpi4-userdebug
make bootimage systemimage vendorimage -j$(nproc)
```

6. Make flashable image:

```
./rpi4-mkimg.sh
```

Also look into [Linux kernel build instructions](https://github.com/raspberry-vanilla/android_kernel_manifest/tree/android-13.0).
