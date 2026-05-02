# Wireless Network Deployment – Ajman University J2 Building
 
A Cisco Packet Tracer simulation of a full enterprise wireless network deployment for the J2 Building at Ajman University, designed for the course **INT433 – Wireless Networks**.
 
---
 
## Table of Contents
 
- [Abstract](#abstract)
- [Network Design](#network-design)
  - [Building Layout and AP Placement](#building-layout-and-ap-placement)
  - [Topology](#topology)
  - [Design Considerations](#design-considerations)
- [Devices](#devices)
- [IP Addressing](#ip-addressing)
- [Implementation](#implementation)
- [Security](#security)
- [IoT Integration](#iot-integration)
- [Testing and Verification](#testing-and-verification)
- [Conclusion](#conclusion)
- [Tools Used](#tools-used)
---
 
## Abstract
 
This project simulates the deployment of a wireless network for the J2 Building at Ajman University. The objective is to provide wireless access to internet and local services for a large number of students and staff across multiple floors and blocks. Security analysis and implementation of an appropriate WLAN security scheme were the primary focus of this project.
 
---
 
## Network Design
 
### Building Layout and AP Placement
 
The J2 Building consists of **3 floors and a rooftop**, divided into **blocks A, B, and C**, housing approximately 50 halls and laboratories per floor.
 
Access Point (AP) placement strategy:
 
| Area | AP Count | Notes |
|---|---|---|
| Blocks A and B (per floor) | 4 APs each | Mirrored layout; 2 APs for male side, 2 for female side |
| Block C (per floor) | 3 APs | 2 for female side, 1 for male side (arch-shaped layout) |
| Rooftop – Block A | 1 AP | Covers the two halls on block A |
| Rooftop – Block C | 1 AP | Covers the admin management room |
 
A secured **management room** on the third floor of Block C houses the Wireless LAN Controller (WLC) and all servers. An **RFID-secured door** with a single assigned RFID card controls access to this room.
 
### Topology
 
The network uses a **hybrid topology**:
 
| Topology Layer | Role |
|---|---|
| Bus topology | Connects switches horizontally across each floor |
| Star topology | Connects all devices within the management room |
| Tree topology (overall) | APs as leaf nodes, floor core switches as intermediate nodes, management switch as root |
 
This structure ensures reliable performance and easy scalability for future AP additions.

### Network Architecture Diagram

Below is a diagram to illustrate the logical structure for the entire network.
 
<img width="651" height="1359" alt="image" src="https://github.com/user-attachments/assets/6f30791e-636e-400f-8fbe-e0c91f8b92b2" />

### Design Considerations
 
**WLC + AP over Wireless Routers:**
A centralized WLC+AP architecture was chosen over individual wireless routers for the following reasons:
 
- Centralized configuration for all APs from a single management interface
- Seamless handoff between APs for roaming users across floors and blocks
- Uniform subnet across the entire building (wireless routers would create separate subnets per device)
**Switch Meshing:**
Full mesh connectivity between switches was considered but rejected in favor of a simpler floor-to-floor link design. Full mesh, while offering fault tolerance and redundancy, introduces cabling complexity and requires Spanning Tree Protocol (STP) to prevent loops — better suited for mission-critical environments than a university setting.
 
**Server Architecture:**
Combining all services into a single server was considered but dropped. Given the volume of simultaneous traffic from students and staff, services were split into dedicated servers to prevent bottlenecks and simplify management.
 
---
 
## Devices
 
All devices follow the naming convention: `Moatassim-[Device Name]-[Floor No]` (excluding mobile endpoints such as laptops and smartphones).
 
| Device | Ground | 1st Floor | 2nd Floor | Rooftop | Total |
|---|---|---|---|---|---|
| Switch | 5 | 5 | 5 | 1 | 16 |
| LAP-PT (Access Points) | 11 | 11 | 11 | 2 | 35 |
| WLC | – | – | – | 1 | 1 |
| PC (Admin) | – | – | – | 1 | 1 |
| IoT Door | – | – | – | 1 | 1 |
| RFID Reader | – | – | – | 1 | 1 |
| Servers (total) | – | – | – | 7 | 7 |
 
**Servers breakdown:**
 
| Server | Function |
|---|---|
| DHCP Server | Assigns IP addresses dynamically to all wireless clients |
| RADIUS / AAA Server | Authenticates users via WPA2-Enterprise (802.1X) |
| FTP Server | File transfer with permission-based access control |
| DNS Server | Resolves domain names to IP addresses |
| Web Server | Hosts the university web page (ajman.ac.ae) |
| Mail Server | Handles email exchange between registered devices |
| IoT Server | Manages IoT device states (door, RFID) |
 
---
 
## IP Addressing
 
The network uses the subnet `192.168.0.0/16` to accommodate the large number of simultaneous connections.
 
| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| DHCP Server | F0/0 | 192.168.1.2 | 255.255.0.0 | 192.168.1.1 |
| WLC | G0/0 | 192.168.1.3 | 255.255.0.0 | 192.168.1.1 |
| RADIUS / AAA Server | F0/0 | 192.168.1.4 | 255.255.0.0 | 192.168.1.1 |
| PC (Admin) | F0/0 | 192.168.1.5 | 255.255.0.0 | 192.168.1.1 |
| FTP Server | F0/0 | 192.168.1.6 | 255.255.0.0 | 192.168.1.1 |
| DNS Server | F0/0 | 192.168.1.7 | 255.255.0.0 | 192.168.1.1 |
| Web Server | F0/0 | 192.168.1.8 | 255.255.0.0 | 192.168.1.1 |
| Mail Server | F0/0 | 192.168.1.9 | 255.255.0.0 | 192.168.1.1 |
| IoT Server | F0/0 | 192.168.1.10 | 255.255.0.0 | 192.168.1.1 |
| IoT Door | Wireless0 | DHCP | 255.255.0.0 | 192.168.1.1 |
| RFID Reader | Wireless0 | DHCP | 255.255.0.0 | 192.168.1.1 |
| Smartphones (x2) | Wireless0 | DHCP | 255.255.0.0 | 192.168.1.1 |
| Laptops (x3) | Wireless0 | DHCP | 255.255.0.0 | 192.168.1.1 |
| LAP-PT (x35) | G0/0 | DHCP | 255.255.0.0 | 192.168.1.1 |
 
---
 
## Implementation
 
All devices were set up in Cisco Packet Tracer following the designed topology and naming convention. Cabling:
 
- **Straight-through cables** — used between end devices and switches
- **Cross-over cables** — used between switches and between switches and the WLC
Two wireless networks (SSIDs) were created and managed through the WLC:
 
| SSID | Target Users | Security |
|---|---|---|
| `J2-Staff` | University employees | WPA2-Enterprise, 802.1X, RADIUS |
| `J2-Student` | University students | WPA+WPA2, 802.1X, RADIUS |
 
The WLC management interface is accessible via HTTPS for secure administration.
 
---
 
## Security
 
### Security Scheme
 
**WPA2-Enterprise with RADIUS (AAA)** was selected as the security scheme to satisfy the core security requirements of the network:
 
| Requirement | How It Is Met |
|---|---|
| Confidentiality | AES-128 encryption on all communications |
| Integrity | WPA2 frame protection prevents data tampering |
| Availability | Centralized WLC ensures consistent policy enforcement |
| Authentication | IEEE 802.1X via RADIUS server validates user credentials |
| Authorization | Access Control policies applied per user role |
| Accountability | RADIUS server logs all authentication events |
 
### How It Works
 
1. A user attempts to connect to `J2-Staff` or `J2-Student`.
2. The WLC forwards the authentication request to the **RADIUS server** via 802.1X.
3. The RADIUS server verifies the credentials against its internal database.
4. Upon success, the user is granted network access with AES-128 encrypted communication.


### Physical Security
 
Access to the management room is controlled by an **RFID-secured door**. The RFID card is assigned to a single administrator only, enforcing physical confidentiality and integrity of the network infrastructure.
 
---
 
## IoT Integration
 
| Device | Configuration |
|---|---|
| IoT Door | Wirelessly connected; state controlled by the IoT server |
| RFID Reader | Reads RFID card; triggers door unlock via IoT server |
| RFID Card | Assigned to one admin only; conditions set on reader for access control |

The IoT server manages the conditions and logic that govern reader-card interactions and door states.

<img width="400" height="422" alt="image" src="https://github.com/user-attachments/assets/c5428433-ee3d-418e-96af-2f795495ec11" />

---

## Testing and Verification
 
All core functions of the network were tested and verified successfully.
 
 
### 1. Wireless Client Authentication
 
Laptops and smartphones successfully connected to the wireless network using credentials registered in the RADIUS server.

<img width="400" height="505" alt="image" src="https://github.com/user-attachments/assets/04e9d019-b2b3-4c63-af3d-4c0562b9a5fe" />
<img width="400" height="493" alt="image" src="https://github.com/user-attachments/assets/379d049b-21b7-424e-ae69-cf9c0f343514" />

<img width="400" height="146" alt="image" src="https://github.com/user-attachments/assets/4b301e37-564c-47a8-98dd-a84c5c02f76e" />
<img width="400" height="145" alt="image" src="https://github.com/user-attachments/assets/ad9604c7-6930-4fc5-8d85-07a21308fb84" />

---

### 2. Ping Between DHCP-Assigned Clients
 
Ping tests were carried out between devices to verify communication between DHCP-assigned clients.

<img width="940" height="110" alt="image" src="https://github.com/user-attachments/assets/7a1cc487-3322-45fa-ab8f-2b4ac61fda43" />

---
 
### 3. RFID Card Unlocking the IoT Door
 
The RFID card successfully unlocked the IoT door by contacting the RFID reader.

<img width="418" height="370" alt="image" src="https://github.com/user-attachments/assets/646de237-b404-40bd-a60b-0d711bab1aa7" />
<img width="418" height="372" alt="image" src="https://github.com/user-attachments/assets/fdb1dad1-79c1-477b-8647-5f5dfed9db9a" />

---
 
### 4. Web Browsing and DNS Resolution
 
`http://ajman.ac.ae` is accessible in web browsers and domain names were successfully resolved to their IPs.

<img width="400" height="426" alt="image" src="https://github.com/user-attachments/assets/b3d1a193-5a66-4339-be78-eeeaab1f89f2" />

---
 
### 5. Email Exchange
 
Emails were successfully exchanged between registered end devices via the mail server.

<img width="940" height="370" alt="image" src="https://github.com/user-attachments/assets/e5318aa8-5e05-407c-9a40-a907be03fa45" />

---
 
### 6. FTP Permission Enforcement
 
The FTP server correctly enforced access permissions. Attempting to delete an unauthorized file was blocked as intended.

<img width="264" height="350" alt="image" src="https://github.com/user-attachments/assets/7c31dfa8-7f28-44e7-afb2-c313f8eaf31a" />
<img width="265" height="349" alt="image" src="https://github.com/user-attachments/assets/4cc67904-7cfd-413c-a799-df4c44f63f4d" />
<img width="309" height="178" alt="image" src="https://github.com/user-attachments/assets/b9e0a206-a144-4c2d-95d5-a9e5dca55b24" />


## Conclusion
 
The wireless network deployment for the J2 Building successfully meets all design and security objectives set for the project. The centralized WLC+AP architecture enabled seamless roaming across floors and blocks, while the hybrid tree topology provided scalability and partial redundancy. WPA2-Enterprise with RADIUS delivers enterprise-grade authentication and encryption, ensuring the protection of student and staff data. All seven server services operated correctly under testing, and the infrastructure is designed to accommodate future AP expansions as the university grows.
 
---
 
## Tools Used
 
| Tool | Purpose |
|---|---|
| Cisco Packet Tracer | Network simulation and configuration |
| WLC (Wireless LAN Controller) | Centralized AP management |
| RADIUS / AAA Server | User authentication (802.1X / WPA2-Enterprise) |
| Apache Derby / SQL (in report) | IP addressing and device documentation |
 
---
 
> **Note:** This is a simulated network built entirely within Cisco Packet Tracer for academic purposes. No physical hardware was used.


