# VLAN Segmentation & Firewall Access Control Lab

## 1. Project Overview

This project demonstrates the implementation of a segmented network using **pfSense, a managed network switch, and physical endpoint devices**.

The primary goal of the lab was to separate network traffic into multiple VLANs and use pfSense firewall rules to control communication between VLANs and access to the Internet.

The lab includes:

- pfSense firewall/router
- Managed Layer 2/Layer 3-capable switch
- Windows workstation
- Ubuntu workstation
- VLAN 10
- VLAN 20
- Inter-VLAN routing
- Allow and block firewall rules
- Connectivity and security testing

The project was designed as a hands-on networking and cybersecurity exercise to understand how network segmentation can be used to limit unauthorized access and reduce the potential impact of a compromised device.

---

# 2. Lab Objectives

The objectives of this project were to:

1. Configure pfSense as the primary firewall/router.
2. Configure VLAN 10 and VLAN 20.
3. Configure VLAN connectivity through a managed switch.
4. Assign endpoints to their respective VLANs.
5. Configure inter-VLAN communication.
6. Create firewall rules to allow required traffic.
7. Create firewall rules to block unauthorized traffic.
8. Prevent selected VLAN traffic from accessing the Internet.
9. Test connectivity between network segments.
10. Document the results and troubleshooting process.

---

# 3. Network Architecture

### Network Components

| Device | Purpose | IP Address |
|---|---|---|
| pfSense | Firewall/Router | [Enter IP] |
| Managed Switch | VLAN Connectivity | [Enter IP] |
| Windows PC | Management/Test Host | [Enter IP] |
| Ubuntu PC | VLAN 10 Endpoint | [Enter IP] |
| VLAN 20 | Network Segment | [Enter Subnet] |


### Network Diagram

**Internet/Modem → pfSense → Managed Switch → VLAN 10 / VLAN 20 Endpoints**



---

# 4. VLAN Configuration

Two VLANs were created to separate network traffic.

### VLAN 10

- VLAN ID: `10`
- Subnet: `[Enter subnet]`
- Purpose: `[Management/Trusted Network/etc.]`
- Gateway: `[Enter gateway]`

### VLAN 20

- VLAN ID: `20`
- Subnet: `[Enter subnet]`
- Purpose: Restricted/segmented endpoint network
- Gateway: `[Enter gateway]`

The VLAN configuration allows devices to remain physically connected to the same managed switch while logically separating their network traffic.

---

# 5. Managed Switch Configuration

The managed switch was configured to support the VLAN environment.

### Port Configuration

| Switch Port | Device | VLAN Mode | VLAN |
|---|---|---|---|
| Port [X] | pfSense | Trunk | 10, 20 |
| Port [X] | Windows PC | Access | 10 |
| Port [X] | Ubuntu PC | Access | 20 |
| Port [X] | Other Device | [Access/Trunk] | [VLAN] |

The trunk connection between the switch and pfSense allows VLAN-tagged traffic to reach the firewall.

Access ports were used for endpoint devices assigned to individual VLANs.


---

# 6. pfSense Configuration

pfSense was configured as the central firewall and router for the segmented network.

The VLAN interfaces were created under the pfSense interface configuration.

### VLAN Interfaces

- VLAN 10 interface: `[Interface Name]`
- VLAN 20 interface: `[Interface Name]`

Each VLAN was assigned its own gateway IP address.


---

# 7. Firewall Rules

Firewall rules were created to control traffic between the VLANs and the Internet.

## Allow Rules

An allow rule was created to permit required communication between the VLAN and the internal lab network.

Example:

**VLAN 20 → Internal Lab Network → ALLOW**

Purpose:

- Allow VLAN 20 devices to communicate with required internal resources.
- Permit authorized management/testing traffic.



---

## Block Rules

A block rule was created to prevent VLAN 20 from accessing the Internet.

Example:

**VLAN 20 → Internet → BLOCK**

This demonstrates how firewall policies can be used to restrict a compromised or untrusted endpoint from communicating with external networks.



---

# 8. Firewall Rule Order

The firewall processes rules according to their order.

