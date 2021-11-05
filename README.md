# Ambarella H22 Firmware unpacker and packer

**Unpak and re-pack firmwares for action cameras based on the Ambarella H22 SoC**

The **ambarella-h22-firmware-repack** and 
**ambarella-h22-firmware-unpack** are two Python scripts that 
can upack and re-pack the firmwares released for action cameras 
based on the Ambarella H22 SoC.

Unpacking the firmware is necessary if you want to edit some 
settings. E.g. if you want to change video bitrates, GOP 
paramters, gamma and LUT tables, translation strings, etc. Once 
edited the files, you have to re-pack everything into a new 
firmware file, updating CRC32 and MD5 checksums.

The scripts are tested on firmwares for the **SJCAM SJ8 Pro** 
and the Hawk Eye **Firefly X Lite** cameras, they can extract 
each section from the archive, and check the CRC32 checksums.

Generally the firmware sections may consist of RTOS (Real Time 
Operating System) images, Linux kernel images, GNU/Linux root 
filesystem images or Ambarella ROMFS images. Ambarella ROMFS 
images contain files, which are extracted and checked against 
their CRC32 checksums.

The **SCAM SJ10 Pro** camera uses a slightly different format 
for the ROMFS partitions; at the moment the difference cannot be 
autodetected and you must change a constant definition into the 
script.

As far I know, there is no official documentation about 
firmwares file format and it is no clear if the file format is 
common to all the Ambarella H22 cameras or if each maker (SJCAM, 
Hawk Eye, etc.) can customize it freely.

The scripts are based on reverese engeneering and upon some 
softwares available as open source:

* [BitrateEditor](https://github.com/vmax1145/BitrateEditor)
* [dji-firmware-tools](https://github.com/o-gs/dji-firmware-tools)

I tried also the Ambarella Firmware Toolbox 1.3.3 which can 
analyze firmwares for Ambarella A2, A5 and A7. Unfortunately it 
cannot parse H22 files.

As far I know, the existing softwares (I mainly tested 
BitrateEditor) does not re-pack the firmware 100% correctly, 
because it does not update the CRC32 checksums in the firmware 
header. Neverthless the SJ8 Pro accepts to flash firmwares with 
that CRC32 mismatches.

## Programs usage

To extract data from a firmware, use the command:

```
ambarella-h22-firmware-unpack FIRMWARE.bin FIRMWARE_CHECK.ch DST_DIRECTORY
```

where the **.bin** file is actually the firmware and the **.ch** 
file contains the MD5 checksum. The **DST_DIRECTORY** will be 
created and filled with the sections. If one of the section is 
detected as ROMFS, files contained therein will be estracted and 
saved into a subdirectory.

To re-pack the content of a directory, use the command:

```
ambarella-h22-firmware-repack SRC_DIRECTORY DST_FIRMWARE.bin DST_FIRMWARE.ch
```

The **SRC_DIRECTORY** must contains files as they were extracted 
(headers, binary images, files lists and subdirectories), the 
**DST_FIRMWARE.bin** and **DST_FIRMWARE.ch** will be created 
with updated CRC32 and MD5 checksums.

## Firmwares used for testing

* [SJ8 Series Firmware](http://support.sjcam.com/support/solutions/folders/9000184902)
* [Firefly Firmwares](http://www.hawkeyefpv.com.cn/About.aspx?ClassID=35)
* [SJ10 Series Firmware Updates](http://support.sjcam.com/support/solutions/folders/9000192806)

