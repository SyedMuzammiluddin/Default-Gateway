📡 Cisco Packet Tracer – Static Routing Lab

📌 Devices Used

* 3 Routers: Cisco 2911 (R1, R2, R3)
* 3 Switches: Cisco 2960-24TT
* End Devices: PC1, PC2, PC3, Laptop0-2
* Cabling: Cross-over (WAN), Straight-through (LAN)

🔌 Physical Connections

1️⃣ WAN Connections (Backbone)
R1 to R2:

* Cable: Cross-over
* R1: Gig0/0 → 10.1.1.1
* R2: Gig0/0 → 10.1.1.2
* Network: 10.0.0.0/8

R2 to R3:

* Cable: Cross-over
* R2: Gig0/1 → 11.1.1.3
* R3: Gig0/0 → 11.1.1.4 (Confirmed via CLI)
* Network: 11.0.0.0/8

2️⃣ LAN Connections
Network A (Left):

* R1: Gig0/1 → 192.1.1.1 (Gateway)
* PC1: 192.1.1.10

Network B (Center):

* R2: Gig0/2 → 192.2.1.1 (Gateway)
* PC2: 192.2.1.20

Network C (Right):

* R3: Gig0/1 → 192.3.1.1 (Gateway) (Confirmed via CLI)
* PC3: 192.3.1.20

⚙️ Router Configurations

🔹 R1 Configuration
enable
conf t
interface g0/0
ip address 10.1.1.1 255.0.0.0
no shut
interface g0/1
ip address 192.1.1.1 255.255.255.0
no shut
! Default Route to R2 (Exit Interface)
ip route 0.0.0.0 0.0.0.0 GigabitEthernet0/0
end
wr

🔹 R2 Configuration
enable
conf t
interface g0/0
ip address 10.1.1.2 255.0.0.0
no shut
interface g0/1
ip address 11.1.1.3 255.0.0.0
no shut
interface g0/2
ip address 192.2.1.1 255.255.255.0
no shut
! Default Route to R3 (Exit Interface)
ip route 0.0.0.0 0.0.0.0 GigabitEthernet0/1
end
wr

🔹 R3 Configuration
enable
conf t
interface g0/0
ip address 11.1.1.4 255.0.0.0
no shut
interface g0/1
ip address 192.3.1.1 255.255.255.0
no shut
! Default Route to R2 (Next Hop IP)
ip route 0.0.0.0 0.0.0.0 11.1.1.3
end
wr

✅ Verification
Commands: show ip route, show ip int bri

Connectivity Test (From PC1):

1. ping 192.2.1.20 (Center Network)
2. ping 192.3.1.20 (Right Network)

Result: Successful reply (TTL=125) confirms routing is active.

🧠 How It Works

1. PC1 sends traffic to Gateway 192.1.1.1 (R1).
2. R1 forwards via default route to R2.
3. R2 forwards via default route to R3.
4. R3 delivers packet to PC3 on the local LAN.
5. Return Path: R3 sends reply to Gateway of Last Resort (11.1.1.3 / R2), which routes it back to R1.

🎯 Learning Outcome

* Configuring Static Default Routes (using both Exit Interfaces and Next-Hop IPs).
* Verifying end-to-end connectivity across a multi-hop network.
* Troubleshooting interface assignments (Gig0/0 vs Gig0/1).
