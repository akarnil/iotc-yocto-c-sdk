## Build Instructions

These instructions are designed to get a Yocto image up and running for a specific board, First of all you will have to follow the instructions for each specific board you are using and then continue on from this guide.

1. Adding Yocto C SDK layers

    ```bash
    # You will need to clone the repository to a directory where all the other meta-layers exist, this different for each board
    
    git clone https://github.com/akarnil/iotc-yocto-c-sdk.git -b refactor-for-demo ./meta-iotconnect-c

    make env

    echo -e '\n' >> conf/bblayers.conf
    
    # the path is relative from the yocto build folder, this may be different from board to board

    bitbake-layers add-layer ../../meta-iotconnect-c/meta-iotconnect/
    
    bitbake-layers add-layer ../../meta-iotconnect-c/meta-myExampleIotconnectLayer/

    # You will need to make a change to meta-myExampleIotconnectLayer/conf/layer.conf
    # Depending on the image you are build (if it is a "core" image) you will need to select either CORE_IMAGE_EXTRA INSTALL or IMAGE_INSTALL
    # only use CORE_IMAGE_EXTRA_INSTALL for "core" images
    # If you building from these quick start guides, check the Makefile's build target to see if a "core" is being built it should be after the bitbake line
    # for example "st-image-weston" is not a "core" image and you should use IMAGE_INSTALL instead.
        
    # add systemd to the image
    echo -e '\nDISTRO_FEATURES:append = " systemd"\nDISTRO_FEATURES_BACKFILL_CONSIDERED += " sysvinit"\nVIRTUAL-RUNTIME_init_manager = " systemd"\nVIRTUAL-RUNTIME_initscripts = " systemd-compat-units"\n' >> ./conf/local.conf
    

    # Sets the "root" user's password to "avnet"
    echo -e '\n\nEXTRA_IMAGE_FEATURES=""\nINHERIT += "extrausers"\nEXTRA_USER_PARAMS = "usermod -P avnet root;"' >> conf/local.conf 

    exit

    make build
    ```