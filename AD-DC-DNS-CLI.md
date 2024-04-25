The purpose of this readme is to show the process of setting up a Windows Server 2022 with DNS, DHCP, DNS, IIS, through the CLI server version. <br>
========================================================================================== <br>
```powershell
SConfig
```
========================================================================================== <br>

# Write-up

- Rename Server
Rename-Computer -NewName dc01

#Assign IP Configurations

Get-NetAdapter
New-NetIPAddress -InterfaceIndex 3 -IPAddress 192.168.10.10 -PrefixLength 24 -DefaultGateway 192.168.10.1

Set-DnsClientServerAddress -InterfaceIndex 3 -ServerAddresses 192.168.10.10

ipconfig /all

#restart server
Restart-Computer



#Step2-ADDS and DNS Installation


#ADDS role Installation

Install-windowsfeature AD-domain-services -IncludeAllSubFeature -IncludeManagementTools
Import-Module ADDSDeployment

#ADDS configurations

Install-ADDSForest `
-CreateDnsDelegation:$false `
-DatabasePath "C:\Windows\NTDS" `
-DomainMode "WinThreshold" `
-DomainName "netlab.lk" `
-DomainNetbiosName "NETLAB" `
-ForestMode "WinThreshold" `
-InstallDns:$true `
-LogPath "C:\Windows\NTDS" ` 
-NoRebootOnCompletion:$false `
-SysvolPath "C:\Windows\SYSVOL" `
-Force:$true


#Step3 #DHCP Installation 

#DHCP role Installation

Add-WindowsFeature –IncludeManagementTools DHCP
Get-Command -Module DHCPServer

#Create the security groups:
Netsh DHCP Add SecurityGroups
Add-DhcpServerSecurityGroup

#Restart the service. 
Restart-Service DHCPServer

#Authorize the DHCP server in AD DS:
Add-DHCPServerinDC 192.168.10.10

#Add DHCP Scope
Add-DhcpServerV4Scope -Name "Scope 1" -StartRange 192.168.10.100 -EndRange 192.168.10.200 -SubnetMask 255.255.255.0

Set-DhcpServerV4OptionValue -DnsServer 192.168.10.10 -Router 192.168.10.1

Set-DhcpServerv4Scope -ScopeId 192.168.10.0 -LeaseDuration 1.00:00:00
