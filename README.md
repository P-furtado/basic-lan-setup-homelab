## 1. basic-lan-setup-homelab
## Download Packet Tracer Lab
[Download Part 2: Basic VLAN Setup with DHCP](PacketTracerFiles/NETWORKING%20Basic%20VLAN%20Setup%20w%20DHCP.pkt)


## Objectives
- To simulate a small office network with three wired computers, one printer, and a
 wireless laptop, all connected to the same LAN. The router will provide DHCP to all
 devices, and you will test connectivity between them.

## Software Requirements
- Cisco Packet Tracer
   ## Lab Setup
  <p align="center">
  <img src="https://i.postimg.cc/m2th7HD8/Basiclansetup.png" alt="Basic LAN Setup Topology"/>
</p>
# Cisco Packet Tracer Topology Setup
Step 1: Open Cisco Packet Tracer and Add Devices
- Open Cisco Packet Tracer.
- From the End Devices section, drag and drop the following devices onto the workspace:
  - 3 PCs: PC-0, PC-1, PC-2
  - 1 Laptop: Laptop-0
  - 1 Printer: Printer-0
- From the Network Devices section, drag and drop:
  - 1 Router: Router-0
  - 1 Switch: Switch-0
  - 1 Wireless Router: WirelessRouter-0

## Step 2: Physically Connect Devices

Wired Connections:
- Click on the cable icon and select a copper straight-through cable.
- Connect each PC to the Switch using the copper straight-through cable:
  - PC-0 to any port on Switch-0 (e.g., FastEthernet 0/1)
  - PC-1 to FastEthernet 0/2
  - PC-2 to FastEthernet 0/3
- Connect the Printer to the Switch (e.g., FastEthernet 0/4)
- Connect the Router to the Switch using a straight-through cable:
  - Router's GigabitEthernet 0/0 to Switch’s FastEthernet 0/5

Wireless Connections:
- Connect WirelessRouter-0 to the Router:
  - Use a crossover cable to connect WirelessRouter-0’s Internet port to Router’s GigabitEthernet 0/1
 
## Step 3: Configure the Router


Step 1: Access the Router CLI
- Click on Router-0 and go to the CLI tab.
- Enter Privileged EXEC mode and global configuration mode:
  - Router> enable
  - Router# configure terminal

Step 2: Configure the Router’s GigabitEthernet 0/0 Interface (Wired Network)
- Assign an IP address to the interface for the wired network and enable it:
  - Router(config)# interface gigabitEthernet 0/0
  - Router(config-if)# ip address 192.168.1.1 255.255.255.0
  - Router(config-if)# no shutdown
  - Router(config-if)# exit

Step 3: Configure the Router’s GigabitEthernet 0/1 Interface (Wireless Network)
- Assign an IP address to the interface for the wireless network and enable it:
  - Router(config)# interface gigabitEthernet 0/1
  - Router(config-if)# ip address 192.168.2.1 255.255.255.0
  - Router(config-if)# no shutdown
  - Router(config-if)# exit

Step 4: Set Up DHCP Pool for Wired Devices (192.168.1.0/24)
- Create a DHCP pool for wired devices and define the default gateway:
  - Router(config)# ip dhcp pool LAN
  - Router(dhcp-config)# network 192.168.1.0 255.255.255.0
  - Router(dhcp-config)# default-router 192.168.1.1
  - Router(dhcp-config)# exit

Step 5: Set Up DHCP Pool for Wireless Devices (192.168.2.0/24)
- Create a DHCP pool for wireless devices and define the default gateway:
  - Router(config)# ip dhcp pool WLAN
  - Router(dhcp-config)# network 192.168.2.0 255.255.255.0
  - Router(dhcp-config)# default-router 192.168.2.1
  - Router(dhcp-config)# exit

Step 6: Save the Router Configuration
- Save all changes to the router’s startup configuration:
  - Router# write memory


   ## Step 4 Configure the Wireless Router


Step 1: Access the Wireless Router GUI
- Click on WirelessRouter-0 and go to the GUI tab.

Step 2: Set the Internet IP Address
- Configure the Internet connection to obtain an IP from the main router:
  - Select DHCP for the Internet Connection Type

Step 3: Configure Wireless Settings
- Go to the Wireless section and configure the SSID and security:
  - Change the SSID to a network name, e.g., "SmallOfficeWiFi"
  - Set WPA2-PSK security
  - Create a passphrase, e.g., "office123"

Step 4: Configure LAN Settings
- Go to the LAN settings and configure the router’s local network:
  - IP Address: 192.168.2.1
  - Subnet Mask: 255.255.255.0

Step 5: Enable the DHCP Server
- Enable the DHCP server to provide IP addresses on the 192.168.2.0/24 network

## Step 5: Configure the End Devices


Step 1: Configure Wired PCs
- Click on PC-0, go to the Desktop tab, and open the IP Configuration window.
- Set the IP configuration to DHCP so the PC receives an IP address from the router.
- Repeat this step for PC-1, PC-2, and Printer-0 (if using DHCP for printer temporarily).

Step 2: Configure Wireless Laptop
- Click on Laptop-0, go to the Desktop tab, and open the IP Configuration window.
- Select DHCP for automatic IP addressing.
- Go to Laptop-0's Config tab, select Wireless0, and connect to the SSID "SmallOfficeWiFi".
  - Enter the passphrase "office123" and connect.

Step 3: Configure the Printer with a Static IP
- Choose a static IP address for the printer in the same network as wired devices but outside the DHCP pool.
  - Example: If DHCP pool is 192.168.1.2 to 192.168.1.100, set printer IP to 192.168.1.101
- Set the Printer to use a static IP:
  - In the IP Configuration window, uncheck DHCP to disable dynamic IP assignment.
  - Manually enter the following details:
    - IP Address: 192.168.1.101
    - Subnet Mask: 255.255.255.0
    - Default Gateway: 192.168.1.1
    - DNS Server: 192.168.1.1 or a public DNS (e.g., 8.8.8.8)
- Click Apply to save the configuration.

  ## Step 6: Test the Setup
  

Step 1: Test Wired PC Connectivity
- From PC-0, open the Command Prompt and type:
  - Ipconfig
    - Verify that the assigned IP address is in the range 192.168.1.x
  - Ping the router to check the connection:
    - ping 192.168.1.1

Step 2: Test Wireless Laptop Connectivity
- From Laptop-0, open the Command Prompt and type:
  - Ipconfig
    - Ensure the IP is in the 192.168.2.x range
  - Ping the router to check the wireless connection:
    - ping 192.168.2.1

Step 3: Test Connectivity to the Printer
- Click on any of the PCs (e.g., PC-0), go to the Desktop tab, and open the Command Prompt.
- Ping the printer’s static IP address to verify communication:
  - ping 192.168.1.101
- If everything is configured correctly, you should receive reply messages confirming that the PC can communicate with the printer.

 
