---
layout: post
title:  "a 'simple' guide to linux on the surface pro 3"
date:   2016-06-06 22:55:38 +0000
categories: arch linux surfacepro3 guide ubuntu art
---

**edit (2016-07-18): i’ve updated the art software portion of this guide to talk about krita 3.0**

there’s probably a bunch of existing guides on linux on the surface pro 3 out there (most of which i’ve read) but i decided to make my own. each guide was helpful, of course, but i still had to rely on many outside resources to make my linux experience worthwile.

# notes

- to use this guide, you don’t need much linux experience but you should get a bit familiar with the command line and terminals
- i’m not an expert at this stuff but i’ve done quite a bit of research on linux and the surface pro 3 and have had a single boot since september 2015
- i use the surface pro 3 i3 64gb model but this should ofc work on any sp3 device
- i used ubuntu and xubuntu from september 2015 to around december 2015 so my info on those distros may not be as accurate but should still suffice
- i currently use arch linux

# contents

1. starting out: *ubuntu – if you’re new to linux or want an easy to use distro
2. getting fancy: arch linux – if you’ve got a bit of free time and have more experience with linux
3. general reccomendations – post-install stuff you may want to consider
4. conclusion

# starting out: *ubuntu

if you’ve no experience with linux or want the most useable linux experience, go for any of the ubuntu derivatives or plain old ubuntu.

issues i had with a plain install:


1. no battery status (ubuntu and xubuntu)
2. no touchpad support (ubuntu and xubuntu)
3. network issues – could not connect to certain networks (xubuntu)
4. restarting the computer

