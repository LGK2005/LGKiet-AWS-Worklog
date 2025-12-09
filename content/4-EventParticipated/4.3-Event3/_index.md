---
title: "Event 3"
date: "2025-11-17"
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Summary Report: “AWS Cloud Mastery Series #2 – DevOps on AWS”

### Event Objectives

- Introduce core AWS DevOps services and CI/CD pipelines.
- Explain Infrastructure as Code (IaC) concepts and common tooling.
- Cover containerization options on AWS.
- Demonstrate how to achieve monitoring and observability with AWS services.

### Speakers

- **Truong Quang Tinh** – AWS Community Builder, Platform Engineer – TymeX  
- **Bao Huynh** – AWS Community Builder  
- **Nguyen Khanh Phuc Thinh** – AWS Community Builder  
- **Tran Dai Vi** – AWS Community Builder  
- **Huynh Hoang Long** – AWS Community Builder  
- **Pham Hoang Quy** – AWS Community Builder  
- **Nghiem Le** – AWS Community Builder  
- **Dinh Le Hoang Anh** – Cloud Engineer Trainee, First Cloud AI Journey  

### Key Highlights

## DevOps Mindset

**- Culture:** Emphasis on collaboration across teams, high level of automation, continuous learning, and making decisions based on measurable outcomes.

**- DevOps Roles:** Typical roles in a modern DevOps organization include DevOps Engineer, Cloud Engineer, Platform Engineer, and Site Reliability Engineer (SRE), each focusing on reliability, delivery speed, and operational excellence.

**- Success Metrics:**

- Healthy, reliable deployments.
- Improved delivery agility and reduced lead time for changes.
- Overall system stability and resilience.
- Better end-user experience and performance.
- Clear evidence that technology investments provide business value.

| DO                            | DON'T                            |
| :---------------------------- | :-------------------------------- |
| Start with fundamentals       | Stay in tutorial hell             |
| Learn by building real projects | Copy-paste blindly              |
| Document everything           | Compare your progress to others   |
| Master one thing at a time    | Give up after failures            |
| Improve soft skills           |                                  |

**- Continuous Integration:**  
Developers integrate changes frequently into a shared main branch, with automated builds and tests ensuring each commit keeps the codebase in a releasable state and enabling continuous delivery or deployment.

---

## Infrastructure as Code (IaC)

**- Benefits:**  
IaC brings automation, repeatability, and scalability to infrastructure management. Teams can version-control environments, spin them up or tear them down on demand, and collaborate on infrastructure changes as code instead of relying on manual ClickOps.

### AWS CloudFormation

AWS’s native IaC service that uses JSON/YAML templates to describe and provision full stacks of AWS resources in a predictable and auditable way.

**- Stack:**  
A stack is a collection of AWS resources defined in a single template. Operations like create, update, and delete are executed at the stack level, making complex environments easier to manage.

**- CloudFormation Template:**  
A declarative YAML/JSON file that defines resources, their configuration, and relationships. It acts as a blueprint that can be reused across environments (dev, test, prod).

**- How it works (high level):**  

1. Write a template that describes the target infrastructure.  
2. Store it locally or in S3 and create a stack from it.  
3. CloudFormation provisions, updates, or deletes resources to match the template.  

**- Drift Detection:**  
CloudFormation can detect configuration drift when resources have been changed outside of IaC. This helps teams either reconcile the template with reality or roll back unauthorized changes.

### AWS Cloud Development Kit (CDK)

