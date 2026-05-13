# Category Creation & VM Association

## Create a Category

1.  In the **Core** Prism Central, select **\> Administration > Categories**.
    
2.  Select **New Category** in the top left of the window. ![image](images/new_category.2d24dcff.png)
    
3.  In the **Name** text field, type **DR-RPO-User##** where ## corresponds to your user number.
    
4.  The **Purpose** field is optional but helps easily identify how the category is used.
    
5.  In the **Values** field, input **1hr** and then click **Save**.
    

!!! note

    You can create multiple values corresponding to different RPOs. As an example, 1m, 1hr, 6hr, 24hr. These text values can be used to map entities to their corresponding protection policies.

## Associate Category with VM

1.  In the **Core** Prism Central, select **\> Compute & Storage > VMs**.
    
2.  Select the checkbox next to the VM **Linux-User##** where **##** corresponds to your user number.
    
3.  Click **\> Actions > Manage Categories**
    
4.  In the search box under **Set Categories**, type in the name of the category created earlier, **DR-RPO-User##**. Click the plus sign.
    
5.  Click **Save**.
    

## Next Steps

Now our category is associated with this VM. Any policies that reference this category will also apply to this VM. Let's create the policies next.

[← Back: Setup](edge-lab-scenario3-setup.md) | [Home](edge-getting-started.md) | [Next: Create a Protection Policy →](edge-lab-scenario3-protect.md)