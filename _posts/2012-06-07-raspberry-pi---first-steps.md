--- 
layout: post
title: "Raspberry Pi   First Steps"
description: "I had some free time today so I played with my brand new Raspberry Pi. Here is what i have done so far using the Debian Squeeze image. 1) Install the Vim text editor 2) Changing the localization, the keyboard layout and the timezone 3) Disable the overscanning 4) Install bash autocompletion 5) Reconfigure the OpenSSH server 6) Create a new user and remove the user pi 7) Update the system and the firmeware"
category: [Coding]
featured-img: raspberrypi
---
<h3><b>Update: This article is outdated! The old Debian Squeeze image is no longer available. Please use the new <a href="http://www.raspberrypi.org/downloads">Raspbian Wheezy</a> image instead.</b></h3>
<p/>
<p>I had some free time today so I played with my brand new Raspberry Pi. Here is what i have done so far using the Debian Squeeze image.</p>

<h4>1) Install the Vim text editor</h4>
<p>To install my favorite text editor:</p>
<p><pre><code data-language="shell">sudo apt-get install vim</code></pre></p>

<h4>2) Changing the localization, the keyboard layout and the timezone</h4>
<p>To change the localization:</p>
<p><pre><code data-language="shell">sudo dpkg-reconfigure locales</code></pre></p>

<p>To change the keyboard layout:</p>
<p><pre><code data-language="shell">sudo dpkg-reconfigure keyboard-configuration</code></pre></p>

<p>To change the timezone:</p>
<p><pre><code data-language="shell">sudo dpkg-reconfigure tzdata</code></pre></p>

<h4>3) Disable the overscanning</h4>
<p>Overscanning is the black frame around the four edges of the video image. To disable the overscanning you need to create a text file config.txt on your boot partition /boot an put in the following text:</p>

<p><pre><code data-language="shell">disable_overscan=1</code></pre></p>

<h4>4) Install bash autocompletion</h4>
<p>First you need to install the bash completition:</p>
<p><pre><code data-language="shell">sudo apt-get install bash-completion</code></pre></p>

<p>Then you need to activate it by changing the file /etcbash.bashrc. First you need to open it with a text editor:</p>
<p><pre><code data-language="shell">sudo vim /etc/bash.bashrc</code></pre></p>

<p>And remove the hashtags # from the beginning of the following lines:</p>
<p><pre><code data-language="shell"># if [ -f /etc/bash_completion ] &#38;&#38; ! shopt -oq posix; then
#     . /etc/bash_completion
# fi</code></pre></p>

<h4>5) Reconfigure the OpenSSH server</h4>
<p>The SSH server in the Debian Squeeze image is deactivated by default which is good for security reasons. The password for the user pi and the ssh private keys are available in the public. You can start it with the following line:</p>
<p><pre><code data-language="shell">sudo /etc/init.d/ssh start</code></pre></p>

<p><b>5.1) Activate SSH boot by default</b></p>
<p>To activate the ssh server at boot simply rename the file boot_enable_ssh.rc to boot_rc on your boot partition:</p>
<p><pre><code data-language="shell">sudo mv /boot/boot_enable_ssh.rc /boot/boot.rc</code></pre></p>

<p><b>5.2) Create new SSH keys</b></p>
<p>Delete the old ones:</p>
<p><pre><code data-language="shell">sudo rm /etc/ssh/ssh_host_*key</code></pre></p>

<p>And create new keys:</p>
<p><pre><code data-language="shell">sudo dpkg-reconfigure openssh-server</code></pre></p>

<p><b>5.3) Change the SSH port</b></p>
<p>Changing the ssh port will stop many automated attacks and will add a little bit security though obscurity. To change the port of the SSH server you need to edit the file sshd_config with a text editor:</p>
<p><pre><code data-language="shell">vim /etc/ssh/sshd_config</code></pre></p>

<p>Edit the line which states Port 22 and change the number to a port not currently used on the system</p>
<p><pre><code data-language="shell"># What ports, IPs and protocols we listen for
# Port 22</code></pre></p>

<p>to e.g.:</p>
<p><pre><code data-language="shell"># What ports, IPs and protocols we listen for
# Port 50633</code></pre></p>

<p>And restart SSH:</p>
<p><pre><code data-language="shell">/etc/init.d/ssh restart</code></pre></p>

<h4>6) Create a new user and remove the user pi</h4>
<p>First we activate the root account by creating a password for the user root:</p>
<p><pre><code data-language="shell">sudo passwd</code></pre></p>

<p>Now you need to close all sessions and re-login as root.</p>

<p>Because of some configuration error the user pi has been created twice. So we need to manually delete one.  Open the file passwd with a text editor:</p>
<p><pre><code data-language="shell">vim /etc/passwd</code></pre></p>

<p>And delete the line:</p>
<p><pre><code data-language="shell">pi:x:1001:1001::/home/pi:/bin/bash</code></pre></p>

<p>Now we can delete the user pi and his home directory:</p>
<p><pre><code data-language="shell">deluser —remove-home pi</code></pre></p>

<p>And add a new user:</p>
<p><pre><code data-language="shell">adduser username</code></pre></p>

<p>Set the groups:</p>
<p><pre><code data-language="shell">usermod -aG adm,dialout,cdrom,audio,plugdev,users,lpadmin,\
admin,sambashare,vchiq,powerdev username</code></pre></p>

<p>Try to login with the new username and password and try to get sudo. If this works disable the root account:</p>
<p><pre><code data-language="shell">passwd -l root</code></pre></p>

<h4>7) Update the system and the firmeware</h4>
<p><b>To update the system:</b></p>
<p><pre><code data-language="shell">sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade</code></pre></p>

<p><b>To update the firmeware:</b></p>
<p>create a root shell:</p>
<p><pre><code data-language="shell">sudo -s</code></pre></p>

<p>install the git certificates:</p>
<p><pre><code data-language="shell">apt-get install git-core ca-certificates</code></pre></p>

<p>Download the update script to /usr/bin and set the excecutable flag:</p>
<p><pre><code data-language="shell">wget http://goo.gl/1BOfJ -O /usr/bin/rpi-update \
&#38;&#38; chmod +x /usr/bin/rpi-update</code></pre></p>

<p>Excecute the update script:</p>
<p><pre><code data-language="shell">rpi-update</code></pre></p>

<p>Reboot the raspberry pi:</p>
<p><pre><code data-language="shell">reboot</code></pre></p>
