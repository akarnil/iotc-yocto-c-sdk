***IoT-Connect supports the `hardknott` version of Yocto.***



1. Clone `msc-ldk` and use the `hardknott` branch
```bash
git clone ssh://gitolite@msc-git02.msc-ge.com:9418/msc_ol99/msc-ldk && cd msc-ldk && \
git checkout v1.9.0
```
1. Setup build enviroment for SM2S-IMX8MP
```bash
./setup.py --bsp=01047
```
1. Build the base image, this will take a while
```bash
cd build/01047 && make
```
1. Download this repo
```bash
cd ../../ & \
wget https://github.com/avnet-iotconnect/iotc-yocto-c-sdk/archive/refs/heads/main.zip && \
unzip main.zip -d sources/ && \
mv sources/iotc-yocto-c-sdk-main/meta-* sources/ && \
rm -r main.zip sources/iotc-yocto-c-sdk-main/
```


1. Add iot-connect and example layers to `build/01047/conf/bblayers.conf`


1. Activate bitbake
```bash
source sources/yocto.git/oe-init-build-env  ./build/01047/
```

1. Build
```bash
bitbake iot-connect-image
```



Not needed anymore

1. Patch `meta-iotconnect` to `kirkstone`
```bash
sed -i 's/cmake_do_generate_toolchain_file_append/cmake_do_generate_toolchain_file\:append/g' ./sources/meta-iotconnect/recipes-apps/iotConnect/iot-connect_0.1.bb && \ 
sed -i 's/_prepend/:prepend:/g' ./sources/meta-myExampleIotconnectLayer/recipes-apps/iotConnect/iot-connect_%.bbappend && \
sed -i 's/hardknott/kirkstone/g' ./sources/meta-myExampleIotconnectLayer/conf/layer.conf && \
sed -i 's/hardknott/kirkstone/g' ./sources/meta-iotconnect/conf/layer.conf 
```
