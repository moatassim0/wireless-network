### Wireless Network Deployment for Ajman University

This project is about the deployment of a wireless network for the J2 Building in Ajman University, the objective is to provide a wireless access to the internet and local services to the large number of staff and students in campus, and the main priority is about analysing security requirements and implementing an appropriate security scheme to the WLAN.

## Network Design

The J2 Building consists of 3 floors divided into three blocks A to B, with around 50 halls and laboratories in each floor except the rooftop which has only 2 halls on block A. This layout underwent an analysis to strategically place every device for efficient use of resources, full wireless coverage in and off the building for users, and ease of management for the higher ups. 
Access points were designed in the following; blocks A and B share the same design but mirrored, 4 APs have been evenly distributed, 2 of which serve the male side and the other 2 serving the female side, block C however, has 3 APs positioned, 2 of which serve the female side and the remaining one on the male side, this is because block C is shaped like an arch and results in more space for coverage for the female side, lastly, the third floor or rooftop has one AP serving the only two halls on block A, and one AP in block C for administrators in the management room. 
As for administration, a management room in block C of the third floor is set up for governing the WLC and the multiple servers inside it, an RFID Reader is placed outside the room next to a secured door with the RFID Card being assignment to only one admin for confidentiality and integrity reasons.

# Wireless Network Topology

The network uses a hybrid of three topologies, a bus topology for the switches that goes across horizontally in each floor without the use of a backbone, a star topology for the multiple devices in the management room, all of which results in a tree topology for the entire network having all the APs set as a leaf node, core switches on each floor as a child and parent node, and the management switch as a root node, this topology design ensures an a reliable performance and easy scalability for any additional APs to be installed in the future.

# Design Considerations

This network was deployed for a WLC+AP structure instead of Wireless Routers due to many factors, a WLC allows for configuration for all APs from a central management unlike a wireless router that needs separate configuration for every device, another factor is seamless handoff between APs for walking students and staff which is a poor experience for wireless routers due to how each router may create its own different subnet. Another design consideration was for the switches to be completely meshed, this option can achieve many feats like performance boost, fault tolerance, redundancy and less network congestion. However, the cabling complexity can be a nightmare for troubleshooting and maintenance, and it requires a Spanning Tree Protocol to avoid loops. Meshed structures are more appropriate in mission critical networks like emergency services, the military or space agencies, and it isn’t suitable for a university, the design of connecting the core switches on each floor together in this case is more fitting to keep simple cabling and ease of management. Another minor consideration was for the servers to be all combined into one server that handles every service in the network, this decision was dropped because the traffic students and staff generate may overwhelm the server and cause interruptions, therefore every service was split into its own device for easier management.

# Network Architecture Diagram

Below is a diagram to illustrate the logical structure for the entire network.
 
<img width="651" height="1359" alt="image" src="https://github.com/user-attachments/assets/6f30791e-636e-400f-8fbe-e0c91f8b92b2" />

## Implementation

Every device is set up within Cisco Packet Tracer in accordance with the topology that has been designed as well as the naming scheme, all of which used straight-through cables to connect to switches except for in-between switches where cross-over cables were used with the addition of the WLC.

# Configuration Steps

A subnet of 192.168.0.0/16 is chosen for this network to capacitate the large number of students and staff connected simultaneously, below is an addressing table for all devices.

| Device           | Interface  | IP Address     | Subnet Mask   | Default Gateway |
|------------------|------------|----------------|---------------|-----------------|
| DHCP Server      | F0/0       | 192.168.1.2    | 255.255.0.0   | 192.168.1.1     |
| WLC              | G0/0       | 192.168.1.3    | 255.255.0.0   | 192.168.1.1     |
| RADIUS/AAA Server| F0/0       | 192.168.1.4    | 255.255.0.0   | 192.168.1.1     |
| PC               | F0/0       | 192.168.1.5    | 255.255.0.0   | 192.168.1.1     |
| FTP Server       | F0/0       | 192.168.1.6    | 255.255.0.0   | 192.168.1.1     |
| DNS Server       | F0/0       | 192.168.1.7    | 255.255.0.0   | 192.168.1.1     |
| Web Server       | F0/0       | 192.168.1.8    | 255.255.0.0   | 192.168.1.1     |
| Mail Server      | F0/0       | 192.168.1.9    | 255.255.0.0   | 192.168.1.1     |
| IoT Server       | F0/0       | 192.168.1.10   | 255.255.0.0   | 192.168.1.1     |
| IoT Door         | Wireless0  | DHCP           | 255.255.0.0   | 192.168.1.1     |
| RFID Reader      | Wireless0  | DHCP           | 255.255.0.0   | 192.168.1.1     |
| Smartphone x 2   | Wireless0  | DHCP           | 255.255.0.0   | 192.168.1.1     |
| Laptop x 3       | Wireless0  | DHCP           | 255.255.0.0   | 192.168.1.1     |
| LAP-PT x 35      | G0/0       | DHCP           | 255.255.0.0   | 192.168.1.1     |


