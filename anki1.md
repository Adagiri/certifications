What are VPC Flow Logs and what interfaces do they monitor?	VPC Flow Logs capture information about IP traffic entering and leaving network interfaces in your VPC. They monitor traffic at three interface levels: VPC, Subnet, and ENI (Elastic Network Interface).

What are the main purposes of VPC Flow Logs?	VPC Flow Logs help with troubleshooting and monitoring connectivity issues, diagnosing overly restrictive security group rules, monitoring traffic reaching instances, and determining traffic direction to/from network interfaces.

Where can VPC Flow Logs data be sent for storage and analysis?	Flow log data can be published to three destinations: Amazon S3, Amazon CloudWatch Logs, or Amazon Kinesis Data Firehose.

Besides user-created interfaces, what AWS managed services can generate VPC Flow Logs?	AWS managed interfaces that generate flow logs include: ELB (Elastic Load Balancer), ElastiCache, Amazon WorkSpaces, Transit Gateway, NAT Gateway, and RDS.

What is the key difference between Security Groups and NACLs in terms of statefulness?	Security Groups are stateful - if inbound traffic is allowed, outbound traffic is automatically allowed. NACLs are stateless - even if inbound traffic is allowed, outbound traffic may be rejected independently.

What tools can be used to query and analyze VPC Flow Logs?	VPC Flow Logs can be queried using Amazon Athena (for logs stored in S3) or CloudWatch Logs Insights (for logs stored in CloudWatch Logs).

What does the "Action" field in VPC Flow Logs indicate?	The Action field indicates the success (ACCEPT) or failure (REJECT) of the request due to Security Group or NACL rules.

Which VPC Flow Log fields are most useful for troubleshooting network issues?	Key troubleshooting fields include: srcport & dstport (for problematic ports), srcaddr & dstaddr (for problematic IP addresses), and the action field (for identifying blocked traffic).

What types of traffic related to DNS and instance metadata are NOT captured by VPC Flow Logs?	VPC Flow Logs do NOT capture: traffic to Amazon DNS servers (including private hosted zones), traffic to/from 169.254.169.254 (instance metadata), and traffic to/from 169.254.169.123 (Amazon Time Sync Service).

What types of system and infrastructure traffic are NOT captured by VPC Flow Logs?	VPC Flow Logs do NOT capture: DHCP traffic, Windows license activation traffic, traffic to VPC router reserved IP addresses, and mirrored traffic (only mirrored target traffic is captured).

What specific load balancer traffic is NOT captured by VPC Flow Logs?	Traffic between VPC endpoint ENIs and Network Load Balancer ENIs is NOT captured by VPC Flow Logs.

Do VPC Flow Logs affect network performance?	No, VPC Flow Logs are collected outside the path of network traffic and do not affect network throughput or latency.

What type of data do VPC Flow Logs capture?	VPC Flow Logs capture metadata about IP traffic (source/destination IPs, ports, protocols, packet counts, timestamps, actions) but do NOT capture the actual packet payload or content.

How frequently are VPC Flow Logs aggregated and when do they become available?	Flow logs are typically aggregated every 10 minutes (1 minute for Nitro-based instances). Logs become visible approximately 15 minutes after creating a flow log.

How does the stateful nature of Security Groups vs stateless nature of NACLs affect troubleshooting with VPC Flow Logs?	With stateful Security Groups, you'll see bidirectional traffic if one direction is allowed. With stateless NACLs, you must explicitly check both inbound and outbound rules as each direction is evaluated independently, which may result in asymmetric ACCEPT/REJECT patterns in the logs.

What are AWS security breaches?	Incidents where sensitive data stored on AWS cloud infrastructure gets compromised, typically involving customer misconfigurations rather than AWS infrastructure vulnerabilities.

What was the Capital One breach (2019)?	Paige Thompson exploited a firewall misconfiguration to steal 100+ million credit applications from misconfigured AWS S3 buckets. Breach lasted 4 months undetected.

What are the most common AWS attack vectors in 2025?	Weak or No Credentials (45.7%), Misconfiguration (34.3%), and API/UI compromise (17.1%).

What is the shared responsibility model?	AWS secures the underlying infrastructure, customers secure their data, applications, and configurations.

What are the 4 core IAM security principles?	1) No root account usage 2) MFA enforcement 3) Least privilege principle 4) Regular access reviews

