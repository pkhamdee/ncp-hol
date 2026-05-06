# NKP Advanced Hands-on Lab

Note

-   Expected lab duration: 10 minutes

# [#](#multi-tenancy) Multi-tenancy Lab

There are several ways to design and build multi-tenant solutions with Kubernetes. Each of these methods comes with its own set of tradeoffs that impact the isolation level, implementation effort, operational complexity, and cost of service.

In summary, we could map these multi-tenancy models to two scenarios:

-   **Multiple teams ("soft")**. A common form of multi-tenancy is to share a cluster between multiple teams within an organization, each of whom may operate one or more workloads. This model is based on Kubernetes Namespace resources which maps to NKP Projects.
    
-   **Multiple customers ("hard")**. The other major form of multi-tenancy frequently involves a Managed Service Provider (MSP). The customers workloads run on dedicated clusters for each tenant (customer). This model maps to NKP Workspaces.
    

## [#](#multi-tenancy-in-nkp) Multi-Tenancy in NKP

Multi-tenancy in NKP is an architecture model where a single NKP Ultimate instance serves multiple organization’s divisions, customers or tenants. In NKP, each tenant system is represented by a workspace. Each workspace and its resources can be isolated from other workspaces (by using separate Identity Providers), even though they all fall under a single Ultimate license.

#### [#](#global-or-workspace-ui) Global or Workspace UI

The UI is designed to be accessible for different roles at different levels:

-   _Global_: At the top level, platform engineers or MSP admins manage all clusters across all workspaces
    
-   _Workspace_: platform engineers or tenant admins manage multiple clusters within a workspace
    
-   _Projects_: platform engineers or tenant developers manage workloads configuration and services across multiple clusters
    

![Multitenancy diagram](/cloudnative/assets/multitenancy_diagram.2923c533.png)

In this lab we'll take a high level look at NKP Workspaces and NKP Projects.

Did You Know?

**Multi-tenancy** requires NKP Ultimate