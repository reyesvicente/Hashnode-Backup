---
title: "Deploying OutlineVPN to Ubuntu 20.04"
datePublished: Wed Jan 19 2022 08:46:18 GMT+0000 (Coordinated Universal Time)
cuid: ckylawejo04z395s1djb41pd9
slug: deploying-outlinevpn-to-ubuntu-2004
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1642581897199/HZOf-ixSCg.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1642581900236/eZ_EuEQvH.jpeg
tags: security

---

*This post includes affiliate links; I may receive compensation if you purchase products or services from the links provided in this article.*

Continuing on what I left off on my previous article. I’m learning and doing everything I need to protect myself online after the horrible instance happened to me last month. I was supposed to have spun OutlineVPN on my server and have written about the process and challenges a week ago but the virus struck my family and I hence the late publish. 

Spinning up a server and installing OutlineVPN is not as easy as taking a candy from a baby. Here’s a brief walk-through on how I setup OutlineVPN on an Ubuntu 20.04 server with [Vultr](https://www.vultr.com/?ref=9025791) so you won’t make the same mistake as I did.

First thing’s first, register for an account at [Vultr’s website](https://www.vultr.com/?ref=9025791). 


![Register.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1642581612057/gN7zwlUDit.png)

Once you’ve created and verified your email, you should be able to create a server by clicking on the `+` and `Deploy New Server` option.


![Deploy.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1642581639700/KGrK1IOva.png)

Choose a server instance and location nearest to where you are.


![Location.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1642581655536/IEBXPa4FO.png)

Then choose a plan.


![Plan.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1642581669861/2-TKh4jv5.png)

Next, check the necessary options needed and add your SSH keys if needed and name your server to whatever name you want to name it then click `Deploy Now` 


![Last.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1642581685798/tVIRVB7rW.png)

One of the mistakes I made was checking the `No Public IPv4 Address` which didn’t allow me to SSH to the server.

While waiting for the server to initialize, [download the Outline Manager on their website](https://getoutline.org/get-started/#step-1v) and the OutlineVPN on the Mac App Store or [on their website](https://getoutline.org/get-started/#step-3) and install it both the Outline Manager and OutlineVPN on your machine and OutlineVPN on your mobile phone/tablet. 


![Screenshot 2022-01-19 at 4.34.16 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1642581734724/j3Uruxkp5.png)

SSH into your server then run `sudo bash -c "$(wget -qO- [https://raw.githubusercontent.com/Jigsaw-Code/outline-server/master/src/server_manager/install_scripts/install_server.sh](https://raw.githubusercontent.com/Jigsaw-Code/outline-server/master/src/server_manager/install_scripts/install_server.sh))".` You’ll be asked to installed Docker - just type Yes or Y then press enter to continue. 

Once installation on your server is done, you’ll get your `apiURL` which will be pasted on your Outline Manager. Open the necessary ports needed on your server by running ``sudo ufw allow <port_number>`

You should then be able to generate new keys which will be needed on the OutlineVPN Clients.

Cheers!