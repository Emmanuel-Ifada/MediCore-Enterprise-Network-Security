# MediCore Healthcare Services
### Enterprise Network Security | Firewall Administration | Network Segmentation | Intrusion Detection

MediCore Healthcare Services is a fictional healthcare organization used as the scenario for this hands-on network security project.

I built a virtual enterprise network to explore how firewall policies, network segmentation and security monitoring can help protect different parts of a healthcare IT environment.

The lab focuses on three network segments: a Corporate LAN for internal users, a DMZ hosting a web server, and an isolated Security Testing network. I used pfSense to manage traffic between these segments, Suricata to detect suspicious activity, and Wireshark to inspect network communications.

This project gave me an opportunity to work through practical firewall configuration, access control, security testing and traffic analysis in a controlled environment.

---

## Project Overview

The environment represents a simplified healthcare organization where internal workstations, public-facing services and security testing systems need to operate within clearly defined network boundaries.

The main objectives were to:

- Design and implement a segmented enterprise network.
- Configure pfSense firewall interfaces and access policies.
- Restrict unnecessary communication between network segments.
- Deploy an Apache web server in the DMZ.
- Use Kali Linux to perform controlled network scanning.
- Configure Suricata for intrusion detection and alert monitoring.
- Capture and investigate network traffic using Wireshark.
- Evaluate the effectiveness and limitations of the implemented controls.

## Network Architecture

![MediCore Healthcare Services Network Architecture](medicore_network_architecture%20%281%29.png)
The lab consists of three internal network segments connected through a central pfSense firewall.

| Network | Subnet | Gateway | Main System |
|---|---|---|---|
| Corporate LAN | 192.168.10.0/24 | 192.168.10.1 | Windows 11 |
| DMZ | 192.168.20.0/24 | 192.168.20.1 | Ubuntu Apache Server |
| Security TEST | 192.168.30.0/24 | 192.168.30.1 | Kali Linux |

**Firewall:** pfSense CE 2.8.1

**WAN:** VMware NAT network (VMnet8)

### Network Design

The Corporate LAN represents internal healthcare staff workstations. The DMZ hosts the web server, while the Security TEST network provides an isolated environment for controlled security testing.

The firewall acts as the central point for managing traffic between these networks.

<!-- Architecture diagram will be added here -->

## Technologies and Tools

| Technology | Purpose |
|---|---|
| VMware Workstation Pro 17 | Virtual lab infrastructure |
| pfSense CE 2.8.1 | Firewall administration and network segmentation |
| Kali Linux | Controlled network scanning and security testing |
| Ubuntu | DMZ web server |
| Apache | HTTP and HTTPS services |
| Suricata | Network intrusion detection and alert monitoring |
| Wireshark | Packet capture and traffic analysis |
| Nmap | Network discovery and service identification |

## Implementation

### 1. Firewall Configuration

I configured pfSense with separate WAN, LAN, DMZ and TEST interfaces.

Firewall policies were created to control communication between the network segments. The rules included:

- Permitting selected HTTP and HTTPS traffic from the Corporate LAN to the DMZ.
- Blocking other tested LAN-to-DMZ traffic, including SSH.
- Restricting DMZ access to the Corporate LAN and TEST networks.
- Allowing required DNS, HTTP and HTTPS traffic from the DMZ.
- Controlling access from the Security TEST network to the DMZ.

### 2. DMZ Web Server

I deployed an Ubuntu server in the DMZ and installed Apache.

Both HTTP (port 80) and HTTPS (port 443) were configured and tested. The server provided a practical target for connectivity checks and security testing.

### 3. Network Security Testing

I used Kali Linux to perform controlled network scans against the DMZ web server.

Nmap successfully identified the Apache HTTP service running on the Ubuntu server. Firewall restrictions were also tested to verify permitted and restricted communication paths.

### 4. Intrusion Detection with Suricata

I installed and configured Suricata on the pfSense TEST interface.

During Nmap testing, Suricata generated an alert:

**ET SCAN Possible Nmap User-Agent Observed**

The alert identified scanning activity originating from the Kali Linux system and directed toward the DMZ web server.

Suricata detection was successfully demonstrated. Automated host blocking was not independently validated, and inline IPS prevention was not demonstrated.

### 5. Packet Analysis with Wireshark

I captured traffic on the pfSense TEST interface and opened the capture in Wireshark for inspection.

The analysis included:

- TCP three-way handshake.
- HTTP HEAD request from Kali to the DMZ web server.
- HTTP 200 OK response from Apache.
- Source and destination IP addresses.
- TCP connection termination.

This provided packet-level confirmation of the permitted HTTP communication path.

## Testing and Results

| Test | Expected Behaviour | Observed Result |
|---|---|---|
| LAN to DMZ HTTP | Allow | Passed |
| LAN to DMZ HTTPS | Allow | Passed |
| LAN to DMZ SSH | Block | Blocked |
| DMZ DNS resolution | Allow | Passed |
| DMZ outbound HTTP/HTTPS | Allow | Passed |
| DMZ access to tested LAN and TEST gateways | Restrict | Blocked |
| Kali to DMZ HTTP | Allow | Passed |
| Nmap service identification | Identify Apache | Successful |
| Suricata Nmap detection | Generate alert | Successful |
| Wireshark traffic inspection | Observe HTTP exchange | Successful |

## Key Findings

The implementation demonstrated that:

- Network segmentation can establish boundaries between internal users, hosted services and security testing systems.
- Firewall rules can permit required services while restricting other communication.
- Suricata can identify and generate alerts for selected scanning activity.
- Wireshark provides useful visibility into network connections and application-layer traffic.
- Successful intrusion detection does not automatically demonstrate successful intrusion prevention.

## Limitations and Future Improvements

This project was conducted entirely within a virtual lab. It does not represent a production hospital network or involve real patient information.

Some areas for further improvement include:

- Validating automated intrusion prevention and host-blocking behaviour.
- Exploring inline IPS deployment.
- Expanding firewall rule testing and security monitoring.
- Introducing additional logging and centralized monitoring capabilities.

These are potential future improvements rather than capabilities claimed as completed.

## Project Environment

The project was implemented using VMware Workstation Pro 17 on a Windows host. All systems and network segments operated within a controlled virtual environment.

**Project type:** Personal technical portfolio project  
**Scenario:** Fictional healthcare organization  
**Focus:** Network security engineering and practical firewall administration

---

*This project was independently developed as a hands-on learning and portfolio exercise. MediCore Healthcare Services is fictional and is not affiliated with a real healthcare provider.*
