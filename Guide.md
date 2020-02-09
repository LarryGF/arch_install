# The Arch Linux installation guide for the unlucky noob with limited connectivity

First of all, let me begin by saying that, of all the distros that I've tried, Arch is my favorite one by far (and believe me, I've tried a lot: Debian, Ubuntu, Kubuntu, mostly every Manjaro flavour out there, Linux Mint...). It's highly customizable, it's bleeding-edge (meaning that you'll have access to the latest packages with little to no delay), it has a very granular installation process (it's almost af if you were building your OS with Lego blocks, you use small blocks and end up building whatever you need), an extremely efficient and functional package manager, and, what's probably my favorite feature (albeit not being an inherent feature of the distro): "The Arch Wiki", if you have a problem, chances are you're gonna find the solution in the wiki, or at least get a hint as to what you can do to solve it (bonus points for being available as a package too, but more on that in a while).
Having said that, even as I strongly recommend it as a day-to-day distro, I wouldn't recommend to use it for your production servers unless you're a person that really enjoys the thrill of knowing that your production servers can crash and burn at any moment if you're not careful.

## Reason of this walkthrough

I'm not gonna lie, if you have little to no experience with Linux, Arch will be a handful, well, to be honest, even people that have experience with other Linux distributions will sometimes get frustrated with Arch, but don't worry, usually that frustration fades away in a couple weeks and turns into worship :laughing:. To be fair, the hardest part for people that are new to Arch is the installation process, after that, everything is very similar to most modern Linux distributions. Because of that ,I've argued with my colleagues several times over the years over the practicality of encouraging people that are new to Linux to start using Arch Linux as their first distribution. Although I think that: "you can't punish a person that has no clue about Linux by forcing them to install a distro that goes out of its way to make things hard for newbies" is a valid point, I also think that if you manage to pull through on the first weeks, get your installation working, make a ton of mistakes and fix all the errors that you're likely to get on your first try, you'll be extremely well prepared to handle whatever this crazy _Free Open Source Software_ world may throw your way. Just transitioning from having nothing to having a fully fledged Arch installation can be a big difference in the learning process of someone that's just starting in the Linux world.

Since I'm usually tasked with helping out students or friends that are planning of diving in into the Arch world for the first time, I've found myself fixing the same errors and explaining the same concepts over and over again. To be fair, I wasn't looking forward to doing it again, and since `Anything that you do more than twice has to be automated.(Adam Stone, CEO, D-Tools)` this left me with two choices to free me from the hassle of repeating the lecture one more time:

- Start convincing people to use something that uses Arch, but is easier to install (like **Manjaro**). Frankly this goes against everything that I was trying to achieve by motivating people to install Arch, so this was a hard **no**.

- Making a walkthrough that explained the installation process in the simplest, most intuitive way I can, trying to show some useful tools, and solving some common problems that won't only help them during the installation process, but in their day-to-day use.

Honestly, the second one was always the obvious choice :man_shrugging:

## Things to point out

- If you're looking for a simple, step-by-step, "do this then that" tutorial, this article is definitely not for you, a simple Google search can give you easily four or five of those. On the other hand, if you like to find out new things by yourself, researching, making mistakes, and don't mind breaking a few things once in a while, the you'll probably want to start with the official [Installation Guide](https://wiki.archlinux.org/index.php/Installation_guide) and start branching to other articles from there as you see fit.

- Take everything I say with a grain of salt, sadly, not everyone will have the same problems I've had (even though I've had enough for two or three lifetimes :sweat_smile:), nor everyone will use the same tools as me, or even agree with me on the way I do things, everyone has their own way of doing stuff and whatever works well for you it's usually the best way (except when it's not :laughing:). This article is basically a compilation of "best practices" I've learned from installing Arch over and over again and dealing with my own and other people's errors.

