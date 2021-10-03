# Case Studies

Prior chapters of this course focused on individual components with a bit of a 
dive into the key products within the Google Cloud Platform. The next section
will focus on what is a large portion of the exam, being applying cloud concepts
to solve business and technical requirements at a higher level.

The exam will consist of four cases studies, all of which are available before
the exam. You can expect to see roughly 30% of questions related to the case 
studies, and they will be related to all four. This will equate to about 15-20
questions in total.

The questions you will want to ask about each case study are listed below. We 
will then dive into each of the case studies in detail and attempt to answer 
each of the questions in the context of the case study in the following 
sections.

1. Company Overview - What does the company do?
2. What are their pain points / why are they looking to use GCP?
3. Solution Concept - What are their goals / what are they looking to achieve?
4. Big Picture - What do they care about / what is their leadership looking for?
5. What are their business / technical requirements?
6. If applicable, what is their current environment and how would it translate to GCP given their above requirements?
7. If applicable, what does their infrastructure / data lifecycle look like?

The list of case studies can be found at the below link. Note, they
are subject to change over time so when viewing this, the case study
may have have differed to the notes in the following sections.

https://cloud.google.com/certification/guides/professional-cloud-architect

## Mountkirk Games

Link to case study: [mountkirk games](https://services.google.com/fh/files/blogs/master_case_study_mountkirk_games.pdf)

1. What does the company do?
   * Mobile game creator

2. What are their pain points?
   * Legacy platform inability to provide the insights they want
   * Inability to scale up / down on demand

3. What are their goals?
   * Expand to other platforms outside of the mobile space
   * Creation of a multi-platform FPS game
     * Ability to join specific geolocation arena from multiple locations
     * Realtime banner displaying leaders across all arenas
   * Game backend deployed on GKE
     * Ability to scale rapidly
     * Can use Google Global LoadBalancer to route players to nearest arena
   * Storage for global leaderboard on multi-regional spanner instance

4. What do they care about?
   * Reducing latency
   * Capturing analytics
   * Scaling
   * Global reach
   * Make use of managed services
   * Increase business agility
   * Reduce costs

5. Business requirements
   * Support multiple gaming platforms
   * Support multiple regions
     * Multi-regional spanner instance
     * Multi-regional GKE cluster with Global load balancer
   * Support rapid iteration of new features
   * Minimise latency
     * Low latency global load balancer
   * Optimise for dynamic scaling
     * Horizontally scalable database solution
     * Ability to scale up/down pods in GKE with demand
   * Use managed services / pooled resources
     * Managed Kubernetes
     * Managed database solutions
   * Cost savings
     * Autoscaling to only use what is needed

6. Technical requirements
   * Dynamically scale based on game activity
     * Stackdriver to moniter efficiency/performance
     * Automatically scale size of GKE cluster based metrics 
   * Publish scoring data in near real-time based on game activity
     * PubSub to provide multi regional data ingestion points
     * Dataflow to process activity data and store in Spanner
   * Store activity logs in structured files for future analysis
     * Store in json/avro format inside GCS bucket
     * Expose data for future analytics with BigQuery External Table 
   * Use GPU processing to render graphics server side for multi platform support
   * Support eventual migration of legacy games to new platform
     * Ability to containerise and run on new platform

7. Current environment / translated to GCP
   * Recently started migrating to GCP
     * 5 games migrated using lift and shift, running on VMs in GCE
   * New games isolated inside their own GCP project
   * Upper folder contains network policies for game projects nested under it
   * Legacy games all contained inside a single GCP project
   * Seperate environments for dev and test

8. Infrastructure / data lifecycle

![Solution](./assets/016-mountkirk-games.png)

Additional solution references:
* https://cloud.google.com/architecture/mobile-gaming-analysis-telemetry

## ERH Healthcare

Link to case study: [erh healthcare](https://services.google.com/fh/files/blogs/master_case_study_ehr_healthcare.pdf)

1. What does the company do?
   * Offers SAS products for maintaing electronic health records to the medical industry

2. What are their pain points?
   * Costs associated with colocation model (time and money)
   * Inconsistencies between environments

3. What are their goals?
   * Migrate from their on-premise colocation model to cloud 
   * Uplift DR model to support cloud transition
   * Continuous Deployment capabilities
   * Update software at a rapid pace
   * Scale environments with demand

4. What do they care about?
   * Resiliant platform with high availability
   * Consistency between environments
   * Improve monitoring to enable insights / analytics
   * Compliance with regulators
     * HIPPA (Health Care)
     * PII (Personally Identifiable Information)

5. Business requirements
   * Onboard new providers as quickly as possible
   * Minimum 99.9% availability for all customer facing applications
     * 'hot' dr patterns
     * Ensure replication servers are running / data replicated and auto cutover solutions (Cloud SQL)
     * Multi regional
   * Visibility on system performance / usage
     * Reports created based off cluster metrics 
   * Increase ability to provide insights into healthcare trends
     * Analytics capabilities (Big Query / Big Table)
   * Reduce latency to all customers
     * Global Load Balancer routing to nearest service
   * Maintain regulatory compliance
   * Decrease infra costs
     * Scale down when demand is not there.
     * Pay for what you use
   * Make predictions based off industry trends
     * Cloud ML reading from data ingested from sources

6. Technical requirements
   * Maintain legacy interfaces to insurance providers with connectivity between onprem / cloud
     * Cloud Interconnect options
   * Consistent way to manage Container based applications
     * GKE to run container based applications
     * Clusters setup to autoscale with demand
   * High performance connection  between onprem / cloud
     * Direct / Partner Interconnect for high bandwith
   * Consistent logging, log retention, monitoring and alerting
     * Stackdriver to capture logs, metrics and setup alerts based off that
     * Options for alerts to notify non email communication methods
   * Dynamically scale and provision new environments
     * Resources defined via code and provisioned using Cloud Deployment Manager or alternate (Terraform / Ansible)
     * Provides consistency between environments which need to be provisioned
   * Create interfaces to ingest data from new providers
     * PubSub provides global data ingestion points
     * Possibility to introduce Cloud IOT if IOT devices potentially upload data in the future

7. Current environment / translated to GCP
   * Data Centers hosted in multi colocation facilities
   * Lease on one facility due to expire
   * Containerised web based applications running on Kubernetes
   * Data is a combination of Relational / NoSQL databases
   * Legacy File/API based applications integrate with insurance providers - No plan to migrate
   * User management via Active Directory
   * Monitoring performed with open source tools
   * Alerting via emails which has been ineffective
   * Cloud Migration Strategy
     * Assess: 
       * Biggest ROI with the applications running on datacentre with expiring lease. 
       * Focus on containerised applications.
     * Pilot: 
       * Setup of projects, mapping of Active Directory to cloud IAM.
     * Move Data First: 
       * MySQL / MS SQL Databases -> Cloud SQL
       * MongoDB Databases        -> Cloud Datastore
       * Redis Databases          -> Managed redis instance
       * Assess the data transfer options available depending on size / bandwith
     * Move Applications Next:
       * Containerised Applications -> Managed GKE cluster

8. Infrastructure / data lifecycle
   * TODO: Include diagram for data lifecycle / solution

Additional solution references:
   * DR Patterns:
      * High availability will mean hot patterns need to be applied.
      * Additional costs here to do so

## Helicopter Racing League

Link to case study: [helicopter racing league](https://services.google.com/fh/files/blogs/master_case_study_helicopter_racing_league.pdf)

1. What does the company do?
   * Helicopter racing league
   * Streaming service of all the races

2. What are their pain points?
   * Current platform is unable to support real time predictions
   * Only able to predict race outcomes / season long events

3. What are their goals?
   * Migrate to a new service to make use of managed AI / ML
   * Expand into emerging regions with a global footprint
   * Move content closer to the end users

4. What do they care about?
   * Compliance with regulators
     * AEMA (European Countries)
     * PII if there is any Personal Information Stored

5. Business requirements
   * Reduce latency in emerging regions
   * Expose predictive services via APIs to partners
   * Increase predictive capabilities during / before races:
      * Race results / mechanical failures / crowd sentiment
      * Live data ingestion being fed into models with results displayed
   * Increase telemetry / insights
      * Stackdriver monitoring
   * Measure fan engagement in new predictions
     * Stackdriver monitoring
   * Enhance global availability / quality of broadcasts
     * Multi Regional 
   * Create a merchandising revenue stream 

6. Technical requirements
   * Increase prediction throughput
   * Reduce viewer latency
   * Increase transcoding latency
   * Real time analytics of viewer platform
   * Creation of a Data Mart to enable processing of large volumes of race data

7. Current environment / translated to GCP
   * Cloud first
   * Video recording done on track and uploaded to cloud for encoding in cloud
   * Mobile data centres at tracks mounted by trucks
   * Content stored in an object service           -> (GCS)
   * Video encoding / transcoding performed in VMs -> (GCE)
   * Predictions done using TensorFlow inside VMs  -> (Cloud ML)

8. Infrastructure / data lifecycle

Additional solution references:

## TerramEarth

Link to case study: [terramearth](https://services.google.com/fh/files/blogs/master_case_study_terramearth.pdf)

1. What does the company do?
   * Heavy Machinary manufactorer

2. What are their pain points?
   *  High growth rate

3. What are their goals?
   * Create ecosystem of new products enabling access to their data
   * Increase automation of vehicles
   * Eventual migration of legacy apps to the cloud

4. What do they care about?
   * 
  
5. Business requirements
   * Predict / detect vehicle malfunction and rapidly ship parts for just in time repair
      * Vehicle data uploaded via Cloud IOT
      * Data ingestion via Cloud PubSub
      * Alerting on top to detect malfunctions - Cloud Monitoring / Alerting
      * Data processed / stored to enable models to be build / run for predictive purposes
   * Decrease operational costs / adapt to the season
      * Scale down when demand is low - During off seasons
   * Increase speed / reliability of development workflows
      * Automated CI / CD pipelines
      * Cloud Build
   * Allow remote developers to be productive without compromising code / data security
   * Create a flexible / scalable platform for developers to create APIs for partners
      * Deployment model which allows scaling of services. GKE / Cloud Run / App Engine Flex
  
6. Technical requirements
   * Create an abstraction layer for HTTP API access to their legacy systems to enable a gradual move to the into the cloud without disruption.
      * HTTP Loadbalancer which can have rules modified when eventually want to point towards migrated services on the cloud
   * Modernise CI/CD to allow fast deployment of containerised applications
      * Cloud Build for automated CI
      * Build application images inside CI and push to Container Registry
      * Trigger CD pipeline (Harness / Jenkins) to on successful build
    * Allow developers to run experiments without compromising security and governance
    * Self service portal for internal / partners to create/request resources and manage access to APIs
      * Deployment Manager??
    * Cloud native solutions for key and secrets management
      * Cloud Secret Manager for secrets manager
      * Cloud KMS for key management
    * Standardise on tooling for application / network monitoring
      * Stackdriver monitoring
  
7. Current environment / translated to GCP
   * Vehicle data aggregation lives in GCP
   * Legacy Inventory & Logistics Systems contained on prem
      * Sensor data from manufactoring plants sent to systems on prem
   * On Prem connectivity established with GCP via Partner Interconnect
   * Web UI for customers is running on GCP
      * Has access to stock management / logistics
  
8. Infrastructure / data lifecycle
   * Cloud IOT for vehicles
   * Cloud PubSub for data ingestion points
   * Cloud Dataflow to process data and store in a location for further analysis (BigQuery / BigTable)
  
Additional solution references:
