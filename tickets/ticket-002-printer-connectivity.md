# Ticket #002 – Printer connectivity troubleshooting on Windows 11 Pro

**Status:** Resolved  
**Date:** 2026-06-29
**Category:** Networking / Office - Printer connectivity
**Client PC Environment:** Pc running on Windows 11 Pro
**Host PC Environment:** HP LaserJet Pro M404-M405 printer

---

## Issue
User reported that they are unable to access & make use of the office printer wirelessly (wifi) over local network.

## Observations
- User is connected to local network via wifi connection.
- Printer is connected to the same local network via ethernet cable, but has been confirmed to also have capacity for wireless connection.

## Troubleshooting Steps
1. Verified that the IPv4, subnet mask, & default gateway of the printer & pc match & are valid. All results were positive, yet ticket #002 remains unresolved as user is unable to print from printer via wireless connection.
2. Tested ping to the printer from the user's pc. Result was positive; 2 of 4 packets delivered successfully, yet ticket #002 remains unresolved as user is unable to print from printer via wireless connection.
3. Navigated to device manager to confirm printer's driver is up to date: 'Windows key + X > Device Manager > Printers'. Result was negative; did not find printer in the list of printers.
4. Confirmed that the printer is listed in the pc's devices list by navigating to Windows settings: 'Windows key + I > Bluetooth & devices  > Printers & scanners'. Result was positive.
5. Opened the particular printer's settings by clicking on the printer in the list.
6. Inside the printer's settings, navigated to & clicked 'Print test page'. Result was negative; no feedback.
7. Still inside the printer's settings, navigated to 'Open print queue' to check the status of the file sent to the printer. File was tagged as pending.
8. Still in the printer's print queue, cancelled & cleared all pending print jobs.
9. Back inside the printer's settings, navigated to & clicked on 'Remove' to delete printer from pc device list. Result was positive; printer was deleted from pc devices list.
10. In 'Printer & scanners' settings, navigated to & clicked on 'Add device' > Printer listed, to re-add/install printer to pc's devices list afresh. Result positive; printer was successfully added.
11. Again, opened the particular printer settings by clicking on the printer in the pc's devices list.
12. Inside the printer's settings, navigated to & clicked 'Print test page'. Result was positive; test page request sent,  received & printed successfully.
13. Ticket #002 finally resolved; user's pc successfuly connected with printer via wireless connection, & printed successfully.

## Root Cause
Existing printer configuration on user's pc was outdated.

## Resolution
The issue was resolved by bypassing the GUI & using Windows Command Prompt (CMD) on the host pc:
1. Opened Windows settings 'Windows key + I'
2. Navigated to printer's settings 'Windows settings > Bluetooth & devices  > Printers & scanners > Printer'
3. Inside the printer's settings, deleted printer from pc device list 'Printer > Remove'
4. Re-add printer to pc's device list 'Printers & scanners > Add device'

## Verification:
1. Test page file sent to respective printer, & printed successfully over local network wireless connection.
2. User confirmed respective pc-printer wireless connection operational.

## Status:
Resolved.

## Lessons Learned
- Always try the easiest & most obvious fixes first for printer related issues.