- Since I live in a country where internet access is very limited there will be some parts where I will propose some "workarounds" for those in the same situation as me, for the immense majority of you these won't be relevant, but hey!, just in case there's a zombie apocalypse and the only way to stop the annihilation of the human race is to install Arch without internet access.

## Installation

### Preparing your environment

Before you can start the actual installation process there are some preparations you must do, all of these will get a dedicated section later, so don't worry if you don't know what to do at first glance:

1. Get the installation image

2. Create a bootable drive for your installation (you should use an _usb flash drive_ for this, we're living in the XXI century)

3. Verify your computer's boot mode

4. Connect to the internet or use a local repository

5. Partition and format the disks

6. Mount the file systems

#### 1. Get the installation image

If you already have an `.iso` file you can skip this step, the `.iso` files get updated periodically (it's a `bleeding edge` distro after all), it doesn't hurt to have the latest image at hand, but it won't stand in the way of having an updated system, there can be some inconveniences using an older image with a current repository, but more on this later...

First you need to download the image that contains all the basic packages and tools for your Arch installation, you can download from [here](https://www.archlinux.org/download/), select the mirror closest to your country for lower latency. You should have a file named similar to this: `archlinux-2019.11.01-x86_64.iso`.

If you downloaded the image from an `http` server and you're worried that your image could have been tampered with you can also find the image signature in the [Downloads](https://www.archlinux.org/download/) page under `Checksums`.

#### 2. Create a bootable drive for your installation

After you have the image the next step would be to create a bootable media, there are two ways of doing this depending if you come from **Windows** or **another Linux distro**:

- Windows: I can't provide much technical support here, I usually go with [Rufus](https://rufus.ie/), then use the `arch .iso` and then it will prompt you to choose between a "traditional copy" and `DD mode`, go with the latter.

- Linux: here you'll have to use the `dd` command, this isn't a risky process by itself, but if you're not paying attention to what you're doing you can end up loosing all your data, so **PAY ATTENTION TO WHAT YOU'RE DOING FROM NOW ON**:

  - With your USB plugged in (obviously), find out the name of your USB with the `lsblk` command, it should output something similar to this:

    ```
    NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sda           8:0    0 931.5G  0 disk
    ├─sda1        8:1    0 886.5G  0 part /run/media/larry/Data
    ├─sda2        8:2    0    16M  0 part
    └─sda3        8:3    0    45G  0 part
    nvme0n1     259:0    0 238.5G  0 disk
    ├─nvme0n1p1 259:1    0   511M  0 part /boot
    ├─nvme0n1p2 259:2    0   3.5G  0 part [SWAP]
    ├─nvme0n1p3 259:3    0   100G  0 part /
    └─nvme0n1p4 259:4    0 134.5G  0 part /home
    ```

    As you can see, this shows all the drives in your computer (under `TYPE` they have **disk**) and their partitions (under `TYPE` they have **part**). You need to find the `NAME` of your _drive_ **not** of the _partitions_ of that drive. You also must to verify that the _drive_ or any of its _partitions_ are **not** mounted (there's nothing under `MOUNTPOINT` for those entries).

    An easy way to find out which one is your USB drive is to see which matches the `SIZE` of your flash drive.

- After finding out the name of your USB (let's say, for the sake of this guide that it was `sdb`) you need the contents of the file you downloaded in `Step 1` (let's assume that it's located under `~/Downloads`, so the path to the `.iso` would be: `~/Downloads/archlinux-2019.11.01-x86_64.iso`). You have to execute the following command as `root`, I'll explain what all of the options mean below:

  - bs (block size): the sizes of the blocks that will be written to your media

  - if: the input file from which the command will read the data (it's the path to your installation media)

  - of: where the output should be written, all **block devices** can be found in the `/dev` folder, we were using block device `sdb` so the destination must be `/dev/sdb`

  - status=progress: by default `dd` doesn't show anything, and since this is an operation that can take some time, it can be frustrating staring to a blank screen for a while.

  - &&: in `bash` you can do `foo && bar` to first run `foo` and after it finishes, run `bar`

  - sync: when you're copying files the content of the files first get stored in RAM and then they get copied to persistent storage over time, running `sync` forces the synchronization of cache and persistent storage. This is really important because even when the `dd` process shows that it has finished, there might still be some lingering data that hasn't been copied to the USB.

    ```bash
    dd bs=4M if=~/Downloads/archlinux-2019.11.01-x86_64.iso of=/dev/sdb status=progress && sync
    ```

#### 3. Verify your computer's boot mode

\*\* Something to clear out: when I write `BIOS` I'm referring to the BIOS firmware, and when I write **BIOS** I'm referring to your motherboard's Basic Input/Output System,

There are two types of firmware from which your computer can boot: `BIOS` and `UEFI`, the later is more modern and has become the standard in almost every modern computer, so chances are very high that this is your computer's default boot mode. Depending on which one you are using your system will need an `MBR Partition Table` (for `BIOS`) or a `GPT Partition Table` (for `UEFI`). You can install Linux on a `UEFI` system with an `MBR Partition Table` but I can't think of a good reason as to why anyone would want to do that (this is not the case for Windows, since it forces you to use `MBR/BIOS` or `GPT/UEFI` so keep it in mind if you want a dual boot system). I will only cover the `UEFI` method in this guide since it's the more modern one and the one I believe everyone should be using, `MBR/BIOS` has given me too much bitter moments in the past...

To check which boot mode your computer supports you can go to your **BIOS** settings (usually pressing `F2`,`F10` or `Del` keys during boot) and looking for the pertinent option. If it says `UEFI` you're good to go, some **BIOS** can have a `MBR`, `BIOS` or `Legacy` option that can either mean that they're booting in `BIOS` or that they're booting by default on `UEFI` but if they detect an `MBR/BIOS` setup they'll boot `BIOS` instead, this is up to your motherboard's manufacturer to be honest and you'll have to figure that out on your own.

Once you have booted into your installation media (you can do this either by going to your **BIOS** settings and changing the boot order and giving your USB top priority or using the option that comes in most computers to select which media to boot from) don't be scared, it will greet you with an empty prompt, and that will mean it worked. Arch don't come with an installer, in this case **you** are the installer, Arch will only provide you with the tool to install it on your system.

Once you're in the shell, you can check if your system has booted into `UEFI` mode by typing:

```bash
ls /sys/firmware/efi/efivars
```

If the directory does not exists your system may have booted in `BIOS` mode, you should do some tweaking in your **BIOS** settings.

#### 4. Connect to the internet or use a local repository

For you to be able to install Arch you need an internet connection (roughly 800MB of data should be downloaded for a basic installation, and around 1-2GB if you want to use a Desktop environment). If you don't have internet access you can still install if you have a **local copy of the Arch repository** (how to do this is beyond the scope of this guide for now), this can be stored in your computer, an external hard drive or in another machine connected through your computer via LAN.

##### Repository stored on an external hard drive or your computer

These two approaches are very similar, first you search for the partition where your repository resides, you can use the `lsblk` command for this, and then you mount it to a folder, you can then mount that partition in a folder you create using these commands (let's assume it's located on `/dev/sdc1` and we'll create a folder called `repo` on the _root_ folder):

```bash
# Create the /repo folder
mkdir /repo
```

```bash
# Mount the partition in the /repo folder
mount /dev/sdc1 /repo
```

##### Connecting to the internet or to another machine over LAN

To know the status of your network interfaces you can use the `ip` command:

```bash
ip a
```

This should output something similar to this, it's highly likely that the ethernet and wireless interfaces have a different name on your computer, but they'll follow a similar naming convention:

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp7s0f1: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN group default qlen 1000
    link/ether 98:28:a6:0b:8f:f7 brd ff:ff:ff:ff:ff:ff
3: wlp0s20f3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 94:b8:6d:c7:ab:5a brd ff:ff:ff:ff:ff:ff
    inet 192.168.2.2/24 brd 192.168.43.255 scope global dynamic noprefixroute wlp0s20f3
       valid_lft 2031sec preferred_lft 2031sec
    inet6 fe80::d54a:d815:bed7:91bd/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```

- lo: the loopback interface is a virtual interface that your computer uses to communicate with itself

- enp7s0f1: the ethernet adapter

- wlp0s20f3: the wireless adapter

Let's assume you'll use the ethernet cable to connect to the internet or to your second computer with the repository. It's highly probable that if you're connecting to the internet your computer will use `DHCP` to automatically get an IP address (`dhcpd` is enabled by default in the installation media). In case your network doesn't have `DHCP` you can always set your ip address manually to match the configuration of your network:

- If, as shown before, the status of your ethernet interface is: `state DOWN`, you can bring it up using:

  ```bash
  # change enp7s0f1 with your interface's name
  ip link set enp7s0f1 up
  ```

- After that, you can change your ip/subnet using by typing:

  ```bash
  #change ip/subnet and interface to match your network
  ip a add 10.42.0.34/24 dev enp7s0f1
  ```

- You can check if you're connected to the internet by 'pinging' `google.com`:

  ```bash
  ping google.com
  ```

\*\* If you're going to connect 2 computers in LAN you have to make sure that they're both in the same subnet, subnetting is beyond the scope of this guide, but as a rule of thumb you can always set them with `{your prefix}`.`{any number from 1 to 254}`/24, so {your prefix} could be `10.42.0` and one machine could have the ip `10.42.0.1/24` and the other `10.42.0.2/24`. You can verify that they're connected if they can `ping` each other: `ping 10.42.0.1` from `10.42.0.2` and vice versa.

\*\* Once your computers are connected, if you want to have access to your repository you can serve it over `http`, a very easy to start an `http server` from the command line is to navigate to the folder where your repository resides and execute:

```bash
python -m http.server
```

This will start an http server listening over port 8000, if you want to use a different port, you can run it with the `-p` parameter:

```bash
python -m http.server -p 9000
```

#### 5. Partition and format the disks

##### Partitioning

When you're installing an Operating System, it seems that the installer is doing some magic and weird voodoo on your computer, but what's actually happening is that it's copying a bunch of files that contain the packages, tools and configurations neede for your system to run... it doesn't look very magical now, eh?

With this in mind, it's clear that what we must guarantee in this step is that your files get written to your hard drive. In order to achieve this you must first create the necessary partitions, format them, and the mount them to your installation media's filesystem.

This is not carved in stone, but this partition scheme works for me and I think it will work for you too, it keeps everything neat and separated. I'll keep the sizes of the partitions as percentages so it serves as a rule of thumb:

```
- EFI partition: 512MB
- Root partition: 30%
- Home partition: 65%
- Swap partition: 5%
```

- EFI system partition: this is the partition where your boot manager, kernel and initramfs will reside, honestly, those files won't take more than 300MB, but those 200MB won't hurt anyone.. It's the first partition loaded by your **BIOS** and it's in charge of starting your system (more on this later).

- Root partition: this is where your system and all your installed packages will reside, it's advisable to give it at least 60GB so you can have some maneuverability and don't have to be uninstalling packages.

- Home partition: this is where your users configuration will be, it's safer to have all your users data in a separate partition in case you have to reinstall you won't loose your configurations and data.

- Swap partition: it's a part of your hard drive that is used for `virtual memory`, basically, this partitions serves as a "backup RAM" in case your RAM runs out of space. This is not a magic solution that will automatically double your RAM or something like that, this is a failsafe in case your computer runs out of memory. Windows does something similar to this with a `pagination file` but you don't have much control over that. As a rule of thumb it should double your RAM size, but to be honest, with more than 8GB of RAM you'll rarely swap.

Ok, now we'll begin with the actual partitioning process, to do that I'll use the `parted` package that comes preinstalled in the installation media. Be aware that if you intend to keep any data you should back it up at this point if you haven't already, because these steps **WILL ERASE ALL YOUR DATA**. Of course, if you already have a GPT partition table, and want to risk it it's okay too, but make a backup just in case.

When you type `parted` on your shell it will automatically select a block device for you, sometimes, it doesn't select the drive you wanna install your system to, so you can do (assuming you wanna install to `/dev/sda`):

```bash
parted /dev/sda
```

and then `parted` will greet you and give you an interactive shell:

```
GNU Parted 3.3
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) _
```

You can use `print` to see your drive's information and partitions:

```
(parted) print
Model: ATA ST1000LM048-2E71 (scsi)
Disk /dev/sda: 1000GB
Sector size (logical/physical): 512B/4096B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system  Name                          Flags
 1      1049kB  952GB   952GB   ext4
 2      952GB   952GB   16.8MB               Microsoft reserved partition  msftres
 3      952GB   1000GB  48.3GB  ntfs         Basic data partition          msftdata
```

In the example you can see the disk already has a `Partition Table: gpt`, but assuming that you want to start from scratch (**THIS WILL DELETE EVERYTHING IN YOUR HARD DRIVE AND IT'S INCREDIBLY HARD TO RECOVER, SO WATCH OUT**) you must first execute:

```bash
(parted) mklabel gpt
```

This will erase everything and create a new `GPT` partition table. Now we need to actually create the partitions. The command to create partitions is `mkpart` and you use it:

- part-type: is one of `primary`, `extended` or `logical`, and is meaningful only for MBR partition tables.
- fs-type: is an identifier for the type of file system your partition will be using, it does not actually create the file system, the `fs-type` parameter will simply be used by `parted` to set a 1-byte code that is used by boot loaders to "preview" what kind of data is found in the partition.
- start/end: where does the partition starts/ends on the device

  ```bash
  mkpart part-type fs-type start end
  ```

Assuming we're working on a 1TB hard drive the command sequence to obtain the desired partition scheme:

- Create the EFI System Partition (ESP):

  ```
  mkpart primary fat32 1MiB 512MiB
  ```

  The ESP needs to be in `fat32` in order for your **BIOS** to recognize it, and we give it 512MB (roughly). After that we need to set the `esp` flag for the drive, this way the **BIOS** knows which partition to boot from, and we achieve it with this command (1 is the number of the `ESP` partition because it was the first to be created, if you created another partition check with `print` before you do it):

  ```
  set 1 esp on
  ```

- Create Root and Home partitions:

```
#root
mkpart primary ext4 512MiB 30%
```

```
#home
mkpart primary ext4 30% 95%
```

- Create Swap partition:

```
mkpart primary linux-swap 95% 100%
```

And then exit `parted` by typing `quit` on the shell.

##### Formatting

All the partitions are created, now it's necessary to format them with the appropriate filesystem, and for that we'll be using the `mkfs` command (assuming you created the partitions in the same order as I did, and that your drive is `/dev/sda`):

- ESP:
  ```bash
  mkfs.vfat -F 32 /dev/sda1
  ```
- Root and Home:
  ```bash
   mkfs.ext4 /dev/sda2
  ```
  ```bash
  mkfs.ext4 /dev/sda3
  ```
- Swap:
  ```bash
  mkswap /dev/sda4
  ```

Just by formatting the swap partition is not enough, we have to tell the system to use it as swap, and we do it with the command:

```bash
swapon /dev/sda4
```

#### 6. Mount the file systems

In previous sections I talked about file systems, and made you mount disks without giving much explanation, if you did all of that without doing some research of your own, thank you for your trust! Well, this is a good moment to give some background as to what file systems are and what happens when you mount a disk. Honestly, this section could have been literally 3 commands, but I think it's a wonderful opportunity to learn something new that could help you understand your system better.

A filesystem is a hierarchy of directories that is used to organize files on a computer or storage media. On computers running Linux, the directories start with the root directory, which is the directory that contains all other directories and files on the system and which is designated by a forward slash (`/`). Mounting is the attaching of an additional filesystem to the currently accessible filesystem of a computer. When you mount a file system you need two things: a `storage device` and a `mount point`. The storage device can be a partition on a hard drive, a CD-ROM, a USB drive, or any other external media. The mount point is the directory (usually an empty one) in the currently accessible filesystem to which an additional filesystem is mounted. It becomes the root directory of the added directory tree, and that tree becomes accessible from the directory to which it is mounted, any original contents of a directory that is used as a mount point become invisible and inaccessible while the filesystem is still mounted.

Let's say that you have a partition (`/dev/sdb3`) that contains the following folder structure:

```
root_of_your_device
├─ foo_folder
└─ bar_folder
   └─ random_file
```

And you mount it to the `~/arch` folder that contains the following folder structure (`mount /dev/sdb3 ~/arch`):

```
arch
├─is
├─the
└─best
```

When you check the contents of the `~/arch` folder it will be exactly what was in `/dev/sdb3`, the contents of the `~/arch` folder will be hidden because, to your system, the files and folders inside `/dev/sdb3` will effectively be the contents of the `~/arch` folder:

```
arch
├─ foo_folder
└─ bar_folder
   └─ random_file
```

As I said before, the installer just creates the folder structure and copies everything your system needs to run into your system(`/`) partition, starting from the root directory. Let's think some more as to what you have to do to guarantee that everything gets copied correctly.

Right now you're working on an Arch installation that's running on the filesystem of your USB drive. First you'll have to mount the file system of your `root` partition somewhere in your running Arch installation, in theory, this would be enough, when you start copying all of the needed files it will create the necessary folder structure and you'd be good to go... if you were using `BIOS/MBR` maybe this would have been enough, but using `UEFI` doing it this way will render your system unable to boot. Not to mention that having all your system files and user data in one partition makes it a single point of failure and as soon as something breaks, everything breaks. What to do then?

- We were on the right track, you have to mount your `root` partition in a folder, for the sake of this guide, mount it to the `/mnt` folder:

  ```bash
  #since the previous step my root partition was /dev/sda2
  mount /dev/sda2 /mnt
  ```

- Now, your user data and configs reside in `/home/{your user}`, if the system fails or you break something, you wouldn't wanna loose your data, then you'll have to create a `/home` folder in the root of your system partition and mount your `Home partition` in it (remember that your system partition is located in `/mnt`):

  ```bash
  #create the home folder in your system partition
  mkdir /mnt/home
  ```

  ```bash
  #since the previous step my home partition was /dev/sda3
  mount /dev/sda3 /mnt/home
  ```

- If you noticed in the previous step, your bootloader, kernel and initramfs need to be located in the `EFI System Partition`, otherwise your system won't boot. During the installation process those files get written in the `/boot` folder, therefore, you'll have to create a `boot` folder in your root partition, mount your `ESP` partition there so when all the files are getting copied, they get copied to the `ESP` instead of your system partition:

```bash
  #create the boot folder in your system partition
  mkdir /mnt/boot
```

```bash
#since the previous step my ESP was /dev/sda1
mount /dev/sda1 /mnt/boot
```

After you finish doing this when you run `lsblk` it should output something similar to this:

```
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda    259:0    0 238.5G  0 disk
├─sda1 259:1    0   511M  0 part /mnt/boot
├─sda2 259:3    0   100G  0 part /mnt/
├─sda3 259:4    0 134.5G  0 part /mnt/home
└─sda4 259:2    0   3.5G  0 part [SWAP]
```