What's the difference between AWS Secrets Manager and Parameter Store?	Secrets Manager: $0.40/month per secret, for sensitive data like passwords. Parameter Store: Free for standard parameters, for configuration values like URLs.

How do applications typically access secrets and configs?	Fetch once at server startup from Secrets Manager/Parameter Store → Load into environment variables → App reads from environment variables during runtime.

What happens when you update a secret or config?	Applications don't get auto-notified. They use cached values until restart. Solutions: manual restart, polling, or event-driven updates.

What is the cost of basic data protection (encryption at rest)?	$0 - SSE-S3 encryption for S3, RDS encryption, and EBS encryption have no additional cost.

What are the three types of AWS encryption?	SSE-S3 (free), SSE-KMS ($1/month per key), and Customer-managed keys (highest control, highest cost).

What does S3 encryption protect against?	Physical theft of AWS storage devices, rogue AWS employees, but NOT public bucket misconfigurations or data in transit.

What is encryption in transit?	HTTPS/TLS for web traffic, SSL connections for databases, and encrypted API calls between services.

What's the difference between SSL, TLS, and HTTPS?	SSL is outdated (deprecated 2015), TLS is the modern encryption protocol, HTTPS is HTTP over TLS. People still say "SSL" out of habit.

What are the 4 network security components?	1) VPC with private/public subnets 2) Security Groups (instance-level firewall) 3) NACLs (subnet-level firewall) 4) No direct internet access for private resources.

What should go in public vs private subnets?	Public: Web servers, load balancers. Private: Databases, internal APIs, cache servers.

What are Security Groups?	Instance-level firewalls with default deny policy. Example: Allow HTTP(80) from anywhere, MySQL(3306) from web servers only.

What is CloudTrail and what does it cost?	Records all AWS API calls (who did what, when). Costs ~$2 per 100k events, typically $5-15/month for startups.

What is VPC Flow Logs and how is it different from CloudTrail?	Records network traffic (IP addresses, ports, bytes). CloudTrail = API calls, VPC Flow Logs = network packets. Costs ~$0.50/GB.

Does CloudTrail send automatic alarms?	No. CloudTrail only stores logs. You must configure CloudWatch Alarms on CloudTrail logs for automatic notifications.

What are the top 3 monitoring services for startups?	1) CloudWatch Alarms ($2-5/month) 2) CloudTrail ($5-15/month) 3) GuardDuty ($20-50/month).

What is GuardDuty?	AI-powered threat detection service that identifies malware, suspicious IPs, and brute force attacks automatically.

What are dependencies in software?	External libraries/packages your application uses (express, lodash for Node.js; requests, pandas for Python).

Why are dependency updates critical?	Old versions have known security vulnerabilities that hackers actively exploit. Examples: Log4j vulnerability, Equifax breach via outdated Apache Struts.

What are examples of configurations vs secrets?	Configurations (public): Database URLs, feature flags, API endpoints. Secrets (private): Passwords, API keys, encryption keys.

How much do 10 secrets cost in AWS?	Storage: 10 × $0.40 = $4/month. API calls: ~$0.0005 (negligible). Total: ~$4/month.

What compliance frameworks require encryption?	HIPAA (healthcare), SOC2 (B2B SaaS), GDPR (any EU users). Basic AWS encryption covers most requirements.

Who needs HIPAA compliance?	Healthcare apps, telemedicine platforms, fitness apps storing health data, medical device companies.

Who needs SOC2 compliance?	SaaS companies serving enterprises, financial services, cloud storage providers, any B2B handling sensitive data.

Who needs GDPR compliance?	ANY company with EU users/customers, including small startups with even one EU visitor.

What does full compliance cost annually?	$10K-50K/year including audits ($5K-15K), legal fees ($200-500/hour), security tools, and penetration testing.

What is SQL injection?	Malicious code injected into database queries via user input. Example: User enters "'; DROP TABLE users; --" in login form.

How does SQL injection work in cloud applications?	1) User sends malicious input via web form 2) App builds vulnerable query with user input 3) Database executes malicious commands.

How do you prevent SQL injection?	Use parameterized queries instead of string concatenation. Example: cursor.execute("SELECT * WHERE email = %s", (user_input,))

Can AWS storage devices be physically stolen?	Yes, through data center breaches, insider threats, improper disposal of decommissioned drives, or natural disasters.

If someone dumps an encrypted RDS database, is the dump encrypted?	No. The attacker gets decrypted data. AWS encryption only protects storage, not extracted data.

