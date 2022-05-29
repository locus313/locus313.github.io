---
title: "Bank-to-Cloud VPN: Cost-Saving Options for Integrating AWS with AT&T Netbond"
layout: post
categories: [AWS, Network]
tags: AWS Network VPN
excerpt_separator: <!--more-->
---
One of the first things we evaluate with a new PortX customer is their network access. Most connect via Internet Protocol Security (IPsec) VPN tunnels to an on-premise firewall. However, we recently established a connection for a new customer with [AT&T’s Netbond](https://www.business.att.com/products/netbond.html) cloud networking solution, which presented our team with exciting new integration alternatives. We’ll cover the unique options for connecting [Amazon Web Services](https://aws.amazon.com) (AWS) to AT&T Netbond, the issues we uncovered, and how we developed a cost-saving solution for the customer.
<!--more-->

## AT&T Netbond connection options
The [PortX Integration Platform](https://modusbox.com/portx-platform) runs on AWS. AT&T Netbond requires a Direct Connect network connection with an AWS account. Unfortunately, Direct Connect is a relatively expensive solution compared to other connection options offered by AWS. And we attempt to avoid unnecessary costs whenever possible because financial-grade solutions should be affordable and secure – [among ten other requirements](https://modusbox.com/six-software-design-principles-that-make-portx-a-financial-grade-integration-platform). 

Fortunately, during our evaluation process, we discovered that the customer had an existing AWS account with Direct Connect already tied into their AT&T Netbond network. This allowed us to investigate three additional cost-saving options for connecting to PortX:

1. [AWS Transit Gateway sharing](https://docs.aws.amazon.com/vpc/latest/tgw/transit-gateway-share.html)
2. [AWS Transit Gateway peering](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-peering.html)
3. [IPsec VPN tunnels](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html)

## Option #1: AWS Transit Gateway sharing
We immediately found there were significant challenges with the standard AT&T Netbond deployment that our customer was using. First, the standard deployment does not support AWS Transit Gateway sharing. Additionally, it presented challenges around the type of typology we are trying to achieve. As a possible workaround, the customer conferred with the Netbond team to redeploy AWS Direct Connect to a new Transit Gateway that would allow for more advanced configurations. With this newly established Transit Gateway, we could properly evaluate the sharing option. 

However, when we attempted the initial sharing of the Transit Gateway, we discovered that the request was not showing up for the customer. Our investigation uncovered that Transit Gateway sharing only works within the same AWS region. Unfortunately, PortX and the customer’s AWS accounts were located in separate regions. 

## Option #2: AWS Transit Gateway peering
We turned our attention to the second cost-effective connection option – Transit Gateway peering. The customer’s cloud team worked with AT&T Netbond to establish the routing settings of its Netbond network and obtained the subnets required to route through the customer’s AWS account to PortX. After some collaboration, we established the routing between PortX’s AWS account and their on-prem equipment through the AT&T Netbond network. 

Once the routing was correctly set up, our next step was to apply the customer’s security requirements to the connection. Here, we ran into another major hurdle. The network topology did not allow a firewall to be established between the PortX AWS account and the customer’s on-prem network, preventing the customer from establishing required security controls. 

## Option #3: IPsec VPN Tunnel
The firewall restraint forced the team to resort to the third connection option – IPsec VPN tunnels. The customer established a VPN on a redundant part of their virtual firewalls running on [EC2 instances](https://aws.amazon.com/ec2/?ec2-whats-new.sort-by=item.additionalFields.postDateTime&ec2-whats-new.sort-order=desc) in their AWS account. 

We took down the original Transit Gateway peering setup and created IPsec VPN tunnels connected to the Transit Gateway in our AWS account. The customer built the tunnels on their side and worked with the Netbond team to adjust the routing. When we had completed testing, the connection between PortX and the customer’s on-prem equipment was established. The customer was now able to apply their required security controls on the traffic that PortX sent to their network. 

## What we learned
In the end, IPsec VPN tunnels proved to be a significant cost-saving option compared to Direct Connect. And when connected to a firewall appliance, it provided the flexibility needed for the customer to apply the necessary security controls.

If you have had a similar experience connecting AT&T Netbond to AWS or are currently working through these issues, we’d love to hear from you or share additional information about our solution. Comment below or [start a conversation](https://modusbox.com/contact-us/) with our team today.

originally posted on:\
[https://modusbox.com/bank-to-cloud-vpn-cost-saving-options-for-integrating-aws-with-att-netbond](https://modusbox.com/bank-to-cloud-vpn-cost-saving-options-for-integrating-aws-with-att-netbond)
