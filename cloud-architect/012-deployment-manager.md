# Cloud Deployment Manager

Cloud Deployment Manager is googles infrastructure as code product offering, 
providing a similar capability to terraform and ansible. It automates the  
creation/management of GCP resources through configuration files and templates.

These can only be invoked through the command line with yaml config files. 
Under the hood it calls each resource API to create the resource with the specified
properties.

Each config file can contain templates which can be in python or jinga2 format.
The final output manifest is generated and in a read only view.
