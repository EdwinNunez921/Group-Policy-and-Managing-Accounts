<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Group Policy and Managing Accounts  (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Video Demonstration</h2>

- ### [YouTube: Group-Policy-and-Managing-Accounts](https://youtu.be/Z41e761hNfA)
<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Deployment and Configuration Steps</h2>

<h2>Step 1: Configure Account Lockout Policy</h2>

<p>
<img src="https://github.com/user-attachments/assets/f0f8879b-d5d1-4505-b3bf-70c22763af50" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log in to DC-1 using RDP.

Click the Search bar in the lower-left corner of the screen.

Right-click the Windows icon in the lower-left corner and select Run.

Type gpmc.msc into the Run dialog box and press Enter.

In the Group Policy Management Console, expand Domain.

Right-click Default Domain policy and select Edit.

In the new window that opens, navigate to:
Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Account Lockout Policy.

Double-click Account Lockout Duration.

Select Define this policy setting, and set the duration to 30 minutes.

Click Apply, then click OK.

Double-click Account Lockout Threshold.

Select Define this policy setting, and set the threshold to 5 attempts.

Click Apply, then click OK.

Note: This policy ensures that if a user exceeds 5 incorrect login attempts, the account will be locked for 30 minutes before another attempt can be made.
</p>
<br />

<h2>Step 2: Verify Group Policy on Client-1</h2>

<p>
<img src="https://github.com/user-attachments/assets/8c8d583c-502e-42c8-9625-75bb3583483e" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log in to Client-1 using the credentials for mydomain.com\jane_admin.

Once logged in, click the Search bar in the lower-left corner.

Type Command Prompt, right-click it, and select Run as Administrator.

In the Command Prompt, type the following command to update the group policy:
gpupdate /force  
Once the update is complete, type the following command to view the applied policies:
gpresult -r  

Review the output and confirm that the Account Lockout Policy we created is listed under the applied policies.
</p>
<br />

<h2>Step 3: Test and Unlock Account Lockout Policy</h2>

<p>
<img src="https://github.com/user-attachments/assets/6a293e5a-e266-476f-9608-d428d908603c" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Attempt to log in to Client-1 using the credentials of a user created by the script, but deliberately enter the wrong password 5 times.

Confirm that the account gets locked as per the group policy implemented earlier.

Log in to DC-1 using RDP as mydomain.com\jane_admin.

Right-click mydomain.com in Active Directory Users and Computers.

Select Find, then type in the name of the locked user account and click Find Now.

Double-click the user account from the search results.

Navigate to the Account tab.

Check the box for Unlock account, then click Apply and OK.

Return to Client-1 and log in using the correct credentials for the previously locked user.

Verify that the user can now successfully log in to Client-1.
</p>
<br />

<h2>Step 4: Disable and Re-enable a User Account</h2>

<p>
<img src="https://github.com/user-attachments/assets/61959136-aff2-4ca4-a1f1-56433562af52" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log in to DC-1 using RDP as mydomain.com\jane_admin.

Open Active Directory Users and Computers.

Right-click mydomain.com, then select Find.

Search for the user you previously logged in as and click Find Now.

Right-click the user account from the search results and select Disable Account.

Verify the account is disabled by confirming the blue arrow pointing down icon appears on the user account.

Attempt to log in to Client-1 using the credentials of the disabled user account.

Confirm that login is not possible for the disabled account.

To re-enable the account, return to Active Directory Users and Computers on DC-1, right-click the disabled user account, and select Enable Account.

Attempt to log in to Client-1 again using the re-enabled account to confirm functionality.
</p>
<br />

<h2>Step 5: Monitor Security Logs for User Account Activity</h2>

<p>
<img src="https://github.com/user-attachments/assets/c50b1197-9862-4a2b-a3ca-2e30520213a2" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log in to DC-1 using RDP as mydomain.com\jane_admin.

Click the search bar in the lower-left corner and type Event Viewer, then open it.

Expand Windows Logs and select Security.

Right-click Security, select Search, and type in the username of the account you previously enabled and disabled.

Observe the logs to verify activities related to enabling and disabling the account.

Switch to Client-1 and log in as the user whose account you re-enabled.

On Client-1, click the search bar in the lower-left corner, type Event Viewer, and right-click it to select Run as Administrator.

Log in using mydomain.com\jane_admin and the corresponding password.

Expand Windows Logs and select Security.

Right-click Security, select Search, and type in the username of the account you enabled and disabled.

Observe the security logs to confirm the activities related to the account.
</p>
<br />
