# Troubleshooting #36: Freshly Racked Leaf Can't Reach the Spine

_Level: Easy — BuildTheNet 3D Sandbox troubleshooting — Interfaces_

**Status: Verified.** Completed on 2026-11-16; every requirement was machine-checked against the network built in BuildTheNet and passed.

Snapshot taken 2026-11-16. Built in a simulation — no physical or production equipment was involved.

## The ticket

**Reported symptom:** DC-Host can't reach ARI-SPINE through the new leaf; the uplink cable tested good.

Helpdesk Ticket #136: A freshly racked Arista leaf, ARI-LEAF, is cabled to ARI-SPINE, but DC-Host cannot reach the spine. The uplink cable was tested and is good.

_Area: Interfaces_

## Required resolution

Bring the leaf's uplink into service so DC-Host reaches ARI-SPINE.

## Checks

| # | Check | Status | Checked by |
|---|---|---|---|
| 1 | DC-Host to ARI-SPINE works | Passed | Sandbox check |
| 2 | DC-Host keeps its documented addressing | Passed | Sandbox check |
| 3 | ARI-SPINE keeps its documented addressing | Passed | Sandbox check |

## Root cause

ARI-LEAF Ethernet2 (the router uplink) is administratively down.

## The fix

What was changed to resolve the ticket, compared with the network as it was reported:

Compared with the starting topology, the learner changed the configuration of 1 device.

**ARI-LEAF**

```diff
  interface Ethernet1
+  ip address 172.20.1.2 255.255.255.0
  interface Ethernet2
-  shutdown
+  no shutdown
+ interface Vlan1
+  no shutdown
+  ip address 172.20.1.2 255.255.255.0
```

## Result

Every check passed on this snapshot:

> The leaf uplink is in service and DC-Host reaches ARI-SPINE.

## Topology

![Topology diagram](topology.svg)

## Addressing

| Device | Interface | Address | Mask | Mode | Gateway |
|---|---|---|---|---|---|
| ARI-LEAF | Ethernet1 | 172.20.1.2/24 | 255.255.255.0 | VLAN 1 |  |
| ARI-LEAF | Vlan1 | 172.20.1.2/24 | 255.255.255.0 | routed |  |
| ARI-SPINE | Ethernet1 | 172.20.1.1/24 | 255.255.255.0 | routed |  |
| DC-Host | FastEthernet0 | 172.20.1.50/24 | 255.255.255.0 | VLAN 1 | 172.20.1.1 |

## Device configurations

Running-configs as `show running-config` prints them in the BuildTheNet sandbox. Passwords, secrets, keys and SNMP communities are replaced with `<removed>`.

- [ARI-LEAF](configs/ARI-LEAF.txt)
- [ARI-SPINE](configs/ARI-SPINE.txt)

---

Built on [BuildTheNet](https://www.buildthenet.io/sandbox)
