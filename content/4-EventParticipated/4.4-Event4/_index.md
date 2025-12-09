---
title: "Event 4"
date: "2025-11-19"
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# Summary Report: “Secure Your Applications: AWS Perimeter Protection Workshop”

### Event Objectives

- Introduce Amazon CloudFront as a foundation for perimeter protection.
- Explain AWS WAF and application-layer protection patterns.
- Hands-on: Optimize a web application using CloudFront.
- Hands-on: Secure an internet-facing web application with WAF.

### Speakers

- **Nguyen Gia Hung** – Head of Solution Architect  
- **Julian Ju** – Senior Edge Services Specialist Solutions Architect  
- **Kevin Lim** – Senior Edge Services Specialist GTM  

---

### Key Highlights

## Amazon CloudFront

### Customer security needs and positioning

Different customer profiles require different levels of protection:

- **Small website owners:** Need simple, cost-effective protection against basic threats.  
- **Business users:** Require broader coverage for DDoS, bots, and malicious traffic patterns.  
- **Scaling businesses / enterprises:** Need advanced configuration such as origin failover, origin offload, and tight integration with WAF and Shield.

CloudFront was positioned as the edge foundation that can cover all three, with predictable pricing, global presence, and tight integration with AWS security services.

### CloudFront Flat-Rate Pricing

CloudFront now provides flat-rate plans that bundle CDN, WAF, DDoS protection, DNS, and storage under a single monthly charge:

- **Plans:**
  - Free: 0 USD/month  
  - Pro: 15 USD/month  
  - Business: 200 USD/month  
  - Premium: 1000 USD/month  

- **Example usage envelopes:**
  - Free: 1M requests + 100 GB data  
  - Pro: 10M requests + 50 TB data  
  - Business: 125M requests + 50 TB data  
  - Premium: 500M requests + 50 TB data  

If you exceed the “soft limit”, CloudFront does not immediately charge overages; instead, performance can be throttled and alerts are raised so you can upgrade tiers or optimize usage.

### Perimeter protection with CloudFront

- **Distributed defense:** Attacks are absorbed at edge locations close to the attacker, rather than hitting your origin directly.  
- **AWS Shield Advanced integration:** Provides visibility into infrastructure-layer DDoS events and access to the Shield Response Team (SRT) 24/7.  
- **Volumetric attack mitigation:** STN proxy and global routing help mitigate SYN floods and other large-scale network attacks inline.

---

### Cost optimization with CloudFront

- **Free data transfer from AWS services to CloudFront:** Data going from many AWS origins to CloudFront typically has reduced or zero egress cost compared to direct internet egress.  
- **HTTP compression:** Object compression can significantly reduce payload sizes (often ~80%), improving performance and reducing bandwidth usage.  
- **TLS optimizations:**
  - Automatic TLS certificate issuance and renewal (ACM integration).  
  - Free HTTPS support with managed security policies for ciphers/TLS versions.  
  - Support for modern TLS (TLS 1.3, ECDSA certificates, and post-quantum–ready options).  
  - Automatic redirect from HTTP to HTTPS at the edge.

- **Upcoming mutual TLS:** Ability to require client certificates for mutual authentication at the edge.  
- **Origin cloaking:**
  - **Origin Access Control (OAC):** Signs requests to S3, Lambda Function URLs, and other origins with short-lived credentials, preventing direct public access.  
  - **Custom origins:** Restrict access to CloudFront IP ranges and require a custom secret header.

- **Content protection:**
  - Signed URLs / signed cookies to prevent direct hotlinking and copy-paste abuse.  

- **Caching & availability:**
  - Cache content using TTL and serve from edge to reduce origin load.  
  - Serve stale content when origin is slow or temporarily unavailable to improve perceived uptime.  
  - Built-in origin failover to secondary origins when the primary is unhealthy.  
  - Graceful failure with custom error pages and cached error responses.

---

### Performance enhancements with CloudFront

- **Multi-layer caching:** Uses edge locations, Regional Edge Caches, and Origin Shield to collapse requests and increase cache hit ratio.  
- **Connection optimizations:**
  - Multiplexed connections to download assets in parallel.  
  - Persistent connections to origins to avoid repeated TCP handshakes.  
- **AWS global backbone:** Traffic between edge and AWS origins stays on the AWS network, reducing latency and avoiding public internet congestion.  
- **Logic at the edge:**
  - CloudFront Functions and Lambda@Edge for redirects, URL rewrites, A/B testing, and geo/device-based routing.  
  - Implement rate limiting, API mocking, health checks, and custom error handling close to users.

---

### CloudFront common use cases

- **Static websites and assets:** High cache hit ratio yields large performance gains and lowers origin cost.  
- **Full website delivery:** Combine performance, security, and high availability for dynamic + static content.  
- **API acceleration:** Reuse long-lived connections and selectively cache responses to reduce latency.  
- **Media streaming:** Serve large audiences at low latency and cost with HLS/DASH streaming from the edge.  
- **Large file downloads:** Use range requests and edge caching for large binaries, often achieving significant cost savings.

### CloudFront best practices

- **End-to-end visibility:** Monitor real user metrics, internet-facing performance, and backend infrastructure.  
- **Maximize caching:** Normalize cache keys, cache dynamic responses when safe, and cache error pages appropriately.  
- **Block unwanted traffic:** Use WAF rules for malicious patterns, rate limiting, and API-level filtering.  
- **Offload logic to the edge:** Handle CORS, redirects, and some cookie/header manipulation without hitting origin.  
- **Automatic failover:** Combine Route 53 failover policies with CloudFront origin groups for resilient routing.

