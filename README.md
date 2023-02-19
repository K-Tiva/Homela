# Homela

Installed ESXi, add command as bellow,

esxcli network vswitch standard set -v vSwitch0 -m 9000
esxcli network ip interface set -i vmk0 -m 9000

# Disable IPv6 on Windows Server
Get-NetAdapter | Disable-NetAdapterBinding -ComponentID ms_tcpip6
netsh interface ipv6 isatap set state disable
netsh interface ipv6 6to4 set state disable
netsh interface teredo set state disable

# Install AD-DS
Install-windowsfeature -name AD-Domain-Services
install-addsforest -domainname example.com
SafeModeAdministratorPassword: ********
SafeModeAdministratorPassword を確認してください: ********

# Configure DNS Forwarding
Get-DnsServerForwarder
Add-DnsServerForwarder -IPAddress x.x.x.x

# Add RTP Zone
Add-DnsServerPrimaryZone -NetworkID 10.0.0.0/24 -ZoneFile "0.0.10.in-addr.arpa.dns" -DynamicUpdate None -PassThru 

# Add A recored for DNS
Add-DnsServerResourceRecordA -Name "hostname" -ZoneName "test.local" -IPv4Address "x.x.x.x" -CreatePtr -PassThru

# Install OpenSSH Server in Windows Server 
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

Start-Service sshd
Set-Service -Name sshd -StartupType 'Automatic'
