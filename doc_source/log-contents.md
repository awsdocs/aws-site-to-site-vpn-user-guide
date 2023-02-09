# Contents of Site\-to\-Site VPN logs<a name="log-contents"></a>

The following information is included in the Site\-to\-Site VPN tunnel activity log\.


| Field | Description | 
| --- | --- | 
|  VpnLogCreationTimestamp  |  Log creation timestamp in human readable format\.  | 
|  VpnConnectionId  |  The VPN connection identifier\.  | 
|  TunnelOutsideIPAddress  |  The external IP of the VPN tunnel that generated the log entry\.  | 
|  TunnelDPDEnabled  |  Dead Peer Detection Protocol Enabled Status \(True/False\)\.  | 
|  TunnelCGWNATTDetectionStatus  | NAT\-T detected on customer gateway device \(True/False\)\. | 
|  TunnelIKEPhase1State  | IKE Phase 1 Protocol State \(Established \| Rekeying \| Negotiating \| Down\)\. | 
| TunnelIKEPhase2State | IKE Phase 2 Protocol State \(Established \| Rekeying \| Negotiating \| Down\)\. | 
| VpnLogDetail | Verbose messages for IPsec, IKE and DPD protocols\. | 

**Topics**
+ [IKEv1 Error Messages](#sample-log-ikev1)
+ [IKEv2 Error Messages](#sample-log-ikev2)
+ [IKEv2 Negotiation Messages](#sample-log-ikev2-negotiation)

## IKEv1 Error Messages<a name="sample-log-ikev1"></a>


| Message | Explanation | 
| --- | --- | 
|  Peer is not responsive \- Declaring peer dead  |  Peer has not responded to DPD Messages, enforcing DPD time\-out action\.  | 
|  AWS tunnel payload decryption was unsuccessful due to invalid Pre\-shared Key  |  Same Pre\-Shared key needs to be configured on both IKE Peers\.  | 
|  No Proposal Match Found by AWS  |  Proposed Attributes for Phase 1 \(Encryption, Hashing and DH Group\) are not supported by AWS VPN Endpoint\. e\.g 3DES  | 
|  No Proposal Match Found\. Notifying with "No proposal chosen"  |  No Proposal Chosen error message is exchanged between Peers to inform that correct Proposals/Policies must be configured for phase 2 on IKE Peers\.  | 
|  AWS tunnel received DELETE for Phase 2 SA with SPI: xxxx  | CGW has sent the Delete\_SA message for Phase 2 | 
|  AWS tunnel received DELETE for IKE\_SA from CGW  | CGW has sent the Delete\_SA message for Phase 1 | 

## IKEv2 Error Messages<a name="sample-log-ikev2"></a>


| Message | Explanation | 
| --- | --- | 
|  AWS tunnel DPD timed out after \{retry\_count\} retransmits  |  Peer has not responded to DPD Messages, enforcing DPD time\-out action\.   | 
|  AWS tunnel received DELETE for IKE\_SA from CGW  |  Peer has sent the Delete\_SA message for Parent/IKE\_SA  | 
| AWS tunnel received DELETE for Phase 2 SA with SPI: xxxx | Peer has sent the Delete\_SA message for CHILD\_SA | 
|  AWS tunnel detected a \(CHILD\_REKEY\) collision as CHILD\_DELETE  |  CGW has sent the Delete\_SA message for the Active SA, which is being rekeyed\.  | 
|  AWS tunnel \(CHILD\_SA\) redundant SA is being deleted due to detected collision  | Due to Collision, If redundant SAs are generated, Peers will close redundant SA after matching the nonce values as per RFC | 
|  AWS tunnel Phase 2 was unable to establish while keeping Phase 1  | Peer was unable to establish CHILD\_SA due to negotiation error e\.g incorrect proposal\.  | 
| AWS: Traffic Selector: TS\_UNACCEPTABLE: received from responder | Peer has proposed Incorrect Traffic Selectors/Encryption Domain\. Peers should be configured with identical and correct CIDRs\. | 
| AWS tunnel is sending AUTHENTICATION\_FAILED as the response | Peer is unable to Authenticate the Peer by verifying IKE\_AUTH message's contents | 
| AWS tunnel detected a pre\-shared key mismatch with cgw: xxxx | Same Pre\-Shared key needs to be configured on both IKE Peers\. | 
| AWS tunnel Timeout: deleting un\-established Phase 1 IKE\_SA with cgw: xxxx | Deleting the half\-opened IKE\_SA as peer has not proceeded with negotiations | 
| No Proposal Match Found\. Notifying with "No proposal chosen" | No Proposal Chosen error message is exchanged between Peers to inform that correct Proposals must be configured on IKE Peers\. | 
| No Proposal Match Found by AWS | Proposed Attributes for Phase 1 \(Encryption, Hashing and DH Group\) are not supported by AWS VPN Endpoint\. e\.g 3DES | 

## IKEv2 Negotiation Messages<a name="sample-log-ikev2-negotiation"></a>


| Message | Explanation | 
| --- | --- | 
|  AWS tunnel processed request \(id=xxx\) for CREATE\_CHILD\_SA  |  AWS has received the CREATE\_CHILD\_SA request from CGW  | 
|  AWS tunnel is sending response \(id=xxx\) for CREATE\_CHILD\_SA  |  AWS is sending CREATE\_CHILD\_SA response to CGW  | 
| AWS tunnel is sending request \(id=xxx\) for CREATE\_CHILD\_SA | AWS is sending CREATE\_CHILD\_SA request to CGW | 
|  AWS tunnel processed response \(id=xxx\) for CREATE\_CHILD\_SA  |  AWS has received CREATE\_CHILD\_SA response form CGW  | 