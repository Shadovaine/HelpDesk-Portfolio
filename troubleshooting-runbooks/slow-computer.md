# Runbook: Slow Computer

**Symptom:** General sluggishness, long boot times, apps freezing
**Applies to:** Windows 10/11 workstations (Linux notes inline)
**Typical resolution time:** 20–40 minutes
**Escalation trigger:** Failing disk (SMART errors), memory upgrade needed,
malware beyond AV remediation

## Triage Questions

1. Slow at all times, or only when doing something specific?
2. Slow since when? (Update, install, age — "it's always been slow" needs
   quantifying: boot time, app launch time)
3. How much free disk space is showing?

## Procedure

### Step 1 — Quantify first (don't guess)

```powershell
Get-PSDrive C                       # free space — anything under ~10% is a problem
Get-CimInstance Win32_OperatingSystem |
  Select-Object TotalVisibleMemorySize, FreePhysicalMemory   # memory pressure
```

Task Manager → Performance tab: CPU/RAM/Disk graphs while reproducing the slowness. The pegged resource tells you which path to follow.

### Step 2 — Disk near full → clean up

1. Storage Sense / Disk Cleanup: temp files, recycle bin, Windows Update cache
2. User-profile hogs: Get-ChildItem $env:USERPROFILE -Recurse | Sort-Object Length -Descending | Select-Object -First 20
3. Move/Archive large personal files per policy

### Step 3 — CPU/RAM pegged → find the process

Task Manager → sort by CPU and Memory. Common culprits:

1. Endpoint AV scans (schedule off-hours instead)
2. Browser tab overload / runaway renderer
3. Backup or sync agents (OneDrive initial sync is notoriously heavy)
4. Rogue process → investigate before killing; check with the user first

### Step 4 — Check disk health

Get-PhysicalDisk | Get-StorageReliabilityCounter
Get-Disk   # watch for SMART warnings

Clunky HDDs in modern environments are a legitimate upgrade ticket (SSD swap) — document measured boot/app times to justify it.

### Step 5 — Startup bloat

Task Manager → Startup tab: disable non-essential autoruns (reboot afterwards and re-measure). Linux: systemd-analyze blame, systemctl --user list-units.

### Step 6 — Malware sweep (if unexplained)

Full AV scan; check browser extensions; document findings before removal. Follow security team's process for anything suspicious.

## Resolution Notes

Record the measured before/after numbers (boot time, free space, RAM headroom). "Felt faster" is not a resolution metric.

## Escalate When

    SMART failure imminent → hardware replacement queue immediately
    Malware found → security incident process (do not handle solo)
    RAM/disk upgrade justified → procurement workflow

