# Windows Server 2022: 
# Domain Controller, DNS, DHCP, and GPO Implementation


- [Abstract](#abstract)
- [Background](#background)
  - [2.1 Why Windows Server 2022 Administration Matters](#21--why-windows-server-2022-administration-matters)
  - [2.2 Relevant Microsoft Certifications](#22--relevant-microsoft-certifications)
  - [2.3 Career Paths and Opportunities](#23--career-paths-and-opportunities)
  - [2.4 Key Skills Developed](#24--key-skills-developed)
- [Procedure](#procedure)
  - [3.1 Virtual Machine Preparation](#31-virtual-machine-preparation)
  - [3.2 Initial Server Setup](#32-initial-server-setup)
  - [3.3 IP Addressing and Connectivity Check](#33-ip-addressing-and-connectivity-check)
  - [3.4 Active Directory Domain Services Deployment](#34-active-directory-domain-services-deployment)
  - [3.5 Cisco Switch Configuration 8](#35-cisco-switch-configuration)
  - [3.6 Domain Integration and Active Directory Structure](#36-domain-integration-and-active-directory-structure)
  - [3.7 DNS and DHCP Configuration and Verification](#37-dns-and-dhcp-configuration-and-verification)
  - [3.8 Group Policy Implementation and Final Testing](#38-group-policy-implementation-and-final-testing)
- [Reflection](#reflection)

# Abstract

This project successfully established and managed a complete Windows Server 2022 based domain within a controlled lab environment. Using VirtualBox, we deployed four virtual machines with bridged networking to emulate a small business network connected to a Cisco switch.

One server was promoted to a domain controller for the NTWK250.local domain, configured with Active Directory Domain Services (AD DS), DNS, and DHCP. The remaining three servers were set up as clients, receiving static IP addresses, and successfully joined to the new domain.

Concurrently, we secured the network's foundation by configuring VLAN 50, port security, and other hardening features on the Cisco switch, confirming reliable end-to-end connectivity. Key administrative tasks were executed, including designing Organizational Units (OUs), creating users and security groups, delegating permissions, and implementing several Group Policy Objects (GPOs). These policies covered crucial functions like software deployment, network drive mapping, application restriction, password/lockout enforcement, and applying a standardized desktop wallpaper.

Overall, the project provided hands-on proof of concept for centralized identity management, name resolution, dynamic addressing, secure switching, and policy-based endpoint control in a modern Windows Server 2022 domain environment.


# Background

## **2.1  Why Windows Server 2022 Administration Matters**

**Windows Server** remains the foundational platform for identity and access management across countless organizations. **Windows Server 2022** continues this legacy, offering enhanced security, performance, and features optimized for hybrid cloud environments.

Learning to install, configure, and manage this operating system is essential as it teaches the core principles of **centralized authentication**, **authorization**, and **policy enforcement** via **Active Directory (AD DS)**, alongside critical network services like **DNS** and **DHCP**. These technologies are fundamental to daily IT operations, governing everything from user logons and name resolution to IP address assignment and security policies for workstations and other servers.

By building a complete domain from a bare installation to a fully functional, managed system, we gain concrete experience with concepts often treated only in theory. Practical skills such as designing **OUs**, managing user and group accounts, and deploying **Group Policy Objects (GPOs)** establish a strong foundation for supporting real-world enterprise networks, enforcing security standards, and troubleshooting common directory and networking issues.

## **2.2  Relevant Microsoft Certifications**

The tasks completed in this project directly reflect the skillsets required for various Microsoft infrastructure and administration certifications. Specifically, the hands-on experience with installing Windows Server, managing AD DS, configuring core network services (DNS/DHCP), and implementing GPOs is vital for exams leading to:

- **Microsoft Certified: Windows Server Hybrid Administrator Associate**  This certification focuses heavily on managing Windows Server in both on-premises and hybrid cloud settings, covering AD DS, DNS, DHCP, and Group Policy.

Older or related Windows Server certifications also test on similar core skills around domain controllers, identity management, and essential networking. This project gives us a practical, real-world starting point to pursue these professional credentials, translating abstract exam objectives into applied experience.

## **2.3  Career Paths and Opportunities**

Proficiency in Windows Server administration is a highly sought-after skill directly applicable to numerous entry-level and mid-level IT roles:

- **Help Desk / Service Desk Technician**  Tasks include domain-joining new machines, resetting passwords, and resolving GPO or logon issues.

- **Desktop Support Technician**  Focuses on managing user profiles, troubleshooting domain-joined workstations, and ensuring connectivity to domain resources.

- **Junior Systems Administrator / Systems Administrator**  This involves the core maintenance of domain controllers, managing DNS/DHCP, designing logical OU structures, and deploying complex GPOs.

- **Network Administrator (entry level)**  Collaborates with system administrators on switch configuration, IP address schemes, and ensuring seamless integration between the Windows domain and the physical network infrastructure.

This project accurately simulates the daily responsibilities these roles handle: integrating clients into a domain, securing the physical network via **VLANs** and **port security**, managing accounts, and deploying standardized configurations using group policy.

## **2.4  Key Skills Developed**

Through the completion of this lab, we developed practical, hands-on expertise in:

- Installing and performing initial configuration of **Windows Server 2022** on virtual machines.

- Promoting a server to a **Domain Controller** and creating a new **Active Directory forest and domain**.

- Configuring and verifying network connectivity using static IP addresses, gateways, DNS settings, and tools like $ping$ and $ipconfig$.

- Managing **DNS zones and records** and validating name resolution with $nslookup$.

- Deploying and troubleshooting a **DHCP scope** for automatic IP address assignment.

- Designing **Organizational Units (OUs)**, creating **user accounts and security groups**, and delegating specific administrative permissions.

- Creating and applying **Group Policy Objects (GPOs)** to deploy software, map network drives, block applications, enforce security policies (password/lockout), and standardize desktop wallpaper.

- Implementing fundamental **Cisco switch configuration**, including **VLANs**, **port security**, and Spanning-Tree features, to build a secure Layer 2 environment.


# Procedure

## **3.1 Virtual Machine Preparation**

We began by setting up four virtual machines (VMs) in **VirtualBox**, one on each group member's laptop. Each VM was provisioned with an 80 GB virtual hard drive and 2 GB of RAM. Critically, we configured the network adapter on all VMs to **Bridged Networking** to ensure they would communicate directly with the physical Cisco switch on the same Layer 2 network.

The VMs were named as follows:

- **NTWK250-DC**

- **NTWK250-Client1**

- **NTWK250-Client2**

- **NTWK250-Client3**

We attached the Windows Server 2022 installation ISO to each VM to initiate the installation process.

## **3.2 Initial Server Setup**

We installed Windows Server 2022 on all four virtual machines. During the setup, we created the local **Administrator** account and set the password to **RvccNTWK250**, as required. Post-installation, we performed the following initial configuration steps on all systems:

- Renamed the servers to **FinalWS2K22-DC**, **FinalWS2K22-Client1**, **FinalWS2K22-Client2**, and **FinalWS2K22-Client3**.

- Disabled **Windows Defender Firewall** to simplify and expedite lab connectivity testing.

- Enabled **Remote Desktop** and confirmed **Remote Management** was active on each server.

- Set the time zone to **Eastern Time** for logging and consistency.

### 
## **3.3 IP Addressing and Connectivity Check**

We then assigned static IP addresses within the **172.16.100.0/24** network scheme:

| Server | Role | IP Address | Subnet Mask | Gateway | Preferred DNS | Alternate DNS |
|---|---|---|---|---|---|---|
| FinalWS2K22-DC | Domain Controller | 172.16.100.1 | 255.255.255.0 | 172.16.100.254 | 172.16.100.1 | 172.16.100.2 |
| FinalWS2K22-Client1 | Client 1 | 172.16.100.2 | 255.255.255.0 | 172.16.100.254 | 172.16.100.1 | (None) |
| FinalWS2K22-Client2 | Client 2 | 172.16.100.3 | 255.255.255.0 | 172.16.100.254 | 172.16.100.1 | (None) |
| FinalWS2K22-Client3 | Client 3 | 172.16.100.4 | 255.255.255.0 | 172.16.100.254 | 172.16.100.1 | (None) |

We connected each laptop/VM to the Cisco switch on ports 0/1 through 0/4. A basic $ping$ test between all servers confirmed that the IP addressing was correct and the bridged networking was functioning properly.

## **3.4 Active Directory Domain Services Deployment**

On **FinalWS2K22-DC**, we used **Server Manager** to install the **Active Directory Domain Services (AD DS)** role. Following the role installation, we ran the post-deployment configuration wizard to **promote the server to a domain controller**. We chose to create a new forest and domain named **NTWK250.local**, accepted the default DNS settings, and allowed the system to reboot. After restarting, we logged in with the domain administrator account and confirmed that the domain controller and DNS server roles were active and healthy.

## **3.5 Cisco Switch Configuration**

We configured the Cisco switch to ensure a secure and logically segmented **Layer 2 network** for the four connected hosts. The configuration steps included:

- **Port Management:** Disabled all unused ports, leaving only ports 0/1 and 0/4 active.

- **Security:** Set required console, VTY, and enable secret passwords.

- **VLAN Configuration:** Created **VLAN 50**, named it **NTWK250**, and assigned ports 0/1 and 0/4 as access ports within VLAN 50.

- **Port Security:** Implemented port security on the active ports (0/1 and 0/4), restricting the maximum number of MAC addresses to **one**, using sticky MAC addresses, and setting the violation mode to **shutdown**.

- **Switch Hardening:** Disabled **DTP** and enabled **Spanning-Tree PortFast** and **BPDU Guard** on the access ports to protect against topology changes from end devices.

- **SVI:** Configured a Switched Virtual Interface (**SVI**) for VLAN 50 with the IP address **172.16.100.5/24** to allow for switch management and testing from the servers.

After applying the switch configuration, we re-verified connectivity by successfully pinging the switch's VLAN 50 IP address from the domain controller and clients.

## **3.6 Domain Integration and Active Directory Structure**

With the domain controller operational, we integrated the three client servers into the **NTWK250.local** domain. We first verified that the primary DNS setting on each client pointed to the DC's IP address. We then changed the system membership from a workgroup to the domain, provided domain administrator credentials, and rebooted. Post-reboot, each client successfully showed membership in **NTWK250.local**.

In **Active Directory Users and Computers (ADUC)**, we designed and created a logical OU structure:

- COMPTIA

- Cisco

- Microsoft

- NetUsers

- NetComputers

We moved the three client computer accounts into **NetComputers**. Next, we created ten user accounts in **NetUsers** following the assignments naming convention and assigned the temporary password **NTWK2501**. We then created the **HelpDesk**, **Engineers**, and **Admin** security groups and added the specified users to each. Finally, we used the **Delegation of Control Wizard** to grant the designated user in each group the specific ability to reset passwords and force password changes upon next logon.

## **3.7 DNS and DHCP Configuration and Verification**

Using **DNS Manager**, we manually added **A records** for the domain controller, each client server, and the switch, ensuring each hostname resolved to the correct IP address. We then confirmed proper name resolution for all configured hosts using the $nslookup$ command from the clients.

We installed the **DHCP Server** role on the domain controller and created a scope using the specified IP range and necessary exclusions. We configured the scope options to correctly provide the default gateway and DNS server information to clients. After activating the scope, we configured the client NICs to obtain addresses automatically, released and renewed their IP addresses, and confirmed they successfully received valid leases from the DHCP server. Any dynamically assigned addresses were updated in DNS as needed.

## **3.8 Group Policy Implementation and Final Testing**

The final step involved implementing several **Group Policy Objects (GPOs)** to automate and secure the client configuration:

1. **Software Deployment GPO:** Configured to automatically install Google Chrome on all domain-joined clients using a centrally stored MSI package.

2. **Logon Script GPO:** Targeted the **Engineers** group to map drive $H:$ to a shared folder ($C:\Cisco$) located on the domain controller.

3. **Restriction GPO:** Used software restriction or application control policies to block users from running the Chrome browser.

4. **Security GPO:** Applied to the **Admin** group, enforcing a minimum 10-character password and an account lockout policy after three failed attempts for a 30-minute duration.

5. **Desktop Wallpaper GPO:** Applied a custom team logo image as the standardized background for users.

We concluded by running $gpupdate /force$ on the client machines and thoroughly testing each policy: we verified that Chrome installed, that the $H:$ drive appeared for members of the Engineers group, that Chrome was successfully blocked where intended, that the lockout policy functioned as configured, and that the custom wallpaper was applied. These successful tests confirmed that all installation, configuration, and policy deployment objectives for the project were fully met.


# Reflection

This project was an invaluable, end-to-end exercise in building and managing a functional, small-scale Windows Server domain and LAN environment. Before starting, concepts like domain controllers, Group Policy, and VLAN-based segmentation were largely abstract. By actively installing Windows Server 2022, promoting a Domain Controller, and integrating clients into the NTWK250.local domain, these ideas became tangible and significantly easier to grasp. It was particularly illuminating to see the tight interdependence of services like AD DS, DNS, and DHCP a simple misconfiguration in one (such as pointing DNS to the wrong server) could halt critical processes like a domain join.

Working with Group Policy was a major highlight, demonstrating the sheer power of centralized configuration. Automatically deploying Chrome, mapping a specific network drive for a security group, restricting application usage, enforcing robust security policies, and pushing a team wallpaper all controlled from the domain controller showed exactly how administrators maintain control over user and computer environments without manually touching every machine. This also reinforced the necessity of careful GPO planning to ensure policies target the correct users and computers without causing conflicts.

The network infrastructure portion, especially configuring the Cisco switch with VLAN 50, port security, and Spanning-Tree protections, clearly demonstrated the crucial link between the Windows domain and the physical Layer 2 network. Features like Port Security and BPDU Guard illustrate the vital role of switch-level security in protecting the domain from unauthorized devices and unintended topology changes.

If we were to repeat this project, the key areas for improvement would be building a more detailed design document *before* starting the implementation, meticulously documenting each configuration step in real-time, and dedicating more time to testing failure scenarios (e.g., failed domain joins, DHCP misconfigurations, or GPO targeting errors). Overall, the lab provided practical, relevant experience that directly supports entry-level roles in help desk, desktop support, and junior system or network administration, creating a solid foundation for future study and Microsoft certification pursuits.

During this project, we encountered two primary issues: the Group Policy Object (GPO) responsible for deploying Google Chrome to all computers, and the logon script used to map the H: drive for the Engineers OU. Both issues were resolved by correcting the security inheritance settings on the respective hosts.

We identified and reviewed the errors using the Group Policy Management console via Server Manager on the domain controller. After confirming that each GPO was properly configured and applied on the domain controller, we executed the ```?gpresult /r?``` command on each client virtual machine to validate which GPOs were applied to each OU, as well as whether they affected computer or user settings. This allowed us to verify successful applications and address any remaining configuration discrepancies on the affected clients. In our case, we confirmed that the Google Chrome deployment GPO was correctly configured as a computer-level setting, while the Engineers OU logon script for mapping the H: drive was adjusted to ensure it was deployed as a user-level setting within the GPO.
