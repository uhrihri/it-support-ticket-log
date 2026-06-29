# Ticket #001 – Network Shared Folder Access Error on Windows 11 Pro | QuickBooks 16.0 - Network Path Not Found (0x80070035)

**Status:** Resolved  
**Date:** 2026-06-26  
**Category:** Networking / Office - QuickBooks Enterprise 16.0
**Client PC Environment:** Intuit QuickBooks Enterprise 16.0 in Windows 11 Pro
**Host PC Environment:** Windows 10 Pro

---

## Issue
User (client pc) tried to access a shared folder located on the host pc, via shared local network in order to load into QuickBooks 16.0, but denied access on Windows with error feedback "Windows cannot access \\DESKTOP-IIRP5E1" (host pc), & on QuickBooks 16.0 with error code: "0x80070035 (The network path was not found)".

## Troubleshooting Steps
1. Verified that the IPv4, subnet mask, & default gateway of the server & host pc's match & are valid. All results were positive, yet ticket #001 remains unresolved as network directory remains "empty" or inaccessible from the client pc.
2. Tried to manually access shared folder via file explorer on the client pc using directory path search with \\ + the host pc's IP address; result was an empty folder with feedback "The folder is empty".
3. Tested ping to the host pc from client pc; result was successful (3 of 4 packets delivered succesfully), yet ticket #001 remains unresolved as targeted network directory remains "empty" or inaccessible from the client pc.
4. Disabled firewall defender & other network security restrictions on host pc via the respective & only active antivirus app, yet ticket #001 remains unresolved as targeted network directory remains "empty" or inaccessible from the client pc
5. Issue unresolved as of this point. I changed troubleshooting approach from this point to manually giving client pc access permissions into the host pc's shared folders using cmd on the host pc; on the host pc's cmd (ran as Admin), I set up new credentials with 'net User' & the folder's local permission with 'icacls', after which I set up network share using 'net share'. These were completed through the following steps:
6. Created a new standalone & dedicated user account via host pc's cmd that exists only for accessing host pc's shared directories over the network, using command: 'net User NetworkUser YourPassword /add' (replace NetworkUser & YourPassword with brand new credentials for this non-existent & new account. I used client pc's name as NetworkUser credential).
7. Granted local NTFS (New Technology File System) permissions using icacls command: 'icacls "C:\Path\To\Your\Folder" /grant HostComputerName\NetworkUser:(OI)(CI)F /t' (found HostComputerName using command 'hostname' in host pc's cmd, NetworkUser is the new credential created in the previous step).
8. Created the network share with command: 'net share ShareName="C:\Path\To\Your\Folder" /grant:HostComputerName\NetworkUser,full' (replace ShareName with preferred placeholder. Replace HostComputerName & NetworkUser with same used in previous steps).
9. Tested by successfully accessing the host pc's shared folder remotely from client pc, & successfully loaded host pc's shared file inside QuickBooks 16.0 on client pc.
10. Also tested by successfully connecting to the host pc remotely from client pc's file explorer with direct network directory search: '\\HostComputerIP\ShareName' (replace placeholders respectively)
11. Ticket #001 finally resolved; client pc can now remotely access directories & files on the host pc.

## Root Cause
Windows 10 Pro's (on host pc) default "Password Protected Sharing" policy was blocking the "Everyone" group used by the GUI sharing method, resulting in Windows authentication failure & a misleading 0x80070035 QuickBooks 16.0 error (on client pc).

## Resolution
The issue was resolved by bypassing the GUI & using Windows Command Prompt (CMD) on the host pc:
1. Opened Command Prompt as Administrator.
2. Created a dedicated local user account for network access using: 'net User NetworkUser YourPassword /add'
3. Granted explicit NTFS (file system) permissions to the QuickBooks folder using: 'icacls "C:\Path\To\Your\Folder" /grant HostComputerName\NetworkUser:(OI)(CI)F /t'
4. Created the network share with explicit user permissions using: 'net share QB_ri_ShareName="C:\Path\To\Your\Folder" /grant:HostComputerName\NetworkUser,full'

## Verification:
1. Successfully accessed '\\192.168.0.121\QB_ri_ShareName' on the host pc remotely from the client PC after authentication using the newly created NetworkUser credentials.
2. The host pc's shared direectories opened remotelyover the network on the client pc without errors.
3. The user successfully loaded the QuickBooks company file on the host pc remotely into the QuickBooks 16.0 app on client pc.

## Status:
Resolved. End-user confirmed operational.

## Lessons Learned
- Always test ping from the client pc's cmd first for network-related issues in order to narrow down root cause earlier.
