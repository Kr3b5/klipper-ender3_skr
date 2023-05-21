<h3 align="center">Klipper - Ender 3 - SKR Mini E3</h3>
  <p align="center">
    This is a small setup guide for the Ender 3 with klipper
    <br />
    <a href="https://github.com/Kr3b5/klipper-ender3_skr/issues">Report Bug</a>
  </p>
  <br />

---

## :wrench: Hardware & Software

* **Ender 3 - Pro (2020)**
  * Board: BIGTREETECH SKR Mini E3 V3.0 
  * Hotend: Creality Spider Hotend 
  * Autoleveling: BLTouch *(:construction: in progress not in cfg yet)*
<br />

* Raspberry Pi 3 - Model B
  * 16 GB SD Card
<br />

* [MainsailOS (Klipper)](https://github.com/mainsail-crew/mainsail) 


## :hammer: Hardware Setup 

:construction: Coming soon


## :computer: Software Setup 

### MainsailOS (Klipper)

1. Download and install the latest [Raspberry Pi Imager](https://www.raspberrypi.com/software/)
2. Flash MainsailOS 
   * OS > Other specific-purpose OS > 3D printing > MainsailOS(32 bit recommend)
   * Settings: 
     * Activate SSH 
     * Add wifi credentials
     * (optional) Change hostname *(Default: `http://mainsailos(.local)`)*
3. When the writing has finished successfully, remove the SD card and insert it into your Pi

:warning: The first start of MainsailOS will take some time to expand your file system. The larger the SD card, the longer the first start-up process will take. *(16GB = ~ 5 min)*


:white_check_mark: As soon as the boot process is finished, Mainsail should be available at http://mainsailos.local or http://mainsailos. Replace `mainsailos` with the hostname you may have set when you flashed the image.


### Flash Ender 3

1. Connect remote to your pi with SSH
```
ssh [user]@[ip from pi]
```

2. Run `menuconfig`
```
cd ~/klipper 
make menuconfig
```

```
[*] Enable extra low-level configuration options
    Micro-controller Architecture (STMicroelectronics STM32)  --->
    Processor model (STM32G0B1)  --->
    Bootloader offset (8KiB bootloader)  --->
    Clock Reference (8 MHz crystal)  --->
    Communication interface (USB (on PA11/PA12))  --->
    USB ids  --->
()  GPIO pins to set at micro-controller startup
```

3. Exit (Q) and save the configuration 
4. Run `make` to create a klipper.bin file in `klipper/out`
```
make
```

   * :information_source: My firmware bin : [/files/firmware.bin](./files/firmware.bin)

5. Copy this file to a local SD card and rename it to `firmware.bin`. There are different ways:

   * with scp in terminal
        ```
        scp [user]@[ip from pi]:~/klipper/out/klipper.bin [path SD card]/firmware.bin
        ```
   * with [WinSCP](https://winscp.net/eng/download.php) 
   * copy the file to config directory and download it with MainsailOS GUI *(Menu > Machines)*
        ```
        cp ~/klipper/out/klipper.bin ~/printer_data/config/firmware.bin
        ```
    :warning: SD Format: FAT32 

6. Insert the SD card into the switched off printer
7. Power on the printer (the display should show nothing)
8. Wait 1 min and power off the printer, remove the SD card and connect the raspberry pi with USB 

:white_check_mark: if it was successful the file ``FIRMWARE.CUR`` should be on the SD card 

### Configurate Klipper (with MailsailOS GUI)

For the configuration of Klipper the ``printer.cfg`` must be created in the ``config`` folder in the menu section ``MACHINE``

* My actual config file: [/configs/printer.cfg](./configs/printer.cfg)

:information_source: Klipper docu: https://www.klipper3d.org/Config_Reference.html

