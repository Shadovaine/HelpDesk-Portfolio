# Runbook: Printer Not Printing

**Symptom:** Jobs stuck in queue, "offline" status, blank or garbled output
**Applies to:** Network and USB printers, Windows primarily
**Typical resolution time:** 10–25 minutes
**Escalation trigger:** Print server issues affecting many users; hardware
faults requiring vendor service

## Triage Questions

1. One user or everyone? (Single user → local issue; everyone → printer
   itself or the print server)
2. Stuck in queue, spooling forever, or no job appears at all?
3. Any recent change: new printer driver, moved printer, IP change?

## Procedure

### Step 1 — Restart the print spooler (fixes ~half of all tickets)

```powershell
Restart-Service Spooler
# or clear stuck jobs entirely:
Stop-Service Spooler
Remove-Item "$env:WINDIR\System32\spool\PRINTERS\*" -Force
Start-Service Spooler
```

Then resubmit a test print.

### Step 2 — Check printer status/IP

1. Is the printer physically on, out of paper/jam/error on its panel?
2. Ping the printer's IP — network printers move IPs after DHCP renewals and the queue follows a ghost address. Set a DHCP reservation or static IP.
3. Windows: printer properties → Ports tab → confirm the port IP matches the printer's actual IP

### Step 3 — Driver issues

1. Garbled/symbol output → wrong or corrupt driver
2. Remove and re-add the printer; download the current driver from the vendor (avoid Windows Update generics for multifunction units)
3. Use the vendor's universal driver if model-specific is unavailable

### Step 4 — Print a local test page

Printer properties → Print Test Page.

1. Test page prints but documents don't → application-specific issue
2. Test page fails too → queue/driver/hardware path is the problem

### Step 5 — Physical checks (after IT-level checks)

1. Paper jams, toner levels, cover switches
2. Power-cycle the printer (clears its internal queue state)

## Resolution Notes

Note in the ticket: queue state (cleared/retained), driver version installed, and whether a static IP/reservation was applied.

## Escalate When

    Multiple users affected on the same printer → print server team
    Firmware/hardware faults → vendor support
    Print server queue corrupted → print server rebuild per server team

