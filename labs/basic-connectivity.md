## Lab – Basic Connectivity and Default Gateway Troubleshooting

### Objective

Understand and troubleshoot basic network connectivity issues by analyzing network behavior, communication flow, and OSI layers instead of using trial-and-error configuration.

---

### Scenario

A workstation reports that it has no Internet connectivity.
The task is to identify the cause of the problem and restore proper communication using a structured troubleshooting approach.

---

### Network Topology

* 1 PC
* 1 Switch (Cisco 2960)
* 1 Router (Cisco 2911 or equivalent)

Connections:

* PC → Switch (copper straight-through)
* Switch → Router (copper straight-through)

Logical topology:

```
PC ─── Switch ─── Router
```

---

### Initial Configuration (Intentional Misconfiguration)

#### PC Configuration

* IP Address: `192.168.1.10`
* Subnet Mask: `255.255.255.0`
* Default Gateway: `192.168.1.254` (incorrect)
* DNS Server: `8.8.8.8`

#### Router Configuration

```text
enable
configure terminal
interface g0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
```

---

### Initial Test (Do Not Fix Yet)

From the PC command prompt:

```text
ping 192.168.1.1
```

Expected behavior:

* Initial request may time out
* Subsequent replies are successful

---

### Analysis (Thinking Before Fixing)

Questions to evaluate:

* Does the PC have an IP address? Yes
* Is the PC in the same network as the router? Yes
* Is physical connectivity present? Yes

Observation:

* The default gateway configured on the PC does not match the router IP address.

---

### Technical Explanation

The ping to `192.168.1.1` succeeds because:

* Both the PC (`192.168.1.10/24`) and the router (`192.168.1.1/24`) are in the same subnet.
* The default gateway is **not used** for destinations within the local network.
* The PC resolves the router MAC address using ARP and communicates directly.

The initial timeout occurs because:

* The first ICMP packet triggers ARP resolution
* The first packet is lost while ARP completes
* Subsequent packets succeed once the MAC address is cached

TTL value:

* `TTL=255` indicates a direct response from a Cisco router with no intermediate hops.

---

### Corrective Action

Update the PC default gateway to:

```text
192.168.1.1
```

---

### Verification

From the PC:

```text
ping 192.168.1.1
```

Result:

* Successful replies

```text
ping 8.8.8.8
```

Result:

* Successful replies after gateway correction

---

### Root Cause

Incorrect default gateway configuration on the host.

---

### Key Concept Reinforced

* The default gateway is only used when the destination is **outside** the local network.
* Local communication does not depend on the gateway.
* A misconfigured gateway may not be immediately noticeable.

---

### Affected Layer

* Network layer (IP)

---

### Conclusion

This lab demonstrates real-world troubleshooting by:

* Identifying the correct OSI layer of failure
* Understanding local vs remote communication
* Using logic and protocol behavior instead of random changes
* Observing ARP and ICMP behavior in practice

This reflects the mindset and workflow of a real junior network technician.

---
