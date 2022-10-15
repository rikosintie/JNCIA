# Junos Fundamentals

**Software Release types**  
* R First revenue ship (FRS) or maintenance release software. R1 is FRS. R2 onward are maintenance releases.  
* F Feature velocity release. Feature velocity releases are only in Junos OS Release 15.1.  
* B Beta release software.  
* I Internal release software. These are private software releases for verifying fixes.  
* S Service release software, which are released to customers to solve a specific problem—this release will be maintained along with the life span of the underlying release. The service release number is added after the R number, for example, 14.2R3-S4.4. Here S4 represents the 4th service release on top of 14.2R3 and is the 4th re-spin.  
* X Special (eXception) release software. X releases follow a numbering system that differs from the standard release numbering.  


##Routing Engine (RE)  
* The RE will also monitor chassis health (for example, temperature, fan speed, etc).  
* MGD Management Daemon - show commands, netconf, rpc,   
* DCD Device Configuration Daemon - create an IP address, show log dcd   
* ChassisD - After RE finishes booting, chassid brings up all the cards.  
* DHCPD - DHCP server  
* INETD - Provides Internetwork connectivity to Junos  
* KMD - Key Management Daemon  
* l2cpd - layer2 control plane daemon  
* RPD - Routing Daemon 
* VRRPD - Virtual Routing Redundancy Protocol Daemon  
* SNMPD - snmp daemon                
       
       
|----------------------|     
|       Kernel         |     
|----------------------|  
|  x86/Flash/SSD/RAM   |  


 

Control Plane
Routing Information Base
Forwarding Information Base
Active route has a * next to it
