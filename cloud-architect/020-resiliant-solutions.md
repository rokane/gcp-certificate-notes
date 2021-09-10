# Disaster Recovery

We should always have a plan in case something goes drastically wrong. A well
planned DR plan will ensure you can recover from a catastophe in minimal time.

When building a disaster recovery plan, you must have an idea of the below 
recovery metrics as they will all factor into the depth of the DR solution.

* **Recovery Time Objective (RTO)**: Maximum acceptable time the application can be offline
* **Recovery Point Objective (RPO)**: Maximum acceptable time which data can be lost from your application due to an outage.

The shorter your RTO/RPO needs, the higher the cost will be. This is because 
you will be paying for added resiliancy inside your cloud based solution.

Disaster Recovery Patterns can be broadly classified into cold/warm/hot, depending
on how quickly you are able to recover from the outage / disaster.
* Cold being a slower recovery time, usually manual, but with less cost.
* Warm being a moderate recover time, usually manual with moderate costs.
* Hot being the fastest, auto switch to a recovery instance which is always being replicated. Comes with the additional costs.

## Backup and Recovery Methods in GCP 

Here we will review the already visited topics throughout this course which touch
on creating backups which can be used for DR purposes.

**Backup of GCE Instances:**

* This is achieved through Disk Snapshots
* Each snapshot is incremental in that it captures the diff between its previous snapshot
* Snapshots can be scheduled via a cron job or through a snapshot scheduler

**Backup / Recovery of Cloud Storage**

* Object versioning capability on objects inside GCS bucket
* Delete and Rollback protection when versioning is enabled
* Can easily rollback to an earlier version if data was deleted

**Distributed Computing Application Rollback (Livestock Model)**

* GCE managed instance group / App Engine won't use snapshots
* Rollback to an earlier verion
  * Compute Engine managed instance group via rolling updates. Optionally set target less than 100%
  * App Engine via versioning control and canery updates with % traffic split

## Exam Scenarios

Won't be many scenarios on the exam, if there is, they will likely be of the format
which is similar to the two described below:

1. Rollback plan for a set of instances running on a managed instance group
     * This is the livestock model
     * Here set object versioning on data in GCS
     * Rolling updates
     * Not the use of Snapshots

2. Need to push a risky update via App Engine to a live environment
     * Versioning and update via a traffic split
     * Send risk version to a smaller % of users as a canery update
