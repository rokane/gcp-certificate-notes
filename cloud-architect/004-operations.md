# Operations

Stackdriver is the suite of tools which provide application operation 
capabilities within your Google Cloud Environment. Stackdriver provides multi 
cloud monitoring which means you can observe your applications across different
cloud providers.

## Stackdriver Logging

Provides a single repository to store log data / events from multiple sources.
From here you can search and analyze logs in realtime.

The below list contains the different types of logs, which explains "who did
what, where and when?"
  * `Audit Logs`
    * Examples:
      * Admin/System Activity: Admin actions and API calls
      * Access Transparency: Access logs when GCP staff access your data
      * Data Access: Logs API calls that create, modify or read user data.
  * `Agent Logs`
    * Logs data from third party applications

Log Pricing depends on the log type, and the amount of logs consumed by 
stackdriver logging:
  * First 50 GB free per month, $0.50/GB following that
  * Admin and System logs exempt

### Log Retention & Long Term Storage

The table below shows the retention period for each type of log. After the 
retention period has expired, logs are deleted and no longer recoverable.
Therefore it is important to export your logs to Cloud Storage (long term 
storage) and Big Query (big data analysis) to ensure you can access them long
term.

When exporting logs you need to setup the below items:
  * A project
  * A `filter` which specifies which logs to export
  * A `destination` service (Big Query / Cloud Storage)
  * The filter and destination will be held in a `sink`
    * Only new entries will be exported after the sink creation

| Log Type            | Retention Period  |
|:-------------------:|:-----------------:|
| Admin Activity      | 400 days          |
| System Event        | 400 days          |
| Access Transparency | 400 days          |
| Data Access         | 30 days           |
| All other logs      | 30 days           |

### IAM Roles for Logging

* Logging Admin: Full access and can add others to Logging IAM
* Logs Viewer: Able to view logs
* Logs Writer: Grant service accounts ability to write logs
* Logs Configuration Writer: Create metrics and export sinks

## Stackdriver Monitoring

Provides full stack monitoring capabilites, helping us understand what is up,
what is down, what is overloaded etc. It provides dashboarding and alerting 
capabilities and integrates tighly with Stackdriver Logging.

To perform uptime checks on External Applications outside of GCP, you must 
ensure the application is public and also allow firewall traffic from uptime
source IP.

### Stackdriver Agent

It is recommended to also install the Stackdriver Agent on your VM, to further
enhance your monitoring capabilities. Without the agent, you can still access
CPU, disk/network traffic and uptime info, but the agent allows us to access
additional resource and application service information.

It also provides monitoring capabilities for a number of third party 
applications such as Apache and NGINX.

### Best Practices

It is recommended to create a single project for Stackdriver Monitoring so that:
  * The single project monitors all applications / resources across GCP/AWS
  * IAM controls can be applied in isolation to ensure the monitoring data
    access is controlled seperately, and seperated from other IAM roles.

## Stackdriver Trace

Used to discover performance bottlenecks by providing a capbility which allows
you to `trace` the path a request took through while processing, providing 
insights into the latency at each step along the way.

Trace comes integrated into App Engine by default, and can be integrated with
Google Kubernetes Engine and Google Compute Engine. Commonly used within 
microservices architecture where performance bottlenecks are not obvious.

## Stackdriver Error Reporting

Used to provide real-time error monitoring and alerting within your application.
Provides methods to help understand errors within your application, through 
integrating with Stackdriver Logging.

Automatically built into App Engine and Cloud Functions, and can be enabled on
GKE and GCE with the installation of the Logging Agent.

## Stackdriver Debug

Provides capabilites for you to debug your application in production. Can 
observe application state, insert breakpoints etc, without stopping or slowing
down your application performance.

Automatically built into App Engine, and can be configured on GKE and GCE.

## Profiler

Provides abilities to profile resource intensive resource components. Will 
collect CPU/RAM usage data and associate with source code which might be 
causing this spikes.
