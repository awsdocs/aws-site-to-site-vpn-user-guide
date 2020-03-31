# Using redundant Site\-to\-Site VPN connections to provide failover<a name="vpn-redundant-connection"></a>

To protect against a loss of connectivity in case your customer gateway device becomes unavailable, you can set up a second Site\-to\-Site VPN connection to your VPC and virtual private gateway by using a second customer gateway device\. By using redundant Site\-to\-Site VPN connections and customer gateway devices, you can perform maintenance on one of your devices while traffic continues to flow over the second customer gateway's Site\-to\-Site VPN connection\. 

The following diagram shows the two tunnels of each Site\-to\-Site VPN connection and two customer gateways\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/Multiple_Gateways_diagram.png)

For this scenario, do the following:
+ Set up a second Site\-to\-Site VPN connection by using the same virtual private gateway and creating a new customer gateway\. The customer gateway IP address for the second Site\-to\-Site VPN connection must be publicly accessible\.
+ Configure a second customer gateway device\. Both devices should advertise the same IP ranges to the virtual private gateway\. We use BGP routing to determine the path for traffic\. If one customer gateway device fails, the virtual private gateway directs all traffic to the working customer gateway device\.

Dynamically routed Site\-to\-Site VPN connections use the Border Gateway Protocol \(BGP\) to exchange routing information between your customer gateways and the virtual private gateways\. Statically routed Site\-to\-Site VPN connections require you to enter static routes for the remote network on your side of the customer gateway\. BGP\-advertised and statically entered route information allow gateways on both sides to determine which tunnels are available and reroute traffic if a failure occurs\. We recommend that you configure your network to use the routing information provided by BGP \(if available\) to select an available path\. The exact configuration depends on the architecture of your network\.

For more information about creating and configuring a customer gateway and a Site\-to\-Site VPN connection, see [Getting started](SetUpVPNConnections.md)\.