# Ticket #005 – Switch Connection

**Status:** Resolved  
**Date:** 2026-07-31  
**Category:** Networking / Office / Switch / Ethernet  
**Server Environment:** RJ45 Ethernet Wall Sockets, Access Point Router  
**Hosts Environment:** PC's running on Windows 11 Pro   

---

## Issue
Groundfloor users (hosts) have an issue where the adjacent wall ethernet double-port outlet (rj45 female sockets) can only connect 2 ethernet cables (rj45 male connectors) at a time, but there is need for 3 connections (for 2 users, 1 access point router) at a time. So when both users need to be connected to the local server to work on Quickbook, the wifi internet access point router would have to be unplugged, & cosequently disabled for use, & vice versa as the priority requires.

## Troubleshooting Steps
1. Purchased a simple 8-port desktop swwitch.  
2. Connected one end of the ethernet cable into port 1 in the switch, & the other end of the same ethernet cable into either of the wall ethernet sockets, in order to connect the switch directly to main router.  
3. Connected one end of another ethernet cable into the second female rj45 wall socket, & the other end into the access point router, in order to connect the access point directly to main router.
4. Connected one end of another ethernet cable into port 2 in the switch, & the other end into the first user's pc; repeated same connection for the second user's pc, in order to connect the users to the main router via the desktop switch.
5. Ticket #004 resolved as the access point router, & all hosts are all connected to the main router through the wall ethernet sockets via the switch with both LAN & WAN access active.

## Root Cause
Insufficient adjacent ethernet sockets (rj45 female sockets) to serve clients.

## Resolution
The issue was resolved by implementing a simple desktop switch to the network connection in order toexpand the LAN capacity;  
1. Connected the switch to the main router via one of the two available wall sockets using an ethernet cable with rj45 connectors.  
2. Connected the access point router to the main router via the second wall socket using an ethernet cable with rj45 connectors.  
3. Separately connected both user pc's to the switch in order to access the main router, using ethernet cables with rj45 connectors.  

## Verification
- Users successfully accessed the internet using wifi connection from the access point router.
- Users successfully accessed the main server through the main router via the LAN connections on the switch.  

## Status
Resolved. End-users confirmed operational.

## Notes
- For routers that do not have default access point (AP) modes (where you have to switch to AP by manually setting router's ip *[default gateway ip]* & disabling dchp), connect ethernet cable into the router's ethernet LAN 1 port & not the WAN port, for connection to the main router or switch.
- For routers that have default access point (AP) modes, connect ethernet cable into the router's ethernet WAN port, for connection to the main router or switch.


## Lessons Learned
- Familiarize with the type of router that is being used as access point, & take the notes above into consideration.

---