What's the environment separation strategy?	Separate AWS accounts for Production and Staging, cross-account role assumptions, environment-specific IAM policies.

What essential services should be set up initially?	Database (RDS/DynamoDB), servers (EC2/ECS), storage (S3), CDN (CloudFront), and budget monitoring.

What are the 8 backup and recovery essentials?	Automated RDS backups (7-day retention), S3 versioning, cross-region backup for critical data, quarterly recovery testing.

What is the 5-day startup package delivery process?	Day 1: Discovery & planning call. Days 2-4: Complete setup (landing page, AWS environments, security, CI/CD). Day 5: Handoff & documentation.

What security guardrails should be implemented day one?	Multi-factor authentication, least privilege access, security monitoring, compliance-ready configurations.

What's included in the Complete Startup Package?	Professional landing page, 2 AWS environments, $1000 AWS credits application, professional email, CI/CD workflows, comprehensive documentation.

How often should security reviews be conducted?	Daily: Check CloudWatch alarms. Weekly: Review IAM access logs. Monthly: Remove unused users, rotate keys, test backups.

What are red flags to monitor immediately?	Root account usage, failed logins >5/hour, unusual API calls outside business hours, large data transfers, new IAM users/roles.

What's the cost difference between SSE-S3 and SSE-KMS?	SSE-S3: Free. SSE-KMS: $1/month per key + $0.03 per 10k requests. Use SSE-S3 unless you need custom key management.

What happens when you download a file from an encrypted S3 bucket?	AWS automatically decrypts it before sending. User receives plain, readable file. Encryption only protects storage, not transmission.

What's the total monthly cost for basic startup monitoring?	CloudWatch Alarms ($2-5) + CloudTrail ($5-15) + GuardDuty ($20-50) = $25-70/month total.

How do you implement encryption in Infrastructure as Code?	Terraform example: aws_s3_bucket_server_side_encryption_configuration with sse_algorithm = "AES256", aws_db_instance with storage_encrypted = true.

What's the performance impact of encryption?	~1-2% overhead, handled by AWS hardware acceleration. Unnoticeable for most applications.

What are the 4 stages of implementing security for startups?	Week 1: IAM setup, MFA, basic monitoring. Week 2: Network security, encryption. Week 3: Secrets management, logging. Week 4: Backup strategy, documentation.

# AWS Global Infrastructure & History - Anki Cards

When was AWS first launched internally at Amazon and what was the motivation?	AWS was launched internally in 2002 at amazon.com because they realized that IT departments could be externalized. Amazon's infrastructure was one of their core strengths and they thought they could do IT for other people.

What was AWS's first public offering and when was it launched?	SQS (Simple Queue Service) was AWS's first public offering, launched in 2004.

What three services did AWS launch when they expanded their offering in 2006?	In 2006, AWS relaunched with SQS, S3 (Simple Storage Service), and EC2 (Elastic Compute Cloud).

What is AWS's current market position according to Gartner's Magic Quadrant?	AWS is a leader in Gartner's Magic Quadrant and has been the case for many years. As of 2023, AWS has $90 billion in revenue and accounts for about 31% of the market in Q1 2024, with Microsoft being second at 25%.

How long has AWS been a pioneer and leader in the cloud market?	AWS has been a pioneer and leader of the cloud market for 13 consecutive years and has over 1 million active users.

What are the main components of AWS global infrastructure?	AWS global infrastructure consists of: AWS Regions, Availability Zones, Data Centers, Edge Locations, and Points of Presence.

What is an AWS Region?	An AWS Region is a cluster of data centers located in a specific geographic area (e.g., Ohio, Singapore, Sydney, Tokyo). Regions have names like US-East-1 or EU-West-3, and most AWS services are region-scoped.

What are the four key factors to consider when choosing an AWS Region?	1) Compliance (government data locality requirements), 2) Latency (deploy close to your users), 3) Service availability (not all regions have all services), 4) Pricing (varies between regions).

How many Availability Zones does each AWS Region typically have?	Each AWS Region usually has 3 Availability Zones (minimum is 3, maximum is 6, but typically 3).

What is an Availability Zone in AWS?	An Availability Zone is one or more discrete data centers with redundant power, networking, and connectivity. They are separate from each other to be isolated from disasters but connected with high bandwidth, ultra-low latency networking.

