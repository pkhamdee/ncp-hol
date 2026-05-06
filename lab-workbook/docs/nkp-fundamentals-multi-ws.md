# NKP Advanced Hands-on Lab

# [#](#nkp-workspaces) NKP Workspaces

Workspaces align with "hard" multi-tenancy by allowing teams or tenants to manage their own clusters. Workspaces are a logical grouping of clusters that maintain a similar configuration, with certain configurations automatically federated to those clusters.

#### [#](#dedicated-login-url-for-each-tenant) Dedicated login URL for each tenant

NKP provides workspaces with a dedicated login page, where users in the NKP UI can enter their SSO credentials to access their workspace or create a token to access a cluster’s kubectl API. Other tenants and their SSO configurations are not visible.

The URL format is https://`<DOMAIN>`/token/landing/`<WORKSPACE_NAME>`

#### [#](#default-workspace) Default Workspace

To get started immediately, NKP deploys a default workspace when applying an NKP Ultimate license. However, take into account that you cannot move clusters from one workspace to another after creating/attaching them.

In this lab you won't be creating workspaces, but instead check the default workspace and the managed clusters staged for your upcoming labs.

1.  Go back to the NKP management UI. If you closed the tab, you can open it using https://#.#.#.**16**/dkp/kommander/dashboard (replace #.#.# with your subnet - available in the initial connection details page)
    
2.  Go to the NKP **Global** scope, select workspaces on the sidebar menu, and take a look to the information provided on the page
    
    ![List of workspaces](/cloudnative/assets/workspaces_list.328f3ace.png)
    
3.  Click on the `Default Workspace` to change the scope
    
    Pro tip
    
    You can also use the navigation bar.
    
    ![Workspace menu](/cloudnative/assets/workspace_menu.21405c4c.png)
    
4.  On the workspace page, you can easily identify the number of clusters and their cost, projects, or the aggregated analytics of your workspace clusters reported by NKP Insights
    
    ![Workspace dashboard](/cloudnative/assets/workspace_dashboard.a5f37201.png)