An open-source framework that lets you define CloudFormation stacks using real programming languages (TypeScript/JavaScript, Python, Java, C#/.NET, Go).

**- Construct:**  
Constructs are the building blocks in CDK and represent one or more resources plus configuration. Three construct levels were explained:

- **L1 constructs:** Low-level, map 1:1 to CloudFormation resources for maximum control.  
- **L2 constructs:** Higher-level, intent-based APIs that wrap common best practices and sensible defaults.  
- **L3 constructs:** Opinionated “patterns” that compose multiple resources into complete architectures (for example, an API Gateway + Lambda + DynamoDB stack).

### AWS Amplify

A platform focused on building and deploying web and mobile applications. Amplify uses CloudFormation behind the scenes to provision backend and hosting infrastructure, providing a higher-level developer experience with CLI and console workflows.

### Terraform

A popular multi-cloud IaC tool where infrastructure is described in HashiCorp Configuration Language (HCL) and then planned and applied against one or more providers (AWS, Azure, GCP, etc.).

**- Strengths:**  

- Multi-cloud support with a consistent language and workflow.  
- State tracking, enabling Terraform to understand changes between current and desired infrastructure definitions.

### How to Choose IaC Tools?

**- Criteria to consider:**

- Are you targeting a single cloud (e.g., AWS-only) or multiple providers?  
- Is your primary role closer to application development or platform/ops?  
- How well does your chosen cloud and its ecosystem support the tool (docs, examples, official modules, support)?

---

## Container Services on AWS

### Dockerfile

A Dockerfile describes how to build a container image: base image, dependencies, build steps, environment variables, and entrypoint. This ensures the application behaves consistently across environments that support Docker.

**- Images:**  
Images are immutable blueprints built from a Dockerfile via layered file systems. They are used to run containers consistently in development, staging, and production.

**- Workflow:**  
Write Dockerfile → build Docker image → run containers locally or in AWS → push the image to a registry such as Amazon ECR or Docker Hub.

### Amazon ECR

A fully managed, secure, and scalable private container registry on AWS where teams can store, manage, and share container images.

**- Key features:**

- Image scanning to detect vulnerabilities.  
- Immutable tags to avoid accidental overwrites.  
- Lifecycle policies to clean up old images.  
- Encryption and IAM integration for secure access.

### Container Orchestration

Container orchestrators manage container scheduling, scaling, and lifecycle:

- Restart failed containers.  
- Scale out or in based on traffic or metrics.  
- Balance traffic and place workloads across compute capacity efficiently.

### Kubernetes (K8s)

Open source container orchestration platform that automates deployment, scaling, and self-healing of containerized applications.

**- Components:**

- **Master (Control Plane):** Manages the cluster state, scheduling, and API.  
- **Worker Nodes:** Run application workloads inside pods.  
- **Pods:** Smallest deployable unit, typically one or a small set of tightly coupled containers.  
- **Services:** Stable endpoints and load balancing over sets of pods.

### ECS vs EKS

| Feature                  | Amazon ECS (Elastic Container Service)                      | Amazon EKS (Elastic Kubernetes Service)                          |
| :----------------------- | :---------------------------------------------------------- | :---------------------------------------------------------------- |
| **Core Technology**      | AWS-native container orchestration                          | Managed Kubernetes (open-source standard)                         |
| **Complexity**           | Simpler and easier to operate                               | More flexible but higher operational complexity                   |
| **Knowledge Required**   | No Kubernetes knowledge required                            | Requires Kubernetes concepts (pods, deployments, services, etc.)  |
| **AWS Integration**      | Deep integration with AWS services                          | Standard Kubernetes integration, more ecosystem tooling           |
| **Use Case / Benefits**  | Fast deployments, low ops overhead                          | Multi-cluster, portability, more customization                    |
| **Ecosystem / Community**| AWS-native tooling and community                            | Large Kubernetes ecosystem and community                          |
| **Summary**              | Great when you want managed, AWS-centric container platform | Great when you need Kubernetes features or portability            |

### App Runner

A fully managed service suitable for quickly deploying containerized web applications and APIs from source or container registries, ideal for smaller teams or workloads that want minimal infrastructure management.

---

## Monitoring & Observability

### CloudWatch

- Central service to monitor AWS resources and applications in near real-time.  
- Provides metrics, logs, alarms, and dashboards to improve reliability and cost visibility.  
- Integrates with other services (e.g., Auto Scaling, EventBridge) for automated responses.

**- CloudWatch Metrics:**  
Metrics capture performance and health data from AWS services and on-prem workloads (with the CloudWatch Agent), and they can be wired into alarms, scaling policies, and DevOps workflows.

### AWS X-Ray

**- Distributed Tracing:**  
X-Ray traces requests across microservices, visualizing call graphs and latency between components. This helps identify slow or failing segments in complex architectures.

**- Performance Insight:**  
Used for root-cause analysis of performance issues and errors, highlighting where time is spent and where failures occur, and providing support for real user monitoring scenarios.

---

### Event Experience

This event was especially relevant to our project because it aligned with our plan to move from ClickOps to IaC with AWS CDK, improving maintainability, repeatability, and collaboration around infrastructure. The additional details on CloudWatch and X-Ray also supported our monitoring and data visibility requirements.

The speakers also addressed several of our team’s questions:

- **Q:** Our project is currently ClickOps-based, and we want to migrate to CDK. Is there a tool that can scan existing infrastructure and convert it to CDK/CloudFormation?  
  **A:** There is currently no tool that can reliably reverse-engineer full IaC from an existing environment, so the recommended path is to rebuild the infrastructure using IaC and gradually align the real environment with code.

- **Q:** X-Ray tracing looks similar to CloudTrail. How do they differ?  
  **A:** X-Ray focuses on tracing application requests and service-to-service calls for performance and debugging, while CloudTrail focuses on auditing API calls and user/role activity in the AWS account.

- **Q:** Our project is built around GuardDuty findings. Any suggestions for reliably generating findings for demo scenarios?  
  **A:** Port scanning is one way to trigger findings, and GuardDuty can also be configured with custom threat lists (malicious IPs/domains) to generate alerts when related activity is detected.

This was also the first time some speakers presented these topics:

- The DevOps and IaC sections were delivered clearly and structured well.  
- The Monitoring & Observability part showed some understandable nervousness but still provided useful, practical insight.

#### Some event photos

![Group picture during the event taken by speaker Tran Dai Vi](/images/4-Event/CM2GroupPic.jpg)
