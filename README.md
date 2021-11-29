# Remote access to a 3D printer running Klipper using SSH tunneling

Many users of 3D printers that run Klipper as their kinematic firmware want to access the controls of their 3D printer remotely, from a tablet, phone or laptop. However, doing this securely can be fristrating even for someone well versed in computers and/or linux. The largest problem is that, while the mainsail and fluidd are excellent projects for UI/UX, remote security is not a primary concern so exposing their webpages directly on the internet is a huge risk, given their ability to manipulate a physically dangerous device capable of starting a fire.

While there are 3rd party product that set up an "VPN" or a "Tunnel" for you, they often have a cost associated and/or force you to rely on a company for access to you own personal network, potentially giving them access or at lease delegating authentication to them or a 4th party, relying on then to maintain service.

## The components of remote access

To access any device on your home network from the internet you, at least, need the following three things:

* The current external address of your router (An IP address or preferably a constantly updated DNS entry)
* A port forward on your internet router for an internet facing port to the internal IP address of the device you want ot access
* A service running on the device listening on a port that the router is forwarding to.

There are exceptions to the above, for example cases of good ISP's providing multiple external IP addresses (but those have their own considerations) or bad ISP's double NATing your home network making port forwarding impossible. There is also the possibility of IPV6 with I won't be covering in this guide.

## What we will be using

In this guide I will be walking through a specific setup that is reasonably secure and free, save for the components you, most likely, already have set up for your 3D Printer:

# Your klipper raspberry pi
# SSHd, the secure shell daemon built into the Raspberry Pi OS flavour of Linux
# ConnectBot (An Android SSH client) or PuTTY (A windows SSH client)
# SSH tunneling

`Text like this is to be entered into the raspberry pi's shell, using an SSH client like PuTTY`

### 1. Set up a new user on your Raspberry Pi

SSH can provide full access to your Raspberry Pi and from their your whole network so it's important to lock down any users. if you haven't changed the password for the "pi" user do that now and ensure it is a strong password (See here for very valid points about passwords https://xkcd.com/936/)

To create a new user (we are calling it "pfuser"):

`sudo adduser --disabled-password --gecos "" pfuser`

Next we are going to "become" the new user and do some setup

`sudo su pfuser`

Create a keypair for passwordless, key-based SSH access

`ssh-keygen -t ed25519 -f output_keyfile`
