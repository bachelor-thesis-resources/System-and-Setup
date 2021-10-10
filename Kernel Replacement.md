# Obtaining the Kernel

Refer to the custom [Xenial Kernel Patch](https://gitlab.com/thesis-sustainable-software-systems/eteam)
project. The README file describes how to build this particular kernel on your own. 

Please make sure to backup the kernel packages to some external system or drive. Building
them is time consuming. They might come in handy.

# Injecting the Kernel

It is highly recommended to not use the Kernel on a machine thats purposes for other tasks than
experimenting with energy measurements using Intel RAPL. (For example: don't use it in operative 
environments) Please be aware that using this kernel doesn't come with any warranty. Installing 
this kernel for machines with a version (or distribution) different from Ubuntu Xenial LTS
comes with even less waranty than mentioned before.

## Installing the obtained `.deb` - files

The first step is fairly simple (and obiously not the hardest in this particular case):

Copy the mentioned files in a separate directory, then run:

```bash
sudo dpkg -i ./*.deb
```

Running the command should already update `initramfs`.

Please make sure to not use USB 3.0 ports for connecting mouse and/or keyboard. The drivers will likely be missing in the installed kernel. 

## Change the GRUB Bootloader Configuration

Default Ubuntu Xenial configuration doesn't bring up the GRUB interface on system startup.
It is up to you, how to configure GRUB to be able to boot from the recently installed kernel.
However, after configuration you should be able to do the following things:

- choosing the kernel to boot with after powering on your machine
- entering recovery mode for the installed kernel.

### Reinstall missed packages

Some of the Kernel packages will likely fail to install, due to version conflicts between packages of your currently running kernel (4.15.0 I think).
To fix this, change to the directory where your kernel *.deb files are located and repeat aforementioned `dpkg` command.


## Fix your X-environment

After booting with the installed kernel, you likely won't be able to interact with your system
through your X environment. In order to fix that, there are two possibilites:

1. Boot into recovery mode (using our kernel). Then enable network support. After that, enter a root shell
2. Switch to TTY when requested to login by your X environment and login using your credentials. (In Ubuntu Xenial you probably will have to use `CTRL-ALT-F[1-7]`)

On your shell, you should firstly uninstall lightdm, ubuntu-desktop and gdm (if any):

`apt --purge remove lightdm ubuntu-desktop gdm`

After that, reinstall `ubuntu-desktop`. (`gdm` might be required).

The installed kernel makes some changes to the standard linux thread scheduling. 
This will propably lead to issues with real time scheduling, namely `pulseaudio` which is using `rtkit`.
You should uninstall them as well:

`apt --purge remove pulseaudio rtkit`

In addition to that, you should run a full upgrade for your packages: 

`apt full-upgrade`

Then reboot and try log in by using your X environment.