Using Sydney region as an example, what would the Availability Zone naming convention look like?	Sydney region code is ap-southeast-2. The three Availability Zones would be: ap-southeast-2a, ap-southeast-2b, and ap-southeast-2c.

How many Points of Presence (Edge Locations) does AWS have globally?	AWS has more than 400 Points of Presence in 90 cities across 40 countries, used to deliver content to end users with the lowest latency possible.

What are examples of AWS Global Services?	Global services include: IAM (Identity and Access Management), Route 53, CloudFront, and WAF (Web Application Firewall).

What are examples of AWS Regional Services?	Regional services include: Amazon EC2, Elastic Beanstalk, Lambda, and Rekognition.

In the AWS Console, how can you identify if a service is global or regional?	Global services show "Global" in the top-right corner of the console. Regional services show the specific region name (e.g., "Ireland" or "Northern Virginia").

What are the two main ways to find AWS services in the console?	1) Click "Services" in the top-left to browse by alphabetical order or category, 2) Use the search bar to type the service name and get direct results.

Where can you check if a specific AWS service is available in your chosen region?	You can check the AWS Regional Services table (found by searching "AWS global infrastructure" or "AWS regional services") which lists services by region.

Why is it important to stay in the same region throughout an AWS course or project?	Because regional services show different resources based on the selected region. If you switch regions, you'll see a different view of your resources, which can be confusing during learning or development.

What should you consider when selecting a region for lowest latency?	Choose a region that is geographically close to you or your users. For example, if you're in Europe, choose Ireland (EU-West-1); if you're in Africa near Cape Town, choose the Cape Town region.

How are Availability Zones designed to handle disasters?	Availability Zones are isolated from each other so that if something happens to one AZ (like ap-southeast-2a), it won't cascade to other AZs (ap-southeast-2b or ap-southeast-2c) in the same region.

What happens when you use an AWS service in one region vs another region?	When you use a service in one region and try to use it in another region, it's like using the service for the first time - you won't see the same resources or configurations from the previous region.

What is the purpose of AWS's private network connecting regions?	AWS regions are connected through a private AWS network that enables secure, high-speed communication between regions for services that need to operate across multiple geographic locations.

What types of companies and applications use AWS?	AWS is used by diverse companies including Netflix, McDonald's, 21st Century Fox, Activision, Dropbox, Airbnb, and NASA. Use cases include enterprise IT, backup and storage, big data analytics, website hosting, mobile/social app backends, and gaming servers.

