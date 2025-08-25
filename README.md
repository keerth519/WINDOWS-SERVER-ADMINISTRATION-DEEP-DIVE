

**Module 1 – Windows Server Basics**

	•	Editions & Licensing (2019 vs 2022, Standard vs Datacenter).
	•	Server Manager & Windows Admin Center.
	•	Installation types (Core vs Desktop Experience).
	•	Real-time: Build a test lab in VMware/Azure/AWS.
 
**Module 1 — Windows Server Basics**


1)** **Editions & Licensing (2019 vs 2022, Standard vs Datacenter**)**


**Editions**

	•	**Standard**
 
	•	Best for small/medium workloads.
	•	Virtualization rights: includes licenses for 2 Windows Server VMs per license. (Stack more licenses to run more VMs.)
	•	Missing advanced features like Software-Defined Networking (SDN), Storage Spaces Direct (S2D), and Shielded VMs.
 
	•	**Datacenter**
 
	•	Best for large/virtualized environments.
	•	Virtualization rights: unlimited Windows Server VMs on the licensed host.
	•	Includes S2D, Storage Replica (unlimited), Shielded VMs, SDN, Host Guardian Service, etc.
	•	Essentials (limited, small orgs): up to 25 users / 50 devices, no virtualization rights.

**Licensing (quick, practical)**

	•	Core-based: license all physical cores (min 16 cores per server and 8 per CPU).
	•	CALs: you also need User or Device CALs for anyone/anything accessing the server.
	•	RDS CALs: required for Remote Desktop Session Host scenarios (published apps/desktops).

**2019 vs 2022 (what actually matters)**

	•	**Security:**
 
	•	2022 → Secured-core server (TPM 2.0, VBS/HVCI, firmware protection), TLS 1.3 by default, SMB compression; SMB over QUIC is available with the Azure Edition of 2022.
	•	Hybrid/Cloud: better Azure Arc integration, Automanage/Hotpatch (Azure Edition).
	•	Containers: smaller images, better Kubernetes compatibility in 2022.
	•	Networking/perf: improved UDP stack, faster SMB (compression).

⸻

**2) Server Manager & Windows Admin Center (WAC)**

	•	Server Manager (built-in GUI)
	•	Add/remove Roles & Features, manage local/remote servers, basic performance, events.
	•	Use on Desktop Experience servers, or via RSAT tools on an admin workstation.
	•	Windows Admin Center (WAC) – web-based (recommended)
	•	Single pane to manage Core/GUI servers, clusters, Hyper-V, Storage, AD, IIS, certificates, updates, etc.
	•	Install on a Windows 10/11 PC or a server; browse to https://<server>:6516.
	•	Works great for Server Core (no GUI) management.

⸻

**3) Installation Types (Core vs Desktop Experience)**

	•	Server Core (default)
	•	No full desktop GUI; smaller footprint; fewer patches; more secure.
	•	Manage with PowerShell, SConfig, WAC, or remote MMCs/RSAT.
	•	Quick tasks:
	•	Launch menu: sconfig
	•	Rename: Rename-Computer -NewName "SRV-CORE01" -Restart
	•	Add role: Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
	•	Desktop Experience (GUI)
	•	Full GUI (Explorer, Server Manager, MMCs). Easy for beginners/labs.
	•	Slightly larger attack surface and patch load.

**Tip: For real servers → Core + WAC. For labs/learning → Desktop Experience can be convenient.**

⸻

******4) Real-time: Build a Test Lab on AWS EC2 (Windows Server 2019/2022)**

****

Below is a clean, safe, step-by-step guide you can follow right now.
We’ll create one Windows Server. (Later, add a second server to join a domain, etc.)

**A. Prep (one-time)**

	1.	Login to AWS console → pick a region (e.g., ap-south-1 Mumbai).
	2.	Create a Key Pair (EC2 → Key Pairs → Create):
	•	Type: RSA, Format: .pem (for Windows, .ppk works with PuTTY; you can convert from .pem).
	•	Download and keep it safe (needed to decrypt the Administrator password).
	3.	Security Group (EC2 → Security Groups → Create):
	•	Inbound rule: RDP (TCP 3389) from Your IP only (use “My IP” button).
	•	(Optional) Another rule for SMB (445) or WinRM (5985/5986) if you need; keep it private when possible.
	4.	(Optional, safer) Create a dedicated VPC with public & private subnets, and put servers in private subnets + use SSM Session Manager instead of RDP. For a first lab, the default VPC is okay.

**B. Launch the Windows Server 2019/2022 EC2**

	1.	EC2 → Launch Instance.
 
	2.	Name: WS-2022-LAB (or WS-2019-LAB).
 
	3.	Application and OS Images (AMI):
	•	Choose Windows Server 2022 Base or Windows Server 2019 Base.
 
	4.	Instance type:
	•	Minimum: t3.medium (2 vCPU, 4 GB). For smoother GUI: t3.large (2 vCPU, 8 GB).
 
	5.	Key pair: select the .pem you created.
 
	6.	Network settings:
	•	VPC: default (or your lab VPC).
	•	Subnet: pick one; Auto-assign Public IP: Enable (for simple RDP).
	•	Security group: choose the RDP-only-from-My-IP SG.
 
	7.	Storage:
	•	Root volume: 60 GB gp3 (32 GB is okay, but 60 GB gives room for roles/tools).
	•	Add a second EBS volume if you plan to test FSRM/DFS/Storage.
 
	8.	Launch the instance.

