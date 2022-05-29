---
title: IPSec VPN with Netgear FVS318v3
date: 2012-02-08 09:00:00 -0800
layout: post
categories: [Network]
tags: Network
---
First you have to set up your FVS318 router to accept the connections.

  1. Log on to your router and go to the "VPN Wizard" in the left hand menu.
  2. Just click "Next"

  ![image](/wp-content/uploads/2012/02/MWSnap038-300x128.jpg)
  
  3. You have to set a name for your connection and a pre-shared key (PSK). Select "A remote VPN client" as connection type.

  ![image](/wp-content/uploads/2012/02/MWSnap039-300x132.jpg)

  4. You will get a confirmation screen next. Just click "Done".

  ![image](/wp-content/uploads/2012/02/MWSnap040-300x213.jpg)

Now your router is up to speed and you need to download the VPN client from<https://www.shrew.net/download>  
Ones installed it's time to set up your new connection.


  1. In the router admin page select "IKE Policies" in the left hand menu. The two pieces of information you are interested in is "Local ID" and "Remote ID". 

  ![image](/wp-content/uploads/2012/02/MWSnap042-300x44.jpg)

  2. Now start Shrew Soft VPN Access Manager and click "Add". 

  ![image](/wp-content/uploads/2012/02/MWSnap043-300x230.jpg)

  3. Now enter your DynDNS, or static WAN address if you have one, in the "Host Name or IP Address" field.
  4. Set "Auto Configuration" to "disabled".
  5. Set "Local Host" - "Address Method" to "Use an existing adapter and current address".

  ![image](/wp-content/uploads/2012/02/MWSnap045.jpg)

  6. Now go to the "Name Resolution" tab. If you know the addresses to wins server and/or dns server on the remote network enter them here. If not uncheck the check boxes. 

  ![image](/wp-content/uploads/2012/02/MWSnap050.jpg)

  7. Now go to the "Authentication" tab and set "Authentication Method" to "Mutual PSK".
  8. "Local Identity" should be the field "Remote ID" on the routers "IKE Policies" page. "Identification Type" should be "Fully Qualified Domain Name". 

  ![image](/wp-content/uploads/2012/02/MWSnap046.jpg)

  9. On the "Remote Identity" tab the "Identification Type" should be "Fully Qualified Domain Name" and "FQDN String" should be the "Local ID" from the routers "IKE Policies" page. 

  ![image](/wp-content/uploads/2012/02/MWSnap047.jpg)

 10. Moving on to the "Credentials" tab fill in your PSK in the "Pre Shared Key" field. In this case "areallylamekey".

 ![image](/wp-content/uploads/2012/02/MWSnap048.jpg)

 11. Then you go to main tab "Policy".
 12. Uncheck the "Obtain Topology Automatically or Tunnel All" check box.
 13. Click the "Add" button.
 14. Type in your network. To route all the 192.168.0.x addresses over the VPN tunnel enter address 192.168.0.0 and netmask 255.255.255.0. If you have the same network address range at home and in your current location you can enter specific addresses or add an other topology entry that excludes those addresses.
 
 ![image](/wp-content/uploads/2012/02/MWSnap049.jpg)

 15. Then hit "Save" and you will return to the mane window.
 16. Dubbel click your connection and select "Connect". That's it!

 ![image](/wp-content/uploads/2012/02/MWSnap052.jpg)

 Your now up and running with your own secure IPSec tunnel to your home or office!

The content of this post is from <https://www.hackviking.com/2010/10/ipsec-vpn-with-netgear-fvs318v3/>. Want to make sure i give credit to source.
