# **Cisco Packet Tracer Lab Report: SOHO Network Design**

## **Project Overview**
This project demonstrates the design, implementation, and testing of a small office/home office (SOHO) network using Cisco Packet Tracer. The network is designed to meet the following specifications for XYZ company:

### **Requirements**
1. Use one Cisco router and one Cisco switch.
2. Create three VLANs for different departments:
   - **Admin/IT** (VLAN 10)
   - **Finance/HR** (VLAN 20)
   - **Customer Service/Reception** (VLAN 30)
3. Implement wireless connectivity for each VLAN.
4. Configure dynamic IPv4 address assignment using DHCP.
5. Enable communication between VLANs via inter-VLAN routing.
6. Ensure all departments can communicate while maintaining network segmentation.

---

## **Network Design**
![image](https://github.com/user-attachments/assets/640c24b3-c647-4cd3-8366-4ee24c3bdafd)

The network consists of the following components:
### **Hardware**
- **Router:** Cisco 2911
- **Switch:** Cisco 2960
- **End Devices:**
  - PCs, laptops, printers, smartphones, and tablets
- **Wireless Access Points:** 3 (one for each VLAN)

### **IP Addressing Scheme**
The network uses the **192.168.1.0/24** address block, divided into subnets for each VLAN:

| VLAN  | Department                | Subnet             | Default Gateway       |
|-------|---------------------------|--------------------|-----------------------|
| 10    | Admin/IT                  | 192.168.1.0/26     | 192.168.1.1          |
| 20    | Finance/HR                | 192.168.1.64/26    | 192.168.1.65         |
| 30    | Customer Service/Reception| 192.168.1.128/26   | 192.168.1.129        |

---

## **Configuration Details**

### **Router Configuration**
1. **Inter-VLAN Routing**:
![VLAN ENCAPSULATION](https://github.com/jnj3uf212121/Design-and-Implementation-of-a-Small-Office-Home-Office-Network--SOHO/blob/main/DOTQ-ENCAP.png?raw=true)
   Configured sub-interfaces for each VLAN:
   ```plaintext
   interface GigabitEthernet0/0.10
   encapsulation dot1q 10
   ip address 192.168.1.1 255.255.255.192
   no shutdown

   interface GigabitEthernet0/0.20
   encapsulation dot1q 20
   ip address 192.168.1.65 255.255.255.192
   no shutdown

   interface GigabitEthernet0/0.30
   encapsulation dot1q 30
   ip address 192.168.1.129 255.255.255.192
   no shutdown
   ```

2. **DHCP Configuration**:
![DHCP CONFIG](https://github.com/jnj3uf212121/Design-and-Implementation-of-a-Small-Office-Home-Office-Network--SOHO/blob/main/ROUTER-DHCP.png?raw=true)
   Configured DHCP pools for each VLAN:
   ```plaintext
   ip dhcp pool Admin-Pool
   network 192.168.1.0 255.255.255.192
   default-router 192.168.1.1
   domain-name Admin.com

   ip dhcp pool Finance-Pool
   network 192.168.1.64 255.255.255.192
   default-router 192.168.1.65
   domain-name Finance.com

   ip dhcp pool CS-Pool
   network 192.168.1.128 255.255.255.192
   default-router 192.168.1.129
   domain-name CS.com
   ```

---

### **Switch Configuration**
1. **VLAN Creation**:
![VLAN ASSIGNMENT](https://github.com/user-attachments/assets/a4cd9855-25ac-4a93-831b-1aaa2b8a700f)
   VLANs were created and named:
   ```plaintext
   vlan 10
   name Admin_IT
   vlan 20
   name Finance_HR
   vlan 30
   name CS_Reception
   ```

3. **Assigning VLANs to Ports**:
![image](https://github.com/user-attachments/assets/49d7a365-e3d6-443c-b211-214b813be5de)
   Ports were assigned to their respective VLANs:
   ```plaintext
   interface range FastEthernet0/1-4
   switchport mode access
   switchport access vlan 10

   interface range FastEthernet0/5-8
   switchport mode access
   switchport access vlan 20

   interface range FastEthernet0/9-12
   switchport mode access
   switchport access vlan 30
   ```

4. **Trunk Port Configuration**:
![image](https://github.com/user-attachments/assets/ca129c73-36d5-464a-9950-084f1386ce6f)
   Configured the trunk port to connect the switch to the router:
   ```plaintext
   interface GigabitEthernet0/1
   switchport mode trunk
   ```

---

### **Testing**
#### 1. **Connectivity Tests**
   - **Intra-VLAN Communication**:
     Devices within the same VLAN successfully communicated using ICMP (ping).
    
   - **Inter-VLAN Communication**:
     Devices from different VLANs successfully communicated via the router.
<p align="center">
  <img src="https://github.com/user-attachments/assets/7f558dec-48ae-4185-a1b4-52df53f1328e" alt="VLAN10-PING-VLAN20" width="300"/>
  <img src="https://github.com/jnj3uf212121/Design-and-Implementation-of-a-Small-Office-Home-Office-Network--SOHO/blob/main/VLAN20-PING-VLAN30.png?raw=true" alt="VLAN20-PING-VLAN30" width="300"/>
   <img src="https://github.com/jnj3uf212121/Design-and-Implementation-of-a-Small-Office-Home-Office-Network--SOHO/blob/main/VLAN30-PING-VLAN10.png?raw=true" alt="VLAN30-PING-VLAN10" width="300"/>
</p>

#### 2. **Dynamic IP Addressing**
   - All devices received correct IP addresses dynamically via DHCP.
 <p align="center">
  <img src="https://github.com/user-attachments/assets/6c670409-6e19-4498-8736-7cebed579475" alt="DHCP-CONFIG-ADMIN" width="300"/>
  <img src="https://github.com/user-attachments/assets/01a042fa-66a3-4779-8c0b-1d3fbc5670e9" alt="DHCP-CONFIG-FINANCE" width="300"/>
   <img src="https://github.com/user-attachments/assets/8a8ba2f7-bd65-4025-9c31-1e32ec4e417c" alt="DHCP-CONFIG-RECEPTION" width="300"/>
</p>

#### 3. **Wireless Connectivity**
   - Laptops, smartphones, and tablets successfully connected to their respective VLAN wireless networks.

---

## **Reflection**
This project provided hands-on experience with:
- VLAN segmentation and configuration.
- Inter-VLAN routing for enabling communication across departments.
- Dynamic IP addressing using DHCP.
- Wireless network integration into a segmented VLAN environment.

### **Future Enhancements**
1. Implement Access Control Lists (ACLs) to restrict access between certain VLANs.
2. Add redundancy to the network using additional routers or switches.
3. Monitor network performance using tools like SNMP.

---

## **DISCLAIMER**
This project was built with the help of Gurutech Networking Training on YouTube. The project is free and available online to help individuals gain experience with enterprise networking concepts. All credit for the tutorial and guidance goes to Gurutech. This lab report is my adaptation of the project for learning and portfolio purposes.

