# How-I-Fixed-a-Site-to-Site-IPsec-VPN-That-Temporarily-Broke-My-LAN-Pings-Cisco-Packet-Tracer-Lab-
I recently completed a site-to-site VPN lab in Cisco Packet Tracer. OSPF routing worked perfectly, and both PCs communicated without issues…
…but the moment I applied the IPsec crypto map, LAN-to-LAN pings stopped working.
If this has ever happened to you, here’s exactly what caused it and how it resolved itself.

The Problem
When the crypto map was applied, the routers began treating LAN-to-LAN traffic as "interesting traffic" that must be encrypted via IPsec.
But the IPsec tunnel hadn’t finished negotiating yet, so the first packets were dropped.
This made it look like the VPN “broke” the network
But really, it was just waiting for the tunnel to fully establish.

What I Did 
Honestly?
 Nothing.
 I simply waited a little while.
As soon as the IKE/IPsec negotiations completed, the tunnel came up automatically and the pings immediately succeeded. 
This is normal behavior:
IPsec often needs a few seconds to establish the first Security Associations (SAs), especially in lab environments.

Why It Happened
The crypto ACL marked LAN traffic for encryption
The first packets were dropped because no SA existed yet
The ping itself triggered negotiation
When IKE Phase 1 + Phase 2 completed, pings began to work again
No configuration mistake, just tunnel establishment delay.

Commands I Used to Verify the Tunnel
If you want to confirm tunnel status, these commands are your best friends:
show crypto isakmp sa
show crypto ipsec sa
show crypto map
show access-lists 120

When the tunnel came up:
ISAKMP SA showed "ESTABLISHED"
IPsec SA showed increasing packet counters
PCs began pinging successfully.

Takeaway
Sometimes the issue isn't your configuration, the tunnel just needs time to negotiate.
If your site-to-site VPN suddenly "breaks" pings right after applying a crypto map, give it a few seconds. Then verify SAs before troubleshooting anything else.

