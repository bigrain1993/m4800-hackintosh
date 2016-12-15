#Dell Precision M4800 Hackintosh Installation Guide


These are files needed to get a Dell Precision M4800 going on macOS
All files will be divided into parts of what point you're at in your setup
Side note: This may also work on a M6800, 
seeing as it's the same internals, just different size since it's a 17.3" laptop.

Also! The Dell Precision M4800 and M6800 both can accept two WiFi cards simultaneously, 
I've confirmed this myself by installing a second wifi card alongside my Intel AC7260. 
Just make sure your antenna connectors are long enough to reach your hackintosh wifi card. T
hey may stretch a little but try not to break one like I did. 
A small bump will appear on the bottom when you've put the back cover on, but that shouldn't be a big deal. 
Make sure you disable the Hackintosh card in Windows to avoid network conflicts.

Warning: Users with the AMD FirePro M1500 such as myself, 
will have a bit of trouble getting everything going
Lucky for the NVIDIA Users, 
they have it the easy way since they only need the web drivers.
I'm not sure about Switchable Graphics spoofing on the NVIDIA GPUs though, 
I'll look into it once I get my new Quadro M2000M installed.

I haven't had any luck using a Sierra USB Installer yet, 
so if anyone has got it going, post an issue here, 
I'll credit you appropriately and make sure you get some love for contributing to the improvement of this guide.


#Chapter 1: The Pre-setup

If you have the El Pwn DMG off TechReviews, 
then installing El Capitan should be a breeze
Don't use Niresh's Yosemite even though I had success on it, 
that distro doesn't like to be friendly
as I've had the 0xdeadbeef KP happen to me a lot using that installer.
Here are the boot flags to get you going, 
make sure you got a working KB/M setup on your M4800
The USB ports can be picky on which want and don't want to work


Boot Flags to use: -v maxmem=8192 GraphicsEnabler=Yes USBBusFix=Yes

NVIDIA Users: -v maxmem8192 GraphicsEnabler=No USBBusFix=Yes


Chapter 2: The Installation

Once you've successfully booted into the Installer, install as usual. 
If you're dual booting macOS and Windows,
make sure you install OS X first if you're doing a dual boot on a single drive. 
Those using separate drives for each OS don't need to do this. 
Once the first phase is done and you are asked to restart the laptop, 
reneter the boot flags according to your GPU and finish the post setup.


#Chapter 3: Reaching the desktop

After the installation is finished, 
make sure you have a handy USB flash drive or external drive on you to get everything going. 
First things to get installed is Clover Configurator, EFI Mounter V3, 
grab Kext Wizard off hackintosh.zone since the one off any other site does not work after one use for some reason. 
Go to Finder at the top left, go to Preferences, go to General and enable Hard Disks. 
This will allow you to see the EFI partition once you've got clover installed.

After you got those installed, 
install Clover and make sure you select ESP, not UEFI or Legacy. 
Then after it's done, the EFI partition should appear on the desktop and you should be ready for the next portion of this guide.


#Chapter 4: Getting everything going

So once Clover's in, here's where the fun part begins. 
Go to the Clover Files folder in this github repo, download CLOVER.zip, extract to your downloads, then head into your EFI partition, then into your EFI folder then drag and drop. Let it replace everything in the CLOVER folder you already have. 
If you don't have an i7 4700MQ like I do, use ssdtPRgen.sh off the net to generate SSDTs for your CPU so you get native power management, no need for NullCPUPowerManagement.kext, make sure you delete SSDT-1.aml through SSDT-8.aml before you generate replacement ones with ssdtPRgen.sh

If you have a BCM94352HMB card like I do, keep SSDT-9.aml since it's responsible for having the card partially working.

Make sure to also install VoodooHDA off their sourceforge download, I have the VoodooHDA kext here since I modified it to be at 100% volume. 

I highly recommend you don't patch AppleHDA since it's a pain in the ass, VoodooHDA will do just as good.

Now, open Kext Wizard and drag all the kexts you got from the Kexts Folder off this repo. 
Install them, then head to Maintenance to rebuild your caches and reboot, 
you should successfully boot into OS X perfectly, 
if you have any issues, post an issue here and I'll be able to help you out to my best abilities. 
NVIDIA users, install your web drivers before rebooting, CUDA Drivers as well if it's possible. 
Make sure you find the Web Drivers for your necessary macOS version prior to reboot. 
If you want iMessage and FaceTime going, make sure you do it after you reboot. 
Here's the link for the guide: http://www.fitzweekly.com/2016/02/hackintosh-imessage-tutorial.html

Chapter 5: Upgrading to Sierra

If you think you want to go the extra mile and upgrade to 10.12.x, you won't have much trouble. 
Those running the AMD FirePro M5100, will have absolutely no problem upgrading since the Verde.kext you installed will let you go through without GPU issues that I experienced without that kext installed. 
It's a different story though, the OS would crash at login and not go any further. 
Thanks to the folks at insanelymac for providing the Verde.kext, we can now upgrade to Sierra without any problems.

The NVIDIA guys, make sure you have nv_disable=1 before you upgrade, 
that way you can install your web drivers after upgrading and get QE/CI after rebooting.


#Chapter 6: The Conclusion

So after you've got everything installed, upgraded and you're hunky dory with your hack, 
you can do whatever you want since everything runs like a charm. 
Your only problem is Sleep on closing the lid. 
That is the only problem I have so far, 
but removing the sleepimage from /var/vm/sleepimage and replacing it with a folder of the same name and as well as setting your hibernatemode to 25 via "sudo pmset -a hibernatemode 25" will be your only way of sleeping, 
via Fn+F1 of course. NVIDIA guys, post an issue here if you got lid sleep working. 
Before trying any games on macOS Sierra, make sure you change your display via System Preferences > Displays, Optimize For. That's how you switch between Intel Graphics and "AMD 7700 Series" video cards. 
I wrote the modified Dashimaki Framebuffer one night but I'll be looking into getting a full framebuffer working with the DisplayPort, VGA and as well as the E-Port Replicator ports. 
If you guys also have the FIPS Fingerprint or Regular Swipe, 
it won't work in macOS AFAIK. 
Using something like MacID via Bluetooth will work just fine though. 
Sleep also works if you switched to the AMD GPU, 
I've confirmed that and for some funny reason it works without a problem. 
Some USB Ports also don't work for some reason so if someone could get a fix on that, 
report it as an issue and I'll update the repo.

Final Note: The modified Dashimaki Framebuffer I made doesn't really work that well and only lets you connect HDMI as far as I know from my experiences.

##Enjoy your M4800/M6800 hackintosh!

------------------------------------
#Special Thanks to: 
okiookio for some DSDT patches, OSX Latitude's howoarang for the main files, tonymacx86 for the ATI framebuffer and device ids, insanelymac for the Verde.kext & Framebuffer modding guide, autumnrain and slice2009 for VoodooHDA, RehabMan for almost all the kexts necessary for this hackintosh guide, Clover Bootloader team for giving us the holy grail of running macOS on PCs.

