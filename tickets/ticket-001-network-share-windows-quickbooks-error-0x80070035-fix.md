# Ticket #001 – Network Shared Folder Access Error on Windows 11 Pro | QuickBooks 16.0 - Network Path Not Found (0x80070035)

**Status:** Resolved  
**Date:** 2026-06-26  
**Category:** Networking / Office - QuickBooks Enterprise 16.0
**Client PC Environment:** Intuit QuickBooks Enterprise 16.0 in Windows 11 Pro
**Host PC Environment:** Windows 10 Pro

---

## Issue
User (client) tried to access a shared folder located on the server pc, over local network in order to load file into QuickBooks 16.0, but denied access on Windows with error feedback "Windows cannot access \\DESKTOP-IIRP5E1" (server), & error on QuickBooks 16.0 with code: "0x80070035 (The network path was not found)".

## Troubleshooting Steps

1. Verified that the IPv4, subnet mask, & default gateway of the server & client computers match & are valid. All results were positive, yet ticket #001 remains unresolved as the server remains inaccessible by the client.

2. Tried to manually access shared folder via file explorer on the client using unc directory path search with \\ + the server's IP address; result was an empty folder with feedback "The folder is empty".

3. Tested ping to the server from the client; result was successful (3 of 4 packets delivered succesfully), yet ticket #001 remains unresolved as the server remains inaccessible by the client.

4. Disabled firewall defender & other network security restrictions on server using the active antivirus app, yet ticket #001 remains unresolved as the server remains inaccessible by the client.

5. Issue unresolved as of this point. I changed troubleshooting approach from this point towards manually giving client access permissions into the targeted directory on the server using cmd on the server; on the server's cmd (ran as Admin), I set up new credentials with command 'net user' & the folder's local permission (NTFS) with 'icacls', after which I set up network share using 'net share', & then attempted to connect to the server directly using the newly created credentials. These were completed through the following steps:

6. Created a new standalone & dedicated user account via server's cmd that exists only for remotely accessing targeted directory on server over the local area network, using command: 'net user NetworkUser YourPassword /add' (replace NetworkUser & YourPassword with brand new credentials for this non-existent & new account. I used clients hostname as NetworkUser credential).

7. Granted local NTFS (New Technology File System) permissions using icacls command: 'icacls "C:\Path\To\Your\Folder\On\Server" /grant ServerHostname\NetworkUser:(OI)(CI)F /t' (found ServerHostname using command 'hostname' in server's cmd. NetworkUser is the newly created credential in the previous step).

8. Tested icacls command success by using command 'icacls "C:\FirstFolderPathLevel"' (i.e. 'icacls "C:\Path"').

9. Created the network share with command: 'net share ShareName="C:\Path\To\Your\Folder" /grant:ServerHostname\NetworkUser,full' (replace ShareName with preferred alias placeholder for explicitly defining targeted resource/directory; this is the share name clients would use to access this shared resource or directory. Replace ServerHostname & NetworkUser with same used in previous steps).

10. To connect client to server with new credentials, using unc path (without mapping new drive letter for server file share resource on client); in GUI: open windows explorer 'win+e', in address box (works the same using run dialog 'win+r'; paste unc path too) type unc path '\\serverIP\ShareName' or '\\serverHostname\ShareName', enter credentials when prompted (e.g. username: ServerHostnameOrIp\ShareName, use YourPassword.

11. To connect client to server with new credentials, using CMD to connect (without mapping a drive letter), use command 'net use \\ServerIp\ShareName /user:ServerHostname\NetworkUser YourPassword /persistent:yes' (persistent:no means you have to enter credentials for every new session after you log off or reboot the client, while the yes option saves your credentials & gives you access after you navigate to the resource).

12. To map server's explicit directory (targeted shared resource/directory) as new drive for direct access on client by connecting client to server using credentials once (doesn't expire sessionally), use command 'net use Z: \\ServerIp\ShareName /user:ServerHostname\NetworkUser YourPassword /persistent:yes'. Z: is the preferred new drive letter. 'user:' is a static command flag. Password is for the newly created user account (NetworkUser) used to configure the net share earlier. Replace ShareName placeholder with earlier created share name for targeted resource directory.

13. Tested by successfully accessing the server's shared directory remotely from the client using the different methods listed above, & successfully loaded server's targeted file inside QuickBooks 16.0 on client.

14. Also tested by successfully connecting to the server remotely from the client using unc path search in file explorer address box with: '\\HostComputerIP\ShareName' (replace placeholders respectively).

15. Ticket #001 finally resolved; client pc can now remotely access directories & files on the server pc.

## Root Cause
Windows 10 Pro's (on host pc) default "Password Protected Sharing" policy was blocking the "Everyone" group used by the GUI sharing method, resulting in Windows authentication failure & a misleading 0x80070035 QuickBooks 16.0 error (on client pc).

## Resolution
The issue was resolved by bypassing the GUI & using Windows Command Prompt (CMD) on the host pc:
1. Opened Command Prompt as Administrator.
2. Created a dedicated local user account for network access using: 'net user NetworkUser YourPassword /add'
3. Granted explicit NTFS (file system) permissions to the targeted QuickBooks directory on the server using: 'icacls "C:\Path\To\Your\Folder\On\Server" /grant ServerHostname\NetworkUser:(OI)(CI)F /t'
4. Created the network share with explicit user permissions using: 'net share ShareName="C:\Path\To\Your\Folder\On\Server" /grant:ServerHostname\NetworkUser,full'

## Verification:
1. Successfully accessed server's targeted directory remotely from the client pc after authentication using the newly created NetworkUser credentials.
2. The server's shared directories opened remotely over the network on the client pc without errors.
3. The user successfully loaded the QuickBooks company file remotely from the server into the QuickBooks 16.0 app installed on the client.

## Status:
Resolved. End-user confirmed operational.

## Notes:
- To disconnect a session since a non-persistent session remains active until you log off or explicitly disconnect it; in server's cmd use command 'net use \\ServerIp\ShareName /delete', or to delete all sessions use command 'net use * /delete'.
- Use command 'net use' to view all active connections.

## Lessons Learned
- Always look out for spelling errors in usernames & hostnames.
- Use correct direction of slashes; for network paths backward slashes (\), for command options or flags use forward slashes (/).
- Always doubletap; verify & confirm action after each command.






