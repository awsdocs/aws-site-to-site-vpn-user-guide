# Modifying a Site\-to\-Site VPN connection's target gateway<a name="modify-vpn-target"></a>

You can modify the target gateway of AWS Site\-to\-Site VPN connection\. The following migration options are available:
+ An existing virtual private gateway to a transit gateway
+ An existing virtual private gateway to another virtual private gateway
+ An existing transit gateway to another transit gateway
+ An existing transit gateway to a virtual private gateway

After you modify the target gateway, your Site\-to\-Site VPN connection will be temporarily unavailable for a brief period while we provision the new endpoints\.

The following tasks help you complete the migration to a new gateway\. 

**Topics**
+ [Step 1: Create the transit gateway](#step-create-gateway)
+ [Step 2: Delete your static routes \(required for a static VPN connection migrating to a transit gateway\)](#step-update-staic-route)
+ [Step 3: Migrate to a new gateway](#step-migrate-gateway)
+ [Step 4: Update VPC route tables](#step-update-routing)
+ [Step 5: Update the transit gateway routing \(required when the new gateway is a transit gateway\)](#step-update-transit-gateway-routing)
+ [Step 6: Update the customer gateway ASN \(required when the new gateway has a different ASN from the old gateway\)](#step-update-customer-gateway-asn)

## Step 1: Create the transit gateway<a name="step-create-gateway"></a>

Before you perform the migration to the new gateway, you must configure the new gateway\. For information about adding a virtual private gateway, see [Create a virtual private gateway](SetUpVPNConnections.md#vpn-create-vpg)\. For more information about adding a transit gateway, see [Create a transit gateway](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-transit-gateways.html#create-tgw) in *Amazon VPC Transit Gateways*\.

If the new target gateway is a transit gateway, attach the VPCs to the transit gateway\. For information about VPC attachments, see [Transit gateway attachments to a VPC](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-vpc-attachments.html) in *Amazon VPC Transit Gateways*\.

When you modify the target from a virtual private gateway to a transit gateway, you can optionally set the transit gateway ASN to be the same value as the virtual private gateway ASN\. If you choose to have a different ASN, then you must set the ASN on your customer gateway device to the transit gateway ASN\. For more information, see [Step 6: Update the customer gateway ASN \(required when the new gateway has a different ASN from the old gateway\)](#step-update-customer-gateway-asn)\.

## Step 2: Delete your static routes \(required for a static VPN connection migrating to a transit gateway\)<a name="step-update-staic-route"></a>

This step is required when you migrate from a virtual private gateway with static routes to a transit gateway\. 

You must delete the static routes before you migrate to the new gateway\.

**Tip**  
Keep a copy of the static route before you delete it\. You will need to add back these routes to the transit gateway after the VPN connection migration is complete\.

**To delete a route from a route table**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Route Tables**, and then select the route table\.

1. In the **Routes** tab, choose **Edit**, and then choose **Remove** for the static route to the virtual private gateway\.

1. Choose **Save** when you are done\.

## Step 3: Migrate to a new gateway<a name="step-migrate-gateway"></a>

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Site\-to\-Site VPN Connections**\.

1. Select the Site\-to\-Site VPN connection and choose **Actions**, **Modify VPN Connection**\.

1. Under **Change Target**, do the following:

   1. For **Target Type**, choose the gateway type\.

   1. Configure the connection target:

      \[Virtual private gateway\] For **Target VPN Gateway ID**, choose the virtual private gateway ID\.

      \[Transit Gateway\] For **Target transit gateway ID**, choose the transit gateway ID\.

1. Choose **Save**\.

**To modify a Site\-to\-Site VPN connection using the command line or API**
+ [ModifyVpnConnection](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_ModifyVpnConnection.html) \(Amazon EC2 Query API\)
+ [modify\-vpn\-connection](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-vpn-connection.html) \(AWS CLI\)

## Step 4: Update VPC route tables<a name="step-update-routing"></a>

After you migrate to the new gateway, you might need to modify your VPC route table\. The following table provides information about the actions you need to take\. For information about updating VPC route tables, see [Route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html) in the *Amazon VPC User Guide*\.


**VPN gateway target modification required VPC route table updates**  

| Existing gateway  | New gateway | VPC route table change | 
| --- | --- | --- | 
| Virtual private gateway with propagated routes | Transit gateway | Add a route that points to the transit gateway ID\. | 
| Virtual private gateway with propagated routes | Virtual private gateway with propagated routes | There is no action required\. | 
| Virtual gateway with propagated routes | Virtual private gateway with static route | Add an entry that contains the new virtual private gateway ID\. | 
| Virtual gateway with static routes | Transit gateway | Update the VPC route table and change the entry that contains to the virtual private gateway ID to the transit gateway ID\. | 
| Virtual gateway with static routes | Virtual private gateway with static routes | Update the entry that points to the virtual private gateway ID to be the new virtual private gateway ID\. | 
| Virtual gateway with static routes | Virtual private gateway with propagated routes | Delete the entry that contains the virtual private gateway ID\. | 
| Transit Gateway | Virtual private gateway with static routes | Update the entry that contains the transit gateway to the virtual private gateway ID\. | 
| Transit Gateway | Virtual private gateway with propagated routes | Delete the entry that contains the transit gateway ID\. | 
| Transit Gateway | Transit Gateway | Update the entry that contains the transit gateway ID to the new transit gateway ID\. | 

## Step 5: Update the transit gateway routing \(required when the new gateway is a transit gateway\)<a name="step-update-transit-gateway-routing"></a>

 When the new gateway is a transit gateway, modify the transit gateway route table to allow traffic between the VPC and the Site\-to\-Site VPN\. For information about transit gateway routing, see [Transit gateway route tables](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-route-tables.html) in the *Amazon VPC Transit Gateways*\.

**Important**  
 If you deleted VPN static routes, you must add the static routes to the transit gateway route table\.

## Step 6: Update the customer gateway ASN \(required when the new gateway has a different ASN from the old gateway\)<a name="step-update-customer-gateway-asn"></a>

 When the new gateway has a different ASN from the old gateway, you must update the ASN on your customer gateway device to point to the new ASN\. 