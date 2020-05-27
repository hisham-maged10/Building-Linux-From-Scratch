## [DAY 1]  (5/24/2020)

1. installed virtual box using ubuntu universal repository

2. found out that virtualbox doesn't work with **_kernel56_**

3. found out there is a patch to make virtualization compatible with _kernel56_

4. Found out to apply patch I would have to compile virtual box from source with added patch [DISCARDED}

5. Found out there's a new virtualbox version that supports _kernel56_ but not in ubuntu's universal repository yet

6. Added virtualbox's repository to the system, updated current virtualbox 

7. Virtualization now works, Tried Ubuntu 18.04 live CD
_
8. Persisting on LIVE CD failed, Ubuntu is too slow, need something that is **_GUI-LESS with persistence ability_**

9. no ability to install guest additions on live-cd ubuntu

10. Tried ubuntu setup, canceled midway

11. Downloaded **_Arch linux_**

12. created an image for arch, same settings 

13. no ability to change resolution without configuring Arch [DISCARDED]
    
##### OUTPUT: No Live-CD working, No idea how to set resolution without guest-additions
    
##### ELAPSED: 9 hours

## [DAY 2] // 5/25/2020

1. Researched on LIVE-CD distros

2. Decided that I want a distro that is lightweight, **_debian based for APT support and universal repository_**

3. Found Antix to be most suitable, _NET_ version which has no software what so ever and no X-Server

4. Downloaded, **_AntiX-net_**

5. created image on virtual box
    settings:
   
        general > advanced > bidirectional clipboard
        system > boot order optical then hard, 2 cpu enable pae/nx
        display > 128 mb video, enable 3d acc
        Storage  > removed IDE controller, put SATA controller, put optical cd as live cd, hard as VDI
        rest is default

6. Tried persistence mode, failed miserabely due to optical drive of virtualbox being read only which makes sense

7. Researched persistence, found out that it makes no sense to virtualize a live-cd with persistence so it's not an option

8. Tried using virtual machine's save state instead of live-cd perisistence, found out that if program crashes, all will be lost

9. went with it and tried adding guest additions, needed reboot, so I knew save machine state won't work

10. Tried Changing iso to vdi using VBOxManage cli-tool, worked but not resizable so can't persist

11. Tried Arch linux once more, Followed a youtube video, GRUB installation failed [DISCARDED]
    ```
    GRUB (referes to Grub2): is the easiest type of bootloader for linux
    bootloader: is a piece of software started by the firmware (BIOS or UEFI) and is responsible for loading the kernel with wanted kernel parameters based on its config file
    firmware: is a very basic software held in ROM that provide low-level interface with device's specific hardware
    ```

12. went back to AntiX, did a CLI-Install on VDI, partitioned 8 gb to 500 mb efi boot [EFI Type], 7.5 gb root [Linux filesystem Type]

13. AntiX now working, Resolution sucks

14. Can't install guest additions

15. can't locate a lot of packages

16. found out I need to update cache of available packages in repository which makes sense [apt update]

17. Did an upgrade of current packages for completeness [apt upgrade]

18. Installed vim [Located Successfully] [apt install vim]

19. Tried Researching guest additions for terminal-based virtualization

20. apt install virtualbox-guest-dkms 

21. installed but still failed to change resolution

22. Xrandr is out of question cuz, X itself doesn't exist :')

23. Tried adding virtualbox guest additions cd while on boot

