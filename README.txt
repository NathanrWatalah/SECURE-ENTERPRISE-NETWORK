PROJECT 5: SECURE ENTERPRISE NETWORK
Cisco Packet Tracer Project Documentation
Student Name: Nathan R. Watalah
Program: Information Technology
Level: Sophomore 2, System Administration
Software Used: Cisco Packet Tracer
Project Title: Secure Enterprise Network
Project Type: Network Design, VLANs, Access Control Lists, and Network Security
1. Introduction
Computer networks help employees communicate, share files, access servers, and use company resources. However, a company must protect its network from unauthorized access.
For example, employees in the Finance Department should not automatically have access to every resource used by the IT Department. Network administrators need ways to separate departments and control which devices can communicate.
This project demonstrates how to build a small enterprise network using VLANs, router configuration, and Access Control Lists (ACLs).
A VLAN separates devices into different logical networks. An ACL is a set of rules that allows or blocks selected network traffic.
Cisco Packet Tracer will be used to design, configure, test, and document the network.
2. Project Scenario
Imagine a company with three departments:
•	IT Department: Manages computers and network devices.
•	Finance Department: Handles financial information.
•	HR Department: Manages employee information.
The company wants to separate these departments and apply rules to control communication between them.
The IT Department should be able to communicate with all three departmental networks. Finance and HR should be prevented from initiating traffic to each other's departmental networks.
This is a simplified training scenario, not a complete real-world security solution.
3. Problem Statement
If every computer belongs to the same network, it can be harder to manage departmental access and limit unnecessary communication.
The company needs a network design that separates departments and applies access rules to reduce unauthorized communication.
This project addresses the problem using VLANs, router-based inter-VLAN routing, and ACLs.
4. Project Aim
To design and configure a secure enterprise network that separates departments using VLANs and controls communication using ACLs.
5. Project Objectives
1.	Design a network with three departments.
2.	Configure a router and switch.
3.	Create VLANs for IT, Finance, and HR.
4.	Assign switch ports to the correct VLANs.
5.	Configure inter-VLAN routing.
6.	Create ACL rules to control traffic between departments.
7.	Test permitted and blocked communication.
8.	Troubleshoot common VLAN, routing, and ACL problems.
9.	Document the configuration and testing results.
6. Devices Required
Device	Quantity	Purpose
Router	1	Routes traffic between VLANs
Managed switch	1	Separates departmental VLANs
PCs	6	Represents employees in three departments
Copper straight-through cables	As required	Connect PCs and router to the switch
Cisco Packet Tracer	1	Builds and tests the network
Use a router and switch model that support the interfaces and commands needed for this project.
7. Network Topology
                         Router
                    Inter-VLAN Routing
                             |
                        Trunk Link
                             |
                           Switch
                 ____________|____________
                |            |            |
              VLAN 10      VLAN 20      VLAN 30
                IT         Finance         HR
              PC1, PC2     PC3, PC4     PC5, PC6
The trunk link carries traffic for the three VLANs between the switch and router.
The router provides a gateway for each VLAN and applies the configured access rules.
8. IP Addressing Plan
Each department will have its own IPv4 subnet.
Department	VLAN ID	Network	Default gateway
IT	10	192.168.10.0/24	192.168.10.1
Finance	20	192.168.20.0/24	192.168.20.1
HR	30	192.168.30.0/24	192.168.30.1
Subnet mask for all three networks: 255.255.255.0.
PC Addressing Table
Device	Department	IP address	Default gateway
PC1	IT	192.168.10.10	192.168.10.1
PC2	IT	192.168.10.11	192.168.10.1
PC3	Finance	192.168.20.10	192.168.20.1
PC4	Finance	192.168.20.11	192.168.20.1
PC5	HR	192.168.30.10	192.168.30.1
PC6	HR	192.168.30.11	192.168.30.1
Configure each PC through:
Desktop → IP Configuration
Enter its IP address, subnet mask, and default gateway from the table.
9. Project Implementation
Step 1: Build the Topology
Add one router, one switch, and six PCs to Packet Tracer.
Connect the PCs to the switch:
•	PC1 and PC2 to switch ports Fa0/2 and Fa0/3.
•	PC3 and PC4 to switch ports Fa0/4 and Fa0/5.
•	PC5 and PC6 to switch ports Fa0/6 and Fa0/7.
•	Router G0/0 to switch Fa0/1.
Port names may differ according to your device model.
Step 2: Create the VLANs
Open the switch CLI and enter:
enable
configure terminal

