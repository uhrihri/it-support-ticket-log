# Ticket #001 – QuickBooks error code: 0x80070035 

**Status:** Resolved  
**Date:** 2026-06-26  
**Category:** Networking / Office - QuickBooks
**Client PC Environment:** Intuit QuickBooks Enterprise 16.0 in Windows 11 Pro
**Host PC Environment:** Intuit QuickBooks Enterprise 16.0 in Windows 10 Pro

---

## Issue
Trying to access a shared folder on the shared local network, but denied access with an error feedback; "Windows cannot access \\DESKTOP-IIRP5E1" with error code: 0x80070035 (The network path was not found.).

## Troubleshooting Steps
1. Verified that the IPv4, subnet mask, and default gateway of the server & host pc's match & are valid. All results were positive.
2. Tried to manually access shared folder via file explorer on the client pc using \\ servers IP address. Result was an empty directory saying "The folder is empty".
3. Tested ping to the server pc from client pc. Result was successful, yet ticket #001 remains unresolved; network directory remains "empty" or inaccessible.
4. Toggled off firewall defender on host pc, yet ticket #001 remains unresolved; network directory remains "empty" or inaccessible.
5. Issue unresolved as of this point. Changed troubleshooting approach from this point; I set up network share using 'net share' from the host pc's cmd (run as Admin), & after that I set up the folder's local permission with 'icacls'. These were completed through the following steps:
6. Created a new standalone & dedicated user account in host pc's cmd that exists only for accessing this specific shared folder over the network, using command: 'net User NetworkUser YourPassword /add' (replace NetworkUser & YourPassword with brand new credentials for this non-existent & new account. I used client pc's name as NetworkUser credential).
7. Granted local NTFS permissions using icacls command: 'icacls "C:\Path\To\Your\Folder" /grant HostComputerName\NetworkUser:(OI)(CI)F /t' (found HostComputerName using command 'hostname' in host pc's cmd, NetworkUser is the new credential created in the previous step).
8. Created the network share with command: 'net share ShareName="C:\Path\To\Your\Folder" /grant:HostComputerName\NetworkUser,full' (replace ShareName with preferred placeholder. Replace HostComputerName & NetworkUser with same used in previous steps).
9. Tested by successfully accessing the host pc remotely from client pc.
10. Also tested by successfully connecting to the host pc remotely from client pc file explorer with directory search: \\HostComputerIP\ShareName (replace placeholders respectively)
11. Ticket #001 finally resolved; client pc can now remotely access directories & files on the host pc.

## Root Cause
The GUI failed because it relied on the "Everyone" (Guest) account, which Windows 10 Pro's default "Password Protected Sharing" policy blocks over the network.

## Resolution
CMD method succeeded by creating and authenticating a specific local user account that the policy allowed.

## Lessons Learned
- Always test ping from the client pc's cmd first for network-related issues in order to narrow downroot cause.
- `ip route` is a fast way to see if a default route exists.
- Documenting my home network’s actual gateway in a note would prevent future mistakes.
