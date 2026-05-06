## Migrate Share

1.  Once it's started, the plan is validated and then progresses once the validation succeeds. Click `In Progress`
    
    ![](/images/files-migration-plan14.3f2dfeaf.png)
    
2.  You can monitor the migration status here. Like before with VMs, it will go through seeding. This will be quick as the amount of data to migrate is small. Once seeding is complete, the status becomes `Ready to Cutover`.
    
    This should take 1-2 minutes.
    
    ![](/images/files-migration-plan15.d8da0144.png)
    
3.  To cutover, Select the source and Click `Actions` and then `Cutover`. Click `Confirm` for confirmation.
    
    ![](/images/files-migration-plan16.5077c8db.png)
    
4.  Once cutover is complete, the status will show **Completed**
    
    ![](/images/files-migration-plan17.76ef5edf.png)
    

## View Target Share

Now that we have completed the migration, let’s look at the target share from our Windows client.

1.  Log into your Remote Desktop Session. If needed, please refer to instructions in [the appendix](/migrate/appendix/remote_desktop.html).
    
2.  Open the target share by typing `\\nextfiles\user##` within the **Search** field and pressing **Enter**.
    
    ![](/images/files-migration-plan18.0108d0b5.png)
    
3.  The target share has the 100MB file we saw earlier on the source share.
    
    This target share can now be mapped as a network share by users. Any new files they create will be stored on the new share within Nutanix Files.
    
    ![](/images/files-migration-plan19.43ca02f3.png)
    

🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉🎉

CONGRATULATIONS!! Now you have moved your file shares onto Nutanix, and consolidated your infrastructure onto a single platform.