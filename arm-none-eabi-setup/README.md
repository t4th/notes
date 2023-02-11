# arm-none-eabi
##install
arm none eabi downloaded form ubuntu package manager is missing some files (cstdint, etc),
thats why it is better to install official release from ARM (https://developer.arm.com/downloads/-/gnu-rm)

```
sudo apt install curl
curl -O https://developer.arm.com/-/media/Files/downloads/gnu-rm/10.3-2021.10/gcc-arm-none-eabi-10.3-2021.10-x86_64-linux.tar.bz2

sudo tar xjf gcc-arm-none-eabi-10.3-2021.10-x86_64-linux.tar.bz2 -C /usr/share/
```

create links:
```
sudo ln -s /usr/share/gcc-arm-none-eabi-10.3-2021.10/bin/arm-none-eabi-gcc /usr/bin/arm-none-eabi-gcc 
sudo ln -s /usr/share/gcc-arm-none-eabi-10.3-2021.10/bin/arm-none-eabi-g++ /usr/bin/arm-none-eabi-g++
sudo ln -s /usr/share/gcc-arm-none-eabi-10.3-2021.10/bin/arm-none-eabi-gdb /usr/bin/arm-none-eabi-gdb
sudo ln -s /usr/share/gcc-arm-none-eabi-10.3-2021.10/bin/arm-none-eabi-size /usr/bin/arm-none-eabi-size
sudo ln -s /usr/share/gcc-arm-none-eabi-10.3-2021.10/bin/arm-none-eabi-objcopy /usr/bin/arm-none-eabi-objcopy

sudo ln -s /usr/share/gcc-arm-none-eabi-10.3-2021.10/bin/* /usr/bin/
```

## flags
doc: https://gcc.gnu.org/onlinedocs/gcc/index.html

arm: https://gcc.gnu.org/onlinedocs/gcc/ARM-Options.html#ARM-Options

overall options: https://gcc.gnu.org/onlinedocs/gcc-6.3.0/gcc/Overall-Options.html