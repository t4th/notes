# c2000

## calculate crc of data during linktime
Add crc_table to a group in the linker script
```
GROUP
{
 .text
 .const
 .cinit
} crc_table(calculated_crc, algorithm = CRC32_PRIME)
```
Linker will generate the result in form of a CRC table and copy it to symbol name provided as argument (calculated_crc in this case).

Generated CRC tables are placed in .TI.crctab region, but it is not assigned by default to any GROUP.
It must be added manually:
```
GROUP
{
 .TI.crctab
}
```

All generated CRC tables can be previewed in MAP file after building:
```
data_crc @ 000bc000 records: 3, size/record: 8, table size: 26
.text:  algorithm=CRC32_PRIME(ID=0), page=0, load addr=000ba000, size=00000100, CRC=11111111
.const: algorithm=CRC32_PRIME(ID=0), page=0, load addr=000ba100, size=00000100, CRC=22222222
.cinit: algorithm=CRC32_PRIME(ID=0), page=0, load addr=000ba200, size=00000100, CRC=33333333
```
Size in table is in c2000 bytes, ie. 16 bit words.

The CRC table can be accessed in code by using symbol name:
```
#include "crc_tbl.h"
extern CRC_TABLE calculated_crc;
UINT32 final_crc = calculated_crc.recs[0].crc_value;
```
Header is provided with TI SDK and should be part of default include path.

Linker is using CRC32_PRIME defined in crc_defines.h. Default parameters:
```
polynomial = 0x04c11db7
initial value = 0
output XOR = 0
input data reflection = false
output data reflection = false
```
