#JunOS Routing

Deactivate a route - in routing-options start the line with deactivate
A deactivated route will not show in the routing table
Activate a deactivated route - In rouing-options start the line with activate

A routed interface is an L3 interface   
Default - inet.0  

* FT - Forwarding table
* PFE - Packet Forwarding Engine
* IMP - import policy
* EXP - export policy

##Routing Policy Syntax  
matching (from) criteria on same line, generally OR'd
Exception to above, route-filters
Generally, (from) criteria on separate lines are AND'd

**[edit policy-options]**  
```
show  
policy-statement EXP-FOOBAR {  
     term OSPF_OR_STATIC_RFC1918 {  
        from {  
        route-filter 172.16.0.0/12 orlonger;   ----------  
        route-filter 192.168.0.0/16 orlonger;   Logic OR ------------  
        route-filter 10.0.0.0/8 orlonger;      ----------               Logic AND  (Route filters AND'd with Protocol)  
        protocol [static ospf];  
                   |logic OR|----------------------------------------  
        }  
        then reject;  
    }  
    term cath-all {  
        then accept;  
    }  
}  
```


**Create the policy**
```
edit policy-options
set policy-statement EXP-FOOBAR term OSPF_OR_STATIC_RFC1918 from route-filter 172.16.0.0/12 orlonger
set policy-statement EXP-FOOBAR term OSPF_OR_STATIC_RFC1918 from route-filter 192.168.0.0/16 orlonger
set policy-statement EXP-FOOBAR term OSPF_OR_STATIC_RFC1918 from route-filter 10.0.0.0/8 orlonger
set policy-statement EXP-FOOBAR term OSPF_OR_STATIC_RFC1918 from protocol [static ospf]
set policy-statement EXP-FOOBAR term OSPF_OR_STATIC_RFC1918 then reject
set policy-statement EXP-FOOBAR term catch-all then accept
```

**show policy options from top**
```
root@TEST-Router# show policy-options
policy-statement EXP-FOOBAR {
    term OSPF_OR_STATIC_RFC1918 {
        from {
            protocol [ static ospf ];
            route-filter 172.16.0.0/12 orlonger;
            route-filter 192.168.0.0/16 orlonger;
            route-filter 10.0.0.0/8 orlonger;
        }
        then reject;
    }
    term catch-all {
        then accept;
    }
}
```

**route-filter Action Shortcuts**  
It's possible to override a term's action  
in this case we put an accept after the "orlonger"

```
policy-statement EXP-FOOBAR {
    term OSPF_OR_STATIC_RFC1918 {
        from {
            protocol [ static ospf ];
            route-filter 172.16.0.0/12 orlonger;
            route-filter 192.168.0.0/16 orlonger;
            route-filter 10.0.0.0/8 orlonger accept;
        }
        then reject;
    }
    term catch-all {
        then {
           accept;
           metric add 5;
    }
}
```
**Default Routing Policy**  
```
BGP Import accept all valid BGP route  
    Export accept and advertise all active BGP routes  

OSPF Import Accept all OSPF routes into inte.0 from LSDB SFP calc 
     Export Reject All (Static, BGP, Aggregate, other protocols) 
     (OSPF interface addresses advertised) 

ISIS Import Accept all ISIS routes into inte.0 from LSDB SFP calc 
     Export Reject All (Static, BGP, Aggregate, other protocols) 
     (ISIS interface addresses advertised) 
```

**Policy and link state**  
Within a Link state area - Every router must hold an identical copy of the stable.  


Static route - preferenece of 5 only thing that beats it is directly connected  


**Route Preference** 
Ranks routes received from different sources  
Primary selection criteria the active route
```
source          Junos Pref      IOS AD
direct          0               0
Local           0               0
Static          5               1
OSPF Internal   10              110
RIP             100             120
OSPF External   150             110
BGP             170             eBGP 170 / iBGP 200
```

Junos only supports P2P interfaces for a next hop!  

**Static routing**  
You can assign a metric to a static route  
Maybe you want to redistribute it into OSPF. It could become the value in OSPF/BGP

```
edit
edit routing-options static

[edit routing-options static]
set route 200.200.200.2/32 next-hop 30.30.0.100
top 
show routing-options
show | compare
commit
```

run show ip route

**Discard vs Reject**  

REJECT - Throw packet away and generate an ICMP unreachable 
```
[edit  routing-options static]
set route 200.200.200.2/32 reject
top
commit
```
ping 200.200.200.2  
no route to host  
 


DISCARD - Throw packet away siliently
```
[edit routing-options static]
edit routing-options static route 200.200.200.2/32
[edit routing-options static route 200.200.200.2/32]
set discard
```
ping 200.200.200.2  
nothing - discarded silently  


**Recursive resolution**  
3 routers in a line r1/r2/r3  

r1 - 10.10.0.1/24  
r2 - 20.20.0.1/24  
r3 - 40.40.0.1/24  

R1  
40.40.0/24 --> 20.20.0/24  

You need to add the resolve keyword  



