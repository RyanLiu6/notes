# Introduction to Google Cloud

## Cloud Computing and Google Cloud
Cloud computing has five fundamental attributes:

1. On-demand self-service: Users can provision resources without human intervention.
2. Broad network access: Resources are accessible over the network.
3. Resource pooling: Multiple customers share resources.
4. Rapid elasticity: Resources can be scaled up or down.
5. Measured service: Pay for what you use.

GKE, Google Kubernetes Engine, is a managed Kubernetes service that runs on Google Cloud Platform, and is the focus of this course.

GCP offers a wide range of services for compute, each satisfying a different need:
- Compute Engine: Infrastructure as a service. Virtual machines on demand.
- GKE: Managed Kubernetes clusters.
- App Engine: Platform as a service. Run code and let Google deal with the infrastructure.
- Cloud Run: Serverless containers on demand.
- Cloud Functions: Serverless functions on demand.

With most applications, you'll need a database of some sort. With Compute Engine, you can spin up a database on the same virtual machine as your application. However, we can also use Google's Storage services, such as Cloud Bigtable, Cloud SQL, Cloud Spanner, etc.

### Quiz
**Which statements are true about cloud computing? Mark all that are true (2 correct answers).**
- [ ] Human intervention is required to stop using cloud resources once reserved, and payment continues until the change is confirmed.
- [x] Customers who need more resources can get them rapidly
- [ ] Cloud computing providers dedicate particular physical resources to a particular customer.
- [x] Customers pay for the resources they use or reserve

**Which of these Google Cloud compute services provides environments for execution of code, in which users don't have to worry about infrastructure management? Choose all that are correct (2 correct answers).**
- [x] App Engine
- [ ] Compute Engine
- [ ] Google Kubernetes Engine
- [x] Google Cloud Functions

## Resource Management
GCP provides resources in multi-regions, regions, and zones.

Multi-region:
- Americas, Europe, and Asia-Pacific

Regions:
- Independent geographic areas
- Roudn trip network latencies of under one millisecond (95th percentile)

Zones:
- Independent data centers within a region

We'll need to deploy our applications across multiple zones and regions to ensure high availability and fault tolerance.

### Edge Networks
When an user requests a resource, GCP will respond to the user's request from an edge network location, providing lowest delay or latency.

You can specify a resource's geographical location, including the level it is deployed at: Zonal, Regional, or Multi-Regional.
- Zonal resources are deployed within a single zone, meaning if that zone becomes unavailable, so does that resource.
- Regional resources are deployed across multiple zones, within the same region.
- Multi-Regional or Global resources, as the name suggests, are deployed across multiple regions.

Some examples:
- Zonal: Compute Engine VM instances, Persistent Disks
- Regional: Datastores, Regional GKE clusters
- Multi-Regional: HTTP(s) load balancers, Virtual Private Cloud networks

### Projects
In GCP, a project is a container for resources. Resources MUST belong to a project. Zones and regions physical organize resources, while projects logically organize resources.

Projects can belong to a folder, for which folders can be nested within other folders. This is another great way of organizing resources, especially at the organization level. There actually is an organization "folder", which is the root of the folder tree. This lets us set policies at the organization level, and apply them to all resources within the organization.

Organization
├── Department A
│   ├── Team 1
│   │   └── Production
│   │   └── Development
│   └── Team 2
│   │   └── Store Front
└── Department B

Identity and Access Management (IAM) is the service that manages access to resources, which can be defined at the organization, folder, or project level. These propagate down to the resources within the hierarchy.

### Quiz
**Within which of these Google Cloud geographic scopes are network latencies generally less than 1 millisecond? Choose all that are correct (2 correct answers).**
- [ ] Global
- [ ] Multi-Region
- [x] Region
- [x] Zone

**What type of resource is a Compute Engine virtual machine?**
- [x] Zonal
- [ ] Regional
- [ ] Multi-Regional
- [ ] Global

**What is the base-level organizing entity for creating and using Google Cloud resources and services?**
- [x] Project
- [ ] Folder
- [ ] Cluster
- [ ] Region

## Billing
Billing is accumulated at the project level, but billing accounts can be linked to multiple projects.

There are various tools to help manage billing:
- Budgets and Alerts: Set budgets and receive alerts when you're approaching or exceed them. (Project or Billing Account level)
- Billing export: Export billing data to BigQuery for analysis.
- Reports: Visual tool to monitor billing data.

### Quotas
Quotas are designed to prevent overuse of resources in case of an error or malicious activity. Quotas are set at the project level.

There are two types of quotas:
- Rate: Limit the rate of requests per second.
- Allocation: Limit the total number of resources.

### Quiz
**At what level in the Google Cloud resource hierarchy is billing set up?**
- [x] Project
- [ ] Individual users
- [ ] Organization
- [ ] Folder

**Which type of quota resets at regular intervals?**
- [x] Rate quotas
- [ ] Allocation quotas

## Interacting with GCP
There are four ways to interact with GCP:
- GCP Console: Web-based GUI for managing resources.
- GCP Shell + SDK: Command-line tool for managing resources.
- GCP Mobile App: Mobile app for managing resources.
- GCP API: Programmatic interface for managing resources.

The GCP SDK contains several commands of which the following will be most relevant for this course, `gcloud`, `kubectl`, and `gsutil`.

If you can't install the SDK, you can use the Cloud Shell, which is a web-based shell that provides a command-line interface to GCP.

### Quiz
**Which of these ways to interact with give you access to the gcloud and kubectl commands? Choose all that are correct (2 correct answers).**
- [x] Cloud SDK
- [ ] Cloud Console Mobile App
- [x] Cloud Shell
- [ ] Console

## Summary Quiz
**You are developing a new product for a customer and need to implement control structures in Google Cloud to help manage the Google Cloud resources consumed by the product and the billing for the customer account. What steps should you take to manage costs for this product and customer?**
- [ ] Configure quotas and limits for the product folders.
- [ ] Configure the billing account for each project associated with the product.
- [x] Set up budgets and alerts at the project level for the product.
- [ ] Configure the billing account at the product folder level in the resource hierarchy.

**You are considering deploying a solution using containers on Google Cloud. What Google Cloud solutions are available to you that will provide a managed compute platform with native support for containers?**
- [ ] Compute Engine Autoscaling Groups
- [x] Google Kubernetes Engine Clusters
- [ ] Container Registry
- [ ] Cloud Functions

**You are ready to start work building an application in Google Cloud. What IAM hierarchy should you implement for this project?**
- [ ] Create new projects and resources inside departmental folders for the resources needed by the component applications.
- [ ] Create a new organization for the project and create all projects and resources inside the new organization.
- [ ] Create new projects for each of the component applications and create folders inside those for the resources.
- [x] Create a new folder inside your organization and create projects inside that folder for the resources.

**One of the key characteristics of cloud computing is the concept of measured service. What is the primary customer benefit of the measured service aspect of cloud computing?**
- [ ] You can get more resources as quickly as you need them.
- [x] You pay only for the resources you consume.
- [ ] Resources can be allocated automatically.
- [ ] You share resources from a large pool enabling economies of scale.

**You need to write some automated scripts to run periodic updates to the resources in your Google Cloud environment. What tools can you install in your own computers to allow you to run those scripts?**
- [ ] The Google Cloud Console
- [x] The Cloud SDK
- [ ] The Cloud Shell
- [ ] The Cloud Console Mobile app
