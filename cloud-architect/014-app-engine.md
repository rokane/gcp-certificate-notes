# App Engine

The Cloud Architect exam focuses more on the management side of App Engine rather
than the building of apps on App Engine. For that, the Cloud Developer exam 
would play a larger focus.

## Standard vs Flexible Environment

When choosing to run your application on App Engine, you get choice on whether
you wish to run on the standard / flexible runtime environment. 

The standard has faster scale up time, and will scale to zero if no traffic is
being served. It is more constrained with only Python, Java, Go and NodeJS 
runtime languages. More suitable for low cost / low traffic applications.

The flexible environment has less contraints but more management back on the 
user. Additional runtime languages are available or any containerised application.
Slower scale up time as traffic is always being served, rather than scaling to
zero. This is also the only option which allows VPC access.

## Memcache

Memcache is an option which can be selected when using App Engine to store 
common queries inside an in memory cache to improve performance.

There are two service levels:
* Shared 
  * Free / default option 
  * Best effort caching where cache is shared with others
* Dedicated
  * Fixed cache capability dedicated to your application
  * Cost assigned with this service

## Version Management

When updating your application you can choose to either deploy and direct all
traffic to your new version, or do a canary deployment and slowly move traffic
accross. When doing a canary deployment, you can specify the `--no-promote` flag
and gradualy increase the % traffic which gets directed to the new version 
over time.

## Connecting to External Resources

Your application may need to connect to resources which exist outside of GCP. 
One example might be a database which lives back on premise. To do this, you can
handle this through making your resource (such as a database) public to the 
internet and handling access to it from GCP through firewall rules, however this
would not be a desired way.

A better approach would be to use a VPN to connect your App Engine app to the 
On Premise resource. To do this, your App Engine app would need to be deployed 
inside a VPC which is only possible by using the Flexible environment option.