---

## AWS WAF & Application Protection

### Threats and business impact

Key categories of threats:

- Denial-of-service and resource exhaustion.  
- Exploits of application vulnerabilities (XSS, SQLi, path traversal, etc.).  
- Malicious or abusive bot traffic.

Consequences: data theft, credential compromise, spam/abuse, downtime, increased infrastructure cost, and reputational damage.

### Surge in bot activity

There is a noticeable growth in bot traffic, including AI-powered bots and scrapers, which makes detection and mitigation more important and more nuanced than before.

### Route 53 in perimeter protection

- Globally distributed DNS infrastructure.  
- Built-in DDoS resilience and high availability.  
- Often paired with CloudFront and WAF as the first entry point into the application stack.

### Infrastructure protection with AWS Shield

**At the edge:**

- SYN proxy and packet validation.  
- Filtering and scrubbing of volumetric attacks at scale.  
- Automated routing policies to steer around unhealthy paths.

**At the border:**

- Traffic filtering and resource-level detection.  
- Health-based detection and targeted mitigation for protected resources.

### Application-layer protection with AWS WAF

- WAF is typically placed in front of CloudFront distributions or ALBs.  
- Detects HTTP floods, known bad patterns, and suspicious IPs.  
- Provides bot control (common bots vs evasive bots) and fraud control add-ons.  

### Shield Advanced incident response

- Shield/Shield Advanced publishes metrics and alarms that can trigger incident workflows.  
- 24/7 access to the Shield Response Team for escalations.  
- Helps identify attack vectors, top contributors, and recommended mitigations.

### WAF configuration concepts

- **Rules and rule groups:** Compose managed and custom rules.  
- **Managed rules:** Prebuilt, maintained by AWS or partners (e.g., OWASP top 10).  
- **Custom rules:** Tailored to your application’s paths, headers, and specific patterns.  
- **COUNT mode first:** Start with counting to observe and tune rules before enforcing BLOCK to reduce false positives.  
- **Labels:** Tag requests with metadata from rules to drive more complex logic.  
- **Scope-down statements:** Narrow rules to specific paths or conditions to reduce cost and unwanted matches.

### Web ACL / Protection Pack

- A Web ACL is the main WAF configuration object: rules, rule groups, and default action (allow/block).  
- Associated to resources like CloudFront distributions, ALBs, or API Gateway.  
- Logging and sampling options help review matches and fine-tune rules.

### WAF rules and DDoS at application layer

- Rules inspect attributes (IP, headers, URI, body) and then decide: Allow, Block, Count, or custom response.  
- Rate-based rules track requests per IP (or other keys) and automatically throttle abusive traffic.  
- Application-layer DDoS protection leverages these rules for automatic mitigation of HTTP floods.

### WAF labels & bot types

- **Labels:** Added by managed or custom rules to mark things like geo, IP reputation, bot/fraud category, or rule match.  
- **Common bots:** Self-identifying bots (search engines, social platforms, libraries) that often follow standards. WAF can use signatures (headers, TLS fingerprint, IP reputation) to handle them.  
- **Evasive bots:** Scrapers, credential stuffing tools, scanners that mimic real users and hide behind common headers. Countermeasures include client interrogation and behavioral analysis per session.

---

## CloudFront Hands-on: Perimeter Protection

### S3 static origin vs S3 behind CloudFront

In the lab, we compared direct S3 website hosting with using CloudFront in front of S3:

- CloudFront served cached objects much faster than direct S3 access.  
- Global edge locations and the AWS backbone improved latency versus direct public internet paths.  
- Compression at the edge reduced object sizes, further improving load times.

---

## AWS WAF Hands-on: “Strengthen Your Web Application Defenses”

During the WAF workshop, we created and tested rules to block:

- Cross-Site Scripting (XSS).  
- SQL Injection.  
- Path traversal attacks.  
- Server-side request/asset abuse patterns.  
- Common and evasive bots.  
- API misuse.  
- A “mystery” test scenario using an encrypted header value that needed a custom rule to block.

---

### Event Experience

The workshop was very timely for our team, as we had just started configuring CloudFront the previous night. The flat-rate pricing model with included WAF is especially attractive for our use case because it simplifies both cost estimation and security setup.

The hands-on labs were engaging and practical, and Julian was particularly supportive throughout, frequently checking in and helping debug configuration issues. After the workshop, we had a chance to ask additional questions:

- **Q:** WAF can help control and block bot activity, but what about newer AI agents like Gemini’s browser integration, which acts through a visible browser rather than a headless client?  
  **A:** For Gemini-style agents, traffic is still ultimately API-driven and can often be identified and mitigated at the HTTP level. Some agents, like certain Claude-based accessibility tools, may look almost identical to a real user and are not necessarily harmful, so the risk profile is different and may not require aggressive blocking in the short term.

The event wrapped up with group photos and a Kahoot quiz; placing in the ranking and seeing a “full threat blocked” WAF dashboard during the lab gave a satisfying sense of end-to-end protection working as intended.

#### Some event photos

![Attendee](/images/4-Event/Event5AttendeePic.jpg)  
_Event attendee group picture_

![Full Threat Blocked](/images/4-Event/FullThreatBlockedWAFWorkshop.jpg)  
_Blocked all workshop threat scenarios during the WAF lab_
