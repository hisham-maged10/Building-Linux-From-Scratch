# [Preface] 
---
Let's first start with the basics and motivation behind building linux from scratch
We all hear about what's called a Linux Distribution (distro) but what is it really?
well, to understand that we have to know what Linux actually is,

> ___Linux___
> , This may come as a shock to most, but Linux is actually just the Kernel at the heart of your operating system.

which brings us to the next definition,

> ___Kernel___
> is just a low-level software that is loaded by the bootloader during the boot process in RAM 
> whose responsibility is to provide an interface between Software running in user-space and 
> your hardware, satisfying its requests, where the Kernel does The management of Processes, Memory, I/O requests, I/O devices, System Calls and I/O Access for drivers software 
> to interact with with their corresponding physical devices and the kernel is also responsible to continue with the boot process once loaded in memory

After knowing that Linux is just a kernel, then what is a Linux operating System ?

> ___Linux Operating System___
> is an Operating System whose kernel is Linux, and is actually refered to as the GNU/Linux Project, because all the software used in linux based systems is open-source  
> and is provided by the GNU project which is an open-source community and both togther make up the the GNU/Linux Operating System which is referred to as Linux

> ___Linux Distribution___
> is a combination of selected open-source software, package management functionalities and Desktop Enviroment (Some distros do not provide Desktop Enviroments, They're 
> Console based, like the one used in this repo)
> that uses the linux kernel to interface with hardware of your device, so basically its a Linux Operating System, like Ubuntu but the important part to know is that 
> Linux is ___just the kernel___ not the operating system, and a distribution just picks or develops open source software belonging to the GNU project and community 
> to bring specific features for each category of use, So any software you run, even the Console TTY belongs to the GNU project, has nothing to do with the linux kernel, 
> it just uses it.

> ___Operating System___
> is the whole package that manages our computer's resources and lets us interact with it, an operating system is made up of a bootloader(low level software 
> that your basic firmware loaded in ROM starts, and its responsibility is to load the kernel into memory to continue the boot process), kernel, 
> Device Drivers (software that interfaces and manages your physical hardware through kernel such as your GPU driver), 
> Networking interfaces (which automatically configures how networking works), User interface (Your Console based interface or graphical based interface)
> and a user space (which is any software running on your computer)

> ___Desktop Environment___
> bundles up multiple graphical user interface components such as icons, toolbars, wallpapers, desktop widgets and utilities such as Window Managers and 
> Display Managers, Window Manager, a Compositor all of which need what is called as an X window server to function.
> Examples of DE are `GNOME` which is used in `Ubuntu`, `Pathogen` which is used in `ElementaryOS`

> ___Display Managers___
> They are basically the graphical user interface of your login screen such as `GDM3` which is used by `GNOME`, `SDDM` which is used by KDE, without it, 
> There's a default console based login window called `Getty` where you just access the 
> console, you can actually use it by pressing cntrl + alt + F2 on loading menu of the distro

> ___Window Managers___
> They are X-Clients that manage how the applications window frames look and how they're rendered on screen, how and where they pop on the screen, they determine
> how the borders, title bars look and what their size are and some window managers are made spicifically for a desktop environment to fully interface with 
> its icons, menus like `Kwin` window manager which is used by `Plasma` Desktop Environment used by `KDE` distro and some are more standalone window managers 
> meant for more advanced users, they do not provide any toolbars, menus, compositors, ..etc where it gives the user the choice of 
> which components to use for a more lightweight 
> and use specialized systems such as `i3`, Also there are types of window managers basically defining how windows pop up and positioned, there're `Stacking`
> window managers which is the more common one where windows are floating and can be tiled by draging them to a side of the screen, Some are `Tiling` windows 
> which automatically tiles the windows for the user and is more advanced in terms of their configuration

> ___Compositors___
> They're the components that handle the animations of windows on the screen, such as fading effect, how windows are minimized, maximized, their opacity(in fact
> without compositors, there is no opacity), their drop shadow and much more, an example of them is `Compton` which is wideley used.

