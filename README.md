# NOTES: Creating Arch Linux
## Partitioning

Checked the internet connection by doing the ping -c command.

Next, partition the disk by using cfdisk

- Within the interface of cfdisk I hit enter on "new" to create a new partition.

- Then I created a partition with 512 mega bytes, hit enter and click on "primary". The first partition was creted.

- Then, to create the second partition was created with 29.5G, and hit enter on "primary.

Then, to make these changes permanent, I hit enter on "write" then typed "Yes" to confirm that I wanted it to save. Finally, I hit "quit" to exit.

Then, in relation to partitioning, we print out a list to confirm the above work by typing **fdisk -l** and hit enter.

The list then outputs the two partitions that were made.

## Creating a file system within partitioning

uses the command **mkfs.ext4** which creates a file system on the drive called **/dev/root_partition** where the partitions are located.

To do this we:
- create a file system for **/dev/sda1**
- and another file system for **/dev/sda2**
- and press enter each time each of the file systems are created.

Next, we need to mount the file systems.

- We tell the partitions where they are going to be stored and makes them more accessible
- We do this by typing in the command line **mount /dev/sda2 /mnt** and starting with sda2 rather than sda1.
- For **sda1** we type the following: **mount -- mkdir /dev/sda1 /mnt/boot** 

## Installation

To install essential packages: **pacstrap -K /mnt base linux linux-firmware**

- base, linux, and linux-firmware are all packages being installed

## Fstab

The following command in the terminal:
**genfstab -U /mnt >> /mnt/etc/fstab** somehow labels the partitions in the file.

## Chroot

Here I am going to call a command that changes my room into the new system.

    arch-chroot /mnt 

NOW I AM IN THe SYSTEM!

## Time Zone

To change the time zone of the operating system, I will call the following command in the terminal:

    ln -sf /usr/share/zoneinfo/America/Chicago/etc/localtime

Next, to ensure that the internal hardware clock is the same as the software clock, we create this command:

    hwclock --systohc

## Localization

I edited the locale.gen by using vi.

Then, I ran the command:

    locale-gen

which generates needed locales


Next, we set the language variable to be in English. To do this we edit the file falled:
    
    /etc/locale.conf

and I am going to edit it using **vi**. So it would look as follows in the command line:

    vi /etc/locale.conf

While in the file I added a command called:

    LANG=en_US.UTF-8

This command applies the English language.

## Network Configuration

Now, to creat the hostname file:

    vi /ect/hostname

I called it:

    Admin_Sarah

## Root Password

To create a new password for the root 

    passwd

Then as it prompts me to enter a password: I enter a password of my choice.

## Boot Loader

I used packman to install GRUB

    pacman -S grub

Now, I that the package is intalled, I need to use the followign command to install GRUB to my hard drive.

        grub-install /dev/sda

  Then, I run the following command which generated the grub configuration file.

    grub-mkconfig -o /boot/grub/grub.cfg

(I was confused about grub because most commands specified the target and I did not know what target to use. I ended up running without the target flag and it worked successfully)

### Install network Tools:
Here we install dhcpcd which enables the internet.

    pacman -S dhcpcd

To enable this service:

    systemctl enable dhcpcd


## Reboot

First I exited the chroot by clicking on the keyboard:

    Ctrl+d

Then, to unmount the partitions that I am using, I do:

    unmount -R /mnt

Then I typed:

    reboot

## Post Installation

I installed sugar-fructose:

    pacman -S sugar-fructose

(Here I had originally installed kitty and firefox, but they did not work with sugar, so once I realized this, I installed sugar-fructose.)


I am going to install Sugar DE. This is catered to children ages 5-12. It is DAH best desktop environment... PERIOD!

    pacman -S sugar

Then, we install LightDM and LightDM-gtk-greeter to be the display manager / interface

    pacman -S lightdm lightdm-gtk-greeter

To enable lightdm we:

    systemctl enable lightdm

Then, to make the program start:

    systemctl start lightdm

To create my own user account:

    useradd sarah -m -G wheel



## Summarized notes of the MANY errors that occurred


There was a major problem with logging into the Sugar program.
- The problem derived from not having a user
- I was logging in using root
- I tried dowloading a different program other than Sugar (Pantheon), and it failed AGAIN.
- Finally, after more trial and error, I found out that I needed to create a user account to log into Sugar DE.

Another instance was when I forgot to install dhcpcd:
- I could not access the interned
- I had to reboot into the live iso
- After installinig it, it ran smoothly

Finally, I almost forgot to add you, Prof. Codi West, as a user
- I added your name as Codi
- Then, I added your preferred password as requested on Harvey.

# Notes on creating Prof. Codi West Password

Created a user name for Professor West:
- in the terminal of Suagar, I logged-in as super admin user 
- then, I created a user name and password for Prof. Codi 

        useradd codi -m -G wheel
        passwd codi
- then I entered in the password requested on Harvey
- created a time limit for the password, as per request
    
        chage -d 0 codi
        

## Installed Sudo

Installed sudo using pacman

    pacman -S sudo

Then, visudo command enables Prof. Codi and I to use sudo by un-commenting the line that allows users of the group wheel to use sudo.

    visudo 

## Intalling different shell

To intall the fish shell:

    sudo pacman -S fish

To set the default shell for the user:

    chsh -s /bin/fish

## Install ssh

To install ssh we:

    sudo pacman -S openssh

*Here I had a minor problem installing. Initially I was trying to install ssh without **open**, then after searching, I realized it was **openssh***.

## Color Coding

Since color coding was already added, I did not add color to the terminal ;)

## Boot into the GUI

This was already completed when I installed lightdm and sugar

## Aliases
### I created 3 aliases


To create a fish aliase:

    alias sugar=candy
    funcsave sugar

I created an alias that would colorize the list of the output

    alias ls='ls --color=auto'
    funcsave ls

I created an alias that shows hidden files

    alias l.='ls -d .* --color=auto'
    funcsave l.

This command searches the history quickly and more efficiently

    alias h='history'
    funcsave h


## To shut down program

I learned that to shut down I would type:

    shutdown now


# Instructions:

VM operation and configuration: Show the following in your videoLog into the desktop environment (GUI)
- open the VMware
- right click on the sugar link on the far righ and demonstrate the settings
- click play symbol to open sugar land
- show each aspect of sugar
- end the tour with the terminal

Launch a shell and show the IP address of your VM
- ip a

Show the user list
- less /etc/passwd

Show the sudoers
- getent group wheel

Demonstrate some of your alias
- type in 

        h
        l.
        ls

Launch a browser and navigate to your Github-pages hosted installation documentation and Harvey to show that you have Internet connectivity
- go to the web on the sugar desktop and go tot Github pages.

ssh into the class lab gateway
- go to ssh sysadmin@10.10.1.133 and log into the class

show any cool customizations (if any) you did
- my major customization was the daunting task of using sugar. This was a major life decision. It altered the course of my life... FOREVER!

Pick some package to install from the Arch User Repository (AUR)
- 
