---
title: "Event 5"
date: "2025-11-29"
weight: 5
chapter: false
pre: " <b> 4.5. </b> "
---

# Summary Report: “AWS Cloud Mastery Series #3 – AWS Well-Architected: Security Pillar Workshop”

### Event Objectives

- AWS Cloud Club Introduction
- Pillar 1: Identity a& Access Management (IAM)
- Pillar 2: Detection & Continuous Monitoring
- Pillar 3: Infrastructure Protection
- Pillar 4: Data Protection
- Pillar 5: Incident Response

### Speakers

- **Le Vu Xuan An** – AWS Cloud Club Captain, HCMUTE  
- **Tran Duc Anh** – AWS Cloud Club Captain, SGU  
- **Tran Doan Cong Ly** – AWS Cloud Club Captain, PTIT  
- **Danh Hoang Hieu Nghi** – AWS Cloud Club Captain, HUFLIT  

- **Huynh Hoang Long** – AWS Community Builder  
- **Dinh Le Hoang Anh** – AWS Community Builder  

- **Nguyen Tuan Thinh** – Cloud Engineer Trainee  
- **Nguyen Do Thanh Dat** – Cloud Engineer Trainee  

- **Van Hoang Kha** – Cloud Security Engineer, AWS Community Builder  

- **Thinh Lam** – FCJ Member  
- **Viet Nguyen** – FCJ Member  

- **Mendel Grabski (Long)** – Ex-Head of Security & DevOps, Cloud Security Solution Architect  
- **Tinh Truong** – Platform Engineer at TymeX, AWS Community Builder  

---

### Key Highlights

## AWS Cloud Club

The session opened with an overview of AWS Cloud Clubs as student-led communities focused on cloud learning and peer support:

- Help students explore and build real cloud skills through events, labs, and workshops.  
- Provide opportunities to develop technical leadership (e.g., Cloud Club Captains).  
- Enable networking with peers, AWS professionals, and broader tech communities.  

Cloud Clubs taking part in FCJA include:

- AWS Cloud Club HCMUTE  
- AWS Cloud Club SGU  
- AWS Cloud Club PTIT  
- AWS Cloud Club HUFLIT  

Overall benefits emphasized: skill-building, a strong community, and career opportunities through visibility, mentorship, and practical experience.

---

## Identity & Access Management (IAM)

IAM was presented as the foundation of the Security Pillar, controlling who can do what in the AWS environment.

**Core responsibilities:**

- Manage identities (Users, Groups, Roles) and their permissions.  
- Enforce both authentication and authorization across accounts and workloads.  

**Best practices:**

- Apply the **least privilege** principle everywhere.  
- Avoid using long-lived **root access keys**; remove them after setting up initial configuration.  
- Minimize wildcards (`"*"`) in Actions/Resources; prefer scoped policies.  
- Use **Single Sign-On (SSO)** and AWS Organizations for centralized, multi-account access control.

**Advanced IAM controls:**

- **Service Control Policies (SCPs):**  
  - Set the maximum permission boundary at the organization or OU level.  
  - Filter what accounts *can* do but never grant permissions by themselves.

- **Permission boundaries:**  
  - Define the maximum effective permissions a user or role can receive from their identity policies.  
  - Useful for delegation scenarios where developers can create roles but must stay within guardrails.

**MFA comparison:**

| TOTP (Time-based One-Time Password)         | FIDO2 (Fast Identity Online 2)               |
| :------------------------------------------ | :------------------------------------------- |
| Uses a shared secret and 6-digit codes      | Uses public-key cryptography                 |
| Requires manual code entry                  | Typically a tap or biometric factor          |
| Free to use (authenticator apps)           | May involve hardware token cost              |
| Easier backups and recovery                 | Very strong security, strict/no recovery     |

**Credential rotation with Secrets Manager:**

- Use a rotation Lambda to implement the 4-step cycle: `createSecret → setSecret → testSecret → finishSecret`.  
- Schedule rotations (for example, every 7 days) and send events to EventBridge for coordination.  
- Rotate credentials without downtime and deprecate previous versions safely.

---

## Detection & Continuous Monitoring

The second focus area centered on building multi-layer visibility and automating responses.

**Multi-layer security visibility:**

- **Management events:** API calls and console actions across all accounts (via CloudTrail).  
- **Data events:** Fine-grained logging for S3 object access and Lambda invocations.  
- **Network activity:** VPC Flow Logs for traffic patterns, allowed/denied connections.  
- **Organization-wide coverage:** Aggregated logging across multiple accounts and regions.

**Alerting & automation with EventBridge:**

- CloudTrail events feed into EventBridge to support near real-time event-driven responses.  
- EventBridge rules route suspicious events to Lambda, SNS, SQS, or other targets.  
- Cross-account routing enables a central security account to orchestrate detection and response workflows.

**Detection-as-Code:**

- Use **CloudTrail Lake** or queryable logs to define reusable, SQL-like detection rules.  
- Store rules in version control and deploy them via CI/CD for consistency.  
- Set up organization-wide logging and detection using Infrastructure as Code so that all accounts adhere to the same baseline.

---

## Amazon GuardDuty

GuardDuty was highlighted as an always-on, managed threat detection service.

**Three main data sources:**

| Data Source           | What It Monitors                            | Example Finding                                           |
| :-------------------- | :------------------------------------------- | :-------------------------------------------------------- |
| CloudTrail events     | IAM actions and API calls                    | Someone disables logging or creates high-privilege roles. |
| VPC Flow Logs         | Network traffic to and from resources        | EC2 instance exfiltrating data to a command-and-control.  |
| DNS logs              | DNS queries from inside your environment     | Malware contacting suspicious or cryptomining domains.    |

