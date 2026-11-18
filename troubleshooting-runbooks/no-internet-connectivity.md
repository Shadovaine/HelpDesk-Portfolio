# Runbook: No Internet Connectivity

**Symptom:** User reports "no internet" / "can't reach websites"
**Applies to:** Windows 10/11 workstations, Linux desktops, VMs
**Typical resolution time:** 10–20 minutes
**Escalation trigger:** Multiple users on the same switch/VLAN affected

## Triage Questions (ask the user first)

1. Can you browse *any* site, or only some? (One site down ≠ no internet)
2. Are other people nearby affected? (Isolate single-user vs. outage)
3. Wired or Wi-Fi? Home or office?
4. Did anything change recently? (New cable, update, moved desks)

## Procedure

### Step 1 — Verify from the workstation

**powershell**
```
ipconfig /all          # Windows — check for valid IP, gateway, DNS
ping 8.8.8.8           # raw connectivity
ping google.com        # DNS resolution
```
**Linux**
```
 ip addr show
 ping
 resolvectl status
```
## Decision tree:

1. No IP at all → Layer 1/2: cable, NIC, DHCP (Step 2)
2. Has IP, can't ping 8.8.8.8 → Layer 3: gateway/routing (Step 3)
3. Can ping IP but not names → DNS problem (Step 4)
4. Everything works → application/browser issue (Step 5)

### Step 2 — Physical/DHCP layer

1. Reseat/replace the network cable; try a different port
     Renew lease: ipconfig /release && ipconfig /renew (Windows) or sudo dhclient -r && sudo dhclient (Linux)
2. Check link lights on the NIC and wall jack
3. Wi-Fi: confirm correct SSID, re-enter password, forget/rejoin the network

### Step 3 — Gateway/routing

1. ping <default-gateway> — if this fails, the local network is the problem
2. Restart the NIC: disable/re-enable adapter in Device Manager, or sudo ip link set <iface> down && sudo ip      link set <iface> up
3. If corporate VPN is involved: disconnect VPN, retest, reconnect

### Step 4 — DNS

1. Test with a known-good resolver: nslookup google.com 8.8.8.8
2. If that works, the configured DNS server is bad:
        Windows: ipconfig /flushdns, check adapter DNS settings
        Linux: check /etc/resolv.conf or NetworkManager connection settings

### Step 5 — Application layer

1. Test in a different browser (rule out proxy extensions/settings)
2. Check system proxy settings (Windows: Settings → Network → Proxy)
3. Antivirus/firewall web filtering can block browsers specifically

## Resolution Notes

Record in the ticket: which layer failed, the exact fix, and verification (user confirms a page loads, not just that ping works).

## Escalate When

    Whole floor/building affected → Network team
    NIC hardware failure suspected → Desktop support swap
    Suspected ISP/outage → Confirm with monitoring before escalating


