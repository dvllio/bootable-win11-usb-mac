# Create a Windows Bootable USB on MacOS

All the following steps will happen in a terminal

You will need:
- USB drive (min. 8GB)
- [Windows ISO](https://www.microsoft.com/en-gb/software-download) of your choice

----

1/ Install Homebrew

```/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"```

2/ Install Wimlib

```brew install wimlib```

3/ Find the disk identifier associated with the USB drive you want to use

```diskutil list external```

4/ Format the drive using the MS-DOS format

```diskutil eraseDisk MS-DOS "WINSTALL" MBR disk6```

5/ Within the terminal, navigate to the folder where your Windows ISO is and mount it

_Adapt the ISO name to match yours_

```hdiutil mount Win11_22H2_EnglishInternational_x64v2.iso```

6/ Copy the content of the ISO into the USB drive, bar the **install.wim** file

```rsync -avh --progress --exclude=sources/install.wim /Volumes/CCCOMA_X64FRE_EN-GB_DV9/ /Volumes/WINSTALL```

7/ Use wimlib to split **install.wim** to fit on a FAT32 drive

```wimlib-imagex split /Volumes/CCCOMA_X64FRE_EN-GB_DV9/sources/install.wim /Volumes/WINSTALL/sources/install.swm 4000```

8/ Once that's done, unmount your USB drive

```diskutil unmount /dev/disk6```