**Extended protection plans:**

- **S3 Protection:** Detects unusual object access patterns and scans uploads for malware.  
- **EKS Protection:** Uses Kubernetes audit logs to detect abnormal cluster operations and correlates with S3 or other services.  
- **Malware Protection:** Triggers scans on EBS volumes when compromise is suspected.  
- **RDS Protection:** Monitors database login patterns to detect brute-force attempts.  
- **Lambda Protection:** Observes network behavior from Lambda functions to catch data exfiltration and beaconing.  
- **Runtime Monitoring:** With an agent on EC2/EKS/ECS Fargate, monitors processes, file activity, system calls, and privilege escalation attempts.

**Compliance and standards:**

GuardDuty and related services can help enforce:

- AWS Foundational Security Best Practices.  
- CIS AWS Foundations Benchmark.  
- PCI DSS, NIST and other frameworks (via Security Hub integration).

**Compliance-as-code setup:**

- Use CloudFormation (or other IaC) to deploy GuardDuty, Security Hub, and related configuration.  
- Security Hub runs checks against standard baselines across S3, EC2, RDS, etc.  
- Findings flow into a central account to support audits and continuous compliance.

---

## Network Security Controls

The network layer section organized threats and controls:

- **Attack vectors:**
  - Ingress attacks: DDoS, SQL injection, direct scanning.  
  - Egress attacks: Data exfiltration, DNS tunneling.  
  - Lateral movement: East-west traffic between resources after compromise.

**Core components:**

- **Security Groups (SGs):**  
  - Stateful, instance-level firewalls.  
  - Only allow rules; anything not explicitly allowed is denied.

- **Network ACLs (NACLs):**  
  - Stateless, subnet-level filters.  
  - Ordered rules that can explicitly ALLOW or DENY.  
  - Useful as an extra layer or for broad deny rules.

- **Transit Gateway SG referencing:**  
  - Allows referencing SGs from TGW-attached VPCs to simplify rule management for shared services.  

- **Route 53 Resolver:**  
  - Directs DNS queries to private hosted zones, VPC DNS, or the public internet, supporting split-horizon and centralized DNS patterns.

- **AWS Network Firewall:**  
  - Handles deep packet inspection, egress filtering, and segmentation.  
  - Can enforce domain or protocol-based rules and integrate with threat intel (e.g., GuardDuty findings) for automated blocking.

---

## Data Protection & Governance

**Encryption with KMS:**

- Data is encrypted using data keys, which are themselves wrapped by Customer Managed Keys (CMKs).  
- KMS policies and condition keys control when and by whom encryption/decryption operations can occur.

**Certificate management (ACM):**

- Issues free public certificates for many AWS-integrated endpoints.  
- Manages renewals automatically, typically 60 days before expiry.  
- DNS validation is recommended for automation and stability.

**Secrets management:**

- AWS Secrets Manager eliminates hardcoded secrets in apps and pipelines.  
- Supports automatic rotation via Lambda using the four-step cycle (create, set, test, finish).  

**Service-level security:**

- **S3 & DynamoDB APIs:**  
  - Must be accessed over HTTPS; bucket policies can enforce `aws:SecureTransport = true`.  
- **RDS:**  
  - Requires the client to trust AWS root CAs.  
  - SSL/TLS can be enforced (for example, `rds.force_ssl=1` on PostgreSQL).

---

## Incident Response & Prevention

**Prevention best practices:**

- Prefer temporary credentials (STS, IAM roles) over long-lived keys.  
- Do not expose S3 buckets or other services directly to the internet if unnecessary.  
- Place sensitive workloads in private subnets behind controlled entry points.  
- Use Infrastructure as Code for all environments, so rebuilds and audits are easier.  
- Introduce “double gates” for risky changes: code review plus pipeline approval.

**Incident response lifecycle:**

1. **Preparation:** Document runbooks, practice scenarios, define roles.  
2. **Detection & Analysis:** Use GuardDuty, Security Hub, and logs to identify and investigate incidents.  
3. **Containment:** Isolate resources, revoke or rotate credentials, restrict network access.  
4. **Eradication & Recovery:** Remove the root cause, rebuild or restore from known-good states.  
5. **Post-incident:** Conduct retrospectives, update runbooks, and strengthen controls.

---

### Event Experience

- The workshop was highly relevant to our team’s project on Automated Incident Response and Forensics, especially the parts on GuardDuty, detection pipelines, and incident handling patterns.  

- **Q:** Our project relies on GuardDuty findings for triggering automation, but we’re seeing up to ~5 minutes delay between an incident and the corresponding finding. Is there any way to significantly reduce this latency?  
  **A:** The 5-minute delay is generally expected, because GuardDuty needs time to analyze large volumes of signals and correlate them into high-confidence findings. To improve responsiveness, you can complement GuardDuty with third-party tools (for example, more real-time anomaly detection) and build additional detections around CloudTrail or other telemetry for earlier signals, then enrich with GuardDuty when it arrives.

- After the session, Mr. Mendel Grabski spent additional time discussing our project and offered guidance and support, which was valuable for shaping next steps and potential integrations.

#### Some event photos

![All Attendee Picture](/images/4-Event/Event6AttendeePic.jpg)  
_Picture of all attendees_

![Group Picture With Speaker Mendel Grabski and Speaker Van Hoang Kha](/images/4-Event/Event6PicturewithSpeakers.jpg)  
_Group picture with speakers Mendel Grabski and Van Hoang Kha_