**C. Get the Windows password & RDP**

	1.	Wait until Status checks = 2/2.
	2.	Select the instance → Actions → Security → Get Windows password.
	3.	Upload your .pem key → decrypt → copy the Administrator password.
	4.	On your PC, open Remote Desktop Connection:
	•	Computer: Public IPv4 address of the instance
	•	Username: Administrator
	•	Password: paste decrypted password → Connect.

**D. First-time server setup (post-boot)**

	1.	Rename server (optional):
	•	GUI: Server Manager → Local Server → Computer name → Change.
	•	PowerShell: Rename-Computer -NewName "WS22-LAB" -Restart
	2.	Windows Update:
	•	Settings → Update & Security → Check for updates (or via WAC).
	3.	Time zone (India example):
	•	PowerShell: Set-TimeZone -Name "India Standard Time"
	4.	Install roles/features (examples):
	•	GUI: Server Manager → Manage → Add Roles and Features.
	•	PowerShell (DNS + DHCP as example):Install-WindowsFeature DNS -IncludeManagementTools
                                           Install-WindowsFeature DHCP -IncludeManagementTools
										   
5.Enable RDP NLA (usually default). To toggle via PowerShell (if needed):

Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 0
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

6.Install Windows Admin Center (optional but recommended)

	•	Download WAC MSI on this server (or on your admin PC), install, then browse to
https://<server-name>:6516 → add your server(s) to WAC.

**E. (Optional) Make it a Domain Controller (for later modules)**

	1.	Add the AD DS role:
 
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools

2.Promote to new forest (example: corp.local):
Install-ADDSForest -DomainName "corp.local"

	3.	Reboot when prompted.
 
	4.	Tip (AWS networking for AD):
	•	Give your DC a fixed private IP by assigning a specific secondary private IP to its ENI in EC2 (Windows NIC should still use DHCP).
	•	Keep both servers in the same Security Group and allow all traffic within the SG for easy AD replication in a lab.

**F. Spin up a second Windows instance (member server)**

	•	Repeat B–D and join it to the domain:
Add-Computer -DomainName corp.local -Restart

•	Now you have DC1 and MEMBER1 for AD/GPO/DNS/DHCP/DFS labs.

**G. Cost & security hygiene**

	•	Stop instances when not in use; EBS storage still costs.
	•	Restrict RDP to your IP only; ideally use Session Manager.
	•	Never expose AD ports publicly; keep domain traffic within VPC.

⸻

Quick cheat sheet (you’ll use often)
# See features
Get-WindowsFeature

# Install roles
Install-WindowsFeature DNS -IncludeManagementTools
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools

# Promote DC (new forest)
Install-ADDSForest -DomainName "corp.local"

# Rename & reboot
Rename-Computer -NewName "WS22-LAB" -Restart

# Join domain (from member)
Add-Computer -DomainName corp.local -Restart

# Enable RDP (if disabled)
Set-ItemProperty 'HKLM:\System\CurrentControlSet\Control\Terminal Server' fDenyTSConnections 0
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"



**Step-by-Step: Windows Server Free Tier Lab on AWS**


🔹 Step 1: Log in & Choose Free Tier

	1.	Go to AWS Console → EC2 → Launch Instance.
	2.	Give a name: WinServerLab-DC01.
	3.	Choose AMI:
	•	Search for Windows Server 2019 Base or Windows Server 2022 Base.
	•	Select 64-bit (x86).

⸻

🔹 Step 2: Choose Instance Type

	•	Select t2.micro (Free Tier Eligible).
	•	✅ Free tier
	•	❌ Limited resources (1 vCPU, 1 GB RAM) → okay for Server Core

💡 Later, if you need GUI + AD + DFS, upgrade to t3.medium (2 vCPU, 4 GB RAM) → but stop when not in use.

⸻

🔹 Step 3: Key Pair & Security Group

	1.	Create a new Key Pair → .pem file → download it.
(You’ll use it for RDP/Password decryption).

	2.	Security Group (Firewall rules):
	•	Allow RDP (3389) from your IP only.
	•	Allow ICMP (ping) for testing (optional).

⸻

🔹 Step 4: Storage

	•	Default: 30 GB gp3 is enough.
	•	Keep free tier → don’t add more unless needed.

⸻

🔹 Step 5: Launch

	•	Click Launch Instance.
	•	Wait until Instance State → Running.

⸻

🔹 Step 6: Get RDP Access

	1.	Select the instance → Connect → RDP Client.
	2.	Download Remote Desktop File (.rdp).
	3.	Decrypt Administrator password using .pem key.
	4.	Open RDP and log in with:
	•	Username: Administrator
	•	Password: decrypted password

⸻

🔹 Step 7: Verify Inside Server

	1.	Check Server Core command prompt (default for t2.micro).
 
	•	Run sconfig → server config menu.
	•	Configure hostname, domain join, Windows Update, etc.
 
	2.	Install Windows Admin Center (WAC) locally on your laptop → manage the server remotely via browser.
(Much easier than working in Core).

⸻

🔹 Step 8: Practice Tasks

Now you can practice Module 1 basics:

✅ Rename server, change IP (static), enable RDP.

✅ Explore sconfig options.

✅ Join domain (later, when you create AD).

✅ Try basic PowerShell commands:
Get-WindowsFeature
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools

✅ Connect with Windows Admin Center (WAC).
