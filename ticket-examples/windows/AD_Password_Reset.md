# Account Locked Out / Password Reset

**Priority:** High
**Category:** Account Management (AD)
**Platform:** Windows / Active Directory
**Requester:** Sarah Jenkins, Marketing
**Reported:** Mon 09:12
**SLA:** 1 hour (user locked out — productivity blocked)

## Original Request

> "Hi, I tried to log in 3 times and now it says my account is locked.
> I need to get into my computer ASAP for a meeting in 20 minutes!"

## Triage

**Assessment:** Account lockout from failed logins. No signs of
compromise so far, but identity verification is mandatory before any
account action.
**Verification:** Called the user back on the 555-5555 in the HR directory
(not the 111-1111 in the ticket) and verified name, department, and manager.
**Hypothesis:** Expired password that user kept re-entering old form, or
caps-lock/keyboard layout issue caused the failures.

## Investigation

### Step 1 — Account status

**powershell**
```powershell
Get-ADUser sarah.jenkins -Properties LockedOut, PasswordExpired
```

Output: LockedOut: True, PasswordExpired: False

### Step 2 — Lockout source check

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4740} -MaxEvents 5 |
  Where-Object {$_.Message -match 'sarah.jenkins'}
```

Output: lockout originated from her workstation (not a mobile device or mapped drive somewhere else — stale credentials elsewhere would mean the unlock would be undone within minutes by a device retrying).

## Resolution

Unlock-ADAccount sarah.jenkins

Then walked her through sign-in: her phone had recently changed keyboard language, which had been inserting an extra character. Confirmed successful login with fresh credentials.
## Closure Communication

    Subject: RE: Locked out of my account!

    Hi Sarah,

    Good news — your account is unlocked and I've confirmed you can sign in. The lockout was triggered by repeated failed sign-in attempts, which is a standard security protection. In this case it looks like your phone's keyboard layout changed and was adding an extra character to your password. Switching it back should prevent it happening again.

    If you have any trouble signing in before your meeting, call me directly.

    Best regards, John Doe — IT Support

## Post-Incident

Root cause: Failed logins from keyboard-layout corruption on user's phone triggered automatic account lockout. Prevention: Advised user to re-save credentials after keyboard changes. No wider action needed — lockout policy working as intended. Cross-reference: Runbook: none needed; KB article candidate: "What happens when my account locks."
