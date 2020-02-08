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
