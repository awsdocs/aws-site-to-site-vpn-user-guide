# AWS Site-to-Site VPN User Guide

-----
*****Copyright &copy; 2020 Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.

-----
## Contents
+ [What is AWS Site-to-Site VPN?](VPC_VPN.md)
+ [How AWS Site-to-Site VPN works](how_it_works.md)
   + [Site-to-Site VPN categories](vpn-categories.md)
   + [Tunnel options for your Site-to-Site VPN connection](VPNTunnels.md)
   + [Site-to-Site VPN tunnel authentication options](vpn-tunnel-authentication-options.md)
   + [Site-to-Site VPN tunnel initiation options](initiate-vpn-tunnels.md)
   + [Customer gateway options for your Site-to-Site VPN connection](cgw-options.md)
   + [Accelerated Site-to-Site VPN connections](accelerated-vpn.md)
   + [Site-to-Site VPN routing options](VPNRoutingTypes.md)
      + [IPv4 and IPv6 traffic](ipv4-ipv6.md)
+ [Getting started](SetUpVPNConnections.md)
+ [Site-to-Site VPN architectures](site-site-architechtures.md)
   + [Site-to-Site VPN single and multiple connection examples](Examples.md)
   + [Providing secure communication between sites using VPN CloudHub](VPN_CloudHub.md)
   + [Using redundant Site-to-Site VPN connections to provide failover](vpn-redundant-connection.md)
+ [Your customer gateway device](your-cgw.md)
   + [Example customer gateway device configurations for static routing](cgw-static-routing-examples.md)
   + [Example customer gateway device configurations for dynamic routing (BGP)](cgw-dynamic-routing-examples.md)
   + [Configuring Windows Server 2008 R2 as a customer gateway device](CustomerGateway-Windows.md)
   + [Configuring Windows Server 2012 R2 as a customer gateway device](customer-gateway-windows-2012.md)
   + [Troubleshooting your customer gateway device](Troubleshooting.md)
      + [Troubleshooting connectivity when using Border Gateway Protocol](Generic_Troubleshooting.md)
      + [Troubleshooting connectivity without Border Gateway Protocol](Generic_Troubleshooting_noBGP.md)
      + [Troubleshooting Cisco ASA customer gateway device connectivity](Cisco_ASA_Troubleshooting.md)
      + [Troubleshooting Cisco IOS customer gateway device connectivity](Cisco_Troubleshooting.md)
      + [Troubleshooting Cisco IOS customer gateway device without Border Gateway Protocol connectivity](Cisco_Troubleshooting_NoBGP.md)
      + [Troubleshooting Juniper JunOS customer gateway device connectivity](Juniper_Troubleshooting.md)
      + [Troubleshooting Juniper ScreenOS customer gateway device connectivity](Juniper_ScreenOs_Troubleshooting.md)
      + [Troubleshooting Yamaha customer gateway device connectivity](Yamaha_Troubleshooting.md)
+ [Working with Site-to-Site VPN](working-with-site-site.md)
   + [Identifying a Site-to-Site VPN connection](identify-vpn.md)
   + [Migrating from AWS Classic VPN to AWS VPN](aws-vpn-migrate.md)
   + [Creating a transit gateway VPN attachment](create-tgw-vpn-attachment.md)
   + [Testing the Site-to-Site VPN connection](HowToTestEndToEnd_Linux.md)
   + [Deleting a Site-to-Site VPN connection](delete-vpn.md)
   + [Modifying a Site-to-Site VPN connection's target gateway](modify-vpn-target.md)
   + [Modifying Site-to-Site VPN connection options](modify-vpn-connection-options.md)
   + [Modifying Site-to-Site VPN tunnel options](modify-vpn-tunnel-options.md)
   + [Editing static routes for a Site-to-Site VPN connection](vpn-edit-static-routes.md)
   + [Changing the customer gateway for a Site-to-Site VPN connection](change-vpn-cgw.md)
   + [Replacing compromised credentials](CompromisedCredentials.md)
   + [Rotating Site-to-Site VPN tunnel endpoint certificates](roate-vpn-certificate.md)
+ [Security in AWS Site-to-Site VPN](security.md)
   + [Data protection in AWS Site-to-Site VPN](data-protection.md)
      + [Internetwork traffic privacy](internetwork-traffic-privacy.md)
   + [Identity and access management for AWS Site-to-Site VPN](vpn-authentication-access-control.md)
      + [AWS Site-to-Site VPN service-linked role](vpn-service-linked-roles.md)
   + [Logging and monitoring](logging-monitoring.md)
   + [Compliance validation for AWS Site-to-Site VPN](site-to-site-vpn-compliance.md)
   + [Resilience in AWS Site-to-Site VPN](disaster-recovery-resiliency.md)
   + [Infrastructure security in AWS Site-to-Site VPN](infrastructure-security.md)
+ [Monitoring your Site-to-Site VPN connection](monitoring-overview-vpn.md)
   + [Monitoring VPN tunnels using Amazon CloudWatch](monitoring-cloudwatch-vpn.md)
+ [Site-to-Site VPN quotas](vpn-limits.md)
+ [Document history](WhatsNew.md)