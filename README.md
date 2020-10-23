# Stratasys Fortus 250mc EEPROM hack

Updated version of the tutorial from [Have Blue](https://haveblue.org/?p=1988) to be used on a RPi3, with stratasys 250mc, and with the new library 
[Stratatools](https://github.com/bvanheu/stratatools)
Only the main differences from the tutorial are listed here. 
## Prepare RPI3

Change the modes of pin where it connect to chip

```bash
gpio mode 7 out
gpio write 7 1
```
```bash
sudo raspi-config
```
Interfacing Options->1-Wire->Enable
```bash
sudo modprobe w1-gpio gpiopin=4
sudo modprobe w1-ds2433
```

## After connected to chip
```bash
cd /sys/bus/w1/devices/w1_bus_master1
ls
```
check for folder started with "23-00"
```bash
cd 23-0000022ebe12
xxd -p id
```
output of eeprom id in my case is:
```bash
2312be2e0200005a
```

## With [Stratatools](https://github.com/bvanheu/stratatools) installed
Create a cartridge.txt as showed in Stratatools git.

```bash
stratatools eeprom_encode -t prodigy -e 2312be2e0200005a cartridge.txt cartridge.bin
```
```bash
sudo cp cartridge.bin /sys/bus/w1/devices/w1_busmaster1/23-0000022ebe12/eeprom
```


