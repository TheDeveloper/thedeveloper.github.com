---
layout: post
category : development
title: Fix FTP passive mode issues on EC2
tags : [ftp, passive, mode, issues, amazon, ec2]
---
{% include JB/setup %}

<img src="http://static.gosquared.com/images/liquidicity/09_12_06_geoffdev_790x100.jpg" alt="Dev with Geoff - Development time with our CTO Geoff Wagstaff" width="790" height="100"/>

For a while I was forced to connect to FTP (an installation of <a href="http://vsftpd.beasts.org/">VSFTP</a>) on our EC2 server using Active mode, because passive mode refused to work. While this is OK for FTP clients that can be configured to use active mode, other utilities such as screen capture (e.g. Jing) and the wordpress auto-upgrade could not work with active mode, causing all sorts of erroneous malarky.

_If you're getting errors such as "227 entering passive mode... Connection [Failed/Timed out]" this may work for you_

<!--more-->

I decided enough was enough and set about problem-solving: the developer's favourite. It turns out, as usual, the problem relates to the ports the EC2 firewall opens for its instances, namely, none at all. Since passive mode connects to any random port > 1023, this is a problem. So, what you will need to do is define a fixed port range for VSFTP to use for PASV connections and then allow these in your "Security Groups" firewall rules.

**Note:** This method will probably work on any server, just add the config settings and then open the correct ports in your software firewall or router

#### 1. Specify a port range in which VSFTP will run PASV connections
Add the following lines to your vsftpd.conf file:

	pasv_enable=YES
	pasv_max_port=12100
	pasv_min_port=12000
	port_enable=YES

You also need to add an extra line to specify which IP address VSFTP will advertise in response to a passive connection, so underneath the lines you've already pasted in vsftpd.conf, put:

	pasv_address={your public IP address}

OR if you don't have a fixed elastic IP address:

	pasv_addr_resolve={your public domain or DNS}

#### 2. Authorise required ports in a security group that applies to your instance

This can be done via the <a href="console.aws.amazon.com">AWS management console</a> (Amazon's EC2 web control panel), or in your own console:

	ec2-authorize default -p 20-21
	ec2-authorize default -p 12000-12100

Now restart vsftpd by typing `/etc/init.d/vsftpd restart` in your server's terminal.

If all goes well and it's your lucky day, passive connections should now work properly.
