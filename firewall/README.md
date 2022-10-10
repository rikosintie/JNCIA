# Firewall
 
**Create a policer policy**  

[edit]  
jcluser@vsrx# set firewall policer police-1 if-exceeding bandwidth-percent 1  
jcluser@vsrx# set firewall policer police-1 if-exceeding burst-size-limit 10000   

[edit firewall family inet]  
jcluser@vsrx# set filter test term term-1 from source-address 10.10.10.10/32  
jcluser@vsrx# set filter test term term-1 then policer P1 accept  


[edit firewall]  
show policer P1  
```
if-exceeding {
    bandwidth-limit 64k;
    burst-size-limit 4k;
}
then discard;
```

show  
```
family inet {
    filter test {
        term term-1 {
            from {
                source-address {
                    10.10.10.10/32;
                }
            }
            then {
                policer P1;
                accept;
            }
        }
    }
}
```

**Protecting the control plane**  


```
-----------------
|      RE       |
-----------------
           |
           |------------------------  FXP1
           |
-----------------------
|     PFE      |
----------------
```



The filter is applied to loopback lo0 interface. For example  
set interfaces lo0 unit 0 family inet filter input test  

discard - no icmp messege sent, silently discard
reject - discard but send an icmp "acommunication prohibited by filter" message

count - simply count the number of packets, also the byte count. 3 packets of 100 bytes will report 300. Can be polled by snmp  
syslog - detailed information about the packet. Passed to the RE, logged permanently. Keep in mind that it's intensive and will stop logging if exceeds fxp1 capacity.  
log - All taking place in the PFE. Stored in memory, only 400 lines, lost on reboot.  
policer - rate limiting  



## Moving a filter up  

Terms are inserted in order. If you add a new term, it will be inserted at the bottom. Usually this isn't what you want.  

Juniper makes it simple to move a term.

`insert term ALLOW_OSPF before term CATCH_All`  


<p align="center" width="100%">
    <img width="75%" src="https://github.com/rikosintie/JNCIA/blob/main/routing/images/Juniper-firewall-insert.png"> 
</p>  

 
