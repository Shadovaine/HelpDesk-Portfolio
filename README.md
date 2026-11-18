# Help Desk Portfolio

## IT Support Troubleshooting Labs & Documentation Samples

A hands-on portfolio demonstrating the core competencies expected of a Tier 1/Tier 2 Help Desk technician: structured troubleshooting, clear ticket documentation, user-facing communication, and repeatable knowledge-base articles.

**Status: In active development** — see the [Roadmap](#roadmap) for what's coming next.

---

## Why This Repo Exists

Transitioning from military service and healthcare into IT, I built this portfolio to prove — not just claim — the skills help desk work runs on:

- Diagnosing problems methodically instead of guessing
- Documenting solutions so the next tech (or the user) benefits
- Communicating with frustrated users clearly and empathetically
- Turning one-off fixes into reusable knowledge base articles

Each lab or document in this repo simulates a realistic scenario a help desk tech faces on shift.

---

## Repository Structure

```
help-desk-portfolio/
│
── docs
│   ├── closure-email-template.md
│   ├── escalation-matrix.md
│   └── ticket-template.md
├── knowledge-base-articles
│   ├── map-shared-drive.md
│   ├── safe-browsing-tips.md
│   └── setup-email-mobile.md
├── labs
│   ├── active-directory-basic-ou.md
│   └── ticketing-system-helpdeskru.md
├── README.md
├── scripts
│   ├── bash
│   │   └── disk-report.sh
│   └── powershell
│       ├── Get-Diskusage.psl
│       ├── New-BulkUsers.psl
│       └── Unlock-UserAccount.psl
├── ticket-examples
│   ├── linux
│   │   ├── Disk_Full_linux.md
│   │   ├── Shared_Drive_Unavailable.md
│   │   └── VM_No_Internet.md
│   └── windows
│       ├── AD_Password_Reset.md
│       ├── Bulk_Onboarding_Automation.md
│       ├── Disk_Full_Windows.md
│       ├── New_Hire_AD_Provisioning.md
│       ├── Outlook_Not_Sending.md
│       └── Printer_Offline_Spooler.md
└── troubleshooting-runbooks
    ├── no-internet-connectivity.md
    ├── outlook-not-opening.md
    ├── printer-not-printing.md
    ├── slow-computer.md
    └── vpn-connection-fails.md


```

---

## Featured Documents

### 🔧 [Troubleshooting Runbooks](troubleshooting-runbooks/)
Structured diagnostic guides written the way I actually work a problem: identify symptoms → isolate the layer → test systematically → confirm the fix → document it. Written so a brand-new tech could follow them at 2 PM on a Monday with a queue of 20 tickets.

### 🎫 [Ticket Documentation Examples](ticket-examples/)
Sample tickets showing proper documentation hygiene: clear problem statements, actions taken with timestamps, resolution notes, and plain-language user communication. Good ticket notes are the difference between a help desk and a help burden.

### 📖 [Knowledge Base Articles](knowledge-base-articles/)
End-user-facing guides — because the best tickets are the ones users never have to open. Written at a level that respects the reader without assuming IT fluency.

### ⚡ [Scripts](scripts/)
Small PowerShell utilities for the common triage steps every tech runs daily: gathering system info, clearing DNS caches, checking disk space. Each script includes usage instructions and a comment header explaining what it does and why.

### 🏗️ [Labs](labs/)
Home-lab environments I built and worked through (Active Directory basics, open-source ticketing systems), written up as walkthroughs with what broke and how I fixed it — because the fixes are the actual learning.

---

## Skills Demonstrated

| Skill | Where You'll See It |
|---|---|
| Structured troubleshooting | `troubleshooting-runbooks/` |
| Ticket documentation & SLA awareness | `ticket-examples/` |
| User-facing technical writing | `knowledge-base-articles/` |
| Windows administration & PowerShell | `scripts/` |
| Active Directory fundamentals | `labs/lab-active-directory-basic-ou.md` |
| Escalation judgment | `docs/escalation-matrix.md` |

---

## Roadmap

- [ ] Additional runbooks: Wi-Fi drops, mobile email setup, MFA enrollment issues
- [ ] Escalation matrix expansion: SLA tiers and priority definitions
- [ ] More PowerShell triage scripts with logging
- [ ] Lab: full help desk simulation in a ticketing platform (ticket intake → triage → resolution → KB article)
- [ ] Customer service scripts: de-escalation phrasing samples

---

## About Me

I'm an Army veteran and former healthcare professional building a career in IT support and system administration. I believe in learning in public — this portfolio grows as I do. Check out my companion repo, [SysAdmin-iLabs](https://github.com/Shadovaine/SysAdmin-iLabs), for Linux system administration labs.

---

*Feedback welcome — if you spot a better way to run a diagnostic or phrase a user response, open an issue or reach out.*
