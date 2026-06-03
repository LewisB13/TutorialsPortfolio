
# Active Directory Lab Setup Guide for Workshop

## Prerequisites
- [ ] **Host Machine Capabilities:** Ensure your host machine has at least 16GB of RAM (preferably 32GB) and 100GB+ of free storage.
- [ ] **Hypervisor Software:** Download and install a hypervisor (e.g., [Oracle VirtualBox](https://www.virtualbox.org/) or [VMware Workstation ](https://www.vmware.com/products/workstation-player.html)).
- [ ] **Windows Server 2019 ISO:** Download the evaluation ISO from the Microsoft Evaluation Center.
- [ ] **Windows 10/11 Enterprise/Pro ISO:** Download the client OS evaluation ISOs (Home edition cannot join a domain).

## Phase 1: Virtual Machine Creation
### Server VM (Domain Controller)
- [ ] Create a new VM for Windows Server 2019.
- [ ] Allocate RAM: 4096 MB (4 GB) minimum.
- [ ] Allocate Storage: 40 GB.
- [ ] Attach the Windows Server 2019 ISO to the virtual CD/DVD drive.

### Client VMs (Windows 10/11) - Repeat twice
- [ ] Create a new VM for the Client PC (Name them `Client-PC1` and `Client-PC2`).
- [ ] Allocate RAM: 2048 MB - 4096 MB (2 - 4 GB).
- [ ] Allocate Storage: 40 GB.
- [ ] Attach the Windows 10/11 ISO..

## Phase 2: Operating System Installation
- [ ] **Boot Server VM:** Follow prompts to install Windows Server 2019. **Important:** Choose **Desktop Experience** during setup, otherwise you will get a command-line only interface.
- [ ] **Boot Client VMs:** Install Windows 10/11 on both client machines. During setup, select "I don't have internet" or choose "Domain join instead" to create a local admin account rather than using a Microsoft account.
- [ ] Install Hypervisor Guest Additions / VMware Tools on all three VMs for better performance, clipboard sharing, and resolution scaling.

## Phase 3: Network Configuration 
### Windows Server 2019
- [ ] Log in as local Administrator.
- [ ] Open **Network Connections** (Run `ncpa.cpl`).
- [ ] Right-click the adapter -> Properties -> IPv4.
- [ ] Set a Static IP: 
  - IP Address: `192.168.10.10`
  - Subnet Mask: `255.255.255.0`
  - Default Gateway: Leave blank (or set to your virtual router if you have one).
  - Preferred DNS: `127.0.0.1` (Self-referencing for Active Directory).
- [ ] Rename the server to something recognizable (e.g., `DC01`) via Server Manager and restart.

### Client PCs
- [ ] Log into `Client-PC1` and `Client-PC2`.
- [ ] Open **Network Connections** -> IPv4 Properties(Run `ncpa.cpl`).
- [ ] Set Static IPs for clients (easier for initial lab setup):
  - IP Address (PC1): `192.168.10.20`
  - IP Address (PC2): `192.168.10.21`
  - Subnet Mask: `255.255.255.0`
  - Preferred DNS: `192.168.10.10` (**CRITICAL: Must point to the Server's IP**).
- [ ] Test connectivity: Open Command Prompt on a client and run `ping 192.168.10.10`. *(Note: You may need to disable the Windows Firewall temporarily on the server or allow ICMP echo requests for the ping to succeed).*

## Phase 4: Active Directory Setup on Server
- [ ] On the Server, open **Server Manager**.
- [ ] Click **Add roles and features**.
- [ ] Proceed through the wizard and select **Active Directory Domain Services (AD DS)**. Click Next and Install.
- [ ] Once installed, click the yellow flag icon at the top of Server Manager and select **Promote this server to a domain controller**.
- [ ] Choose **Add a new forest**.
- [ ] Enter a Root domain name (e.g., `workshop.lab` or `corp.local`).
- [ ] Enter a Directory Services Restore Mode (DSRM) password. Keep this safe.
- [ ] Click Next through the rest of the wizard and click **Install**. The server will automatically reboot.

## Phase 5: Joining Clients to the Domain
- [ ] Log into `Client-PC1` with the local admin account.
- [ ] Open System Settings (Search for "About your PC" -> click "Advanced system settings" -> go to "Computer Name" tab -> click "Change").
- [ ] Change the computer name to something identifiable (e.g., `WS-Client01`).
- [ ] Under "Member of", select **Domain** and type your domain name (e.g., `workshop.lab`).
- [ ] When prompted, enter the credentials of the Server Administrator (`workshop\Administrator` and the password you set on the server).
- [ ] You should see a "Welcome to the workshop.lab domain" message.
- [ ] Restart the Client PC.
- [ ] Repeat Phase 5 for `Client-PC2`.

## Phase 6: Verification for Workshop
- [ ] Log into the Server as Domain Administrator.
- [ ] Open **Active Directory Users and Computers** (Run `dsa.msc`).
- [ ] Verify both `WS-Client01` and `WS-Client02` appear in the "Computers" Organizational Unit (OU).
- [ ] Right-click the "Users" OU -> New -> User. Create a test user (e.g., `student01`).
- [ ] Switch to `Client-PC1` or `Client-PC2`.
- [ ] Select "Other User" on the Windows login screen.
- [ ] Log in using the newly created AD test user credentials (`workshop\student01`). 
- [ ] Confirm successful login! Your lab is now ready.

