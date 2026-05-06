# Application Policies

Application Policies are the primary method for controlling network access with Flow Network Security. They're flexible because they allow specification of the exact incoming and outgoing allowed traffic.

Application policies are evaluated after Quarantine, Isolation, and Shared Service policies.

## The Development Application

These instructions will help us protect our development application. Let's revisit the security requirements and get started allowing the required traffic.

1.  In this application, we need to allow inbound access to our dev web VMs from the Parallels client and the Internet on TCP port 80.
    
2.  Our end-user developer VM should only be able to access the Development web server via TCP port 80.
    
3.  The developer VM should also be able to access the Database via TCP 22 and TCP 5432.
    
4.  Within our application, we need to allow our Web VMs to communicate to the Database VMs via port 5432.
    

Our developer web VM has the following three categories assigned

-   **AppTier: User`##`Web**
-   **AppType: User`##`ToDo**
-   **Environment: User`##`Dev**

No other sources are permitted to communicate with our database tier.

There are no requirements to restrict outbound access. That's handled by a physical firewall at the corporate perimeter.

### Creating the Development Security Policy

#### Process Overview

A security policy consists of the following items:

1.  Secured Entity - This is the item at the center of the application policy. This is what we're protecting.
2.  Inound Sources - Items on the left side of the policy. This is what we allow in.
3.  Outbound Destinations - Items on the right side of the policy. This is what we allow out.
4.  Rules - These are the allowed ports and protocols between entities.

Our process below is to

1.  Create all the Secured Entities
2.  Create all the Inbound Sources
3.  Create all the Rules between Inbound Sources and Secured Entities

It's possible to do this differently by creating the Secured Entities, then a single inbound, and then the rules from that inbound to the secured entity. In the end, the outcome is the same.

#### Create the Policy

1.  In Prism Central, make sure you're in **Infrastructure** and navigate to **Network & Security** > **Security Policies**.

![Create Security Policy](/images/create-dev-1.242f3297.png)

2.  Select **\+ Create Security Policy**.
    
    -   Name your security policy **User`##`\-DevSecurityPolicy**
    -   Provide a description for this development security policy
    -   Your policy type is Application : Secure Entities

![Name Security Policy](/images/create-dev-2.9677137c.png)

3.  Before we proceed, note the additional settings for policy hit logs and IPv6. Leave these at the default values.
    
    -   Hit logs are enabled on a per-policy basis and generate Syslog messages for allowed and denied connections.
    -   The IPv6 policy is new in Flow Network Security 7.5 and allows creation of IPv6 rules.
4.  Select Next in the lower right corner to proceed.
    

#### Secured Entity Definition - Web

Next, we'll define the secured entities at the center of our application security policy. This is what we're protecting with the policy.

1.  Select **\+ Add Secured Entity**.

![Name Security Policy](/images/create-dev-3.81fedcec.png)

We are using multiple categories to define our secured entity.

Let's start by defining the web tier for the **User`##`Dev** ToDo application.

2.  In the category drop down (this is searchable), select these categories for the Web Tier of the secured entity:

Note:

When searching for categories, use autocomplete to save key strokes.

Type your user number followed by the next letter of the category. For example `01w` would autocomplete to `AppTier: User01Web`.

-   **AppTier: User`##`Web**
-   **AppType: User`##`ToDo**
-   **Environment: User`##`Dev**

![Name Security Policy](/images/create-dev-4.a6be5e2c.png)

3.  After adding all three categories to the same VM entity, select **Apply**
    
4.  Hover over the Secured Entity to view all of the assigned category values.
    

#### Secured Entity Definition - Database

Next, we will define our **Database Tier**.

1.  Select **\+ Add Secured Entity**

![Name Security Policy](/images/create-dev-5.b0bb2a2d.png)

2.  In the category drop down, select all three of these categories to define the database tier

-   **AppTier: User`##`DB**
-   **AppType: User`##`ToDo**
-   **Environment: User`##`Dev**

![Name Security Policy](/images/create-dev-6.dde2cc5e.png)

3.  Select **Apply**

#### Inbound Source Requirements

Now, we will define the inbound sources and the unique traffic, via TCP port, that will be permitted to communicate with out ToDo application.

Inbound sources can be Categories, Subnets, VPCs, Address Groups, Specific Network Addresses, or Entity Groups.

In our policy, we have the following inbound requirements:

-   The **Web Tier** must be able to communicate to the **DB Tier** via **TCP port 5432**.

![Name Security Policy](/images/create-dev-7.05992657.png)

-   The **User##-developer** desktop needs to be able to access the **Web Tier** via **TCP port 80**. This desktop also needs to access the **DB Tier** via **TCP port 22** and **TCP port 5432**.

![Name Security Policy](/images/create-dev-8.410ff0e5.png)

-   In addition, we need to allow traffic from the **Internet** and the **Parallels subnet** to the **Environment:Dev** ToDo app **Web Tier**.
    
    -   This traffic must be **restricted to TCP port 80**.

![Name Security Policy](/images/create-dev-9.19b47ec3.png)

#### Add the Web Source for Connections to DB

1.  On the Inbound side of the policy on the left, select **\+ Add Source**.

![Name Security Policy](/images/create-dev-10.1d339e97.png)

2.  In the **Add Entity**, Entity dropdown, select **VM**.
    
3.  In the Category section, select the following categories:
    

-   **AppTier:User`##`Web**
-   **AppType:User`##`ToDo**
-   **Environment: User`##`Dev**

![Name Security Policy](/images/create-dev-11.84527c49.png)

4.  Select **Apply**

#### Add The Internet Source

