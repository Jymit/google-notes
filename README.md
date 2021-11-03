# zero2hero
How to get started with Google Cloud Platform for free
GCP currently offers a 3 month free trial[https://cloud.google.com/free] with $300 US dollars of free credit. You can use it to get started, play around with GCP, and run experiments to decide if it is the right option for you.

You will NOT be charged at the end of your trial. You will be notified and your services will stop running unless you decide to upgrade your plan.

I strongly recommend using this trial to practice. To learn, you have to try things on your own, face problems, break things, and fix them. It doesn't matter how good this guide is (or the official documentation for that matter) if you do not try things out.


Consuming resources from GCP, like storage or computing power, provides the following benefits:

No need to spend a lot of money upfront for hardware
No need to upgrade your hardware and migrate your data and services every few years
Ability to scale to adjust to the demand, paying only for the resources you consume
Create proof of concepts quickly since provisioning resources can be done very fast
Secure and manage your APIs
Not just infrastructure: data analytics and machine learning services are available in GCP
GCP makes it easy to experiment and use the resources you need in an economical way.


Optimize your VMs to reduce costs in GCP

Custom Machine Types
GCP provides different machine[https://cloud.google.com/compute/docs/machine-types] families with predefined amounts of RAM and CPUs:

General-purpose. Offers the best price-performance ratio for a variety of workloads.
Memory-optimized. Ideal for memory-intensive workloads. They offer more memory per core than other machine types.
Compute-optimized. They offer the highest performance per core and and are optimized for compute-intensive workloads
Shared-core. These machine types timeshare a physical core. This can be a cost-effective method for running small applications.


Sustained Use Discounts
The longer you use your virtual machines (and Cloud SQL instances), the higher the discount - up to 30%. Google does this automatically for you.

Committed Use Discounts
You can get up to 57% discount if you commit to a certain amount of CPU and RAM resources for a period of 1 to 3 years.

To estimate your costs, use the Price Calculator[https://cloud.google.com/products/calculator]. This helps prevent any surprises with your bills and create budget alerts.


Four types of resources that can be managed through Resource Manager

The organization resource. It is the root node in the resource hierarchy. It represents an organization, for example, a company.
The projects resource. For example, to separate projects for production and development environments. They are required to create resources.
The folder resource. They provide an extra level of project isolation. For example, creating a folder for each department in a company.
Resources. Virtual machines, database instances, load balancers, and so on.

Resources in GCP follow a hierarchy via a parent/child relationship, similar to a traditional file system, where:

Permissions are inherited as we descend the hierarchy. For example, permissions granted and the organization level will be propagated to all the folders and projects.
More permissive parent policies always overrule more restrictive child policies.


Labels
Labels are key-value pairs you can use to organize your resources in GCP. Once you attach a label to a resource (for instance, to a virtual machine), you can filter based on that label. This is useful also to break down your bills by labels.

Some common use cases:

Environments: prod, test, and so on.
Team or product owners
Components: backend, frontend, and so on.
Resource state: active, archive, and so on.
Labels vs Network tags
These two similar concepts seem to generate some confusion. I have summarized the differences in this table:

LABELS                       -	NETWORK TAGS
Applied to any GCP resource	 - Applied only for VPC resources
Just organize resources	     - Affect how resources work (ex: through application of firewall rules)


Identities
In a GCP project, identities are represented by Google accounts, created outside of GCP, and defined by an email address (not necessarily @gmail.com). There are different types:

Google accounts*. To represent people: engineers, administrators, and so on.
Service accounts. Used to identify non-human users: applications, services, virtual machines, and others. The authentication process is defined by account keys, which can be managed by Google or by users (only for user-created service accounts).
Google Groups are a collection of Google and service accounts.
G Suite Domain* is the type of account you can use to identify organizations. If your organization is already using Active Directory, it can be synchronized with Cloud IAM using Cloud Identity.
allAuthenticatedUsers. To represent any authenticated user in GCP.
allUsers. To represent anyone, authenticated or not.
Regarding service accounts, some of Google's best practices include:

Not using the default service account
Applying the Principle of Least Privilege. For instance:
1. Restrict who can act as a service account
2. Grant only the minimum set of permissions that the account needs
3. Create service accounts for each service, only with the permissions the account needs
Roles
A role is a collection of permissions. There are three types of roles:

Primitive. Original GCP roles that apply to the entire project. There are three concentric roles: Viewer, Editor, and Owner. Editor contains Viewer and Owner contains Editor.
Predefined. Provides access to specific services, for example, storage.admin.
Custom. lets you create your own roles, combining the specific permissions you need.
When assigning roles, follow the principle of least privilege, too. In general, prefer predefined over primitive roles.


https://cloud.google.com/deployment-manager/docs
It is Google's Infrastructure as Code service, similar to Terraform - although you can deploy only GCP resources. 


Cloud Operations (formerly Stackdriver)

Cloud Logging
By default, data will be stored for a certain period of time. The retention period varies depending on the type of log. You cannot retrieve logs after they have passed this retention period.
Logs can be exported for different purposes. To do this, you create a sink, which is composed of a filter (to select what you want to log) and a destination: Google Cloud Storage (GCS) for long term retention, BigQuery for analytics, or Pub/Sub to stream it into other applications.
You can create log-based metrics in Cloud Monitoring and even get alerted when something goes wrong.

VPC Flow Logs

Cloud Monitoring
Cloud Monitoring lets you monitor the performance of your applications and infrastructure, visualize it in dashboards, create uptime checks to detect resources that are down and alert you based on these checks so that you can fix problems in your environment. You can monitor resources in GCP, AWS, and even on-premise.


Alerts
To receive alerts, you must declare an alerting policy. An alerting policy defines the conditions under which a service is considered unhealthy. When the conditions are met, a new incident will be created and notifications will be sent (via email, Slack, SMS, PagerDuty, etc).

A policy belongs to an individual workspace, which can contain a maximum of 500 policies.


Google Cloud Storage (GCS)
https://cloud.google.com/storage


Cloud SQL
Cloud SQL provides access to a managed MySQL or PostgreSQL database instance in GCP. Each instance is limited to a single region and has a maximum capacity of 30 TB.

Google will take care of the installation, backups, scaling, monitoring, failover, and read replicas. For availability reasons, replicas must be defined in the same region but a different zone from the primary instances.

Data can be easily imported (first uploading the data to Google Cloud Storage and then to the instance) and exported using SQL dumps or CSV files format. Data can be compressed to reduce costs (you can directly import .gz files). For "lift and shift" migrations, this is a great option.

If you need global availability or more capacity, consider using Cloud Spanner.

Cloud Spanner
Cloud Spanner is globally available and can scale (horizontally) very well.

These two features make it capable of supporting different use cases than Cloud SQL and more expensive too. Cloud Spanner is not an option for lift and shift migrations.


Networking in GCP
Virtual Private Cloud (VPC) 
https://cloud.google.com/vpc

Shared VPC
Shared VPCs are a way to share resources between different projects within the same organization. This allows you to control billing and manage access to the resources in different projects, following the principle of least privilege. Otherwise you'd have to put all the resources in a single project.

To design a shared VPC, projects fall under three categories:

Host project. It is the project that hosts the common resources. There can only be one host project.
Service project: Projects that can access the resources in the host project. A project cannot be both host and service.
Standalone project. Any project that does not make use of the shared VPC.

VPC Network Peering
Shared VPCs can be used when all the projects belong to the same organization. However, if:

You need private communication across VPCs.
The VPCs are in projects that may belong to different organizations.
Want decentralized control, that is, no need to define host projects, server projects, and so on.
Want to reuse existing resources.
VPC Network peering is the right sol

Cloud VPN
With Cloud VPN, your traffic travels through the public internet over an encrypted tunnel. Each tunnel has a maximum capacity of 3 Gb per second and you can use a maximum of 8 for better performance. These two characteristics make VPN the cheapest option.

You can define two types of routes between your VPC and your on-premise networks:

Static routes. You have to manually define and update them, for example when you add a new subnet. This is not the preferred option.
Dynamic routes. Routes are automatically handled (defined and updated) for you using Cloud Router. This is the preferred option when BGP is available.
Your traffic gets encrypted and decrypted by VPN Gateways (in GCP, they are regional resources).

Load Balancers (LB)
In GCP, load balancers are pieces of software that distribute user requests among a group of instances.

A load balancer may have multiple backends associated with it, having rules to decide the appropriate backend for a given request.

There are different types of load balancers. They differ in the type of traffic (HTTP vs TCP/UDP - Layer 7 or Layer 4), whether they handle external or internal traffic, and whether their scope is regional or global:

HTTP(s). Global LB that handles HTTP(s) requests, distributing traffic to multiple regions based on user location (to the closest region with available instances) or URL maps (the LB can be configured to forward requests to URL/news to a backend service and URL/videos to a different one). It can receive both IPv4 and IPv6 traffic (but this one is terminated at the LB level and proxied as IPv4 to the backends) and has native support for WebSockets.
SSL Proxy LB. Global LB that handles encrypted TCP traffic, managing SSL certificates for you.
TCP Proxy LB. Global LB that handles unencrypted TCP traffic. Like SSL Proxy LB, by default, it will not preserve the client's IP, but this can be changed.
Network Load Balancer. Regional LB that handles TCP/UDP external traffic, based on IP address and port.
Internal Load Balancer. Like a Network LB, but for internal traffic.

Cloud DNS
Cloud DNS is Google's managed Domain Name System (DNS) host, both for internal and external (public) traffic. It will map URLs like https://www.freecodecamp.org/ to an IP address. It is the only service in GCP with 100% SLA - it is available 100% of the time.


Run your applications in GCP.
I will present 4 places where your code can run in GCP:

Google Compute Engine
Google Kubernetes Engine
App Engine
Cloud Functions

Google Compute Engine (GCE)

