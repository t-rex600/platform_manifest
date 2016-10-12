Liquid Github
===========

Getting Started
---------------
To get started with the Liquid sources, you'll need to get
familiar with [Git and Repo](http://source.android.com/source/version-control.html).


Create the Directories
----------------------

You will need to set up some directories in your build environment.

To create them run:

    mkdir -p ~/bin
    mkdir -p ~/liquid


Install the Repository
----------------------

Enter the following to download the "repo" binary and make it executable:

curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo && chmod a+x ~/bin/repo

You may need to reboot for these changes to take effect. 
Now enter the following to initialize the repository:

    cd ~/liquid


Initializing the Liquid Source:
---------------

For initializing repo use:

    repo init -u https://github.com/LiquidDark/platform_manifest.git -b n

Syncing repo:

    repo sync -j2 | -j4 |-j8 | -j32 (# of CPUs x2)


Compiling Liquid
---------------

Set up environment:

    . build/envsetup.sh
    
If your device is officially supported by https://github.com/LiquidDark-Devices, you can do now:

    breakfast <device_codename>
    (breakfast bullhead)
        
...to automatically pull all missing repositories. Then:

    brunch <device_codename>
    (brunch bullhead)
    
...to start compilation process.

     mka


Adding support for new device
================

If you want to be official maintainer and add Liquid support for a new device you have to create these two files in device tree:

liquid.mk sample
----------

    # Inherit from those products. Most specific first.
    $(call inherit-product, $(SRC_TARGET_DIR)/product/core_64_bit.mk) -- only for 64bit phones
    $(call inherit-product, $(SRC_TARGET_DIR)/product/full_base_telephony.mk)

    # Inherit from device
    $(call inherit-product, device/<path>/device.mk) -- path to main device makefile

    # Inherit common product files.
    $(call inherit-product, vendor/liquid/config/common.mk)

    # Enhanced NFC
    $(call inherit-product, vendor/liquid/config/nfc_enhanced.mk) -- optional

    # Set those variables here to overwrite the inherited values.
    BOARD_VENDOR := 
    PRODUCT_BRAND := 
    PRODUCT_DEVICE := 
    PRODUCT_NAME := liquid_device
    PRODUCT_MANUFACTURER := 
    PRODUCT_MODEL := 
    TARGET_VENDOR := 

    # Use the latest approved GMS identifiers unless running a signed build
    ifneq ($(SIGN_BUILD),true)
    PRODUCT_BUILD_PROP_OVERRIDES += \
        BUILD_FINGERPRINT=specific for your device \
        PRIVATE_BUILD_DESC="specific for your device"
    endif

liquid.dependencies sample
----------

    [
      {
        "remote": "ldd",
        "repository": "android_kernel_<name>",
        "target_path": "kernel/path",
        "revision": "n"
      },
      {
        "remote": "github",
        "repository": "TheMuppets/proprietary_vendor_<name>",
        "target_path": "vendor/path",
        "revision": "n"
      }
    ]
