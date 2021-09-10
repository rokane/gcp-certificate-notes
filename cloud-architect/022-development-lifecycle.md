# Software Development Lifecycle Concepts

Exam questions assume using the SDLC for application development.
* Keep environments seperate with seperate levels of access
* Same concepts regardless of compute platform

## SDLC Concepts

**Continuous Integration / Continuous Deployment (CI/CD)**

* CI aims to continually integrate your work into the main branch early and often
  * Minimise the cost of integration
* CD aims to automate the deployment process of software
  * Auto deploy each build once verification passes
  * No waiting for a human gatekeeper
* Result: software deployment isn't a scary big bang activity but a continual process of small improvements
* Key components of CI/CD within GCP:
  * Cloud Source Repositories - Private git repositories
  * Cloud BUild - Managed service providing CI capabilities
  * Container Registry - Store container images
  * Jenkins / Spinnaker - Third Party Open Source Software providing automation tools for CD 

**Best Practices**

1. Blue/Green Deployment Model
   * Only one environment is live which the other goes through SDLC cycle
   * Reduce downtime / risk
   * If something goes wrong in Green, swap back to Blue

2. Break monolith into microservices
   * Microservices means smaller pieces / faster deployment cycle

## Testing Application for Resiliancy

* Testing approach should follow the testing pyramid
  * Unit tests -> Integration Tests -> End to End Tests
* Majority of tests done at unit test level, with less done as you go up the pyramid
  * Faster / more reliable tests at unit level
* Load testing should be considered to ensure you application scales with demand
* Chaos can be introduced to test resilancy in your application. Ensuring failover mechanics are triggered and application can handle it.
  * Tools will provide capabilities to randomly shut down VM instances, zones, stateful states
  * Chaos Monkey is a common tool