the first two issues were fixed by simply installing the [neoreeps kernels](https://www.reddit.com/r/SurfaceLinux/comments/3qz8bo/kernels_for_surface_pro_34book/). if you don’t know how to install these kernels, follow the instructions below:


- go to the google drive link and download just one of the folders — as of writing this the folders are `4.2.0`, `4.3.0`, and `latest (torvalds tree)`. i went with 4.3.0.
- open a terminal emulator and cd into the directory where the downloaded folder is (eg `cd Downloads/4.3.0`)
- run `sudo dpkg -i *.deb`. this will install the kernels, which are in deb packages. to use these kernels, restart your computer and choose the kernel you just installed at boot.
- (optional) change the order of the grub entries with [grub customizer](https://launchpad.net/~danielrichter2007/+archive/ubuntu/grub-customizer). in my case, i moved them to the top and reduced grub’s timeout so that startup would be slightly faster.

as for the 3rd issue of networks, i could not connect to my home network with the built in sp3 wifi and had to use a d-link adapter. however, i could connect to my school’s network without the adapter. i never solved this problem when under xubuntu but later had the same issue under arch. unfortunately i can’t remember what the solution was but it involved the fact that more than one [connection manager](https://wiki.archlinux.org/index.php/Wireless_network_configuration#Automatic_setup) was running at once and i had to disable one of them.

the 4th issue of not being able to restart the computer is something i could never fix. “restarting” the computer would shut it down and make it start up again, only to be stuck at the “Surface” screen that would never go away. instead, just shutdown and manually power up your computer again.

for more info on ubuntu on the surface pro 3, check the following links, which will be updated soon:

- [dual booting ubuntu 14.10 on the surface pro 3](http://blog.davidelner.com/dual-booting-ubuntu-14-10-on-the-surface-pro-3/)

# getting fancy: arch linux

do you have a lot of free time on your hands? are you done with ubuntu? do you want some mad internet creds? try arch linux on your sp3!

moving on from that horrible intro, i’d first like to say that my first non-ubuntu distro was [antergos](https://antergos.com/) and it’s a great arch-based distro. i enjoyed it much more than ubuntu, and if you’re too afraid to try installing arch, then i reccomend you try it. and plus, you get to still follow this guide!

as for installing arch, i cannot say much other than follow the [installation guide](https://wiki.archlinux.org/index.php/Installation_guide) and the [beginner's guide](https://wiki.archlinux.org/index.php/Beginners'_guide). and if you find the task a bit too daunting, you can easily find scripts to install arch linux online, as well as video tutorials.

# stuff to do after installing

## get the appropriate kernel

i use matthew wardrop’s [linux-surfacepro3](https://github.com/matthewwardrop/linux-surfacepro3) kernel, and it works pretty well. unfortunately, whenever i attempt to install it via the aur or mkpkg, i always get a kernel panic at boot. but fear not, there’s still a way to install the kernel! the following steps will walk you through it. (and i’m sorry if it sounds very babby-like, but i just want to be as clear as possible)

1. download the repo or clone it: `git clone https://github.com/matthewwardrop/linux-surfacepro3.git`
2. import the kernel maintainer keys, which may take a while. enter these commands into your terminal. (taken from the README):
  - linus torvalds: `gpg --recv-keys 79BE3E4300411886`
  - greg kroah-hartman: `gpg --recv-keys 38DBBDC86092693E`
3. become the package manager and open up the `PKGBUILD` and read it while going through the next few steps
4. find the `pkgver.` depending on what version it is, go to and [download](https://www.kernel.org/pub/linux/kernel/) the corresponding `.tar.xz` and `.tar.sign`. extract the `.tar.xz` into the `linux-surfacepro3` directory
5. apply the patches that the repo provides while in the linux-surfacepro3 directory. you will find the appropriate commands within the `PKGBUILD`

this is the part where i stopped following the PKGBUILD and went on my own. i then followed this guide–[How-to: patch, compile and install a working kernel for the Surface Pro 3](https://github.com/Vistaus/surface3-arch-antergoslinux/issues/8) starting from step 3 (“The actual compiling”) on. just in case that link ever gets taken down, i’ll write the relevant steps here. do these while still in the repo’s directory. and you may or may not want to read the “more info” links on some of the steps.

1. copy the current kernel config
	- `zcat /proc/config.gz > .config`
2. open the menuconfig. i highly reccomend changing the name of the kernel within this menu so that it’s easily identifiable from your other installed kernels.
	- `make menuconfig`
	- [more info](https://www.kernel.org/doc/index-old.html#Creating_a_custom_kernel_configuration)
3. start the actual kernel compilation. as stated earlier, i have the i3 1.5ghz 4gb ram sp3 model. i never recorded the exact time it took me to compile the kernel, but it was less than  2 hours every time i did it. and every time i compiled, the fan turned on and the temperature went up to around 60 degrees celsius. i also used firefox the whole time lol.
	- `make -j3`
4. install the kernel modules. **note**: during one of the next 3 steps, the name of the kernel will be displayed at the end of the output. take note of this, it’s très important.
	- `sudo make modules_instal`
5. more compiling! create the compressed kernel next
	- `make bzImage`
	- [more info](http://linuxdocs.org/HOWTOs/Kernel-HOWTO-5.html)
6. and now install the actual kernel
	- `sudo make install`
7. check if your kernel has installed by heading over to `/boot/` and look at each of the `vmlinuz`. usually, you’ll want to check the file just labeled `vmlinuz` and use the `file` command to see its name. if it matches the name of the kernel that you noted in one of the earlier steps, change its name to `vmlinuz-[INSERT NAME OF KERNEL]`
8. generate the ram disk file with your kernel name. **note**: during one of the next two steps you may get an error about fstab if you have use a btrfs filesystem but it can be ignored.
	- `mkinitcpio -k FullKernelName -c /etc/mkinitcpio.conf -g /boot/initramfs-YourKernelName.img`
	- [more info](https://wiki.archlinux.org/index.php/mkinitcpio)
	- [even more info](https://www.ibm.com/developerworks/library/l-initrd/)
9. upate the initramfs
	- `sudo update-initramfs`
10. inform grub of the changes
	- `sudo grub-mkconfig -o /boot/grub/grub.cfg`
11. (optional): use [grub-customizer](https://aur.archlinux.org/packages/grub-customizer/) to edit the kernel order

wow! that only took forever! now you have a breddy gud arch set up.

# general recommendations

## the hidpi lyfe

i wrote some stuff on [dealing with the hidpi life](https://www.reddit.com/r/unixporn/comments/4g6qe7/bspwm_terminals_are_cool_i_guess/d2nh0wk) on reddit

## touchscreen

i am currently on arch linux with the previously mentioned kernel and have little to no problems with my touchscreen. at times, it may stop working, but this can be fixed easily.

1. find your touchscreen’s id (the touchscreen should be called `NTRG0001:01 1B96:1B05`)
	- `xinput -list`
2. disable and enable the device. replace “15” with the id you found earlier.
	- `xinput disable 15; \ xinput enable 15`
3. just in case this ever happens while my keyboard is unattached, i mapped the previous commands to the super button + volume down. (i use [sxhkd](https://github.com/baskerville/sxhkd) to do this)

i also use the [onboard keyboard](https://www.archlinux.org/packages/community/x86_64/onboard/) when i use my sp3 as a tablet. since i use bspwm, i use `rule -a Onboard state=floating flag=sticky` so that it is on all desktops and is on top of all windows. on my [lemonbar panel](https://github.com/jastronaut/dotfiles/blob/master/bin/panel) i also have an keyboard icon that when clicked, brings up onboard. pretty handy when i’m not using the type cover.

## the sp3 pen

by default, the pen is recognized as another mouse pointer. i suggest you install the [xf86-input-wacom](https://www.archlinux.org/packages/extra/x86_64/xf86-input-wacom/) drivers to get the most out of your pen.

then, the wacom configurations from [this repo](https://github.com/frebib/surface-config/tree/master/xorg.conf.d) to get more options recognized for your pen in xinput. as of version 0.33.0-1 of the wacom drivers, you’ll need to copy the contents of `50-wacom.conf` into the new file `70-wacom.conf` for it to work. (these files are located in `/usr/share/x11/xorg.conf.d/`)

## art software

what i miss the most about windows is how amazing adobe photoshop cc was with the surface pro 3. i’ve tried my best to find an alternative to this with programs in linux, but nothing comes close.

### krita

krita is always recommended as the best painting software, but i’m not sure if i agree.

(this section has been edited to account for [krita 3.0](https://krita.org/en/item/krita-3-0-release-candidate-1-released/) as of 2016-07-18, or the release with animation)

**krita cons**

- ~pen pressure is extremely weak with the sp3 pen (even with tweaking and the above configurations)~ i don’t have this problem anymore with the new krita.
- it detects your hand as input while you’re using the pen so you must disable the touchscreen to make it useable.
- the display is not helpful at all to hidpi users. and there is not much you can do to fix it, even when enabling larger icons.
	- as of the new krita version, i still have an issue with this. see screenshots below for reference.
- this isn’t really the fault of krita itself, but you must install a lot of kde stuff for it to work, which really doesn’t fit my philosophy of having minimal installs.

some screenshots of krita:

![krita under bspwm](/assets/img/krita-normal.png)

*krita under bspwm*

![krita, with the hidpi setting turned on. it’s… pretty bad.](/assets/img/krita-hidpi.png)

**krita pros**

- has many more painting-specific features than any other linux painting program.
- i really love the reference image window. (like in paint tool sai)
- (3.0+) has support for creating animations
- cute mascot
- (3.0+) *some* hidpi compatibility — it doesn’t work in my case, however.

unfortunately, i really can’t reccomend you use krita. it’s really not worth the trouble at the moment, but i do intend to keep up on its progress (or even try to contribute to it) because it has so much potential.

### mypaint

i enjoy mypaint much more than krita for many reasons.

**mypaint pros**

- not as resource intensive as krita, which would always turn my sp3’s fans on.
- undo and redo buttons are on screen and not hidden behind a menu, which is useful for when the keyboard is detached.
- slightly larger interface and incorporates your gtk2 theme.
- simple and easy to use. you could use it as an ms paint alternative if you really wanted to do so.

**mypaint cons**


- despite having a menu to disable the touchscreen so that your hand doesn’t get in the way, it doesn’t always work. like krita, you must disable the touchscreen via xinput to make sure that there will be no interference.
- not as many features as krita, such as selection tools.
- no tabs, so you can only have one thing open at a time 🙁

# conclusion

altogether, using gnu/linux on your sp3 may seem much more work than it’s worth, but i enjoy it so much better than windows. it is much more modular and does not waste as much disk space. in addition, the terminal and easy to install compilers make writing code so much easier, as a hobbyist programmer.

the only thing i really want windows for is adobe photoshop cc for art-related tasks, and if i get a new surface model, i may keep windows on it for that sole reason. other than that, i think linux is great, and i think that whether or not you are a programmer or try it on your sp3, you should look into linux!

thanks for reading and you can email me at [hello@blog.jasdelgado.com](mailto:hello@blog.jasdelgado.com) with any questions or suggestions about this article.

