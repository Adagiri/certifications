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