The lab was configured so that the required internal traffic was permitted while unwanted Internet traffic was blocked.

Example policy:

1. Allow required internal/LAN traffic
2. Block VLAN 20 Internet access
3. Deny remaining unauthorized traffic

This demonstrates the importance of **rule order and traffic direction** when designing firewall policies.

---

# 9. Connectivity Testing

Connectivity tests were performed to verify that the VLAN configuration and firewall rules were working as intended.

### Test 1 — VLAN 20 → pfSense

---

### Test 2 — VLAN 20 → Internal Network

**Expected:** Allow

---

### Test 3 — VLAN 20 → Internet

**Expected:** Block

**Result:** `[PASS/FAIL]`

Example test:

```text
ping 8.8.8.8
```

or

```text
curl ifconfig.me
```

Expected behavior:

```text
Internet access blocked
```

---

### Test 4 — VLAN 10 → Internet

**Expected:** Allow

**Result:** `[PASS/FAIL]`

This test verifies that the Internet restriction applied specifically to the intended VLAN.

---

# 10. Security Testing

The segmentation was tested from the perspective of a device located on VLAN 20.

The objective was to determine whether a device on the restricted VLAN could:

- Access the pfSense gateway
- Access internal network resources
- Access VLAN 10
- Access the Internet
- Communicate with unauthorized devices

### Results

| Test | Expected | Result |
|---|---|---|
| VLAN 20 → Gateway | Allow | [PASS/FAIL] |
| VLAN 20 → Internal Network | Allow | [PASS/FAIL] |
| VLAN 20 → VLAN 10 | [Allow/Block] | [PASS/FAIL] |
| VLAN 20 → Internet | Block | [PASS/FAIL] |
| VLAN 10 → Internet | Allow | [PASS/FAIL] |

---

# 11. Troubleshooting

During the implementation of the lab, several networking issues were encountered and investigated.

Examples included:

- Incorrect IP addressing
- VLAN configuration issues
- Gateway configuration
- Firewall rule ordering
- Network adapter configuration
- Switch port configuration
- Connectivity between VLANs

Troubleshooting was performed using tools such as:

```text
ping
ip addr
ip route
ipconfig
traceroute
```

pfSense firewall logs were also used to determine whether traffic was being permitted or blocked.



---

# 12. Results

The completed lab demonstrated that VLANs can be used to logically separate devices on the same physical network.

pfSense successfully provided firewall enforcement between network segments.

The VLAN 20 security policy successfully restricted Internet access while allowing the required internal network communication.

This demonstrates a practical implementation of **network segmentation and least-privilege access control**.

---

# 13. Security Benefits

Network segmentation provides several security benefits:

- Limits lateral movement
- Separates trusted and untrusted devices
- Reduces the attack surface
- Controls communication between network segments
- Allows different security policies for different devices
- Helps contain compromised endpoints

For example, if a device on VLAN 20 were compromised, firewall restrictions could prevent that device from freely communicating with other network segments or the Internet.

---

# 14. Lessons Learned

Through this project, I gained hands-on experience with:

- VLAN configuration
- Network segmentation
- pfSense firewall administration
- Managed switch configuration
- Inter-VLAN routing
- Firewall rule creation
- Access control
- Network troubleshooting
- Connectivity testing
- Basic network security architecture

One of the most important lessons from this project was that **VLANs provide logical separation, but firewall rules determine what traffic is actually permitted between those segments.**

---


# 16. Evidence

The following screenshots will be added to document the implementation:

- [ ] pfSense VLAN configuration
- [ ] Managed switch VLAN configuration
- [ ] Trunk port configuration
- [ ] VLAN 10 endpoint configuration
- [ ] VLAN 20 endpoint configuration
- [ ] Allow firewall rule
- [ ] Block firewall rule
- [ ] Firewall logs
- [ ] Successful ping test
- [ ] Blocked Internet test
- [ ] Network topology diagram

---

# 17. Conclusion

This project successfully demonstrated the implementation of VLAN-based network segmentation using pfSense and a managed switch.

By creating VLAN 10 and VLAN 20 and applying firewall policies, the lab demonstrated how network administrators can control communication between different network segments.

