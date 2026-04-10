# Enterprise-Security-Lab-FortiGate-Onion
A three-pillar (Networking, Systems, Security) lab simulating enterprise visibility and IDS alerting
# Hybrid Enterprise Lab: Visibility & Detection Engineering

## Executive Summary
This project demonstrates the integration of **Network Engineering**, **Systems Administration**, and **Cybersecurity** to build a functional "Visibility Plane." The lab simulates an enterprise branch network where all egress traffic is monitored by a Security Onion IDS.

The primary technical achievement was engineering a **Layer 2 bypass** for hypervisor-level MAC filtering, ensuring 100% packet integrity for security inspection.

---

## The Architecture

### 1. Networking (The Foundation)
*   **Gateway:** FortiOS 7.x (FortiGate NGFW) managing the internal gateway (192.168.1.1).
*   **Virtualization:** EVE-NG Professional.
*   **The Technical Fix:** Resolved a "Silent Packet Drop" issue by bridging the virtual lab directly to a physical Ethernet interface (VMnet0). This bypassed VMware’s unicast filtering, allowing the IDS to operate in true Promiscuous Mode.

![Lab Topology](Lab-Topology.png)
![VNET Mapping](VNET-Mapping.png)
![fortigate port 2 ip](FortiGate-GUI-Port-addressing.png)

### 2. Systems Administration (The Target)
*   **Endpoint:** Windows 10 Workstation configured with a static IP (`192.168.1.10`).
*   **Connectivity:** Verified bidirectional flow and MTU alignment across the virtual-to-physical boundary.

![SSH SecurtityOnion Ping Verification](SSH-Onion-Windows-VM-Ping-Capture.png)
![Hunt Dashboard Ping Verification](Hunt-Dashboard-Ping-Verification.png)


### 3. Cybersecurity (The Oversight)
*   **Sensor:** Security Onion 2.4 (Suricata/Snort engine).
*   **Validation:** Successfully triggered and logged signature-based alerts for "GPL ATTACK_RESPONSE" using simulated malicious HTTP traffic (testmyids.com).

![GPLAttack Response Alert](Test-My-IDS-Onion-Alert.png)

---

## Lessons Learned
*   **Hypervisor Limitations:** Learned that virtual switches often drop traffic not destined for the VM's specific MAC address, requiring a physical bridge for IDS mirroring.
*   **Data Integrity:** Discovered that trial-license software switches can alter packet headers; shifted to a "Shared Segment" architecture to preserve source-IP visibility for the SOC.

## Next Steps (Phase 2)
*   Integrate Active Directory for centralized identity management.
*   Deploy Sysmon on the Windows endpoint to correlate host logs with network alerts.
