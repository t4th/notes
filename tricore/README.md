# tricore

## persitant register
```
SCU.RSTCON2.B.USRINFO
```
## request hw reset by sw
```
SCU.RSTCON.B.SW = 1;
SCU.SWRSTCON.B.SWRSTREQ = 1;
```
## xilins startup
Reset: pull up and down Program_B for 250 ns
Wait for startup: wait for done_0 and init_b to be high
## memtool script
```
connect
open_file "C:\img.hex"
select_all_sections
add_selected_sections
program
disconnect
exit
```
## ecc detection
ECCD Memory ECC Detection Register
DSPR in one cpu can corrupt ecc in other cpu.

Accessing memory that has been erased will result in ECC trap, because erasing reset ECC values.
If ECC trap is set to reset HW, the code can spiral into endless reset loop.
## enable/disable debuging
Register PROCONDBG in PMU
```
OCDSDIS - OCDS Disabled
DBGIFLCK - Debug Interface Locked;
```
## stuff...
```
EB tresos mcal_init cannot be called for multiple runtiems on a single target with different settings.
No error is detected for mcu_init, but peripherals are not updated with new settings. mcu check init will fail.
```
