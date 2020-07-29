# Internetwork traffic privacy<a name="internetwork-traffic-privacy"></a>

A Site\-to\-Site VPN connection privately connects your VPC to your on\-premises network\. Data that's transferred between your VPC and your network routes over an encrypted VPN connection to help maintain the confidentiality and integrity of the data in transit\. Amazon supports Internet Protocol security \(IPsec\) VPN connections\. IPsec is a protocol suite for securing IP communications by authenticating and encrypting each IP packet in a data stream\. 

Each Site\-to\-Site VPN connection consists of two encrypted IPsec VPN tunnels that link AWS and your network\. Traffic in each tunnel can be encrypted with AES128 or AES256 and use Diffie\-Hellman groups for key exchange, providing Perfect Forward Secrecy\. AWS authenticates with SHA1 or SHA2 hashing functions\. 

Instances in your VPC do not require a public IP address to connect to resources on the other side of your Site\-to\-Site VPN connection\. Instances can route their internet traffic through the Site\-to\-Site VPN connection to your on\-premises network\. They can then access the internet through your existing outbound traffic points and your network security and monitoring devices\.

See the following topics for more information:
+ [Tunnel options for your Site\-to\-Site VPN connection](VPNTunnels.md): Provides information about the IPsec and Internet Key Exchange \(IKE\) options that are available for each tunnel\.
+ [Site\-to\-Site VPN tunnel authentication options](vpn-tunnel-authentication-options.md): Provides information about the authentication options for your VPN tunnel endpoints\.
+ [Requirements for your customer gateway device](your-cgw.md#CGRequirements): Provides information about the requirements for the customer gateway device on your side of the VPN connection\.
+ [Providing secure communication between sites using VPN CloudHub](VPN_CloudHub.md): If you have multiple Site\-to\-Site VPN connections, you can provide secure communication between your on\-premises sites by using the AWS VPN CloudHub\. 