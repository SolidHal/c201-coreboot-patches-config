# Coreboot notes

tested stock coreboot image from 4GB model on a 2GB model, did not work

When testing with a libreboot image on the same 2GB model, got this:

```
coreboot-3ba8246-dirty Wed Sep  7 20:49:58 UTC 2016 bootblock starting...
Exception handlers installed.
Configuring PLL at ff760030 with NF = 99, NR = 2 and NO = 2 (VCO = 1188000KHz, output = 594000KHz)
Configuring PLL at ff760020 with NF = 32, NR = 1 and NO = 2 (VCO = 768000KHz, output = 384000KHz)
Translation table is @ ff700000
Mapping address range [0x00000000:0x00000000) as uncached
Creating new subtable @ff716c00 for [0xff700000:0xff800000)
Mapping address range [0xff700000:0xff718000) as writethrough
Configuring PLL at ff760000 with NF = 75, NR = 1 and NO = 1 (VCO = 1800000KHz, output = 1800000KHz)
SF: Detected GD25Q32(B) with sector size 0x1000, total 0x400000
CBFS @ 20000 size e0000
CBFS: 'Master Header Locator' located CBFS at [20000:100000)
CBFS: Locating 'fallback/romstage'
CBFS: Found @ offset 80 size 59e4


coreboot-3ba8246-dirty Wed Sep  7 20:49:58 UTC 2016 romstage starting...
RAM Config: 2.
Invalid RAMCODE.
```

This indicates to me that something is different between coreboot for 4GB and 2GB models


trying to build coreboot from source with depthcharge version 4.11
https://gist.github.com/SolidHal/3a0113201251a5943f90fc4104bc6e3a

```
git clone --recurse-submodules https://review.coreboot.org/coreboot.git
cd coreboot
git checkout 4.11
git submodule update --init --checkout
rm -rf payloads/external/depthcharge/depthcharge
make crossgcc-arm
```

copy the config file for depthcharge overflow
```
cp payloads/libpayload/configs/config.veyron payloads/libpayload/configs/config.veyron_speedy
```
make the Kconfig modification to `payloads/Kconfig`

set the 

then make
```
make --jobs=12
```
