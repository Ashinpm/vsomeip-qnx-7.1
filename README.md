### vSomeIP

##### Copyright
Copyright (C) 2014-2026 Bayerische Motoren Werke Aktiengesellschaft (BMW AG)
This Source Code Form is subject to the terms of the Mozilla Public
License, v. 2.0. If a copy of the MPL was not distributed with this
file, You can obtain one at http://mozilla.org/MPL/2.0/.

##### License

This Source Code Form is subject to the terms of the Mozilla Public License, v. 2.0.
If a copy of the MPL was not distributed with this file, You can obtain one at http://mozilla.org/MPL/2.0/.

##### Contributing Guidelines

For comprehensive details on how to contribute effectively to the project, please refer to our [CONTRIBUTING.md](./CONTRIBUTING.md) file.

##### vSomeIP Overview
----------------
The vSomeIP stack implements the http://some-ip.com/ (Scalable service-Oriented MiddlewarE over IP (SOME/IP)) Protocol.
The stack consists out of:

* a shared library for SOME/IP (`libvsomeip3.so`)
* a shared library for SOME/IP's configuration module (`libvsomeip3-cfg.so`)
* a shared library for SOME/IP's service discovery (`libvsomeip3-sd.so`)
* a shared library for SOME/IP's E2E protection module (`libvsomeip3-e2e.so`)

##### Build Instructions for QNX 7.1

###### Dependencies

- vSomeIP uses CMake as buildsystem.
- vSomeIP uses Boost >= 1.75.0:
- vSomeIP 3.7.4 

###### Code changes
- Stripped c++20 features like spaceship operator etc.. to work with QNX compiler
- Tested on Raspberry Pi 4B and linux pc
- Works only on io-pkt network stack


###### Compilation Steps

download and extract boost 1.75.0

https://www.boost.org/releases/1.75.0/

create a user-config.jam file with below line

```
cd boost_1_75_0
```
```
echo "using qcc : qnx : q++ -Vgcc_ntoaarch64le ;" > user-config.jam
```

Source QNX env File

```
source qnx710/qnxsdp-env.sh
```

```
./bootstrap.sh
```
specify the boost installation directory

```
./b2 toolset=qcc-qnx target-os=qnxnto link=shared threading=multi   --with-system --with-thread --with-filesystem --with-log   --user-config=user-config.jam   --prefix=../boost_qnx_install install

```
After sucessful compilation of boost for qnx, clone and compile vsomeip for qnx

```
cd ..

```
```
git clone https://github.com/Ashinpm/vsomeip-qnx-7.1.git
```
```
cd vsomeip-qnx-7.1/build_qnx/
```

```
cd vsomeip/build_qnx

```
Turn off DLT to avoid compilation error. DLT can be cross compiled to work with the build

```
cmake ..   -DCMAKE_TOOLCHAIN_FILE=qnx.nto.toolchain.cmake   -DCMAKE_SYSTEM_PROCESSOR=aarch64   -DBOOST_ROOT=../../boost_qnx_install   -DDISABLE_DLT=ON
```
```
make
```