24. got surpriesed that a CD is not mounted automatically on AntiX (that's how lightweight it is)

25. Created a cdrom directory in root

26. found out /dev location of cd in lsblk, mounted /dev/sr0 on /cdrom

27. cd into /cdrom, executed .run file, did somethings, resolution still didn't change, nothing happened, required reboot, rebooted and nothing happened [FAILED]

28. Researched how to change resolution manually, found out that it can be changed in GRUB settings

29. edited /etc/default/grub, uncommented line GRUB_GFXMODE with needed resolution GRUB_GFXMODE=1920x1080 [FAILED]

30. found out GRUB documentation using info -f grub -n "Simple Configuration"

31. Found out I need GRUB_GFXPAYLOAD_LINUX to be keep, to keep GRUB_GFXMODE settings in Terminal resolution, Failed

32. Downloaded MX Linux
    
##### OUTPUT: Failed to persist on live-cd on virtual machine environment, Installed AntiX-net on VDI with no X-Server GUI just plain old TTY mode, Getty login but with invalid resolution
    
##### ELAPSED TIME: 8 hours

## [DAY 3] // 5/26/2020

1. Tried Setting GRUB resolution to 1024x768 and it worked??

2. Searched why 1920x1080 doesn't work, found out that 1920x1080 setting may not be supported by grub

3. needed to check my grub supported settings from grub commands, to enter grub commands, at boot menu press c, enter grub command mode

4. set pager=1 and try vbeinfo, now this is the settings supported by the grub

5. normal to continue execution

6. searched how to add 1920x1080 to grub settings in VM 

7. found a cli-method to add to vm [ vboxmanage setextradata "LFS" "CustomVideoMode1" "1920x1080x32"] which is vboxmanage setextradata <VM_Name> <kEY> <VALUE>

8. messed up vm instansiation by making the value 1920x1080x12, found out that I can clear the given key again by using same command but no value

9. changed to 1920x1080x32, note: third number is bit color depth 12 means 4 bits for each color channel R,G,B and no alpha channel, 16 bit means 4 bits for each R,G,B,A channel and 32 bit means 8 bit for R,G,B,A channel each

10. RESOLUTION WORKED IN GRUB

11. Need to keep resolution at GRUB so final settings are
    GRUB_GFXMODE=1920x1080x32
    GRUB_GFXPAYLOAD_LINUX=1920x1080x32 or can be GRUB_GFXPAYLOAD_LINUX=keep which will use same resolutio as GRUB_GFXMODE 

12. resolution fixed but font size still too large

13. changing font size can be done by editing /etc/default/console-setup and changing FONTSIZE= to 12x24
    final settings
    ACTIVE_CONSOLES="/dev/tty[1-6]"
    CHARMAP="UTF-8"
    CODESET="guess"
    FONTFACE="terminus"
    FONTSIZE="12x24"
    VIDEOMODE=

14. Starting LFS Book

15. Need to know what symbolic links are
```
   Symbolic links are like advanced shortcuts, they point to another files or directories, Advanced because they're not treated as shortcuts, they're treated as pointers
    so it treats the symbolic link as if it was really here but when accessed points to another place
    ** soft-link (Symbolic link) is just a file that has the path only and acts as a pointer to the actual directory or file so accessing it accesses the actual directory or file    but acts as if it was the real file 
    but deleting the original file, will also delete the symbolic link 
    ** hard-link (hard symbolic link) mirrors the file with all its contents not just the path and have the same inode so doing any change in one will affect the other
    and removing one won't remove the other but accessing any of them will be as if it accesses the other
```
16. recommended to use SWAP disk so will remove efi partition as not used and use that space for swap

17. cfdisk, delete efi partition, use free space to create swap(LINUX SWAP), write,quit, mkswap on created partition

18. created new VDI for LFS System other than AntiX drive, Created root of 10GB, 500mb boot, 1.5GB SWAP

19. root, boot partitions must be initialized to be ext4 type > mkfs -v -t ext4 /dev/sdbx

20. swap must be initialized as swap > mkswap /dev/sdbx

21. export $LFS variable, adding it to .bashrc > export LFS=/mnt/lfs

22. reload bashrc without reboot > source ~/.bashrc

23. mkdir -pv $LFS

24. mkdir -pv $LFS/boot

25. mount /dev/sdbx $LFS

26. mount /dev/sdbX $LFS/boot

27. changed in fstab file /etc/fstab to mount the new partitions on boot each time (man fstab)

28. e.g. /dev/sdb1 /mnt/lfs ext4 defaults 1 1
    .separated by tabs or spaces > device_location mount_point drive_type defaults dump(1) nodump(0) priority(root -> 1, others -> 2)

29. created sources directory in $LFS to hold all sources and patches that will be used

30. changed mod of sources directory to be rwx and sticky, sticky means that it can be deleted only by owner
    and added write permissions
    
    
    
    
    
    
    ```
    chmod -v a+wt $LFS/sources
    a -> all users [default]
    
    \+ add next
    w -> write , r -> read, x -> execute, t-> sticky bit
    ```

31. downloaded wget, apt install wget

32. use wget to download all urls from input-file and continue on fail and save in a directory not the CWD
    . wget --input-file=wget-list --continue --directory-prefix=$LFS/sources

33. create wget-list file
    . vim wget-list ( no copy paste between host and guest cuz console )

34. creating a shared folder
    .    added the download links of tools to be used

35. created a bash script to validate needed versions 

36. copied shared files to home directory in vm for safe keeping
    
##### ELAPSED: 4 hours
    
##### OUTPUT: Now Linux vm is fully working with needed resolution and has automatic mounting, LFS system partitions created and mounted, sources collected, needs downloading and compilation only then building starts.
