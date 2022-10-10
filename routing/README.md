# JunOS Routing

Deactivate a route - in routing-options start the line with deactivate  
A deactivated route will not show in the routing table  
Activate a deactivated route - In rouing-options start the line with activate  

A routed interface is an L3 interface   
Default - inet.0  

* FT - Forwarding table
* PFE - Packet Forwarding Engine
* IMP - import policy
* EXP - export policy

## Routing Policy Syntax  
matching (from) criteria on same line, generally OR'd  
Exception to above, route-filters  
Generally, (from) criteria on separate lines are AND'd  

**[edit policy-options]**  

show  
```
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

OSPF Import Accept all OSPF routes into inet.0 from LSDB SFP calc 
     Export Reject All (Static, BGP, Aggregate, other protocols) 
     (OSPF interface addresses advertised) 

ISIS Import Accept all ISIS routes into inet.0 from LSDB SFP calc 
     Export Reject All (Static, BGP, Aggregate, other protocols) 
     (ISIS interface addresses advertised) 
```

**Policy and link state**  
Within a Link state area - Every router must hold an identical copy of the stable.  


Static route - preference of 5 only thing that beats it is directly connected  


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
set route 200.200.200.2/32 next-hop 30.30.0.100 metric 100
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

One of the requirements for defining a static route is that the next hop has to be directly connected. Junos, by default, does not support recursive next hop resolution. This, however, is easily fixed with adding the resolve option at the end of the static route command. As long as the next hop is resolved by an active route in the routing table or by a default route it will install the resolved route as part of the indirect hop. If you do not add the resolve knob the route will remain hidden as there is no valid next hop

3 routers in a line r1/r2/r3  

r1 - 10.10.0.1/24  
r2 - 10.10.0.2/24  
r2 - 20.20.0.3/24  
r3 - 20.20.0.4/24  
r3 - LAN 40.40.0.4/24  

Routers must be running a dynamic routing protocol  
40.40.0/24 --> 20.20.0/24  

You need to add the resolve keyword  

[edit routing-options static]  
set route 40.40.0.0/24 next-hop 10.10.0.2 resolve  
lab@vSRX1# show
```
static {
    route 40.40.0.0/24 {
        next-hop 10.10.0.2;
        resolve; }
}

```

<p align="center" width="100%">
    <img width="75%" src="https://github.com/rikosintie/JNCIA/blob/main/routing/images/Junos-Resolve.png"> 
</p>  

**No readvertisement in junos**  
Usually for default routes to the internet.  
If a branch office has its own Internet connection, you don't want it redistributed into the corporate network.

<p align="center" width="100%">
    <img width="75%" src="https://github.com/rikosintie/JNCIA/blob/main/routing/images/Junos-no-redistribute.png"> 
</p>  

## VRFs  
Junos calls VRFs virtual routers. In Junos, vrf is only used in MPLS.

INET.0 is default routing table

top
edit routing-instance vr3 (VR3.INET.0 but no interfaces assigned)
1 Physical Interface add a vlan for vr3
dot1q

LT is a virtual interface
lt-0/0/0.0

## RIB Groups  
Copying routing between Tables (RIB-Groups)
Basically route leaking  



Prefix lists  
A prefix-list MATCHES THE PREFIX EXACTLY  
prefix-list-filter allows prefix match types  
* exact
* longer
* orlonger
* upto
* prefix-length-range

**Examples**  
**exact** - 192.168.0.0/16 exact  
Routes matching 192.168.0.0 in the first 16 bits AND with a subnet mask of exactly 16 bits.  
Only routes for exactly 192.16.0.0/16 match.  

**Longer** - 192.168.0.0/16 longer  
Routes matching 192.168.0.0 in the first 16 bits AND with a subnet mask LONGER than 16 bits.  
Any subnet of 192.16.0.0/16 matches.
* 192.168.0.0/17  
* 192.168.1.0/18  
* 192.168.100/24  
* 192.168.0.0/16 does NOT match.  

**Orlonger** - 192.168.0.0/16 orlonger  
A route matching 192.168.0.0 in the first 16 bits AND with a subnet mask of 16 bits ORLONGER.  
Any subnet of 192.16.0.0/16 matches.  
* 192.168.0.0/17 matches  
* 192.168.1.0/18 matches  
* 192.168.0.0/20 matches  
* 192.168.100/24  matches  
* 192.168.0.0/16 ALSO matches  

**upto** - 192.168.0.0/16 upto /24  
A route matching 192.168.0.0 in the first 16 bits AND with a subnet mask between 16 and 24.  
Any subnet of 192.16.0.0/16 with a subnet mask between 16 and 24 matches.  
* 192.168.0.0/16 matches  
* 192.168.1.0/17 matches  
* 192.168.0.0/18 matches  
* 192.168.100/24 matches  
* 192.168.1.128/26 does NOT match  

Prefix-Length-Range - 192.168.0.0/16 prefix-length-range /20-/24  
A route matching 192.168.0.0 in the first 16 bits AND with a subnet mask between 20 and 24 bits.  
Any subnet of 192.16.0.0/16 with a subnet mask between 20 and 24 matches.  
* 192.168.0.0/20 matches  
* 192.168.64.0/22 matches  
* 192.168.128.0/23 matches  
* 192.168.10/24 matches  
* 192.168.0.0/16 does NOT match  
* 192.168.1.128/26 does NOT match  
* 192.168.1.0/17 does NOT match  


[edit policy-options]  
show  
```
prefix-list rfc1918 {
    10.0.0.0/8;
    172.16.0.0/12;
    192.168.0.0/16;
}
policy-statement FIRST_EXAMPLE {
    term trem-1 {
        from {
            prefix-list rfc1918; (Exact match only)
        }
        then reject;
    }
}
policy-statement OTHER_EXAMPLE {
    term trem-2 {
        from {
            prefix-list-filter rfc1918 orlonger reject;
        }
    }
}
```
