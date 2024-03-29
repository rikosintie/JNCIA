<p align="center" width="100%">
    <img width="20%" src="https://github.com/rikosintie/JNCIA/blob/main/images/juniper-networks-certified-associate-junos-jncia-junos.png"> 
</p>    


# Resources
Juniper offers a 7 hour video training course called [MIGRATING FROM THE CISCO CCNA TO THE JNCIA-JUNOS](https://learningportal.juniper.net/juniper/user_activity_info.aspx?id=EDU-JUN-WBT-JOL-CCNA-JNCIA-JUNOS). As always, you have to create an account. Unlike Cisco, you have to use a corporate email address, they won't accept Google, Apple,  OAUTH, etc.

When you complete the training they send you an email with an offer to take a sample exam up to 3 times. If you get a 70% or better, they send you a coupon for 75% off of the $200 Pearson exam!

[Getting Started with Networking](https://learningportal.juniper.net/juniper/user_activity_info.aspx?id=769)  
If you are new to networking you should watch these. If you are a CCNA or better at least review because there are L2/L3 and IP addressing questions on the exam.

## Exam Objectives  
Ovbiously you need to know what is covered on the exam:  
[Exam Objectives](https://www.juniper.net/us/en/training/certification/tracks/junos/jncia-junos.html)  

# Additional Resources
Juniper offers quite a bit of free material to study from. Here are the ones I know about. Once you create a Learning Portal account you get access to several communitites. None are dedicated to the exams, but they are good for general knowledge. There is a dedicated [twitter](https://twitter.com/JuniperCertify) certification account.  

## Junos Learning Portal
* Create your free [Juniper Learning Paths](https://learningportal.juniper.net/juniper/user_activity_info.aspx?id=5357) account
* Follow on [twitter](https://twitter.com/JuniperCertify) 
* Free Open Learning CCNA to JNCIA [video](https://learningportal.juniper.net/juniper/user_activity_info.aspx?id=12097) training
* Free practice tests after you complete the video training.  
* [Day One Books](https://www.juniper.net/dayone) - Step by Step instructions on the platform  
* [Beginner's Guide to Junos](https://www.juniper.net/documentation/en_US/day-one-books/junos-beginners-guide.pdf) - Same topics as the video training but a much deeper dive. Definitely read ch1-3, 6-8.
* [Career Forum](https://forums.juniper.net/t5/Training-Certification-and/bd-p/Training_and_Certification) - Ask questions, get answers, share your knowledge with peers  
* [Hardware Deployment Guides](https://www.juniper.net/documentation/)  
* [Juniper Networks Books](https://www.juniper.net/documentation/jnbooks/us/en/day-one-books)


## Juniper forums
  * [Forums](https://forums.juniper.net/)  
  * [JNCIA Study Guide PDF](https://goo.gl/4umoHX) (20 years old!!!)  

## Apps
  * Find all of Junipers applications [here](https://apps.juniper.net/home/)
  * [CLI Explorer](https://apps.juniper.net/cli-explorer/) - Complete CLI reference  
  * [Portable Libraries](https://www.juniper.net/documentation/resources/index.html) Doing a cutover and won't have Internet access? Grab a portable library!  

## Installation videos
* [Self Paced Hardware Installation and configuration Videos](http://juni.pr/3xfLTu5)
   
## Juniper vLabs
  * [Juniper vLabs](https://jlabs.juniper.net/vlabs)  
  * [Sandbox with vQFX, vSRX, vMX](https://jlabs.juniper.net/vlabs/portal/junos-day-one-plus-experience/index.page?icid=junos:note:1:junos_day_one)  
  * [vLab Sandbox: Junos Day One+ Experience – Using the Sandbox](https://jlabs.juniper.net/vlabs/portal/junos-day-one-plus-experience/junos-day-one-plus-experience-using-the-sandbox.page)  
  * [Juniper vLabs User Guide](https://jlabs.juniper.net/assets/pdf/vlabs/vlabs-ug.pdf)  
  * [Gettting Started with Junos OS lab](https://www.juniper.net/documentation/us/en/software/junos/junos-getting-started/index.html)  
  * [Junos OS Evolved](https://www.juniper.net/documentation/us/en/software/junos/junos-getting-started-evo/index.html)  

# Juniper Technical Articles
Along with the Juniper CCNA to JCNIA videos, I think you need to review these articles based on my exam. 

Watching Module 1 Junos fundamentals is important. There will be several questions on what the RE does and what the PFE does. I spent a lot of time watching this video and reading the junos-beginners-guide.pdf chapter 1.  

The module 5/6 videos covered routing, route filters, firewall filters in depth. I would watch the module 5/6 videos several times and have the lab with a vSRX, vQFX and vMX active. That way you can pause the video and jump into the lab.

**Pro Tip:** Once the lab is active, click the "COMMANDS" menu, then "Add Allowed Network Prefixes". This allows you to enter your public IP and then connect without having to use the browser. So much better!  

You will get an email from Juniper with the IP and port number for each device. It looks like this:  

<p align="center" width="100%">
    <img width="100%" src="https://github.com/rikosintie/JNCIA/blob/main/images/Public-IPs.png"> 
</p>    


**Pro Tip 2:** Create a new SSH key pair and use keys to login.  

If you are on windows, switch to Linux, I mean grab puttygen from the putty download page to create your keys.

On *nix  
```
cd ~/.ssh
ssh-keygen -t ed25519
```
You will be prompted for a filename:
```
Enter file in which to save the key (/Users/mhubbard/.ssh/id_ed25519):
```

I would use Juniper_ed25519 so that it's obvious two weeks from now when you are trying to remember how to log in!   

This will create Juniper_ed25519 and Juniper_25519.pub. The .pub is the key you copy to the router.  

Log into each device using the browser to enable root authentication:  
```
configure
set system root authentication plaintext [enter]
<plaintext password>
<confirm password>
show | compare
commit
```

Then use your ssh client to log in:
```
ssh root@<IP Address> -p <port in the email>
```  


Once you are logged in, enter the following in configure mode:

```
root# set system login user vector authentication ssh-ed25519 "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDoDOV0IobtYAgQXMDSvNPHVH7wVsD3iI9QBcF14hYUL"
root# set system login user vector class super-user
```

Obviously, you will need to use your public key, not mine. 

Then, to log in:  
```
ssh -i ~/.ssh/juniper_ed25519_key vector@66.129.234.214 -p 44020
```  

Here are the articles I found useful:  

* [Loopback Interface Configuration](https://www.juniper.net/documentation/us/en/software/junos/junos-getting-started-evo/interfaces-fundamentals/topics/topic-map/interfaces-configuring-the-loopback-interface.html) - Note uses "passive" to advertise into a routing protocol.  
* [How Route Filters Are Evaluated in Routing Policy Match Conditions](https://www.juniper.net/documentation/us/en/software/junos/routing-policy/topics/concept/policy-configuring-route-lists-for-use-in-routing-policy-match-conditions.html#understanding-route-filters-for-use-in-routing-policy-match-conditions__id-10270525)  
* [Understanding Route Filters for Use in Routing Policy Match Conditions](https://www.juniper.net/documentation/us/en/software/junos/routing-policy/topics/concept/policy-configuring-route-lists-for-use-in-routing-policy-match-conditions.html)  
* [Protocol-Independent Routing Properties User Guide](https://www.juniper.net/documentation/us/en/software/junos/static-routing/static-routing.pdf)  
* [Meet the EX3400 Ethernet Switch](https://www.juniper.net/documentation/us/en/day-one-plus/ex3400/id-step-1-begin.html)  
* [Junos Architecture - Control and Data Planes - Introduction to Juniper and JNCIA part 12](https://www.youtube.com/watch?v=9MWUih0qWUc)  
* [Juniper's Routng Table | Introduction to Juniper and JNCIA part 15](https://www.youtube.com/watch?v=D8YZmg0ywW0)  
* [An infromal guide to the-Engines-of-Packet-Forwarding](https://forums.juniper.net/t5/Routing/An-Informal-Guide-to-the-Engines-of-Packet-Forwarding/ta-p/401192)  
* [Overview for Junos OS](https://www.juniper.net/documentation/us/en/software/junos/junos-overview/index.html) You can read it on the website or download the PDF. The PDF was published on 2022-06-15.  
* [Junos OS Overview](https://www.juniper.net/documentation/us/en/software/junos/junos-overview/topics/concept/junos-software-introduction.html) A must read article. It clarifies how Junos works. Read the "Related Documentaion" articles also.  
* [Loading Configuration Files](https://www.juniper.net/documentation/us/en/software/junos/cli/topics/topic-map/junos-config-files-loading.html) load (factory-default | merge | override | patch | replace | set | update) filename <relative> <json>  
* [instance-type](https://www.juniper.net/documentation/us/en/software/junos/mpls/topics/ref/statement/instance-type-edit-routing-instances-vp.html) - Read over this article. I had 2 questions about it on the exam.  
* [show route forwarding-table](https://www.juniper.net/documentation/us/en/software/junos/mpls/topics/ref/command/show-route-forwarding-table-mpls-ex-series.html) - There will be questions about what you see when showing routes and the route forwarding-table. Be sure to lab it up and look at show route vs show route forwarding-table
* [Configuring Default, Primary, and Preferred Addresses and Interfaces](https://www.juniper.net/documentation/en_US/junos9.5/information-products/topic-collections/config-guide-network-interfaces/interfaces-configuring-default-primary-and-preferred-addresses-and-interfaces.html) - Be sure to lab this up. It's important for the exam and is different that Cisco/Aruba.  
* [Actions in Routing Policy Terms](https://www.juniper.net/documentation/us/en/software/junos/routing-policy/topics/concept/policy-configuring-actions-in-routing-policy-terms.html) Make sure you understand "Flow Control Actions"
* [Tracing Global Routing Protocol Operations](https://www.juniper.net/documentation/us/en/software/junos/static-routing/topics/topic-map/tracing-global-routing-protocol-operations.html#id-example-tracing-global-routing-protocol-operations) I had one or two questions on tracing. This is quite different than debug on Cisco/Aruba. Make sure you lab it up and understand how to configure and display tracing.
* [Firewall Filter Match Conditions, Actions, and Action Modifiers for EX Series Switches](https://www.juniper.net/documentation/us/en/software/junos/routing-policy/topics/concept/firewall-filter-ex-series-match-conditions-description.html) Be sure to at least scan this document.
* [instance-type](https://www.juniper.net/documentation/us/en/software/junos/mpls/topics/ref/statement/instance-type-edit-routing-instances-vp.html) Again, this is quite different than Cisco/Aruba. You don't need to commit this to memory but at least scan it.
* [Example: Configuring the MAC Address of an IRB Interface](https://www.juniper.net/documentation/us/en/software/junos/multicast-l2/topics/example/example-configuring-mac-address-of-an-irb-interface.html)
* 

That's about it. I know it seems like a lot for an associate exam but I don't like failing these things. I received a 93%k, passing was 61%. So I could have spent a lot less time studying, but I love the Junos devices and it wasn't really work to me.

Good luck!

# Community Resources  
  * J-NSP Juniper Mailing lists [J-NSP](https://puck.nether.net/mailman/listinfo/juniper-nsp)  
  * Join the [Juniper Slack](networktocode.herokuapp.com/) channel  
  * Join the Juniper commnunity - [elevate](https://community.juniper.net/home857)



# Docker
* [DAY ONE: BUILDING CONTAINERS WITH cSRX, 2ND EDITION](https://www.juniper.net/documentation/en_US/day-one-books/day-one-building-containers-with-docker-csrx.pdf)  
* [Docker Hub](https://hub.docker.com) - Search for juniper  

## GNS3  
Juniper provides images that worn on GNS3 but you have to pay for the Juniper training.

# Blogs

[JNCIA-Junos Passed: Resources and Exam Thoughts](https://journey2theccie.wordpress.com/2021/03/03/jncia-junos-passed-resources-and-exam-thoughts/)  
[Juniper JunOS for Cisco Engineers Pt.1 – CLI Basics](https://journey2theccie.wordpress.com/2021/02/11/juniper-junos-for-cisco-engineers-pt-1-cli-basics/)  
[Juniper JunOS for Cisco Engineers Pt.2 – Static Routing and OSPF](https://journey2theccie.wordpress.com/2021/02/24/juniper-junos-for-cisco-engineers-pt-2-static-routing-and-ospf/)  
[Juniper vSRX Setup & Initial Configuration Guide](https://journey2theccie.wordpress.com/2021/02/04/juniper-vsrx-setup-initial-configuration-guide/)  
[vSRX Deployment Guide for Private and Public Cloud Platforms](https://www.juniper.net/documentation/us/en/software/vsrx/vsrx-consolidated-deployment-guide/index.html)
[Requirements for vSRX on VMware](https://www.juniper.net/documentation/us/en/software/vsrx/vsrx-consolidated-deployment-guide/vsrx-vmware/topics/concept/security-vsrx-vmware-system-requirement.html)  
[JNCIA-DevOps Passed – Resources & Exam Thoughts (Take it for free!)](https://journey2theccie.wordpress.com/2020/06/07/jncia-devops-passed-resources-exam-thoughts-take-it-for-free/)  
[Route-policy in JUNOS](https://ippoint.wordpress.com/2010/09/27/route-policy-in-junos/)  

## AWS EC2 Instances
  * vMX
  * vSRX

**Some websites I found useful in learning/supporting Juniper**  
* [Public key authentication in JUNOS](https://rtodto.net/public-key-authentication-in-junos/)  
* [How to format install EX2300s and EX3400s via USB](https://supportportal.juniper.net/s/article/EX-How-to-format-install-EX2300s-and-EX3400s-via-USB?language=en_US)  
* [Understanding the "Resolved In" field in Problem Reports (PR)](https://supportportal.juniper.net/s/article/Understanding-the-Resolved-In-field-in-Problem-Reports-PR?language=en_US)  
* [How do you send Ctrl C in expect script?](https://quick-advisors.com/how-do-you-send-ctrl-c-in-expect-script/)  - I needed to flash several EX3400s and used minicom with expect on Mac to automatically enter ctrl+c to bring up the boot menu. Here is the terminal command I used  
` minicom -D /dev/cu.usbserial-A9TZ2K1R -S expect.txt`  
I used an FTDI USB-C to serial adapter and it mounted as A9TZ2K1R.  

## Official Juniper Documentation Resources

**JUNOS BASICS**  
* [Junos OS Documentation](https://www.juniper.net/documentation/product/en_US/junos-os#cat=product_documentation)  
* [Overview for Junos OS](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/system-basics/junos-overview.html)  
* [Day1 + JunosOS](https://www.juniper.net/documentation/en_US/day-one-plus/junos-os/id-step-1-begin.html)  
* [Getting Started Guide for Junos OS](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/system-basics/getting-started.html)  
* [User Access and Authentication User Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/system-basics/user-access.html)
* [CLI User Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/junos-cli/junos-cli.html)  
* [CLI Explorer](https://apps.juniper.net/cli-explorer/)  
* [Junos® OS Software Installation and Upgrade Guide](https://www.juniper.net/documentation/us/en/software/junos/junos-install-upgrade/topics/topic-map/software-install-and-upgrade-overview.html#id-junos-os-installation-package-names__d110e95)  
*  [Junos Software Versions - Suggested Releases to Consider and Evaluate](https://supportportal.juniper.net/s/article/Junos-Software-Versions-Suggested-Releases-to-Consider-and-Evaluate?language=en_US)  
* [Software Installation and Upgrade Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/software-installation-and-upgrade/software-installation-and-upgrade.html)  
* [Introducing Junos OS Evolved](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/introducing-evo-guide.html)  

**Routing policies, Protocols, Firewall Filters, and Services**  
* [Routing Protocols Overview](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/config-guide-routing/config-guide-routing-overview.html)  
* [Protocol-Independent Routing Properties User Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/config-guide-routing/config-guide-static-routing.html)  
* [Routing Policies, Firewall Filters, and Traffic Policers User Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/config-guide-policy/config-guide-policy.html)  
* [OSPF User Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/config-guide-routing/config-guide-ospf.html)  
* [BGP User Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/config-guide-routing/config-guide-routing-bgp.html)  
* [RIP User Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/config-guide-routing/config-guide-routing-rip.html)
* [IS-IS User Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/config-guide-routing/config-guide-routing-is-is.html)  
* [MPLS Applications User Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/config-guide-mpls-applications/config-guide-mpls-applications.html)  
[Class of Service User Guide (Routers and EX9200 Switches)](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/cos/config-guide-cos.html)  
* [Class of Service User Guide (Security Devices)](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/security/cos-overview.html)  
* [Adaptive Services Interfaces User Guide for Routing Devices](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/services-interfaces/router-services-application-properties.html)  
* [Broadband Subscriber Services User Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/subscriber-access/subscriber-mgmt-advanced-provisioning.html)  

**ADDITIONAL RESOURCES**  
* [Ethernet Switching User Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/layer-2/layer2-multicast.html)  
* [Ethernet Interfaces User Guide for Routing Devices](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/config-guide-network-interfaces/network-interfaces-ethernet.html)  
* [Interfaces Fundamentals for Routing Devices](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/config-guide-network-interfaces/network-interfaces-fundamentals.html)  
* [Interfaces User Guide for Switches](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/qfx-series/ethernet-interfaces-switches.html)  
* [J-Web for SRX Series Documentation](https://www.juniper.net/documentation/product/en_US/j-web-srx-series)  
* [Chassis-Level User Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/system-basics/router-chassis.html)  
* [Network Management and Monitoring Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/network-management/network-management.html)  
* [REST API Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/rest-api/rest-api.html)  
* [NETCONF XML Management Protocol Developer Guide](https://www.juniper.net/documentation/en_US/junos/information-products/pathway-pages/netconf-guide/netconf.html)  
