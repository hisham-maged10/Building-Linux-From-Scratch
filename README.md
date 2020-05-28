## [DAY 1] (5/24/2020)

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

## [DAY 2] (5/25/2020)

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
    ```
    GRUB (referes to Grub2): is the easiest type of bootloader for linux
    bootloader: is a piece of software started by the firmware (BIOS or UEFI) and is responsible for 
    loading the kernel with wanted kernel parameters based on its config file
    firmware: is a very basic software held in ROM that provide low-level interface with 
    device's specific hardware
    ```

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

## [DAY 3] (5/26/2020)

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

___Hard-link___
> mirrors the file with all its contents not just the
> path and have the same inode so doing any change in one will affect the other
> and removing one won't remove the other but accessing any of them will 
> be as if it accesses the other

16. recommended to use SWAP disk so will remove efi partition as not used and use that space for swap

17. `cfdisk`, delete efi partition, use free space to create swap(LINUX SWAP), write,quit, mkswap on created partition

18. created new VDI for LFS System other than AntiX drive, Created root of 10GB, 500mb boot, 1.5GB SWAP

19. root, boot partitions must be initialized to be ext4 type > mkfs -v -t ext4 /dev/sdbx

20. swap must be initialized as swap > `mkswap /dev/sdbx`

21. export $LFS variable, adding it to .bashrc > `export LFS=/mnt/lfs`

22. reload bashrc without reboot > `source ~/.bashrc`

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
    a -> all users [default]
    \+ add next
    w -> write , r -> read, x -> execute, t-> sticky bit
    ```

27. downloaded wget, `apt install wget`

28. use wget to download all urls from input-file and continue on fail and save in a directory not the CWD
    ` wget --input-file=wget-list --continue --directory-prefix=$LFS/sources`

29. create wget-list file
    ` vim wget-list ( no copy paste between host and guest cuz console )`

30. creating a shared folder and  added the download links of tools to be used

31. created a bash script to validate needed versions 

32. copied shared files to home directory in vm for safe keeping
    
##### ELAPSED: 4 hours
    
##### OUTPUT: Now Linux vm is fully working with needed resolution and has automatic mounting, LFS system partitions created and mounted, sources collected, needs downloading and compilation only then building starts.
--- 

## [DAY 4] (5/27/2020)

---

1. Found out that `/tmp/` gets cleared and `@reboot` of `crontab` executed at boot before `xinit`, note: I still have no ___GUI___ just a ___console___ 
>___/tmp___
> is a directory that gets automatically cleared at boot of system

> ___Crontab___
> A file that can be accessed using `crontab -e` that has jobs to be executed automatically at given times

> ___xinit___
> A command that starts the Xorg Server (X-server)

2. Tried connecting using `WIFI` , found out that no `WIFI` interface was configured automatically
    ```
    ip addr show
    ```
shortcut
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
    which will install them on under /etc/ssl directory

11. `/etc` is the directory used to hold System's Configuration ( Default configuration most of the time )
12. `/usr` is the directory used to hold System's user configuration

> That's why config files existing in `/usr` Overrides config files in `/etc` in terms of which one is used by the System
> config files are checked in `/usr` directory first then `/etc` if not found

13. Now starting `wget`'s download again ___[WORKED]___

##### OUTPUT: Now All needed tools and patches are downloaded in sources directory and all needed ssl certificates and CA are on System
##### ELAPSED: 7 hours

---
