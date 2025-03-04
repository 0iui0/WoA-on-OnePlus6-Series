<img align="right" src="https://github.com/Daniel224455/WoA-on-OnePlus6-Series/blob/main/enchilada.png" width="350" alt="Windows 11 running on enchilada">

# Running Windows on the OnePlus 6

## Partitioning your device

### Prerequisites
- A brain (most important of all)

- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)
  
- [Modded TWRP](https://github.com/Daniel224455/WoA-on-OnePlus6-Series/releases/tag/Recovery)

- [Parted](https://github.com/Daniel224455/WoA-on-OnePlus6-Series/releases/download/Files/parted)

### Notes
> [!WARNING]  
> Do not run the same command twice unless specified.
> 
> DO NOT REBOOT YOUR PHONE! If you think you made a mistake, ask for help in the [Telegram chat](https://t.me/WinOnOP6).
> 
> Do not run all commands at once, execute them in order!
>
> YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!

#### Boot the modded recovery
> Open a CMD window inside the platform-tools folder, then (while your phone is in fastboot mode) run
```cmd
fastboot boot path\to\twrp.img
```

#### Backing up important files
Use TWRP now to back up your Modem and EFS partition (as well as anything else if you have important data). Move this backup to a safe place (e.g your PC) as the next steps will wipe your data.

> [!Warning]
> All of your data will be erased. This is your last chance to back up.
> 
> **IF YOU PROCEED WITHOUT BACKING UP MODEM AND EFS, YOU ARE ON YOUR OWN IF YOU MESS UP**

### Partitioning guide
> Your OnePlus 6 may have different storage sizes. This guide uses the values of the 128GB model as an example. When relevant, the guide will mention if other values can or should be used.

#### Unmount data
- Go to "Mount" in TWRP and unmount data, if it is mounted

#### Preparing for partitioning
> Download the parted file and move it in the platform-tools folder, then run
```cmd
adb push parted /cache/ && adb shell "chmod 755 /cache/parted" && adb shell /cache/parted /dev/block/sda
```

#### Printing the current partition table
> Parted will print the list of partitions, userdata should be the last partition in the list.
```cmd
print
```

#### Removing userdata
> Replace **$** with the number of the **userdata** partition, which should be **17**
```cmd
rm $
```

#### Recreating userdata
> Replace **6559MB** with the former start value of **userdata** which we just deleted (it is probably 6599MB)
>
> Replace **32GB** with the end value you want **userdata** to have
```cmd
mkpart userdata ext4 6559MB 32GB
```

#### Creating ESP partition
> Replace **32.16GB** with the end value of **userdata**
>
> Replace **32.66GB** with the value you used before, adding **0.5GB** to it
```cmd
mkpart esp fat32 32.16GB 32.66GB
```

#### Creating Windows partition
> Replace **32.66GB** with the end value of **esp**
>
> Replace **125GB** with the end value of your disk, use `p free` to find it
```cmd
mkpart win ntfs 32.66GB 125GB
```

#### Making ESP bootable
> Use `print` to see all partitions. Replace "$" with your ESP partition number, which should be 18
```cmd
set $ esp on
```

#### Exit parted
```cmd
quit
```

#### Formatting data
- Format all data in TWRP, or Android will not boot.
- (Go to Wipe > Format data > type yes)

#### Check if Android still starts
- Just restart the phone, and see if Android still works


## [Next step: Installing Windows](/guide/2-install.md)
















