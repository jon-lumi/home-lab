# Home Lab Setup

Since I am new to the Cyber Security field, my goal with this home lab is to explore the different areas within it: network attacks, web exploitation, service exploitation, and more. With that in mind, my home lab includes:

- A Kali Linux VM.
- An Ubuntu Server hosting DVWA.
- A Metasploitable2 VM.
- An isolated network that connects all of these VMs.

I am running this on my Debian machine using KVM. This seemed to be the simplest and most lightweight way to manage my home lab given my desktop setup. Below is a diagram of the topology:

<p align="center">
  <img src="HomeLab-Diagram.jpg" width="600">
</p>

## Kali Linux VM

I will be using the Kali Linux VM to conduct the various attacks on the other components of my home lab. Using Kali Linux seems like a good idea given that it comes with a lot of useful tools out of the box for penetration testing. I have not used Kali Linux before, so this will also be a good opportunity to learn more about it.

I have allocated the following resources to this VM:

- CPU: 4 cores.
- RAM: 6 GB.
- Disk: 60 GB.

This VM is connected to both the NAT network and the isolated home lab network. This is because I want to have internet access from this VM, and this seemed like the easiest way to do it for now. I could also achieve this in a cleaner way by using a bridged network or hosting a virtual router in a separate VM (such as pfSense), so I may revisit this in the future.

Setting this up was straightforward. I downloaded the ISO image and used virt-manager to allocate the resources and add the network interface card for the isolated network (it already had a NIC for the NAT network by default).

## Ubuntu Server VM

This VM hosts the DVWA (Damn Vulnerable Web App) instance and has the following specs:

- CPU: 1 core.
- RAM: 2 GB.
- Disk: 15 GB.

DVWA deliberately exposes many common vulnerabilities, allowing various attacks such as SQL injection, XSS, CSRF, command injection, file upload vulnerabilities, and more. It is built with PHP and MySQL, and uses Apache as the web server. I think it will be a good starting point to learn more about the basics of web security.

The Ubuntu Server was easy to set up. I initially connected it to the NAT network so I could download the required packages for DVWA, and then I switched it to the isolated network afterward.

Once I am done experimenting with this, I will set up the OWASP Juice Shop. This is also a vulnerable web application, but it is built with JavaScript and Node.js.

## Metasploitable2 VM

This VM runs the Metasploitable2 Linux distribution, which deliberately exposes many vulnerable services such as Telnet, FTP, SSH (with weak credentials), vsftpd, outdated Tomcat, vulnerable web apps, NFS, distcc, rpcbind, and many more. It has the following resources:

- CPU: 1 core.
- RAM: 1 GB.
- Disk: 8 GB.

For this VM, I downloaded the prebuilt Metasploitable2 image from the internet and used it in virt-manager to allocate resources and connect it to the isolated network. One problem I was facing was that the VM could not boot up, and it apparently was due to the defualt disk type that virt-manager used (VirtIO). Metsploitable2 is pretty old so it doesn't seem to support VirtIO drivers, which is why I had to switch to the SATA disk type in order to configure the system. 

## Isolated Home Lab Network

This network connects all the VMs in the home lab. The idea is to isolate the target VMs from the internet for security purposes, given that they are intentionally vulnerable. I used virt-manager to set it up. As mentioned above, in the future I may use a bridged network to combine it with the NAT network, or create another VM to act as a router.
