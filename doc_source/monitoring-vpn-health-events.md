# Monitoring VPN connections using AWS Health events<a name="monitoring-vpn-health-events"></a>

AWS Site\-to\-Site VPN automatically sends notifications to the AWS [AWS Health Dashboard](https://phd.aws.amazon.com/phd/home#/) \(PHD\), which is powered by the AWS Health API\. This dashboard requires no setup, and is ready to use for authenticated AWS users\. You can configure multiple actions in response to event notifications through the AWS Health Dashboard\. 

The AWS Health Dashboard provides the following types of notifications for your VPN connections:
+ [Tunnel endpoint replacement notifications](#tunnel-replacement-notifications)
+ [Single tunnel VPN notifications](#single-tunnel-notifications)

## Tunnel endpoint replacement notifications<a name="tunnel-replacement-notifications"></a>

You receive a **Tunnel endpoint replacement notification** in the AWS Health Dashboard when one or both of the VPN tunnel endpoints in your VPN connection is replaced\. A tunnel endpoint is replaced when AWS performs tunnel updates, or when you modify your VPN connection\. For more information, see [Site\-to\-Site VPN tunnel endpoint replacements](endpoint-replacements.md)\.

When a tunnel endpoint replacement is complete, AWS sends the **Tunnel endpoint replacement notification** through a AWS Health Dashboard event\.

## Single tunnel VPN notifications<a name="single-tunnel-notifications"></a>

A Site\-to\-Site VPN connection consists of two tunnels for redundancy\. We strongly recommend that you configure both tunnels for high availability\. If your VPN connection has one tunnel up but the other is down for more than one hour in a day, you receive a *monthly* **VPN single tunnel notification** through an AWS Health Dashboard event\. This event will be updated weekly with any new VPN connections detected as single tunnel, and a new event created monthly, which will clear any VPN connections no longer detected as single tunnel\.