> ___X Window Server___
> commonly refered to as X or X11 is a GUI Framework used by X-clients to provide rendering for graphical components, mouse and keyboard interactions with them 
> and `Xorg` is the open-source implementation of it for linux communities and is a prerequisite for any graphical interface on linux
> as it is the server of these services, and is called X because it does not mandate a specific user interface, desktop environment or window manager, 
> it just provides the means for them to function. note that the OS can work perfectly without an X-Server, it just won't have any GUI and will be console based. 
> infact some distros are made without providing an x-server for giving the users the ability to customize their system to that extent, also X-server can be configured 
> using its config files in /usr/share/xorg or in /etc/xorg and a popular example of a config file for it is `.Xresources` or `.Xdefaults`

Which brings us to the reason I'm building linux from scratch, all of these components can work independently without the other, Distros provide you with selected components combined together to give u an out of the box usage feel as a user, but as you go in deeper into linux and use it more, you face problems that you typically search google on how to solve them and just copy paste the solution to your terminal and voilla, magic happens and everything now works as you need, so I decided that ___I want to understand more___ about how all of these components communicate with each other and how linux is operating so this journey is about understanding how an Operating System actually work and about exploring the different components of the system, you can say, ___I'm building a distro___ as a long term hobby.

---
## What I will be using
---
- will be using
- LFS (Linux From Scratch) Book (Should be refered to for the steps I will be following) [Read it here](http://www.linuxfromscratch.org/lfs/view/stable/)
- Oracle's Virtual Box software to virtualize the whole process instead of bricking my actual computer
- A lightweight X Server less distro for performance and further exploring of the system along the way
- Vim for markdown writing of this journal
- Some Free time

---
## Pre-requiste Knowledge
---
When using a virtual box, The system that the virtual box is installed on is called the ___Host System___ and the system that is being virtualized ( on the virtual box ) is called the ___Guest System___ and when virtualizing a system, you specify the settings of the virtual components that will run that system like RAM, Number of CPUs, Video memory,...etc, and that oracle provides what is called as virtualbox guest additions that modifies the resolution and other gui functionalities for your virtualized system.

---

## [DAY 1] (5/24/2020) [Nothing Works, duh]

1. installed virtual box using ubuntu main repository
    ```
    sudo apt install virtualbox
    ```

> ___Main Repository___
> Main repository is the repository that canonical itself maintains, note: Canonical is the creator of Ubuntu
> and it is the default repository used by `APT` package manager

2. found out that virtualbox doesn't work with **_kernel56_**

3. found out there is a patch to make virtualization compatible with _kernel56_

4. Found out to apply patch I would have to compile virtual box from source with added patch [DISCARDED}

5. Found out there's a new virtualbox version that supports _kernel56_ but not in ubuntu's main repository yet

6. Added `virtualbox's PPA repository` to the system, updated current virtualbox 

> ___PPA___
> PPA stands for: Personal Package Archive and it contains packages that are not maintained by Canonical but by
> the community and still has not arrived at the official repositories ( Universe Repository )

> ___Universe Repository___
> Repository that is maintained by community developers after being tested thoroughly

    ```
        add-apt-repository <Repository_Name>
        <Repository_Name> : ppa:user/repository_name
        // needs to update cache list used by apt to contain packages in ppa
        apt update
    ```
> And is then added to APT's source list which is at `/etc/apt/sources.list`


7. Virtualization now works, Tried Ubuntu 18.04 live CD
_
8. Persisting on LIVE CD failed, Ubuntu is too slow, need something that has  **_ No Xorg Server and  with persistence ability_**

> ___Xorg Server___
> Xorg server is the software that interfaces between kernel and Graphical user interface in linux
> without Xorg, all you have is a console and no GUI

9. no ability to install guest additions on live-cd ubuntu

10. Tried ubuntu setup, canceled midway

11. Downloaded **_Arch linux_**

12. created an image for arch, same settings 

13. no ability to change resolution without configuring Arch [DISCARDED]
    
##### OUTPUT: No Live-CD working, No idea how to set resolution without guest-additions
    
##### ELAPSED: 9 hours

---

## [DAY 2] (5/25/2020) [The journey of "NOTHING MAKES SENSE!"]

---

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
    
> ___GRUB (referes to Grub2)____
> is the easiest type of bootloader for linux

> ____Bootloader____ 
> is a piece of software started by the firmware (BIOS or UEFI) and is responsible for 
> loading the kernel with wanted kernel parameters based on its config file

> ___Firmware___
> is a very basic software held in ROM that provide low-level interface with 
> device's specific hardware

12. went back to `AntiX`, did a `Cli-install` on VDI, partitioned 8 gb to 500 mb efi boot [EFI Type], 7.5 gb root [Linux filesystem Type]

13. `AntiX` now working, Resolution capped at `640x480`

14. Can't install guest additions

15. can't locate a lot of packages

16. found out I need to update cache of available packages in repository which makes sense [apt update]
    ```
    sudo apt update
    ```
> APT uses a cache with packages it knows about so as not to make a request to repositories each time something is neeeded
> it just checks its cache for the package name and gets its corresponding download link
> that's why `apt update` is important, because it updates the cache
> by asking all the repositories in `/etc/apt/sources.list` if they have new packages that system doesn't have
> including the PPA repositories along with ubuntu's repositories

17. Did an upgrade of current packages for completeness [apt upgrade]

18. Installed vim [Located Successfully] `apt install vim`

19. Tried Researching guest additions for terminal-based virtualization

    ```
    apt install virtualbox-guest-dkms 
    ```

20. That installed the module of virtual box guest additions on vm

21. installed but still failed to change resolution

22. If you're wondering, `Xrandr` is out of question because Xorg Server itself doesn't exist, I'm using Console based distro

23. Tried adding virtualbox guest additions cd while on boot

24. got surprised that a CD is not mounted automatically on AntiX (that's how lightweight it is)

25. Created a cdrom directory in root

26. found out `/dev` location of cd in `lsblk`, mounted `/dev/sr0` on `/cdrom`
    ```
    > lsblk
    // find out at which /dev the CD rom is mounted on, mine was /dev/sr0
    > mkdir /cdrom
    > mount /dev/sr0 /crdrom
    ```
> ___\dev___
> Everything in linux is treated as a file even hardware so everything has a file that points to the device

27. cd into `/cdrom`, executed .run file, did somethings, resolution still didn't change, nothing happened, required reboot, rebooted and nothing happened [FAILED]
    ```
    cd /cdrom
    ./virtual_box_guest_additions.run
    ```

28. Researched how to change resolution manually, found out that it can be changed in GRUB settings

29. edited `/etc/default/grub` , uncommented line `GRUB_GFXMODE`with needed resolution `GRUB_GFXMODE=1920x1080` [FAILED]

30. found out GRUB documentation using `info -f grub -n "Simple Configuration"`

31. Found out I need `GRUB_GFXPAYLOAD_LINUX` to be _keep_, to keep `GRUB_GFXMODE` settings in Terminal resolution, Failed

32. Downloaded MX Linux
    
##### OUTPUT: Failed to persist on live-cd on virtual machine environment, Installed AntiX-net on VDI with no X-Server GUI just plain old TTY mode, Getty login but with invalid resolution
    
##### ELAPSED TIME: 8 hours

---

## [DAY 3] (5/26/2020) [Boss of Resolutions, The aha Moment]

---

1. Tried Setting GRUB resolution to 1024x768 and it worked??

2. Searched why 1920x1080 doesn't work, found out that 1920x1080 setting may not be supported by grub

3. needed to check my grub supported settings from grub commands, to enter grub commands, at boot menu press c, enter grub command mode

4. `set pager=1` and `vbeinfo`, now this is the settings supported by the grub
    ```
    // in grub bootloader menu
    //press c
    grub> set pager=1
    grub> vbeinfo
    //find if needed settings exists
    grub> normal
    ```

5. `normal` to continue execution and boot into linux

6. searched how to add `1920x1080` to grub settings in VM 

7. found a cli-method to add to vm 
    ``` 
    > vboxmanage setextradata "LFS" "CustomVideoMode1" "1920x1080x32"` 
    which is vboxmanage setextradata <VM_Name> <kEY> <VALUE>
    ```

8. messed up vm instansiation by making the value 1920x1080x12, found out that I can clear the given key again by using same command but no value

9. changed to 1920x1080x32, 
   ```
   third number is bit color depth 12 means 4 bits for each color channel R,G,B and no alpha channel
   16 bit means 4 bits for each R,G,B,A channel
   32 bit means 8 bit for R,G,B,A channel each
    ```
 10. RESOLUTION WORKED IN GRUB

11. Need to keep resolution at GRUB so final settings are
    ```
    GRUB_GFXMODE=1920x1080x32
    GRUB_GFXPAYLOAD_LINUX=1920x1080x32 
    ```
or can be 
    ```
    GRUB_GFXPAYLOAD_LINUX=keep which will use same resolutio as GRUB_GFXMODE 
    ```

12. resolution fixed but font size still too large

13. changing font size can be done by editing `/etc/default/console-setup` and changing FONTSIZE= to 12x24
    ```
    final settings
    ACTIVE_CONSOLES="/dev/tty[1-6]"
    CHARMAP="UTF-8"
    CODESET="guess"
    FONTFACE="terminus"
    FONTSIZE="12x24"
    VIDEOMODE=
    ```
14. Starting LFS Book

15. Need to know what symbolic links are

> ___Symbolic links___
> Symbolic links are like advanced shortcuts, they point to another files or directories, 
> Advanced because they're not treated as shortcuts, they're treated as pointers
> so it treats the symbolic link as if it was really here but when accessed points 
> to another place

> ___soft-link (Symbolic link)___ 
> is just a file that has the path only and acts as a pointer
> to the actual directory or file so accessing it accesses the actual directory or file
> but acts as if it was the real file 
> but deleting the original file, will also delete the symbolic link 

> ___Hard-link___
> mirrors the file with all its contents not just the
> path and have the same inode so doing any change in one will affect the other
> and removing one won't remove the other but accessing any of them will 
> be as if it accesses the other

16. recommended to use SWAP disk so will remove efi partition as not used and use that space for swap

17. `cfdisk`, delete efi partition, use free space to create swap(LINUX SWAP), write,quit, mkswap on created partition
    ```
    cfdisk
    ```

18. created new VDI for LFS System other than AntiX drive, Created root of 10GB, 500mb boot, 1.5GB SWAP

19. root, boot partitions must be initialized to be ext4 type 
    ```
    mkfs -v -t ext4 /dev/sdbx
    ```

20. swap must be initialized as swap 
    ```
    mkswap /deb/sdbx
    ```

21. export $LFS variable, adding it to `.bashrc` 
    ```
    export LFS=/mnt/lfs
    ```

22. reload bashrc without reboot 
    ```
    source ~/.bashrc
    ```

23. create needed directories for mounting 

     ```
     mkdir -pv $LFS
     mkdir -pv $LFS/boot
     mount /dev/sdbx $LFS
     mount /dev/sdbX $LFS/boot
    ```

24. changed in fstab file /etc/fstab to mount the new partitions on boot each time (man fstab)
    ``` 
    /dev/sdb1 /mnt/lfs ext4 defaults 1 1
    ```
separated by tabs or spaces or both
    ```
    > device_location mount_point drive_type defaults dump(1) nodump(0) priority(root -> 1, others -> 2)
    ```

25. created sources directory in $LFS to hold all sources and patches that will be used

26. changed mod of sources directory to be rwx and sticky, sticky means that it can be deleted only by owner and added write permissions

    ```
    chmod -v a+wt $LFS/sources
    ```

> Chmod means change mode which is changing the permissions of the file
> - a -> all users [default]
> - u > given to current user only
> - g > given to group of current user
> - \+ add next
> - w -> write , r -> read, x -> execute, t-> sticky bit
> - -R > means recursive, given to all files in whole directory
> There's a way to change mode without letters and it is by knowing bitmasks used for permissions, for an owner
> 1. read permissions is 0400
> 2. write permission is 0200
> 3. execute permission is 0100 
> And a bitwise or is done on them to know what to do, so in binary 111 is ORing of 100 and 010 and 001 which are 4,2,1 and output is 7 meaning rwx 
> but according to the previous position it is 0700 
> and that position is meant to be permissions of the owner 
> first bit from left is sticky bit (t) which is 1000
> third bit from left is group permissions 
> 1. read permissions is 0040
> 2. write permission is 0020
> 3. execute permission is 0010 
> And fourth bit from left is the same but for all users
> and if you used long listing of ls before, you're familliar with rwxrwxrwx 
> so 777 which is the same as 0777 means for first,second,third group of rwx I want 7 (4 | 2 | 1) (rwx)
> and 667 means for first and second groups of rwx I want (4|2) rw- and for third group I want 7 which is rwx
> sticky bit is 1000
> so for example trwx is 1700 
> and t-wx is 1300 because 1000 is for sticky bit, and wx is 010 | 001 which is 011 which is 3 
> so you basically give a number that represents a bitmask for each group of rwx representing 3 bits --- for e.g. 101 mean r-x which is 5 for a rwx group position 
> so `chmod 777 <file_name>` is give rwxrwxrwx which is give 111 111 111 for that file
> and `chmod 667 <file_name>` mean 110 110 111 which is rw-rw-rwx by substituting the bitmask by their positions for each group
> and `chmod 235 <file_name>` is 010 011 101 which is -w- -wx r-x

27. downloaded wget

    ```
    apt install wget
    ```

28. use wget to download all urls from input-file and continue on fail and save in a directory not the CWD

    ```
    wget --input-file=wget-list --continue --directory-prefix=$LFS/sources
    ```

29. create wget-list file

    ```
    vim wget-list ( no copy paste between host and guest cuz console )
    ```

30. creating a shared folder and  added the download links of tools to be used

31. created a bash script to validate needed versions 

32. copied shared files to home directory in vm for safe keeping
    
##### ELAPSED: 4 hours
    
##### OUTPUT: Now Linux vm is fully working with needed resolution and has automatic mounting, LFS system partitions created and mounted, sources collected, needs downloading and compilation only then building starts.
--- 

## [DAY 4] (5/27/2020) [SSL/TLS ?? and What the Hell is a TLS Handshake]

---

1. Found out that `/tmp/` gets cleared and `@reboot` of `crontab` executed at boot before `xinit`, note: I still have no ___GUI___ just a ___console___ 
> ___/tmp___
> is a directory that gets automatically cleared at boot of system

> ___Crontab___
> A file that can be accessed using `crontab -e` that has jobs to be executed automatically at given times

> ___xinit___
> A command that starts the Xorg Server (X-server)

2. Tried connecting using `WIFI` , found out that no `WIFI` interface was configured automatically
    ```
    ip addr show
    ```
> shortcut
> ip a

3. Ethernet is the only interface connected automatically, you can know by seeing an IP under the interface name from prev. command
> Ethernet's interface starts with ___eth___ for e.g. __eth0__

4. Starting download progress using 
    ```
    wget --input-file=wget-list --continue --directory-prefix=$LFS/sources/
    ```

5. wget downloaded some files but not all due to errors

6. found out that default of `wget` is `verbose` so to output errors only I need `-nv` flag and `--output-file=logfile` flag

    ```
    wget --input-file=wget-list --continue --directory-prefix=$LFS/sources/ -nv --output-file=wget.log
    ```

7. Found out why wget didn't download all files 
    ```
    ...
    ERROR: The certificate of 'github.com' is not trusted.
    ERROR: The certificacte of 'github.com' doesn't have a known issuer.
    ERROR: The certificate of 'www.sourceware.org' is not trusted.
    ERROR: The certificate of 'www.sourceware.org' doesn't have a known issuer.
    ...
    ```

8. Read more about SSL Certificates and CA and how SSL works

> ___SSL___
> Secure Socket Layer is a protocol that is used with HTTPs data where 
> the HTTP data is encrypted and secured in a way that can't be decrypted
> by anyone except the server and corresponding client as it uses 
> Unique keys per session for corresponding client and server

> ___CA___
> Certification Authority, It's the Authority that gives the SSL certificates 
> to webservers requesting SSL certification to have HTTPs functionality for the site
> and have the ability to encrypt the data being sent from server 
> and decrypt the data sent from client

> ___TLS___
> Transport Layer Security is the improved version of SSL and they're 
> the same but TLS is called SSL usually

> ___Public Key___
> public key is a key generated at the webserver that is exposed to anyone trying to connect to the webserver

> ___Private Key___
> private key is a key that is generated at the webserver and is private to the webserver only, not exposed to anyone

> ___Asymmetric Encryption___
> It's an encryption technique that involves two keys, a public one to encrypt and only one another key to decrypt the message encrypted
> The public key of the server is the one that encrypts, and it can only be decrypted using the private key of the server

> ___Symmetric Encryption___
> An encryption technique that uses only one key to encrypt a message and knowing that key gives the ability to decrypt that message 
> using the same Algorithm of encryption but a reverse engineered version of it

> ___TLS Handshake___
> TLS handshake is is series of operations to be done to verify that you're talking to a valid Domain
> not someone trying to steal data from you (MTIM Attack) what happens is,
> 1. The Client sends a message to the web server to attempt connection, along with it is a client-random string of bytes
> 2. The server responds to the client with with its SSL certificate and from which CA 
> it is authorized from and the server's public key and a string of random bytes
> 3. The Client now checks whether the sent SSL certificate sent is authorized by the CA mentioned or not
> by comparing it with SSL certificates authorized by CAs (That's why `wget` fails)
> 4. After Client verifies SSL certificate, uses the public key sent by Server to encrypt a message and sends it to the server 
> 5. The Server decrypts the send message using its private key
> 6. The server creates a Session key using the decrypted message along with random bytes sent by client and generated by server
> 7. The client and server uses a Symmetric Encryption technique (RSA) to encrypt sent messages and decrypts recieved messages

> ___RSA___
> A very hard to break Symmetric Encryption method
> To give an idea of how long it takes to decrypt RSA without knowing the key
> it would take ___ 2-3 years of continous work using the fastest computers to break a 512 bit RSA key___ 
> The used RSA key in http data encryption is a 2048-bit RSA key and sometimes 4096-bit RSA key
> PS. a single RSA-2048 message can be cracked in around ___300 trillion years___ give or take 

9. `wget` didn't work because current system doesn't have any SSL certicates or Certifcate Authorities to check against, hence

    ```
    ERROR: The certificate of 'github.com' is not trusted.
    ERROR: The certificacte of 'github.com' doesn't have a known issuer.
    ```
    which means there's no SSL certificate for github on client
    and there's no known issuer for it (no known CA) 

> When `wget` starts contacting a HTTPs site, it checks the system's __SSL certificates first__ and verifies against them

10. CA SSL certificates can be installed on debian based client using 
    
    ```
    sudo apt install ca-certificates
    ```
> which will install them on under /etc/ssl directory

11. `/etc` is the directory used to hold System's Configuration ( Default configuration most of the time )

12. `/usr` is the directory used to hold System's user configuration

> That's why config files existing in `/usr` Overrides config files in `/etc` in terms of which one is used by the System

> config files are checked in `/usr` directory first then `/etc` if not found

13. Now starting `wget`'s download again ___[WORKED]___

##### OUTPUT: Now All needed tools and patches are downloaded in sources directory and all needed ssl certificates and CA are on System
##### ELAPSED: 7 hours

---
## [DAY 5] (5/28/2020) 
---

1. Confirming that I have all needed source files (81 files) (you can view them along with wget-list file in the repository)

    ```
    > ls -l | wc -l
    > 81
    ```

2. found out that `HTTP` is at `port 80`, `HTTPS` is at `port 443` and it is reserved by the System and called `System ports`

3. Created $LFS/tools directory which will hold the compiled software 

    ```
    mkdir -v $LFS/tools
    ```

4. Did a symbolic link of `$LFS/tools` on `root` directory for chapter 6

    ```
    ln -sv $LFS/tools /
    ```

5. read about groups and users and Created an unprivilleged group and added a new user to it for the compilation phase, as till now I'm using the root user and compiling as root is dangerous as it could break the system.

> ___Groups___
> groups are a way in linux to organize users, to view groups, they're stored in `/etc/group`
    ```
    groupadd LFS
    ```
> ___Users___
> Users in linux are given specific permissions on which files they can read, write or execute and own, to view users, they're stored in `/etc/passwd`
> 1. New users must get a password before being able to login to the system
> 2. New users must specify if they want a home directory or not using the `-m flag`, default is none
> 3. New users must specify which shell to use by specifying its path using the `-s option`
> 4. New users must specify which files are to be copied to their home directory (only valid if -m flag is used), default is specified in `/etc/skel` using `-k option`
> 5. Users can have what is called as a base-dir which is the directory that they log in to, default is home directory using `-b option`
> 6. password of users is encrypted and is put int `/etc/shadow`

    ```
    useradd -s /bin/bash -g LFS -m -k /dev/null lfs
    passwd lfs
    ```

> ___/dev/null___
> a file used in linux to discard output, it contains nothing and is most of the time used to discard output by redirecting output to it

6. Found out the bits meaning in long listing format of ls command 
    ```
    bits_representing_info <no_of_files_in_directory> <owner> <group> <size_in_bytes> <last_modified_date> <name>

    <bits_representing_info> is for example drwxrwxrwx
    first bit represents if it is a directory(d) or file(-) or symbolic link (l)
    next 3 bits represents permissions of owner 
    next 3 represents permission of group
    next 3 represent permission of all users
    drwxr-xr-x.  4 root root    4096 Jul 11 21:26 Desktop
    means its a directory that has rwx permissions for owner, read, execute for group, read and execute for other users
    , has 4 files inside it, owned by root and is 4096 bytes of size
    first 3 bits after is permissions for owner of file 
    next 3 bits are permissions of users in same group as owner 
    next 3 bits are permissions of users outside of that group (all other users)
    owning user name 
    group in which owner is in
    r > read for files / able to list directory
    w > edit or delete files / able to delete or add content to a directory
    x > able to execute file / able to change directory into that directory
    owner has access to rwx 
    ```

7. Read about ownership and `chown` and changed ownership of $LFS/tools and $LFS/sources to newly created user to get full access to them without having to be root
    ```
    chown -v lfs $LFS/tools
    chown -v lfs $LFS/sources
    ```
> ___chown___
> Changes ownership of the file or directory to given user, note that, an owner of a file has full access to do what he wants with the file 
> Also to be able to use chmod on a file, you have to be the owner of that file and by default the owner of the file or directory is the one created it

8. Logged in to the System with the new user using a virtual console instead of logging out and in or restarting 
    ```
    su - lfs
    ```
> which means start a login shell for given user

##### OUTPUT: Made sure that all sources and tools exist in virtual machine, got to know about permissions, ownership, users and groups
##### ELAPSED: 2 hours
---
