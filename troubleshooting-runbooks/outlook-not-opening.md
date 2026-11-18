
# Runbook: Outlook Won't Open

**Symptom:** Outlook hangs on splash screen, crashes, or opens with errors
**Applies to:** Windows workstations with Outlook (Microsoft 365 / Exchange)
**Typical resolution time:** 15–30 minutes
**Escalation trigger:** Profile corruption requiring mailbox re-sync, or
server-side mailbox issues

## Triage Questions

1. What exactly happens — freeze, error dialog, instant close?
2. Did it ever work on this machine? What changed? (Update, migration, new PC)
3. Can you sign in to Outlook Web (owa.company.com)? (Separates client
   problem from account/server problem)

## Procedure

### Step 1 — OWA test (the great separator)

If mail works in the browser but not the desktop client, the problem is
local to the workstation. If OWA also fails, it's the account/server —
escalate to Exchange team after basic checks.

### Step 2 — Start Outlook safe mode
```
outlook.exe /safe
```

If it opens in safe mode, a broken add-in is the likely cause:
1. File → Options → Add-ins → Manage: COM Add-ins → Go → uncheck all
2. Re-enable one at a time to find the culprit.

### Step 3 — Repair the data file / profile

1. **Repair OST/PST:** Control Panel → Mail → Data Files → open the file
   location → run `SCANPST.EXE` against the data file
2. **Office quick repair:** Settings → Apps → Microsoft 365 → Modify →
   Quick Repair (Online Repair as fallback)

### Step 4 — Recreate the Outlook profile

Last resort before reimage — fixes persistent profile corruption:

1. Control Panel → Mail (32-bit) → Show Profiles → Add new profile
2. Sign in; let the mailbox rebuild (OST re-sync can take time on large
   mailboxes — warn the user)
3. Export/confirm signature and autocomplete are intact or restore them

### Step 5 — Check the basics that get skipped

- Is the mailbox full? (OWA → gear icon → shows quota; full mailbox
  causes bizarre client behavior)
- Cached credentials stale: Windows Settings → Accounts → Access work
  or school → re-authenticate

## Resolution Notes

Document which step fixed it — profile recreation should be noted as
"profile rebuilt" in the ticket, as it resets local cached data.

## Escalate When

- OWA also fails (Exchange/mailbox server issue)
- Licensing/activation errors persist after Online Repair
- Mailbox corruption suspected server-side

