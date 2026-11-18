# Runbook: VPN Connection Fails

**Symptom:** VPN client errors at connect, connects but no traffic flows,
or drops repeatedly
**Applies to:** Corporate VPN (Cisco AnyConnect, Fortinet, OpenVPN, WireGuard)
**Typical resolution time:** 10–30 minutes
**Escalation trigger:** Concentrator/server-side outage; certificate
infrastructure problems

## Triage Questions

1. Exact error code/message? (Codes map to specific causes — capture it)
2. Home network or office? What changed at home recently? (New router,
   ISP, hotel/café Wi-Fi)
3. Does it connect but "do nothing," or fail to connect at all?
4. Have they rebooted since it last worked?

## Procedure

### Step 1 — Baseline connectivity

The VPN rides on top of the internet connection — prove that first per
the no-internet runbook. If baseline fails, fix that first; it's not a
VPN ticket.

### Step 2 — Classic client resets (in order)

1. Quit the VPN client fully (system tray → Exit, not just close) and relaunch
2. Reboot the workstation
3. Reinstall the VPN client (vendor's latest version — stale clients
   fail after concentrator upgrades)

### Step 3 — Error-specific quick fixes

 **Certificate/trust errors:** system clock wrong (misaligned clocks
  break TLS); re-trust/reimport the corporate root cert
 **"No network adapter" / fails after connect:** Windows — reinstall
  the VPN's virtual adapter via Device Manager (View → Show hidden devices)
 **Connects, no traffic:** DNS split-tunnel settings, firewall blocking
  the tunnel interface; test `ping <internal-resource>` and compare name
  vs IP resolution
 **Drops repeatedly:** MTU issues (common on home ISPs) — lowering the
  client MTU or enabling the vendor's "dtls/dpd" tolerance options;
  switching from Wi-Fi to wired as a test

### Step 4 — Local network culprits (remote users)

1. Home router blocking VPN passthrough → check for VPN/IPsec pass-through
   settings or firmware update
2. Hotel/café networks often block VPN ports → hotspot from a phone to test;
   if it works there, the network is the problem
3. ISPCGNAT interference → document for the network team

### Step 5 — Confirm account-side basics

1. Expired password? (Many VPNs authenticate against AD — expired passwords
   fail mysteriously)
2. MFA token working? Time drift in authenticator apps causes MFA loop failures

## Resolution Notes

Always record: client version, error code, network type at the time,
and the exact fix. VPN tickets recur — runbooks depend on this history.

## Escalate When

 Concentrator unreachable by multiple users → Network team (check status
  page first)
 Certificate infrastructure errors → PKI/identity team
 Suspected account lockout/MFA enrollment issue → Identity/IAM team
