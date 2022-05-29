---
title: Exchange 2003 to 2010 mail flow issue
layout: post
categories: [Exchange 2010]
tags: Exchange
---
Durring the exchange 2010 install the routing group connector didn&#8217;t get created so we need to run the following command in the exchange management shell to create the routing group connector:
```powershell
New-RoutingGroupConnector -Name "2010-2003" -SourceTransportServers "Ex2010Hub1.contoso.com" -TargetTransportServers "Ex2003BH1.contoso.com" -Cost 10 -Bidirectional $true -PublicFolderReferralsEnabled $true
```
You also want to make sure that your smtp virtual server on exchange 2003 and exchange 2010 is configured to work on port 25 or you will still have mail flow issues.

After running the above command and verifying that your smtp settings your mail flow should now be working.
