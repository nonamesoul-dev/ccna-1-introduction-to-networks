# Solution – Basic Connectivity Troubleshooting

## Goal
Provide a structured approach to identify and resolve the connectivity issue presented in the lab scenario.

## Initial Observation
- The host has a valid IP address and subnet mask
- Physical connectivity is intact
- Local devices appear reachable

These observations indicate that the issue is not related to the physical layer.

## Step-by-Step Reasoning

### 1. Test Local Connectivity
From the host, test communication with the router interface:

ping 192.168.1.1

Successful replies confirm that:
- The host and router are in the same subnet
- Layer 2 and local Layer 3 communication are working

### 2. Test External Connectivity
Attempt to reach an external IP address:

ping 8.8.8.8

This test fails, indicating a problem with traffic leaving the local network.

### 3. Inspect Default Gateway Configuration
Review the host network configuration:

ipconfig

The default gateway is configured as:

192.168.1.254

This address does not belong to any active device in the network.

### 4. Identify the Root Cause
The default gateway must point to the router interface responsible for forwarding traffic to external networks.

An incorrect gateway prevents traffic destined for other networks from being routed correctly.

## Resolution
Update the default gateway configuration on the host to:

192.168.1.1

## Verification
After correcting the gateway:
- External ping requests succeed
- Normal packet forwarding is restored

## Key Takeaways
- The default gateway is only used for destinations outside the local subnet
- Local communication does not depend on the gateway
- Layered troubleshooting prevents unnecessary configuration changes

## Professional Insight
This scenario reflects a common real-world issue where local connectivity masks routing problems, emphasizing the importance of structured analysis over trial-and-error troubleshooting.