vlan 10
name IT
exit

vlan 20
name FINANCE
exit

vlan 30
name HR
exit

end
copy running-config startup-config
These commands create three separate VLANs.
Step 3: Assign Switch Ports to VLANs
Configure the IT ports:
enable
configure terminal

interface range fastEthernet 0/2-3
switchport mode access
switchport access vlan 10
exit
Configure the Finance ports:
interface range fastEthernet 0/4-5
switchport mode access
switchport access vlan 20
exit
Configure the HR ports:
interface range fastEthernet 0/6-7
switchport mode access
switchport access vlan 30
exit
Each PC port now belongs to its department's VLAN.
Step 4: Configure the Trunk Port
The switch port connected to the router must carry traffic for all three VLANs.
Assuming the router connects to Fa0/1, enter:
interface fastEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
no shutdown
end
copy running-config startup-config
If the selected switch uses different interface names or requires different trunk commands, adjust the configuration to match the model.
Step 5: Configure the Router's Main Interface
Open the router CLI and enter:
enable
configure terminal

interface gigabitEthernet 0/0
no shutdown
exit
Do not assign a normal LAN IP address to the parent interface in this router-on-a-stick design. The IP addresses will be assigned to subinterfaces.
Step 6: Configure Router Subinterfaces
Create the IT gateway:
interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit
Create the Finance gateway:
interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit
Create the HR gateway:
interface gigabitEthernet 0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
exit

end
copy running-config startup-config
These subinterfaces let the router communicate with the three VLANs through one physical connection.
Step 7: Test the VLANs Before Applying Security Rules
From each PC, ping its own gateway.
For example, from PC1:
ping 192.168.10.1
From PC3:
ping 192.168.20.1
From PC5:
ping 192.168.30.1
Also test communication between PCs in the same VLAN.
Make sure these basic tests work before configuring the ACLs. This makes troubleshooting easier.
10. Configure Access Control Lists (ACLs)
For this training project, we will apply these rules:
1.	IT computers can initiate communication with the Finance and HR networks.
2.	Finance computers cannot initiate traffic to the HR network.
3.	HR computers cannot initiate traffic to the Finance network.
4.	Each department can communicate with its own network.
Important security limitation: The simple ACLs below block traffic based on source and destination IP addresses. They do not fully understand application sessions. Because IP ACLs are stateless, return traffic and other permitted traffic may need additional rules in a real deployment. These rules are for learning and do not replace a stateful firewall.
Step 10.1: Configure the Finance ACL
On the router, enter:
enable
configure terminal

ip access-list extended FINANCE-IN
deny ip 192.168.20.0 0.0.0.255 192.168.30.0 0.0.0.255
permit ip any any
exit
This blocks traffic from the Finance subnet to the HR subnet and permits other traffic that reaches this ACL.
Apply the ACL inbound on the Finance subinterface:
interface gigabitEthernet 0/0.20
ip access-group FINANCE-IN in
exit
Step 10.2: Configure the HR ACL
Enter:
ip access-list extended HR-IN
deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255
permit ip any any
exit
Apply it inbound on the HR subinterface:
interface gigabitEthernet 0/0.30
ip access-group HR-IN in
exit