When was AWS first launched internally at Amazon and what was the motivation?	AWS was launched internally in 2002 at amazon.com because they realized that IT departments could be externalized. Amazon's infrastructure was one of their core strengths and they thought they could do IT for other people.
What was AWS's first public offering and when was it launched?	SQS (Simple Queue Service) was AWS's first public offering, launched in 2004.
What three services did AWS launch when they expanded their offering in 2006?	In 2006, AWS relaunched with SQS, S3 (Simple Storage Service), and EC2 (Elastic Compute Cloud).
What is AWS's current market position according to Gartner's Magic Quadrant?	AWS is a leader in Gartner's Magic Quadrant and has been the case for many years. As of 2023, AWS has $90 billion in revenue and accounts for about 31% of the market in Q1 2024, with Microsoft being second at 25%.
How long has AWS been a pioneer and leader in the cloud market?	AWS has been a pioneer and leader of the cloud market for 13 consecutive years and has over 1 million active users.
What are the main components of AWS global infrastructure?	AWS global infrastructure consists of: AWS Regions, Availability Zones, Data Centers, Edge Locations, and Points of Presence.
What is an AWS Region?	An AWS Region is a cluster of data centers located in a specific geographic area (e.g., Ohio, Singapore, Sydney, Tokyo). Regions have names like US-East-1 or EU-West-3, and most AWS services are region-scoped.
What are the four key factors to consider when choosing an AWS Region?	1) Compliance (government data locality requirements), 2) Latency (deploy close to your users), 3) Service availability (not all regions have all services), 4) Pricing (varies between regions).
How many Availability Zones does each AWS Region typically have?	Each AWS Region usually has 3 Availability Zones (minimum is 3, maximum is 6, but typically 3).
What is an Availability Zone in AWS?	An Availability Zone is one or more discrete data centers with redundant power, networking, and connectivity. They are separate from each other to be isolated from disasters but connected with high bandwidth, ultra-low latency networking.
Using Sydney region as an example, what would the Availability Zone naming convention look like?	Sydney region code is ap-southeast-2. The three Availability Zones would be: ap-southeast-2a, ap-southeast-2b, and ap-southeast-2c.
How many Points of Presence (Edge Locations) does AWS have globally?	AWS has more than 400 Points of Presence in 90 cities across 40 countries, used to deliver content to end users with the lowest latency possible.
What are examples of AWS Global Services?	Global services include: IAM (Identity and Access Management), Route 53, CloudFront, and WAF (Web Application Firewall).
What are examples of AWS Regional Services?	Regional services include: Amazon EC2, Elastic Beanstalk, Lambda, and Rekognition.
In the AWS Console, how can you identify if a service is global or regional?	Global services show "Global" in the top-right corner of the console. Regional services show the specific region name (e.g., "Ireland" or "Northern Virginia").
What are the two main ways to find AWS services in the console?	1) Click "Services" in the top-left to browse by alphabetical order or category, 2) Use the search bar to type the service name and get direct results.
Where can you check if a specific AWS service is available in your chosen region?	You can check the AWS Regional Services table (found by searching "AWS global infrastructure" or "AWS regional services") which lists services by region.
Why is it important to stay in the same region throughout an AWS course or project?	Because regional services show different resources based on the selected region. If you switch regions, you'll see a different view of your resources, which can be confusing during learning or development.
What should you consider when selecting a region for lowest latency?	Choose a region that is geographically close to you or your users. For example, if you're in Europe, choose Ireland (EU-West-1); if you're in Africa near Cape Town, choose the Cape Town region.
How are Availability Zones designed to handle disasters?	Availability Zones are isolated from each other so that if something happens to one AZ (like ap-southeast-2a), it won't cascade to other AZs (ap-southeast-2b or ap-southeast-2c) in the same region.
What happens when you use an AWS service in one region vs another region?	When you use a service in one region and try to use it in another region, it's like using the service for the first time - you won't see the same resources or configurations from the previous region.
What is the purpose of AWS's private network connecting regions?	AWS regions are connected through a private AWS network that enables secure, high-speed communication between regions for services that need to operate across multiple geographic locations.
What types of companies and applications use AWS?	AWS is used by diverse companies including Netflix, McDonald's, 21st Century Fox, Activision, Dropbox, Airbnb, and NASA. Use cases include enterprise IT, backup and storage, big data analytics, website hosting, mobile/social app backends, and gaming servers.
VPC Fundamentals
What is Amazon VPC?	VPC (Virtual Private Cloud) is your own private networking space inside AWS where you can launch machines. It's like having your own isolated network within the AWS cloud infrastructure.
How does traditional networking compare to VPC networking?	Traditional networking uses LANs connected by switches and routers. VPC translates this concept to AWS: your physical network becomes a VPC, LANs become subnets, and routers become route tables and gateways.
What is the scope of a VPC in relation to AWS infrastructure?	VPC scope is at the region level. When you create a VPC, you create it in a particular AWS region. You cannot have a single VPC span multiple regions.
Can you have multiple VPCs in the same region?	Yes, you can create multiple VPCs in the same region (default limit is 5, but can be increased). This allows you to isolate different applications or projects at the network layer.
What determines which AZ an EC2 instance is launched in?	EC2 instances are launched in specific AZs based on the subnet they're placed in. Each subnet is mapped to exactly one AZ, so the subnet choice determines the AZ.
What AWS services operate at the AZ level vs VPC level vs Regional level?	AZ level: EC2, RDS (must choose specific AZ via subnet). VPC level: Elastic Load Balancer (distributes across AZs within VPC). Regional level: S3 (not inside VPC, fully managed by AWS).
What AWS services have global scope?	Route 53 (DNS), IAM (Identity and Access Management), and billing services operate globally and don't sit in a particular AWS region.
What are the core components of a VPC?	1) IPv4/IPv6 address range (CIDR), 2) Subnets, 3) Route tables, 4) Internet Gateway, 5) Security Groups (instance-level firewall), 6) Network ACLs (subnet-level firewall), 7) DNS (Route 53 resolver).
What is CIDR notation and how do you calculate IP addresses?	CIDR (Classless Inter-Domain Routing) uses format like 10.0.0.0/16. The number after slash indicates network bits. Formula: Total IPs = 2^(32-prefix). Example: /16 = 2^(32-16) = 2^16 = 65,536 IPs.
How do you calculate the number of IP addresses in 192.168.0.0/24?	32 - 24 = 8 bits for hosts. 2^8 = 256 IP addresses (from 192.168.0.0 to 192.168.0.255).
How do you calculate IP addresses for 10.10.0.0/16?	32 - 16 = 16 bits for hosts. 2^16 = 65,536 IP addresses (from 10.10.0.0 to 10.10.255.255).
What does /32 and /0 mean in CIDR notation?	/32 = Single specific IP address (2^0 = 1 IP). /0 = All possible IP addresses (0.0.0.0/0 means "any IP address" - commonly used in security groups for internet access).
How do you subnet a VPC with 10.10.0.0/16 into /24 subnets?	Each /24 subnet has 256 IPs. First subnet: 10.10.0.0/24 (10.10.0.0-255), Second: 10.10.1.0/24 (10.10.1.0-255), Third: 10.10.2.0/24, etc. You can create 256 such subnets.
What's the smallest subnet size you can create in AWS?	/28 is the smallest subnet (2^4 = 16 IP addresses). However, AWS reserves 5 IP addresses, so only 11 are usable for EC2 instances.
Which 5 IP addresses are reserved in every subnet?	In subnet 10.0.0.0/24: 1) 10.0.0.0 (network address), 2) 10.0.0.1 (AWS VPC router), 3) 10.0.0.2 (DNS server), 4) 10.0.0.3 (future use), 5) 10.0.0.255 (broadcast - not supported).
How many usable IP addresses are in a /24 subnet?	256 total IPs - 5 reserved = 251 usable IP addresses for EC2 instances.
If you need 29 IP addresses in a subnet, what's the minimum CIDR prefix?	You need 29 + 5 reserved = 34 total IPs. /27 gives 32 IPs (insufficient). /26 gives 64 IPs (59 usable), so /26 is the minimum prefix needed.
What creates routing between subnets within a VPC?	Every VPC has a local router and main route table that automatically creates a route for the VPC CIDR range with target "local". This enables all subnets to communicate with each other by default.
Can EC2 instances in different subnets communicate by default?	Yes, all subnets within a VPC can communicate with each other by default due to the local route entry (VPC CIDR → local) that cannot be removed from route tables.
What's required for a VPC to connect to the internet?	1) Internet Gateway attached to VPC, 2) Public IP address on EC2 instance, 3) Route table entry directing internet traffic (0.0.0.0/0) to the Internet Gateway.
What makes a subnet "public" vs "private"?	Public subnet: Route table has entry for 0.0.0.0/0 → Internet Gateway. Private subnet: Route table has NO direct route to Internet Gateway. The distinction is purely about routing configuration.
What happens when you attach a custom route table to a subnet?	The subnet stops following the main route table and uses only the custom route table. Changes to the main route table no longer affect that subnet.
What's the difference between main route table and custom route table?	Main route table: Default for all subnets, cannot be deleted, applied to subnets without custom route tables. Custom route table: Created for specific subnets, provides granular control over routing per subnet.
What route entry always exists in every route table and cannot be removed?	The local route: VPC CIDR range → local router. This enables communication between all subnets within the VPC and cannot be deleted or modified.
In a VPC with public and private subnets, how do you control internet access?	Create separate route tables: Public subnet route table includes 0.0.0.0/0 → IGW. Private subnet route table only has the local route (no internet gateway route).

