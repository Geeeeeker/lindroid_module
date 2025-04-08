# Lindroid module
This is a module (attempt to) make lindroid support stock ROMs and other ROMs, without the need to build a whole rom. 
This module currently is only focused on Android apphwc side, you'll have to follow lindroid's instructions to install the lxc part and libhybris(probably you'll also need to build your patched bionic libc and some other libs )
Currently only tested on Android 14 device, with KernelSU. Magisk may also be supported.(not tested)

-----------------------------
**WARNING: DO NOT FLASH the zip file in release directly, read the following instuctions first**

## Instructions
Before using this module, follow the instructions from [Lindroid](https://github.com/Linux-on-droid/vendor_lindroid) to prepare your kernel for lxc, then either you need to use the libhybri
To use this module, you'll have to:
1. Find out your ROM's tag.
	Usually you can find it inside `/system/build.prop`, as the value of `ro.system.build.id`.
2. Fetch the aosp source matching that tag
	It's for api compatibility. Usually aosp source is just fine.
3. Apply the lindroid and hybris patches
4. Build LindroidUI
    Follow the aosp build instructions to init your build environment. 
    Run `mm LindroidUI` to build the LindroidUI apk and libraries.
5. Get the result apks and libs.
    Usually the path of the results need are:
```
out/target/product/generic_arm64/system/system_ext/app/LindroidUI
out/target/product/generic_arm64/system/system_ext/lib64/libjni_lindroidui.so
out/target/product/generic_arm64/system/system_ext/lib64/vendor.lindroid.composer-ndk.so
```
6. Download the release zip
7. Replace the files inside it with the ones you built.
8. Flash the module and reboot.

Currently you'll have to set selinux to premissive before lanuch LindroidUI. In the future hopefully we can use sepolicy instead.

Depends on your android version, there's a bug in android framework that may cause soft reboot. You can choose to:
- Disable systemd-udevd. See the example at [here](https://github.com/George-Seven/Termux-LXC-Guide/blob/a6d98882a5e93c5c08f4e812083d1e8bb6bc3afa/src/required-lxc-configuration/scripts/utils/utils.pre-start.sh#L68) .
- Install Xposed framework and the Xposed module [uUeventPatch](https://github.com/fish4terrisa-MSDSM/uEventPatch)
- Patch the `services.jar` of your ROM, using apktool or other decompile/recompile tools. Modify the smali code according to this [patch](https://t.me/linux_on_droid/14042).
## Todo
 - [ ] Use sepolicy instead of setting selinux to premissive.
 - [ ] Split the lxc part from the display part so that users can choose to handle the lxc by themselves
 - [ ] Patch the `services.jar` in the install process.
 - [ ] Find a way to apply this [patch](https://gerrit.libremobileos.com/c/LMODroid/platform_frameworks_native/+/12936)
## Credits
 - [Lindroid](https://github.com/Linux-on-droid)
