# Security & Compliance

This is mainly a review of all the different security principles which have been
discussed in the prior sections of the course.

## Security Principles in GCP

**Seperation of Duties:**

* Don't place all organisation resources under a single project
* Principle 1: Different scope of access = different project
* Principle 2: Give fewest rights necessary to complete job

**Securely interact with Google Cloud Storage:**

* Three modes of access: IAM, ACLs and Signed URLs
  * IAM = Bucket Level access
  * ACLs / Signed URLs = Object Level access
* Signed URLs can be used for non GCP users (external user access)

**Penetration Testing:**

* Where we try and simulate an attack on the network / application infrastructure
  * Try to find vulnerabilities in design
* Choose correct environment to conduct pen test
  * Should be as close as possible to a real attack.
  * Attack publicly available applications

**Supporting Encrypted Communication:**

* Applies to secure communications (IoT devices, Chat apps)
* Use Public / Private Key format
  * IoT Core uses MQTT format
  * PKI is another populat format

## Network Design for Security & Isolation

VPCs are our way of providing network isolation. These can get complex in nature
but all follow a similar convention. The methods of isolation we have at our 
disposal include:

* Projects
* VPCs
* Firewalls

How are they structured?

* Organisations have many projects
* Projects can have many VPCs
* VPCs can span many projects via a Shared VPC
* VPCs can include many regions
* Regions can have one or more subnets

**Projects**

Projects are used as the method of seperation for people / accounts. Here we can
apply IAM roles to restrict access to people / service accounts to resources 
within the project.

Access to VPCs are governed via IAM roles, since there can be more than one VPC
per project, there is no way to restrict access to individual VPC inside a 
project to a user. If they can access one, they can access all inside the project.

**Virtual Private Clouds (VPC)**

VPCs provide seperation for resources within a project. All resources within a
VPC are contained on the same private network.

**Firewalls**

Firewalls seperate access to by network traffic. Here we can apply rules to 
ingress / egress traffic to control access to resources via network policies.
Can limit access by:
* Port
* IP subnet/range
* Between subnets
* Tags
* Service Accounts

One thing to note, here LoadBalancers/healthcheck will need to have firewall 
rules set to allow them to access the resources inside an instance group inside 
a VPC.

**Air Gap**

We can provide further restriction by removing external IPs to our resources which
provides an air gap for accessing resources. You can only access resources via 
the internal network.
Here this adds in the complexity for how you access your own services. The two 
options would be via VPN or via a Bastian Host which has access to the internal
network via a specific firewall for that host IP.

**Exam Scenarios:**

1. Instance Group VMs keep restarting every minute
   * This is often caused by a failing health check
   * Can be resolved by ensuring the firewall rules are set to allow access to instance group VMs from Load Balancer
     * Apply rules via Subnet / Tags

2. On Premise network is unable to access cloud network resources
   * Here we would check to see if the firewall ingress rules allow access from on-premise network IP range

3. Failover from on-premise load balancer hosted app to GCP hosted instance group (hot standby)
   * Consider security / compliance regulations
   * Allow firewall access at instance group level (subnet / tags) from outside source

## Audits & Compliance

Audits will be a frequent topic of discussion on the exam. Need to be able to 
design for legal compliance, of that audit plays a large part.

Here `StackDriver Logging` plays the big part, we can enable and provide access 
logs, vpc network flow logs, and data access logs for the review of the audit.

* These logs should be stored somewhere for long term needs and we may only have to access 1-2 times a year
* This should be a good indicator that Cloud Storage is a suitable sink for your access logs
* BigQuery is also an option, but since the frequency of access is so low Cloud Storage is the best option
  * If you needed BigQuery, you could load your logs into BQ, or create an external view to provide BQ search capabilities over the logs
* To allow an auditor to access the logs, you can either create a GCP account with access to the bucket, or provide a signed url

**Compliance**

* Shared responsibility model
  * Most Google services are compliant with the major governing bodies
  * A duel responsibility between Google and the Cloud Organisation to ensure all necessary compliance measures are met
* Previously covered topics like IAM, logging, 2FA authentication all play a big part here

To verify GCP compliance certifications see: [security/compliance](https://cloud.google.com/security/compliance)

**Data Loss Prevention (DLP)**

* Created data often contains Personally Identifiable Information (PII)
* Here we want to make sure any PII data is not leaked into logs where someone with the wrong intentions could use it
* Google have a DLP API which can automatically discover, detect and sanitise PII data 
* This can be used to also ensure you meet compliance obligations

**Analyse Payment Card Industry (PCI) Data**

* PCI DSS (Data Security Standards) is one of the compliance standards when using card information
* Exam might focus on how you can securely stream logs to BigQuery for analysis
  * Need to make sure you are using services which are all PCI DSS compliant
* A basic flow might look like this
  * Credit card numbers are tokentised for protection
  * Relevant log data written to stackdriver
  * Export relevant logs to Cloud Storage by running DLP API over the logs
  * Cloud Storage can then optionally export to BigQuery
  * Share scoped logs with auditors