What are the three types of IP addresses available in AWS VPC?	Private IP address (for communication within VPC), Public IP address (for internet communication, changes on restart), and Elastic IP address (static public IP that doesn't change on restart).

How is a private IP address assigned to an EC2 instance?	Private IP is assigned from the subnet's IP address range when the instance is launched. You can specify which private IP to assign, or AWS will dynamically allocate an available IP from the subnet range.

What happens to public IP addresses when you stop and start an EC2 instance?	The public IP address is released and a new public IP is assigned from Amazon's pool of public IPs. The private IP address remains the same.

What is an Elastic IP and how does it differ from a public IP?	Elastic IP is a static public IP address allocated to your AWS account. Unlike public IP, it doesn't change when you restart the instance and remains with your account until you release it.

How are you billed for Elastic IP addresses?	You're not billed for Elastic IP while it's associated with a running EC2 instance. However, you're charged if the Elastic IP is allocated to your account but not associated with a running instance (to prevent IP address waste).

What is the key difference between IPv4 and IPv6 addresses in VPC?	IPv4 supports both private and public IP addresses within VPC. IPv6 addresses are all public and globally unique - there are no private IPv6 addresses in VPC.

What is dual stack mode in AWS VPC?	Dual stack mode allows AWS resources (like CloudFront, Load Balancers) to have both IPv4 and IPv6 addresses simultaneously, enabling communication over both protocols.

What are the CIDR ranges for IPv6 in VPC?	IPv6 has fixed CIDR ranges: /56 for VPC and /64 for subnets. You cannot choose custom ranges like you can with IPv4.

What are the two types of firewalls in AWS VPC?	Security Groups (operate at EC2 instance level) and Network ACLs (operate at subnet level). Traffic must pass through both firewalls.

What is the key difference between Security Groups and NACLs in terms of statefulness?	Security Groups are stateful - return traffic is automatically allowed. NACLs are stateless - you must explicitly allow both inbound and outbound traffic, including return traffic.

What does "stateful" mean for Security Groups?	If you allow inbound traffic on a port, the return/response traffic is automatically allowed outbound without needing an explicit outbound rule. The same applies in reverse for outbound traffic.

What does "stateless" mean for Network ACLs?	You must explicitly define both inbound and outbound rules. If you allow inbound traffic, you must also create an outbound rule for the return traffic using ephemeral port ranges (32768-65535).

What types of rules do Security Groups support vs Network ACLs?	Security Groups support only ALLOW rules. Network ACLs support both ALLOW and DENY rules, making them useful for blocking specific IP addresses.

How are rules evaluated in Security Groups vs Network ACLs?	Security Groups evaluate all rules and allow traffic if any rule matches. Network ACLs evaluate rules in numerical order (starting from lowest number) and stop at the first match.

What is the default behavior of a new Security Group?	By default, all inbound traffic is BLOCKED (no inbound rules). All outbound traffic is ALLOWED (allows all traffic to internet).

What is the default behavior of the default Network ACL?	The default Network ACL allows ALL inbound and outbound traffic. Custom Network ACLs deny all traffic by default until rules are added.

How can you reference other Security Groups in Security Group rules?	Instead of specifying IP addresses, you can reference another Security Group ID as the source. This allows traffic from any instance attached to that Security Group, making it easier to manage multi-tier applications.

What are ephemeral ports and why are they important for Network ACLs?	Ephemeral ports (32768-65535) are temporary ports used by clients to establish connections. For Network ACLs, you must allow outbound traffic to these port ranges for return traffic from inbound connections.

What is the benefit of using Network ACLs for security?	Network ACLs can DENY specific IP addresses at the subnet level, which Security Groups cannot do. This is useful for blocking malicious IPs while still allowing general access through Security Groups.

At which levels can you apply Security Groups vs Network ACLs?	Security Groups operate at the ENI (Elastic Network Interface) level, effectively instance-level. Network ACLs operate at the subnet level, affecting all instances in that subnet.

What is the default VPC and what are its characteristics?	Every AWS region has one default VPC created automatically with CIDR 172.31.0.0/16. It has one public subnet per AZ (/20 range), internet gateway attached, and main route table with internet route.

How many IP addresses are available in a default VPC subnet?	Each default subnet uses /20 CIDR, providing 4096 total IP addresses (4091 usable after AWS reserves 5 IPs).

Can you recreate a deleted default VPC?	Yes, you can recreate the default VPC through VPC console → Actions → Create Default VPC. It will restore the same CIDR ranges and configuration.

What makes a subnet "public" vs "private"?	Public subnet: Route table has entry for 0.0.0.0/0 pointing to Internet Gateway. Private subnet: Route table has NO route to Internet Gateway, only local VPC routes.

What are the basic steps to create a public subnet?	1) Create VPC with CIDR 2) Create Internet Gateway and attach to VPC 3) Create subnet 4) Create custom route table 5) Add internet route (0.0.0.0/0 → IGW) 6) Associate route table with subnet 7) Enable auto-assign public IP.

