# Create a Protection Policy

1.  In the **Core** Prism Central, select **\> Data Protection > Protection Policies**.

    !!! note

        If you are the first user to get to this portion of the lab, you will see the Protection Policies overview screen.

        Click **Create Protection Policies** to dismiss this screen.

2.  In the **Policy Name** field, type **ProtectionPolicy-##** where ## is your user number.
    
3.  In the **Location** drop-down field for **Primary Location**, select **Local AZ**.
    
4.  In the **Cluster** drop-down field for **Primary Location**, select the local **Core** Cluster and click **Save**.
    
    ![image](images/primary_location_core.932f4297.png)
    
5.  In the **Location** drop-down field for **Recovery Location**, select IP Address that corresponds to the **Cloud** Prism Central.
    
6.  In the **Cluster** drop-down field for **Recovery Location**, select the **Cloud** cluster and click **Save**.
    
7.  In the **Values** field, input 1hr and then click save.
    
8.  Click **Next**
    
9.  In the search box in the **Categories** pane to the left, type in the name of the category that you previously created, **DR-RPO-User##:1hr**. Select the check box that corresponds to your User Number and click **Add**.
    
10.  Click **Create**
    

## Next Steps

The protection policy that selects your specific VM entity is now created. Snapshots are created for your VM on the local **Core** and remote **Cloud** cluster at your configured one-hour RPO. An initial snapshot is also created.

[← Back: Create Category & Associate VM](edge-lab-scenario3-cat.md) | [Home](edge-getting-started.md) | [Next: Create a Recovery Plan →](edge-lab-scenario3-recover.md)