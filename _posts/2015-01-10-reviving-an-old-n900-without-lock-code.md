---
layout: post
title: "Reviving an old N900 without lock code"
date:  2015-01-10 21:00:26 +0200
categories: [Maemo]
featured-img: n900
---
Since it's introduction in 2009 the Nokia N900 has always been a very special Smartphone. And when you have forgotten your lock code you will quickly notice this once again. I had an old N900 lying around which I want to use in a project and of course I have forgotten the lock code. Without your lock code the N900 won't boot and you can not access your phone. To make the phone usable again I had to reflash the N900 with an old version of the firmware, brute force the lock code with a password cracker, reflash it with the newest version of the firmware and activate the community updates.
<center><figure><img src="/images/2015/N900.jpg"><figcaption>The Nokia N900</figcaption></figure></center>


The first question was where can I find the firmeware and the flash tool? Since Nokia is bought by Microsoft and Maemo, the Debian based operating system of the N900, is discontinued everything Maemo related on the Nokia servers is gone (http://tablets-dev.nokia.com/nokia_N900.php), even the unofficial clone of this page (http://skeiron.org/tablets-dev/nokia_N900) is not available anymore. But there is copy of the unofficial clone on archive.org:
- [http://web.archive.org/web/20131117084237/http://skeiron.org/tablets-dev/maemo_dev_env_downloads/](http://web.archive.org/web/20131117084237/http://skeiron.org/tablets-dev/maemo_dev_env_downloads/)
- [http://web.archive.org/web/20131117073524/http://skeiron.org/tablets-dev/nokia_N900/](http://web.archive.org/web/20131117073524/http://skeiron.org/tablets-dev/nokia_N900/)

So if you need any of these files you better download them as quick as you can.

I downloaded the PR1 release of Maemo, the whole process of retrieving the lock code is only possible with the PR1 version of Maemo, I tested the PR1.3 and after a complete flash of the eMMC and rootfs it still asks for the lock code at boottime.
- PR1 Vanilla version of the eMMC content for Nokia N900: <i>RX-51_2009SE_1.2009.41-1.VANILLA_PR_EMMC_MR0_ARM.bin</i>
- the first Maemo 5 Global release for Nokia N900, also called the FIASCO/rootfs image: <i>RX-51_2009SE_1.2009.42-11_PR_COMBINED_MR0_ARM.bin</i>
I also downloaded the Maemo Flasher-3.5 Tool, <i>maemo_flasher-3.5_2.5.2.2.tar.gz</i> and extracted it's content.

(Note: I also downloaded the PR1.3 firmware, Nokia's last official major Maemo 5 update, to flash this version in the last step. The files are called <i>RX-51_2009SE_10.2010.13-2.VANILLA_PR_EMMC_MR0_ARM.bin</i> and <i>RX-51_2009SE_20.2010.36-2_PR_COMBINED_MR0_ARM.bin</i>.) 

After I had everything together I started the flash process:
### 1) Charge the N900 battery till the led is green

### 2) Flash eMMC image
Execute the flasher tool on your computer:
{% highlight bash %}
sudo ./flasher-3.5 -F RX-51_2009SE_1.2009.41-1.VANILLA_PR_EMMC_MR0_ARM.bin -f
{% endhighlight %}
The output will look like this:
{% highlight bash %}
flasher v2.5.2 (Oct 21 2009)
Image 'mmc', size 241163 kB
	Version RX-51_2009SE_1.2009.41-1.VANILLA
Suitable USB device not found, waiting.
{% endhighlight %}
Plug in the powered down N900 while holding the <i>u</i> key to activate RD mode. Text similar to the following will be displayed on the computer:
{% highlight bash %}
USB device found found at bus 002, device address 025.
Found device RX-51, hardware revision 2101
NOLO version 1.4.14
Version of 'sw-release': RX-51_2009SE_20.2010.36-2_PR_MR0
Booting device into flash mode.
Suitable USB device not found, waiting.
USB device found found at bus 002, device address 026.
Raw data transfer EP found at EP2.
Image(s) flashed successfully in 32.528 s (7413 kB/s)!                    
USB device found found at bus 002, device address 007.
Found device RX-51, hardware revision 2101
NOLO version 1.4.14
Version of 'sw-release': RX-51_2009SE_21.2011.38-1_PR_MR0
Booting device into flash mode.
Suitable USB device not found, waiting.
USB device found found at bus 002, device address 008.
Raw data transfer EP found at EP2.
Image(s) flashed successfully in 30.533 s (8382 kB/s)!   
{% endhighlight %}
Remove the N900 battery to power it down. The manuals point out that it is not a good idea to boot the device in this state and removing the battery is the fool proof way.

### 3) Flash FIASCO/rootfs
Execute the flasher tool again:
{% highlight bash %}
sudo ./flasher-3.5 -F RX-51_2009SE_1.2009.42-11_PR_COMBINED_MR0_ARM.bin -f -R
{% endhighlight %}
-R enables a reboot of the N900 after flashing, on the computer: 
{% highlight bash %}
flasher v2.5.2 (Oct 21 2009)
SW version in image: RX-51_2009SE_1.2009.42-11_PR_MR0
Image 'kernel', size 1700 kB
	Version 2.6.28-20094102.3+0m5
Image 'rootfs', size 149376 kB
	Version RX-51_2009SE_1.2009.42-11_PR_MR0
Image 'cmt-2nd', size 81408 bytes
	Version 8.2.2009.34.3-2+0m5
Image 'cmt-algo', size 519808 bytes
	Version 8.2.2009.34.3-2+0m5
Image 'cmt-mcusw', size 5786 kB
	Version 8.2.2009.34.3-2+0m5
Image '2nd', size 14720 bytes
	Valid for RX-51: 2217, 2218, 2219, 2220, 2120
	Version 1.4.13+0m5
Image 'xloader', size 14848 bytes
	Valid for RX-51: 2217, 2218, 2219, 2220, 2120
	Version 1.4.13+0m5
Image 'secondary', size 109440 bytes
	Valid for RX-51: 2217, 2218, 2219, 2220, 2120
	Version 1.4.13+0m5
Image '2nd', size 14720 bytes
	Valid for RX-51: 2101, 2102, 2103
	Version 1.4.13+0m5
Image 'xloader', size 14848 bytes
	Valid for RX-51: 2101, 2102, 2103
	Version 1.4.13+0m5
Image 'secondary', size 109440 bytes
	Valid for RX-51: 2101, 2102, 2103
	Version 1.4.13+0m5
Image '2nd', size 14720 bytes
	Valid for RX-51: 2104, 2105, 2106, 2107, 2108, 2109, 2110, 2111, 2112, 
2113, 2114, 2115, 2116, 2117, 2118, 2119
	Version 1.4.13+0m5
Image 'xloader', size 14848 bytes
	Valid for RX-51: 2104, 2105, 2106, 2107, 2108, 2109, 2110, 2111, 2112, 
2113, 2114, 2115, 2116, 2117, 2118, 2119
	Version 1.4.13+0m5
Image 'secondary', size 109440 bytes
	Valid for RX-51: 2104, 2105, 2106, 2107, 2108, 2109, 2110, 2111, 2112, 
2113, 2114, 2115, 2116, 2117, 2118, 2119
	Version 1.4.13+0m5
Image '2nd', size 14720 bytes
	Valid for RX-51: 2201, 2202, 2203, 2204, 2205, 2206, 2207, 2208, 2209, 
2210, 2211, 2212, 2213, 2214, 2215, 2216
	Version 1.4.13+0m5
Image 'xloader', size 14848 bytes
	Valid for RX-51: 2201, 2202, 2203, 2204, 2205, 2206, 2207, 2208, 2209, 
2210, 2211, 2212, 2213, 2214, 2215, 2216
	Version 1.4.13+0m5
Image 'secondary', size 109440 bytes
	Valid for RX-51: 2201, 2202, 2203, 2204, 2205, 2206, 2207, 2208, 2209, 
2210, 2211, 2212, 2213, 2214, 2215, 2216
	Version 1.4.13+0m5
Suitable USB device not found, waiting.
{% endhighlight %}
Plug in the powered down N900 while holding 'u' key, text similar to the following will be displayed on the computer: 
{% highlight bash %}
USB device found found at bus 002, device address 028.
Found device RX-51, hardware revision 2101
NOLO version 1.4.14
Version of 'sw-release': RX-51_2009SE_20.2010.36-2_PR_MR0
Sending xloader image (14 kB)...
100% (14 of 14 kB, avg. 3625 kB/s)
Sending secondary image (106 kB)...
100% (106 of 106 kB, avg. 13359 kB/s)
Flashing bootloader... done.
Sending cmt-2nd image (79 kB)...
100% (79 of 79 kB, avg. 11357 kB/s)
Sending cmt-algo image (507 kB)...
100% (507 of 507 kB, avg. 21151 kB/s)
Sending cmt-mcusw image (5786 kB)...
100% (5786 of 5786 kB, avg. 23616 kB/s)
Flashing cmt-mcusw... done.
Sending kernel image (1700 kB)...
100% (1700 of 1700 kB, avg. 20492 kB/s)
Flashing kernel... done.
Sending and flashing rootfs image (149376 kB)...
100% (149376 of 149376 kB, avg. 13647 kB/s)
Finishing flashing... done
CMT flashed successfully
{% endhighlight %}
Remove USB cable, the N900 boots, blinking dots and finally Maemo! However the lock code is still set but the automatic lock at boottime is disabled. This is only possible with the PR1 firmware and was changed in later firmeware versions. 

### 4) Retrieve the old lock code
To retrieve the lock code I needed to obtain its hash code and brute force it. To achive this you need to activate the <i>Maemo-Extras</i> repository in the Application Manager:
- Navigate to the application menu (tap the title bar)
- Select <i>Application catalogs</i>
- Select <i>Maemo Extras</i>
- Uncheck <i>Deactivated</i>
I installed the aplication <i>rootsh</i> to get root privileges by executing <i>sudo gainroot</i> on the command line.
After that I activated the repository <i>Extras-devel</i> in the Application Manager:
- Navigate to the application menu (tap the title bar)
- Select <i>Application catalogs</i>
- Select <i>New</i>
- Enter a catalog name of <i>Maemo extras-devel</i>
- Enter a web address of <i>http://repository.maemo.org/extras-devel/</i>
- Enter a distribution of <i>fremantle</i>
- Enter components of <i>free non-free</i>
- Select <i>Save</i>
to install <i>John The Ripper</i> the password cracker tool:
{% highlight bash %}
sudo gainroot
apt-get install john
{% endhighlight %}
Extract the hash to the file /home/user/MyDocs/hash
{% highlight bash %}
echo root:$(grep -A 13 lock_code /dev/mtd1|tail -1): > /home/user/MyDocs/hash
{% endhighlight %}
Now get the key:
{% highlight bash %}
john -i:digits hash
{% endhighlight %}
It took the phone only a couple of milliseconds to brute force the 5 digit code. <i>-i:digits</i> tells the tool to only use numerical digits since the phone only accepts numerical input for the password which speeds up the whole process.
Now write down the lock code, you will need it later, or change it to the phones default <i>12345</i>.

### 5) Install the latest offical Maemo firmware
I followed the same steps as above with the latest PR1.3 release. 
- Charge battery
- Flash eMMC
- Flash FIASCO
When the phone boots it is asking for the lock code again but this time we are prepared! <i>Settings</i> - <i>About product</i> shows Version <i>20.2010.36-2</i>.

### 6) Enable Community SSU
The Community Seamless Software Update (CSSU) is being developed by the Maemo community as a continuation of, and expansion on, Nokia's update support. It aims to deliver fixes which would be difficult to deliver through Extras (like core Maemo packages). It won't, however, bundle software which can (or could) be installed through the Extras repositories. After all these years the community is still alive, the last Maemo update was released 01.01.2015.

You can add the repository by opening <i>http://repository.maemo.org/community/community-fremantle.install</i> in the browser on the phone. The Application Manager will launch and ask to install the <i>Community SSU enabler</i>. After the installation close the Application Manager and tap on the brand new Community SSU icon in the applications menu. The Application Manager will start again and present the <i>Maemo 5 Community SSU Package</i> ready to install.

<span itemprop="author" itemscope itemtype="https://schema.org/Person">
    <meta itemprop="name" content="Sebastian Glahn" />
    <meta itemprop="url" content="http://blog.tldnr.org" />
    <meta itemprop="sameAs" content="https://twitter.com/sglahn" />
    <meta itemprop="sameAs" content="https://github.com/sglahn" />
    <meta itemprop="sameAs" content="https://www.thingiverse.com/sglahn" />
    <meta itemprop="sameAs" content="https://stackoverflow.com/users/1453205/sglahn" />
</span>