How do you connect to a private EC2 instance?	Connect to a public EC2 instance first (bastion/jump host), then SSH from there to the private instance using its private IP address. You need to copy your SSH key to the public instance or use SSH forwarding.

What connectivity does a private subnet instance have by default?	Private instances can communicate with other instances in the same VPC via local routes, but cannot access the internet (no outbound connectivity) unless NAT Gateway/Instance is configured.

# AWS CI/CD and CodeCommit - Anki Cards

What is CI/CD and what are its two main components?	CI/CD stands for Continuous Integration and Continuous Delivery. CI involves developers pushing code frequently to a central repository where it's automatically tested. CD involves automatically deploying tested code to application servers.

What are the main benefits of Continuous Integration (CI)?	CI helps find bugs early and fix them early, saves developer time by automating testing, enables faster code delivery, allows frequent deployments, and creates a healthier development cycle for happier developers.

What is the difference between Continuous Integration and Continuous Delivery?	Continuous Integration focuses on automatically testing code when it's pushed to a repository. Continuous Delivery focuses on automatically deploying tested code to application servers, enabling frequent releases instead of quarterly releases.

What are the core components of AWS CI/CD tech stack?	Code storage (CodeCommit, GitHub, Bitbucket), Build/Test phase (CodeBuild, Jenkins CI), Deploy phase (CodeDeploy, Elastic Beanstalk), and Orchestration (CodePipeline to coordinate the entire process).

