# Ticket #003 – Network Shared Folder Access Error | QuickBooks 16.0 (0x80070035)

**Status:** Resolved  
**Date:** 2026-07-13  
**Category:** Networking / Office / QuickBooks Enterprise 16.0
**Client Environment:** PC running Intuit QuickBooks Enterprise 16.0 on Windows 11 Pro
**Host Environment:** Desktop running on Windows 10 Pro

---

## Issue
User (client) tried to access a shared folder located on the server pc, over local network in order to load file into QuickBooks 16.0, but denied access on Windows with error feedback "Windows cannot access \\DESKTOP-IIRP5E1" (server), & error on QuickBooks 16.0 with code: "0x80070035 (The network path was not found)". Similar to ticket #003.

## Troubleshooting Steps

1. Verified that the IPv4, subnet mask, & default gateway of the server & client computers match & are valid. All results were positive, yet ticket #001 remains unresolved as the server remains inaccessible by the client.

2. Tried to manually access shared folder via file explorer on the client using unc directory path search with **\\ + the server's IP address**; result was an empty folder with feedback *"The folder is empty"*.

3. Tested ping to the server from the client; result was successful (3 of 4 packets delivered succesfully), yet ticket #003 remains unresolved as the server remains inaccessible by the client.
   
5. Manually configured access permissions into the targeted directory on the server for client using cmd on the server; on the server's cmd (ran as Admin), I set up new credentials with command **'net user'** & the folder's local permission (NTFS) with **'icacls'**, after which I set up network share using **'net share'**, & then attempted to connect to the server directly using the newly created credentials. These were completed through the following steps:

6. Created a new standalone & dedicated user account via server's cmd that exists only for remotely accessing targeted directory on server over the local area network, using command: **'net user NetworkUser YourPassword /add'** *(replace NetworkUser & YourPassword with brand new credentials for this non-existent & new account. I used clients hostname as NetworkUser credential)*.

7. Granted local NTFS (New Technology File System) permissions using icacls command: **'icacls "C:\Path\To\Your\Folder\On\Server" /grant ServerHostname\NetworkUser:(OI)(CI)F /t'** *(found ServerHostname using command 'hostname' in server's cmd. NetworkUser is the newly created credential in the previous step)*.

8. Tested icacls command success by using command **'icacls "C:\FirstFolderPathLevel"'** *(i.e. 'icacls "C:\Path"')**.

9. Created the network share with command: **'net share ShareName="C:\Path\To\Your\Folder" /grant:ServerHostname\NetworkUser,full'** *(replace ShareName with preferred alias placeholder for explicitly defining targeted resource/directory; this is the share name clients would use to access this shared resource or directory. Replace ServerHostname & NetworkUser with same used in previous steps)*.

10. Mapped server's explicit directory (targeted shared directory) as new drive for direct access in the client's file explorer side panel, by connecting client to server through authentication with the newly created credentials once (doesn't expire sessionally), use command **'net use Z: \\ServerIp\ShareName /user:ServerHostname\NetworkUser YourPassword /persistent:yes'** *(Z: is the preferred new drive letter. 'user:' is a static command flag. Password is for the newly created user account (NetworkUser) used to configure the net share earlier. Replace ShareName placeholder with earlier created share name for targeted resource directory.)*
    
11. Ticket #003 finally resolved; client pc can now remotely access targeted server directories & files remotely.

## Root Cause
Windows 10 Pro's (server) default "Password Protected Sharing" policy was blocking the "Everyone" group used by the gui sharing method, resulting in Windows authentication failure & a misleading 0x80070035 QuickBooks 16.0 error (on client).

## Resolution
Since user regularly uses this operation daily, the issue was resolved by creating new authentication credentials on the server in order to manually give network access permissions to client, then mapping the server's targeted directory explicitly as a new network drive *Z:* for easy access on client with a one-time authentication:
1. Opened Command Prompt as Administrator.
2. Created a dedicated local user account for network access using: **'net user NetworkUser YourPassword /add'**.
3. Granted explicit NTFS (file system) permissions to the targeted QuickBooks directory on the server using: **'icacls "C:\Path\To\Your\Folder\On\Server" /grant ServerHostname\NetworkUser:(OI)(CI)F /t'**.
4. Created the network share with explicit user permissions using: **'net share ShareName="C:\Path\To\Your\Folder\On\Server" /grant:ServerHostname\NetworkUser,full'**.
5. Mapped the server's targeted directory explicitly as a new network drive for easy access on the client in it's file explorer side panel using: **'net use Z: \\ServerIp\ShareName /user:ServerHostname\NetworkUser YourPassword /persistent:yes'**.

## Verification
- Successfully accessed the server's targeted directory remotely from the client after authentication, using the newly created *NetworkUser* credentials on the client without any errors via gui file explorer network directory.
- Successfully loaded server's targeted file inside QuickBooks 16.0 on client from the server's shared network directory.
- Successfully connected to the server remotely from the client using unc path search in file explorer address box with query prompt: **'\\HostComputerIP\ShareName' (replace placeholders respectively)**.
- Newly mapped network drive *Z:* on client's gui file explorer side panel opened without any errors.

## Status
Resolved. End-user confirmed operational.

## Notes
- To view all active connections use command 'net use'.
- To disconnect a session since a non-persistent session remains active until you log off or explicitly disconnect it; in server's cmd use command 'net use \\ServerIp\ShareName /delete'.
- To delete all sessions use command 'net use * /delete'.

## Lessons Learned
- Always look out for spelling & similar errors in credentials, ip adresses & hostnames.
- Use correct direction of slashes; for network paths on Windows OS use backward slashes (\), for command options or flags in Windows command prompt use forward slashes (/).
- Always doubletap; verify & confirm action after each command.

---

