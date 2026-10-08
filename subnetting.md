

Accenture
@C1:
config t
vlan 20
name ACCENTURE.COM
Interface vlan 20
 desc ACCENTURE.COM
 no shut
 ip add 10.0.0.129 255.255.255.128
ip dhcp excluded-add 10.0.0.129 10.0.0.139
ip dhcp pool ACCENTURE.COM
 network 10.0.0.128 255.255.255.128
 default-router 10.0.0.129
 domain-name ACCENTURE.COM

int e1/0
no shut
switchport mode access
switchport access vlan 20

@S1
config t
int e1/0
no shut
ip add dhcp
do bp

**********************chevron CLIENT**********************

config t
vlan 21
name CHEVRON.COM
Interface vlan 21
 desc CHEVRON.COM
 no shut
 ip add 10.0.8.1 255.255.248.0
ip dhcp excluded-add 10.0.8.1 10.0.8.100
ip dhcp pool CHEVRON.COM
 network 10.0.8.0 255.255.248.0
 default-router 10.0.8.1
 domain-name CHEVRON.COM

@a1
config t
int e0/0
no shut
switchport mode access
switchport access vlan 21
do show vlan brief


@P1
config t
int e0/0
no shut
ip add dhcp
do bp

**********************shell CLIENT**********************
config t
vlan 22
name SHELL.COM
Interface vlan 22
 desc SHELL.COM
 no shut
 ip add 10.0.16.1 255.255.240.0
ip dhcp excluded-add 10.0.16.1 10.0.16.100
ip dhcp pool SHELL.COM
 network 10.0.16.0 255.255.240.0
 default-router 10.0.16.1
 domain-name SHELL.COM
do sh run | sec dhcp

@a2
config t
int e1/0
no shut
switchport mode access
switchport access vlan 22
do show vlan brief


@P2
config t
int e1/0
no shut
ip add dhcp
do bp
**********************Fuel save CLIENT**********************
@c1
config t
vlan 23
name FUELSAVE.COM
Interface vlan 23
 desc FUELSAVE.COM
 no shut
 ip add 10.0.2.1 255.255.254.0
ip dhcp excluded-add 10.0.2.1 10.0.2.100
ip dhcp pool FUELSAVE.COM
 network 10.0.2.0 255.255.254.0
 default-router 10.0.2.1
 domain-name FUELSAVE.COM
do sh run | sec dhcp


@c2
config t
vlan 23
name FUELSAVE.COM
Interface vlan 23
 desc FUELSAVE.COM
 no shut
 ip add 10.0.2.1 255.255.254.0
ip dhcp excluded-add 10.0.16.1 10.0.16.100
ip dhcp pool FUELSAVE.COM
 network 10.0.2.0 255.255.254.0
 default-router 10.0.2.1
 domain-name FUELSAVE.COM
do sh run | sec dhcp



@S2
config t
int e1/0
no shut
ip add dhcp
do bp
DO SH IP DOMAIN


C2
config t



**********************DICT CLIENT**********************


DICT
@C1:
config t
vlan 31
name DICT.GOV.PH
Interface vlan 31
 desc DICT.GOV.PH
 no shut
 ip add 10.0.0.65 255.255.255.192
ip dhcp excluded-add 10.0.0.64 10.0.0.74
ip dhcp pool DICT.GOV.PH
 network 10.0.0.64 255.255.255.192
 default-router 10.0.0.65
 domain-name DICT.GOV.PH

int e1/0
no shut
switchport mode access
switchport access vlan 31

@S1
config t
int e1/0
no shut
ip add dhcp
do bp

**********************DPWH CLIENT**********************

config t
vlan 32
name DPWH.GOV.PH
Interface vlan 32
 desc DPWH.GOV.PH
 no shut
 ip add 10.0.32.1 255.255.224.0
ip dhcp excluded-add 10.0.32.1 10.0.32.100
ip dhcp pool DPWH.GOV.PH
 network 10.0.32.0 255.255.224.0
 default-router 10.0.32.1 
 domain-name DPWH.GOV.PH

@a1
config t
int e0/0
no shut
switchport mode access
switchport access vlan 32
do show vlan brief


@P1
config t
int e0/0
no shut
ip add dhcp
do bp

**********************DEPED CLIENT**********************
config t
vlan 33
name DEPED.GOV.PH
Interface vlan 33
 desc DEPED.GOV.PH
 no shut
 ip add 10.0.128.1 255.255.224.0
ip dhcp excluded-add 10.0.16.1 10.0.16.100
ip dhcp pool DEPED.GOV.PH
 network 10.0.128.0 255.255.224.0
 default-router 10.0.128.1
 domain-name DEPED.GOV.PH
do sh run | sec dhcp

@a2
config t
int e1/0
no shut
switchport mode access
switchport access vlan 33
do show vlan brief


@P2
config t
int e1/0
no shut
ip add dhcp
do bp
**********************PNP CLIENT**********************
@c1
config t
vlan 34
name PNP.GOV.PH
Interface vlan 34
 desc PNP.GOV.PH
 no shut
 ip add 10.0.64.1 255.255.192.0
ip dhcp excluded-add 10.0.64.1 10.0.64.100
ip dhcp pool PNP.GOV.PH
 network 10.0.64.0 255.255.192.0
 default-router 10.0.64.1
 domain-name PNP.GOV.PH
do sh run | sec dhcp



@S2
config t
int e1/0
no shut
ip add dhcp
do bp
do sh ip domain



C2
config t
int e1/0
no shut
switchport mode access
switchport access vlan 34
do show vlan brief
int e1/0


*****************************day 4*************************************
@1 define inside outside

conf t
int g1
ip nat outside
int g3
ip nat inside
access-list 1 permit any
ip nat inside source list 1 int g1 overload
end


@@2create access list
conf t
access-list 1 permit any
end


@@3
conf t
ip nat inside source list 1 int g1 overload
end



sudo su
hostname WEB2
ifconfig eth0 10.11.11.101 netmask 255.255.255.224 up
route add default gw 10.11.11.113
ping 10.11.11.113


sudo su
hostname WEB2
ifconfig eth0 10.21.21.211 netmask 255.255.255.240 up
route add default gw 10.21.21.213
ping 10.21.21.213

conf t
ip nat inside source static tcp 10.11.11.101 80 208.8.8.200 8080
ip nat inside source static tcp 10.11.11.102 443 208.8.8.200 8443

ip nat inside source static tcp 10.11.11.101 443 208.8.8.200 443
ip nat inside source static tcp 10.11.11.102 80 208.8.8.200 80
end


conf t
ip nat inside source static tcp 10.11.11.101 22 208.8.8.105 8765
ip nat inside source static tcp 10.11.11.102 22 208.8.8.105 5432
end







no shut
switchport mode access
switchport access vlan 23
do show vlan brief
