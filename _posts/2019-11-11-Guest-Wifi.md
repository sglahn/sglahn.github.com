---
layout: post
title: "OpenWrt: Randomizing Guest WIFI Passwords"
date: 2019-11-11 06:00:00 +0000
categories: [Command-Line]
featured-img: guest-wifi
---
We have a guest wifi at home for everyone to use. With a growing number of people knowing the password to the wifi I wanted to change it on a regular basis.
Rather than manually coming up with a suitable random password all the time, I wanted to automate the process of generating and setting a new password and make the password easy accessible for our guests at the same time.

I'm using an Linksys WRT-1900 AC wifi router with a custom firmware called OpenWrt which is an open source embedded operating system based on Linux.
OpenWrt allows the installation of additional software and it is even equipped with a package manager. 
It also allows SSH and root access to the router.

There are a lot of guides online on how to [configure a guest wifi][1] in OpenWrt so I won't cover it here. 

# Randomize the Passwords
I wrote a little bash script which generates a new random password for the guest wifi and restarts it. The script is meant to run on the router itself. It is nothing fancy but it works.

The password is constructed out of the uppercase characters *ABCDEFGHJKLMNPQRSTUVWXYZ*, the lowercase characters *abcdefghjklmnpqrstuvwxyz*, the digits *23456789* and the special characters *_-*. It has a length of 12. 
The characters I and O and the numbers 1 and 0 are left out because it is always hard to keep them apart.

The password is written into a file called *.guest_password.txt* which is located in the home directory of the root user.

After the password is written to the text file the script calls a very handy tool called *qrencode*. Qrencode is used to create an SVG image with a QR Code containing the SSID, the security mode and the new created password. 
Later this QR Code can be scanned by a smartphone to directly connect to the wifi network.
Qrencode can be installed with the help of the OpenWrt package manager called Opkg via *opkg install qrencode*.

The QR Code is saved to file called *wifi.svg* in the */www* folder.

Afterwards the script restarts the wifi with the help of OpenWrts configuration management system [UCI][2]. 
The complete script is shown below, it is also located in the home folder of the root user.

~/rotate_guest_wifi_password.sh
{% highlight bash %}
#!/bin/bash

password=`cat /dev/urandom | env LC_CTYPE=C tr -dc _ABCDEFGHJKLMNPQRSTUVWXYZabcdefghjklmnpqrstuvwxyz23456789- | head -c 12; echo;`

echo $password > /root/.guest_password.txt

ssid=123456_Guest
security=WPA
qrencode -o /www/wifi.svg "WIFI:S:${ssid};T:${security};P:${password};;" 

for i in {0..2}; do
    if [ `uci get wireless.@wifi-iface[$i].network` == 'guest' ]; then
        uci set wireless.@wifi-iface[$i].key=$password
        uci commit wireless
        wifi
    fi
done; 
{% endhighlight %}

# Add a Cron Job
Now all that has to be done is to call this script on a regular basis.
I'm using a cron job on the router to call the above script once a month to reset the password and the wifi. 
So now, every first of the month a new password and a new QR Code will be generated and the wifi restarted.

Crontab
{% highlight bash %}
➜ crontab -l
1 0 * * 1 /root/rotate_guest_wifi_password.sh
{% endhighlight %}

# Setup the Website
Now we need a way to show the current password to our guests so that they can login to our wifi.
I'm using a CGI script on the router itself to generate a rudimentary webpage and display the SSID, the current password and the QR Code image.
The CGI script greps the password from the text file in the home directory of the root user and the QR Code file from the /www folder and delivers it to the client.
The webpage will also reload itself every hour.

Below is the script which is located in the folder */www/cgi-bin*.

/www/cgi-bin/guest_password
{% highlight bash %}
#!/bin/bash

password=$(</root/.guest_password.txt)

echo "Content-Type: text/html"
echo ""
echo "<!DOCTYPE html>"
echo '<html lang="en-US">'
echo "<head>"
echo "<title>Guest Password</title>"
echo '<meta name="viewport" content="width=device-width, height=device-height, initial-scale=1.0, minimum-scale=1.0">'
echo '<meta http-equiv="refresh" content="360" />'
echo "</head>"
echo '<body bgcolor="#000">'
echo "<div style='text-align:center;color:#fff;font-family:UnitRoundedOT,Helvetica Neue,Helvetica,Arial,sans-serif;font-size:28px;font-weight:500;'>"
echo "<h1>Guest WIFI</h1>"
echo "<p>SSID: <b>123456_Guest</b></p>"
echo "<p>PASSWORD: <b>$password</b></p>"
echo '<img src=../wifi.svg style="width:50%"></img><br>'
echo "</div>"
echo "</body>"
echo "</html>"
{% endhighlight %}

And here is the very basic website displaying the wifi credentials:

![](/images/2019/guest-wifi.jpg)

# Summary
The setup is very stable and has run for a couple of month now without a problem. 
The most usefull thing is the QR Code because there is no need to type a password anymore.

[1]: https://openwrt.org/docs/guide-user/network/wifi/guestwifi/guest-wlan-webinterface
[2]: https://openwrt.org/docs/guide-user/base-system/uci