end
copy running-config startup-config
The two ACLs block traffic initiated from Finance to HR and from HR to Finance.
The IT subnet is not blocked by these specific rules.
Security note: These rules are not a complete enterprise security policy. They do not block all unwanted traffic, and they do not guarantee that all return traffic will work as expected. A production network needs a carefully designed, tested policy and may need stateful firewall rules.
11. Testing and Verification
Test 1: Check VLAN Configuration
On the switch, enter:
show vlan brief
Confirm that the correct ports belong to VLANs 10, 20, and 30.
Test 2: Check the Trunk
Enter:
show interfaces trunk
Confirm that the router-facing switch port is operating as a trunk and allows the required VLANs.
Test 3: Check Router Interfaces
On the router, enter:
show ip interface brief
Confirm that the main interface and subinterfaces are operational.
Test 4: Check the ACLs
Enter:
show access-lists
Confirm that the Finance and HR ACLs contain the intended rules.
You can also inspect the subinterface configuration to verify where each ACL is applied.
Test 5: Test Permitted Communication
From PC1 in the IT department, try to ping:
ping 192.168.20.10
ping 192.168.30.10
These tests should succeed if routing, addressing, and the rest of the configuration are correct.
Test 6: Test Blocked Communication
From PC3 in Finance, try:
ping 192.168.30.10
From PC5 in HR, try:
ping 192.168.20.10
These tests should fail because the ACLs block traffic in those directions.
Record the actual outcomes. A failed ping alone does not prove that an ACL caused the failure, so verify the ACL counters and routing configuration as well.
12. Troubleshooting
Problem	Possible cause	Solution
PC cannot ping its gateway	Wrong IP settings, VLAN, or interface status	Check the PC configuration, VLAN assignment, and router interface
PCs in the same department cannot communicate	Wrong VLAN assignment or cable issue	Check show vlan brief and the physical connections
Inter-VLAN communication fails	Trunk or router subinterface problem	Check show interfaces trunk and router subinterfaces
All traffic is blocked	ACL applied incorrectly or missing permit rules	Inspect show access-lists and the interface configuration
Finance can reach HR unexpectedly	ACL missing or applied to the wrong interface	Verify the Finance ACL and its inbound application
IT cannot reach another department	Incorrect IP address, route, or ACL	Check the addressing plan and ACL rules
Router subinterface is down	Parent interface or physical link problem	Check the main interface and cable
Always test basic connectivity before adding or changing ACL rules.
13. Expected Results
When configured correctly:
1.	The switch contains VLANs 10, 20, and 30.
2.	Each department's PCs belong to the correct VLAN.
3.	The router provides a gateway for each VLAN.
4.	PCs can communicate within their own department.
5.	IT can initiate communication with Finance and HR under the stated rules.
6.	Finance-to-HR and HR-to-Finance traffic is blocked by the configured ACLs.
7.	The configuration and test results are documented.
These are expected results. Record your actual results after running the tests.
14. Skills and Knowledge Gained
This project develops practical skills in:
•	VLAN configuration.
•	Access port configuration.
•	Trunk configuration.
•	Router-on-a-stick inter-VLAN routing.
•	IPv4 addressing.
•	Extended ACL configuration.
•	Traffic filtering.
•	Network verification.
•	Security troubleshooting.
•	Technical documentation.
15. Benefits of the Project
Department Separation
VLANs organize the network into separate logical networks.
Controlled Communication
ACLs let an administrator permit or block selected IP traffic.
Easier Network Management
Separating departments makes it easier to organize and troubleshoot the network.
Practical Security Experience
The project introduces network access rules and demonstrates why security policies must be tested.
Career Development
The skills provide a foundation for further study in networking, IT support, and system administration.
16. Project Limitations
This is a small simulation. It does not include:
1.	A dedicated stateful firewall.
2.	Authentication for network users.
3.	Encryption or VPN access.
4.	Intrusion detection or prevention.
5.	Centralized logging and monitoring.
6.	Redundant switches or routers.
7.	A complete policy for all protocols and return traffic.
VLANs and ACLs are useful controls, but they do not by themselves provide complete network security.
17. Future Improvements
The project can be improved by:
1.	Adding a dedicated firewall and testing stateful security policies.
2.	Adding a company server and defining which departments can access it.
3.	Configuring SSH for secure router and switch administration.
4.	Adding port security on switch access ports.
5.	Creating a management VLAN.
6.	Adding DHCP for automatic client configuration.
7.	Configuring logging and monitoring.
8.	Testing additional security rules with documented expected results.
18. Conclusion
The Secure Enterprise Network project demonstrates how VLANs can separate departments and ACLs can control selected traffic between them.
By completing the project, students gain practical experience with switch configuration, router subinterfaces, inter-VLAN routing, access control, testing, and troubleshooting.
The project also demonstrates an important security lesson: network rules must be carefully designed and tested because a rule that blocks unwanted traffic can also block traffic that users need.
19. Project Evidence
Capture screenshots of your actual work, including:
1.	The complete network topology.
2.	The switch VLAN configuration.
3.	The switch trunk configuration.
4.	Router subinterface configuration.
5.	Successful pings to each department's gateway.
6.	A successful permitted communication test from IT.
7.	Failed Finance-to-HR and HR-to-Finance tests.
8.	The ACL configuration and counters.
9.	The final successful verification results.
Save the Packet Tracer file as:
Project5-Secure-Enterprise-Network.pkt
Project status: Complete after you configure the devices, verify the permitted and blocked traffic, save the Packet Tracer file, and capture your evidence.