## Testing

Laptops and Smartphones successfully connected to the wireless network via a user id and password set up in RADIUS server. 

<img width="459" height="505" alt="image" src="https://github.com/user-attachments/assets/04e9d019-b2b3-4c63-af3d-4c0562b9a5fe" />
<img width="436" height="493" alt="image" src="https://github.com/user-attachments/assets/379d049b-21b7-424e-ae69-cf9c0f343514" />

<img width="463" height="146" alt="image" src="https://github.com/user-attachments/assets/4b301e37-564c-47a8-98dd-a84c5c02f76e" />
<img width="440" height="145" alt="image" src="https://github.com/user-attachments/assets/ad9604c7-6930-4fc5-8d85-07a21308fb84" />

Ping tests were carried out between devices to verify communication between DHCP-assigned clients and resulted in a success. 

<img width="940" height="110" alt="image" src="https://github.com/user-attachments/assets/7a1cc487-3322-45fa-ab8f-2b4ac61fda43" />

RFID Card Successfully unlocks the IoT Door by contacting the RFID Reader. 

<img width="418" height="370" alt="image" src="https://github.com/user-attachments/assets/646de237-b404-40bd-a60b-0d711bab1aa7" />
<img width="418" height="372" alt="image" src="https://github.com/user-attachments/assets/fdb1dad1-79c1-477b-8647-5f5dfed9db9a" />

http://ajman.ac.ae is up and running in web browsers (without CSS) and domain names have been successfully resolved to their IPs. 

<img width="421" height="426" alt="image" src="https://github.com/user-attachments/assets/b3d1a193-5a66-4339-be78-eeeaab1f89f2" />

E-mails have been successfully exchanged between registered end devices. 

<img width="940" height="370" alt="image" src="https://github.com/user-attachments/assets/e5318aa8-5e05-407c-9a40-a907be03fa45" />

FTP server handles the permissions as intended successfully; this example is deleting a file that you are not authorized for deleting.

<img width="264" height="350" alt="image" src="https://github.com/user-attachments/assets/7c31dfa8-7f28-44e7-afb2-c313f8eaf31a" />
<img width="265" height="349" alt="image" src="https://github.com/user-attachments/assets/4cc67904-7cfd-413c-a799-df4c44f63f4d" />
<img width="309" height="178" alt="image" src="https://github.com/user-attachments/assets/b9e0a206-a144-4c2d-95d5-a9e5dca55b24" />


## Conclusion

The J2 Building's wireless network deployment at Ajman University satisfies the project's network design and security objectives. Planning was carried out in every stage to achieve; wireless coverage throughout the building, resource efficiency, and management simplicity. The centralized WLC+AP approach allowed users to travel freely across floors and blocks, while the hybrid topology structure enabled scalability and partial redundancy.
Security has been the primary objective in this project, by adopting WPA2-Enterprise with RADIUS server, user data is safeguarded and authenticated. All server services, File Transfer, E-Mail, Web Browsing, Domain Name System, and Internet of Things, were tested and found to operate without any issues. Additionally, the network is designed to be future-ready, by facilitating the infrastructure needed for an expansion of Access Points when needed, it will meet Ajman University's needs for many years to come.
All things considered, this project showed how crucial network planning is, execution, and thorough testing, to creating a wireless infrastructure that can sustain an academic setting, while maintaining a reliable security mechanism.



