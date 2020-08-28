# Site\-to\-Site VPN tunnel initiation options<a name="initiate-vpn-tunnels"></a>

By default, your customer gateway device must bring up the tunnels for your Site\-to\-Site VPN connection by generating traffic and initiating the Internet Key Exchange \(IKE\) negotiation process\. You can configure your VPN tunnels to specify that AWS must initiate or restart the IKE negotiation process instead\.

## VPN tunnel IKE initiation options<a name="ike-initiation-options"></a>

The following IKE initiation options are available\. You can implement either or both options for your VPN tunnels\.
+ **Startup action**: The action to take when establishing the VPN tunnel for a new or modified VPN connection\. By default, your customer gateway device initiates the IKE negotiation process to bring the tunnel up\. You can specify that AWS must initiate the IKE negotiation process instead\.
+ **DPD timeout action**: The action to take after dead peer detection \(DPD\) timeout occurs\. By default, the IKE session is stopped, the tunnel goes down, and the routes are removed\. You can specify that AWS must restart the IKE session when DPD timeout occurs, or you can specify that AWS must take no action when DPD timeout occurs\.

You can configure the IKE initiation options for one or both of the VPN tunnels in your Site\-to\-Site VPN connection\.

## Rules and limitations<a name="ike-initiation-rules"></a>

The following rules and limitations apply:
+ To initiate IKE negotiation, AWS requires the public IP address of your customer gateway device\. If you configured certificate\-based authentication for your VPN connection and you did not specify an IP address when you created the customer gateway resource in AWS, you must create a new customer gateway and specify the IP address\. Then, modify the VPN connection and specify the new customer gateway\. For more information, see [Changing the customer gateway for a Site\-to\-Site VPN connection](change-vpn-cgw.md)\.
+ You cannot configure IKE initiation options for an AWS Classic VPN connection\.
+ IKE initiation from the AWS side of the VPN connection is supported for IKEv2 only\.

If you do not configure IKE initiation from the AWS side for your VPN tunnel and the VPN connection experiences a period of idle time \(usually 10 seconds, depending on your configuration\), the tunnel might go down\. To prevent this, you can use a network monitoring tool to generate keepalive pings, for example, by using IP SLA\. 

## Working with VPN tunnel initiation options<a name="working-with-ike-initiation-options"></a>

For more information about working with VPN tunnel initiation options, see the following topics:
+ To create a new VPN connection and specify the VPN tunnel initiation options: [Create a Site\-to\-Site VPN connection](SetUpVPNConnections.md#vpn-create-vpn-connection)
+ To modify the VPN tunnel initiation options for an existing VPN connection: [Modifying Site\-to\-Site VPN tunnel options](modify-vpn-tunnel-options.md) 