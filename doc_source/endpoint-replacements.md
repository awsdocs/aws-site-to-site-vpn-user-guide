# Site\-to\-Site VPN tunnel endpoint replacements<a name="endpoint-replacements"></a>

Your Site\-to\-Site VPN connection consists of two VPN tunnels for redundancy\. Sometimes, one or both of the VPN tunnel endpoints is replaced when AWS performs tunnel updates, or when you modify your VPN connection\. During a tunnel endpoint replacement, connectivity over the tunnel might be interrupted while the new tunnel endpoint is provisioned\.

If your tunnel endpoint has been replaced, AWS sends a notification through a AWS Health Dashboard event\. For more information, see [Monitoring VPN connections using AWS Health events](monitoring-vpn-health-events.md)\.

## Endpoint replacements during VPN tunnel updates<a name="endpoint-replacements-for-aws-updates"></a>

AWS Site\-to\-Site VPN is a managed service, and periodically applies updates to your VPN tunnel endpoints\. These updates happen for a variety of reasons, including the following:
+ To apply general upgrades, such as a patches, resiliency improvements, and other enhancements
+ To retire underlying hardware
+ When automated monitoring determines that a VPN tunnel endpoint is unhealthy

AWS applies tunnel endpoint updates to one tunnel of your VPN connection at a time, during which time your VPN connection might experience a brief loss of redundancy\. It’s therefore important to configure both tunnels in your VPN connection for high availability\. 

## Endpoint replacements during VPN connection modifications<a name="endpoint-replacements-for-vpn-modifications"></a>

When you modify the following components of your VPN connection, one or both of your tunnel endpoints is replaced\.


| Modification | API action | Tunnel impact | 
| --- | --- | --- | 
| [Modify the target gateway for the VPN connection](modify-vpn-target.md) | [ModifyVpnConnection](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_ModifyVpnConnection.html) | Both tunnels are unavailable while new tunnel endpoints are provisioned\. | 
| [Change the customer gateway for the VPN connection](change-vpn-cgw.md) | [ModifyVpnConnection](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_ModifyVpnConnection.html) | Both tunnels are unavailable while new tunnel endpoints are provisioned\. | 
| [Modify the VPN connection options](modify-vpn-connection-options.md) | [ModifyVpnConnectionOptions](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_ModifyVpnConnectionOptions.html) | Both tunnels are unavailable while new tunnel endpoints are provisioned\. | 
| [Modify the VPN tunnel options](modify-vpn-tunnel-options.md) | [ModifyVpnTunnelOptions](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_ModifyVpnTunnelOptions.html) | The modified tunnel is unavailable during the update\. | 