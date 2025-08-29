

🔹 Section 1: Basic Windows & AD Concepts (L1 Level)
Question	Answer & Explanation
1 **What is Active Directory?**

AD is a Microsoft directory service that stores information about users, computers, and other resources in a network. It allows centralized management and authentication.

2. **What is ADUC?**

	ADUC (Active Directory Users and Computers) is a snap-in tool used to manage user accounts, groups, and computers in AD. You’ve used this—mention tasks like password resets, account unlocks, and group membership changes.

3. **What is a Domain Controller?**

	A server that responds to authentication requests and stores AD data. It’s the backbone of AD.

4.* **What is a Group Policy?**

	Group Policy allows admins to control user and computer settings across the domain.

     Example: enforcing password complexity or mapping network drives.

5. **What is DNS and why is it important in AD?**

	DNS translates domain names to IP addresses. AD relies heavily on DNS for locating domain controllers and services.

6. **What is DHCP?**
	
DHCP assigns IP addresses to devices on the network automatically.

7. **What is OU (Organizational Unit)?**

	A container in AD used to organize users, groups, and computers. Helps apply group policies efficiently.

8.  **What is the difference between a user group and an OU?**

	Groups are for permissions; OUs are for organization and policy application.

9. **What is the difference between local user and domain user?**

	Local users exist on one machine; domain users are managed centrally via AD.

10. **What is the purpose of a service account**?	

Used by applications or services to run with specific permissions. Should be managed securely.

🔹 **Section 2: Intermediate Troubleshooting & Admin Tasks**

Question	Answer & Explanation

11. **How do you reset a user password in ADUC?**

	Right-click the user → Reset Password. Confirm password policy compliance.

12. **How do you unlock a user account?**

 Right-click user → Properties → Account tab → Uncheck “Account is locked out.”

13. **How do you add a user to a group?**

	Right-click user → Add to Group → Select group. Example: adding to “Finance” group for shared folder access.

14. **What is the difference between security and distribution groups?**

	Security groups are used for permissions; distribution groups are for email distribution.

15. **How do you check AD replication status?**

	Use repadmin /replsummary or check Event Viewer.

16. **What is Event Viewer used for?**

	To check logs for system errors, login failures, and service issues.

17. **How do you troubleshoot login issues?**

	Check account status, password expiry, group membership, and Event Viewer logs.

18. **What is SolarWinds used for?**

	Monitoring servers and network devices. You’ve used it—mention how you track server health.

19. **What is WSUS/SCCM?**

	Tools for patch management. You’ve applied monthly patches—mention reboot coordination and log verification.

20. **What is BitLocker and how do you manage it?**

	BitLocker encrypts drives. You’ve handled recovery keys and troubleshooting—mention LAPS if used.

🔹 **Section 3: Advanced Scenarios & Real-World Examples**
Question	Answer & Explanation

22. **How do you automate user creation in AD?**

	Use PowerShell scripts. Example: bulk user creation with CSV input.

23. **How do you monitor AD health?**

	Use tools like dcdiag, Event Viewer, and SolarWinds.

24. **What is the difference between forest and domain?**

	Forest is the top-level container; domains are within forests.

25. **How do you apply a GPO to a specific OU?**

	Link the GPO to the OU in Group Policy Management Console.

26. **What is MFA and how do you support it?**

	Multi-Factor Authentication adds a second layer of security. You’ve supported MS Authenticator—mention guiding users.

27. **What is LAPS?**

	Local Administrator Password Solution—manages local admin passwords securely.

28. **How do you handle a P1 ticket for AD login failure?**

	Immediate triage: check account status, replication, DC availability, escalate if needed.

29. **How do you troubleshoot GPO not applying?**	

Use gpresult /r, check scope, inheritance, and replication.

30. **How do you manage shared folder access via AD?**

	Create security groups, assign NTFS(new technology file system ) permissions, add users to groups.

31. **How do you handle stale accounts?**

     Use scripts to identify inactive users, disable or remove after review.


🔹 **Section 4: Behavioral & Situational Questions
Question	How to Answer**


32. **Tell me about a time you resolved a critical issue.**

	Use STAR method: Situation, Task, Action, Result. Example: login failure across region—checked replication, resolved with sync.

33. **How do you prioritize tasks during high ticket volume?**

	Mention ticket severity (P1–P4), SLA awareness, and escalation process.

34. **How do you ensure security in your daily tasks?**

	Mention access control, password policies, MFA, and auditing.

35. **How do you handle a user who repeatedly forgets their password?**

	Educate on password manager, reset securely, suggest self-service portal if available.

36. **How do you communicate with non-technical users?**

	Use simple language, analogies, and patience. Example: explaining VPN like a secure tunnel.


🔟 **Top 10 Interview Questions with Full Explanations**


1. **What is Active Directory (AD)?**


**What it is**: Active Directory is a Microsoft service that stores and manages information about users, computers, and resources in a network. It controls authentication (who can log in) and authorization (what they can access).

Why it matters: It’s the backbone of identity and access management in Windows environments.

Example: When a user logs into their company laptop, AD checks their username and password, confirms their identity, and applies their permissions.

How to explain in interview: "Active Directory is a centralized system that helps manage users, computers, and access rights. I understand how it handles authentication and how tools like ADUC are used to manage user accounts."

2. What is a Domain Controller (DC)?
What it is: A Domain Controller is a server that runs Active Directory services. It stores user credentials and responds to login requests.

Why it matters: Without a DC, users can’t log in or access network resources.

Example: If a DC goes down, users in that domain may not be able to authenticate until it’s back online or another DC takes over.

How to explain in interview: "A Domain Controller is the server that handles authentication and stores AD data. I’m learning how it works with DNS and replication to keep the network running smoothly."

3. What is ADUC (Active Directory Users and Computers)?
What it is: ADUC is a graphical tool used to manage users, groups, and computers in Active Directory.

Why it matters: It’s the primary interface for user account management.

Example: You use ADUC to reset passwords, unlock accounts, create users, and manage group memberships.

How to explain in interview: "I’ve used ADUC to manage user accounts—resetting passwords, unlocking accounts, and adding users to groups. It’s a key tool for daily L1 support tasks."
