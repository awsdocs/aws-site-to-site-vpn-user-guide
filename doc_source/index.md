# AWS Site-to-Site VPN User Guide

-----
*****Copyright &copy; 2019 Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

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
+ [How AWS Site-to-Site VPN Works](how_it_works.md)
   + [Site-to-Site VPN Categories](vpn-categories.md)
   + [Site-to-Site VPN Tunnel Options for Your Site-to-Site VPN Connection](VPNTunnels.md)
   + [Site-to-Site VPN Tunnel Authentication Options](vpn-tunnel-authentication-options.md)
   + [Customer Gateway Options for Your Site-to-Site VPN Connection](cgw-options.md)
   + [Accelerated Site-to-Site VPN Connections](accelerated-vpn.md)
   + [Site-to-Site VPN Routing Options](VPNRoutingTypes.md)
+ [Getting Started](SetUpVPNConnections.md)
+ [Site-to-Site VPN Architectures](site-site-architechtures.md)
   + [Site-to-Site VPN Single and Multiple Connection Examples](Examples.md)
   + [Providing Secure Communication Between Sites Using VPN CloudHub](VPN_CloudHub.md)
   + [Using Redundant Site-to-Site VPN Connections to Provide Failover](VPNConnections.md)
+ [Working with Site-to-Site VPN](working-with-site-site.md)
   + [Identifying a Site-to-Site VPN Connection](identify-vpn.md)
   + [Creating a Transit Gateway VPN Attachment](create-tgw-vpn-attachment.md)
   + [Migrating from AWS Classic VPN to AWS VPN](aws-vpn-migrate.md)
   + [Testing the Site-to-Site VPN Connection](HowToTestEndToEnd_Linux.md)
   + [Deleting a Site-to-Site VPN Connection](delete-vpn.md)
   + [Modifying a Site-to-Site VPN Connection's Target Gateway](modify-vpn-target.md)
   + [Modifying Site-to-Site VPN Tunnel Options](modify-vpn-tunnel-options.md)
   + [Changing the Customer Gateway for a Site-to-Site VPN Connection](change-vpn-cgw.md)
   + [Replacing Compromised Credentials](CompromisedCredentials.md)
   + [Rotating Site-to-Site VPN Tunnel Endpoint Certificates](roate-vpn-certificate.md)
+ [Security in AWS Site-to-Site VPN](security.md)
   + [Data Protection in AWS Site-to-Site VPN](data-protection.md)
      + [Internetwork Traffic Privacy](internetwork-traffic-privacy.md)
   + [Identity and Access Management for AWS Site-to-Site VPN](vpn-authentication-access-control.md)
      + [AWS Site-to-Site VPN Service-Linked Role](vpn-service-linked-roles.md)
   + [Logging and Monitoring](logging-monitoring.md)
   + [Compliance Validation for AWS Site-to-Site VPN](SERVICENAME-compliance.md)
   + [Resilience in AWS Site-to-Site VPN](disaster-recovery-resiliency.md)
   + [Infrastructure Security in AWS Site-to-Site VPN](infrastructure-security.md)
+ [Monitoring Your Site-to-Site VPN Connection](monitoring-overview-vpn.md)
   + [Monitoring VPN Tunnels Using Amazon CloudWatch](monitoring-cloudwatch-vpn.md)
+ [Site-to-Site VPN Limits](vpn-limits.md)
+ [Document History](WhatsNew.md)