What AWS services can CodeDeploy deploy to?	CodeDeploy can deploy to EC2 instances, on-premises servers, Lambda functions, and ECS (Elastic Container Service).

What is AWS CodeCommit and what is its current status as of July 2024?	CodeCommit was AWS's private Git repository service, but AWS discontinued it for new customers on July 25, 2024. AWS now recommends migrating to external Git solutions like GitHub or GitLab.

What are the main benefits of using version control with Git repositories?	Version control allows you to understand changes to code over time, see who committed what changes, track what was added or removed, collaborate with other developers, backup code in the cloud, and roll back to previous versions.

What were the key advantages of CodeCommit over third-party Git services?	CodeCommit offered private Git repositories within your AWS VPC, no size limits, full AWS management, high availability, increased security and compliance, encryption, IAM access control, and integration with AWS CI tools.

What authentication methods did CodeCommit support?	CodeCommit supported SSH keys (users configure SSH keys for Git repo access) and HTTPS (standard login and password access to Git repo).

How was security implemented in CodeCommit?	Security included IAM policies for user/role permissions, KMS encryption for stored code, encryption in transit via HTTPS/SSH protocols, and cross-account access through IAM roles with STS AssumeRole API.

What are the main differences between CodeCommit and GitHub?	Both support pull requests and CI integrations. CodeCommit had full AWS IAM integration and code stayed only on AWS, but had minimal UI. GitHub offers full-featured UI, supports GitHub users/SSO, and can be hosted on GitHub servers or enterprise on-premises.

How can you monitor CodeCommit events?	CodeCommit integrates with EventBridge to monitor events like pull request creation/status changes, new reference creation, and new comments. These events can trigger SNS, Lambda, or CodePipeline for automation.

How do you migrate a Git repository to CodeCommit?	First create your CodeCommit repository, then use 'git clone' to download the entire repository (files, commits, history) to your local computer, then push it to the new CodeCommit repository URL.

How can you achieve cross-region replication for CodeCommit?	When you push/create/delete branches, CodeCommit emits events (referenceCreated/referenceUpdated) to EventBridge. EventBridge can trigger an ECS task or CodeBuild task that does a git clone and replicates to the target repository in another region.

How do you implement branch security in CodeCommit?	Use IAM policies to restrict which branches users can push to. For example, deny policies can prevent junior developers from pushing to production branches while allowing senior developers full access. Resource policies are not supported.

What are pull request approval rules in CodeCommit?	Approval rules ensure code quality by requiring a certain number of people to review and approve pull requests before merging. You specify a pool of users who can approve and the minimum number of approvals needed.

How do you specify who can approve pull requests in CodeCommit?	You can specify IAM principal ARNs including users, federated users, IAM roles, and IAM groups. You can also use templates to automatically apply approval rules to pull requests in specific branches like dev and prod.

What happens when you grant push permission to a CodeCommit repository?	By default, users with push permission can contribute to any branch. To restrict access to specific branches, you must use IAM policies to deny access to certain branches based on user groups or roles.

What is the typical CI/CD flow from code to deployment?	Developer pushes code → Code repository (CodeCommit/GitHub) → Build server tests code (CodeBuild/Jenkins) → If tests pass, deployment server (CodeDeploy) deploys to application servers → New version goes live automatically.

Why does AWS recommend moving away from CodeCommit?	AWS discontinued CodeCommit for new customers in July 2024 and recommends migrating to external Git solutions like GitHub or GitLab for better long-term support and feature development.