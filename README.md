# Cybersecurity-Projects
-I have attached some preliminary images including a few virtual machines I have created using Microsoft Azure.
These virtual machines are being continuously built upon to orchestrate a full SOC and Honeynet. (in progress!)


-In the attached images, there are three virtual machines: windows-vm, linux-vm, and attack-vm. The Windows and Linux virtual machines are based in the Eastern United States while my "attack" virtual machine is based in Australia. I used remote desktop connection to install and configure an SQL server on the Windows virtual machine, which has been arranged to enable logging to be sent into Windows Event Viewer. I have generated failed login attempts using the "attack" virtual machine via RDP on the Windows virtual machine, failed SQL server login attempts on the Windows virtual machine, and failed SSH login attempts on the Linux virtual machine to demonstrate that the machines are working correctly and capable of capturing logs. I have also setup a few accounts on Microsoft Entra ID (formerly named Azure Active Directory) to be used later in my project. So far, I have a tenant level global reader, a subscription reader, and a resource group contributor. More to come soon!