1.  On the Inbound side of the policy on the left, select **\+ Add Source**.
    
2.  In the **Add Entity**, Entity dropdown, select **Address Group**.
    
3.  In the **Address Group** dropdown, select the address group **Public-Internet-Addresses**.
    

![Name Security Policy](/images/create-dev-12.feadd88d.png)

4.  Select **Apply**

#### Add The Parallels Source

Now we will add the Parallel Sessions address group as another Inbound source

1.  On the Inbound side of the policy on the left, select **\+ Add Source**.
    
2.  In the **Add Entity**, Entity dropdown, select **Address Group**.
    
3.  In the **Address Group** dropdown, select the address group **Parallel-Sessions**.
    

![Name Security Policy](/images/create-dev-13.f8e2ea3b.png)

4.  Select **Apply**.

#### Add The Dev Desktop Source

Finally, we will add our developer desktop as an inbound source

1.  On the Inbound side of the policy on the left, select **\+ Add Source**.
    
2.  In the **Add Entity**, Entity dropdown, select **VM**.
    
3.  In the **Category** dropdown, select the following categories:
    

-   **AppType:User`##`Desktop**
-   **Environment: User`##`Dev**

![Name Security Policy](/images/create-dev-14.8cfa2075.png)

4.  Select **Apply**.

#### Add Rules From Internet to Web

Now that we have added our inbound sources, it is time to configure the unique traffic ports allowed to our ToDo application.

1.  On the left side of the policy, select the Inbound source: **Network Address Group Public-Internet-Addresses** by clicking on it once to select it.

![Name Security Policy](/images/create-dev-15.5d66611f.png)

2.  Next, without clicking anywhere else, click the **plus sign +** of the Secured Entity **AppTier:User`##`Web**.

![Name Security Policy](/images/create-dev-16.843d9784.png)

3.  In the Configuration Traffic Filtering Section select **Allow Specific Traffic**.

Note:

Adding a description to the rule makes it easier to see the purpose of this rule when viewing the policy or editing it later.

4.  In the Protocol-Ports / Services area select:
    
    -   **Add New Protocol-Port / Service**
    -   Select **TCP**
    -   Enter port **80**

![Name Security Policy](/images/create-dev-17.e59a4b9b.png)

5.  Select **Save** to save this inbound rule.

#### Add Rules from Parallels to Web

Repeat those steps for the Inbound source Network Address Group Parallels-Sessions

#### Add Rules from Developer Desktop to Web

Next we will configure the rules for access from our our **developer desktop**.

1.  On the left side of the policy, click the Inbound source to select:

-   **AppType: User`##`Desktop**
-   **Environment: User`##`Dev**

2.  Select the plus sign **+** of the **Secured Entity** **AppTier: User`##`Web** to add a rule.
    
3.  In the Traffic Filtering Section select:
    
    -   **Allow Specific Traffic**
4.  In Protocol-Ports / Services area select:
    
    -   **Add New Protocol-Port / Service**
    -   Select **TCP**
    -   Enter port **80**

![Name Security Policy](/images/create-dev-18.336bc202.png)

5.  Select **Save**

#### Add Rules from Developer Desktop to DB

1.  On the left side of the policy, click the Inbound source to select:

-   **AppType: User`##`Desktop**
-   **Environment: User`##`Dev**

2.  Select the **plus sign** **+** of the Secured Entity **AppTier:User`##`DB**.
    
3.  In the Traffic Filtering Section select **Allow Specific Traffic**
    
4.  In Protocol-Ports / Services select:
    
    -   **Add New Protocol-Port / Service**
    -   Select **TCP**
    -   Enter port **22**
5.  Press the return key to add an additional TCP port.
    
    -   Enter **port 5432**

![Name Security Policy](/images/create-dev-19.f0663b50.png)

6.  Select **Save**.

#### Add Rule from Web to DB

Finally, we need to allow our **Web tier** to communicate with our **database tier** on **TCP port 5432**.

1.  On the left side of the policy, click the Inbound source to select:
    
    -   **AppTier:User`##`Web**
    -   **AppType: User`##`ToDo**
    -   **Environment: User`##`Dev**
2.  Select the **plus sign +** of the Secured Entity **AppTier:User`##`DB**.
    
3.  In the Traffic Filtering Section select **Allow Specific Traffic**.
    
4.  In Protocol-Ports / Services select:
    
    -   **Add New Protocol-Port / Service**
    -   Select **TCP**
    -   Enter port **5432**

![Name Security Policy](/images/create-dev-20.c0b45b15.png)

5.  Select **Save**.

!!! note

We could also create traffic rules using pre-built services such as http or ssh instead of directly specifying the TCP port numbers.

#### Check the List or Table View

So far, we've built all of our rules in the visual view. Flow Network Security also offers a list view for creating and viewing application security policies.

1.  In the upper right, navigate to the list view by clicking **List** to see the policy you've just created in table form.

![Tabular FNS View](/images/create-dev-20a.fd00310a.png)

2.  Navigate back to the visual view by clicking **Visual**

#### Review and Save The Policy in Monitor Mode

Once all of the inbound rules have been created, select **Next** in the lower right corner.

You will see the ways in which this policy can be stored. We are going to place our policy in monitor mode.

1.  Select the **Apply (Monitor)** radio button.
    
2.  Select **Confirm** in the lower right corner.
    

![Name Security Policy](/images/create-dev-21.d6cd1499.png)

## Takeaways

The application policy allows in specific traffic. Anything that isn't allowed is dropped. We have saved this particular policy in a monitor mode, so it will still allow traffic and any exceptions to the policy will be visualized.

This is a great policy mode for building new policies without impacting your applications.