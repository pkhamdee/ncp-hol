# Untitled Page

# [#](#create-a-migration-plan) Create a Migration Plan

Next we will create a migration plan in Move for the share.

1.  Login to the Move VM or go back to Move VM tab if its open.
    
    -   **login name** : nutanix
    -   **password** : `dotNext25! or whatever password you gave when setting up Move`
2.  Click the `Files` tab.
    
    ![](/images/files-migration-plan1.bc89bc27.png)
    
3.  Click `New Migration Plan`
    
    ![](/images/files-migration-plan2.e7af219e.png)
    
4.  Enter a name for the Plan `User##-Files-Plan`. Click `Proceed`.
    
    ![](/images/files-migration-plan3.2b905a8f.png)
    
5.  Let's add our target Nutanix Files Server. Click `Add New Target`.
    
    ![](/images/files-migration-plan4.b52d27f8.png)
    
6.  Add the target Nutanix File Server details. Click `Add`.
    
    -   **Name** : Give any name you like. We suggest **Nutanix Files**.
    -   **FQDN Details/Address** : IP Address of nextfiles that you noted in [step 4 of View Source Share](/migrate/migrating-your-workloads/migrating-shares-view-source.html).
    -   **username** : `adminuser##` . _Make sure this is the same username that you used for creating the REST API user in previous section._
    -   **password** : `dotNext25! or whatever password you provided when creating the REST API user`
    
    ![](/images/files-migration-plan5.7b041e09.png)
    
7.  Next let's add our source File Server. Click `Add New Source`
    
    ![](/images/files-migration-plan6.d818f352.png)
    
8.  Add the Source Details. Click `Add`.
    
    -   **Name** : Give any name you like. We suggest **Source Filer**.
    -   **FQDN Details/Address** : `WinFS-Next.ntnxlab.local`
    -   **Username** : `ntnxlab\adminuser##`
    -   **password** : `<Admin user password>`
    
    ![](/images/files-migration-plan7.b4fc7c81.png)
    
9.  After the target and source are added, you can select the replication type. We will keep it sync.
    
    -   **Sync**: Sync replication updates target files with changes from the source, without deleting existing target files.
    -   **Mirror**: Mirror replication creates an exact copy of the source in the target, potentially deleting existing target files not present in the source.
    
    ![](/images/files-migration-plan8.f09d7bdf.png)
    
10.  Next, let's add the share to be migrated.
    
    ![](/images/files-migration-plan9.c921046f.png)
    
11.  Fill the following Details. Click `check mark` and then `Next` at the bottom.
    
    -   Before filling in the **Source** and **Target** click the blue `Refresh Inventory` option. If you don't click Refresh Inventory, you might not see any available sources or targets.
    -   **Source** : `nextlab`
    -   **Target** : `user##`. This is same share that you created in previous section
    
    ![](/images/files-migration-plan10b.7109b372.png)
    
    ![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAF8AAABqCAYAAADa+43rAAAABGdBTUEAALGPC/xhBQAAAAlwSFlzAAAOwwAADsMBx2+oZAAAA/xJREFUeF7tm89rE0EYhv0zPIsn/4ZCrh5KERQ85FbQIqIUb73oTbDUQsFLLfVQEQx4sRcLVrRFrAgGpD8Eg6UVpBIpIW3a7mY3+zmzO2s3cWuTsvqGyfvCA83ObvbbZzbfJCU5tVPbF4KB8oFQPhDKB0L5QCgfCOUDoXwglA+E8oFQPhDKB0L5QCgfCOUDoXwglA+E8oFQPhDKB0L5QCgfCOUDoXwglA+E8oFQPhDKB0L5QCgfCOUDoXwglA+E8oFQPhDKB0L5QCgfCOUDoXwglA+E8oFQPhDKB0L5QCgfCOUDoXwgp4SBhfKBoXxgKB8YygeG8oGhfGAoHxjKB4bygaF8YCgfGMoHhvKBoXxgMpdfn5+X/ZER2R0YkJ2+PivZPX9e9m/cELdQEHFdc+WdJzP5we5uWFBasTZTy+elsbVlLHSWzOQ7Y2OpxfUC+qY7STKRr2c+rahewnv/3thoP5nI130+WYju96WXS3J1wZVzTw/kzBO7uDBelIXL15uu2Xn82NhoP5nId6anmwr5PjphpfQko8OTTdesHXSafyL/2e2HqQXbRNfKH781mVqwTXStfF1YWsE2QflAKB8I5QOhfCCUD4TygVgs35N3+j+1QSDP37SMrTXEUUOlUsv2/4zF8n0pmeeWPV8Gk2Nfg3Dzt003sf9J8KSoJtjZ9lPGjqc35Ks4Ze9wLDP55hw1X/Kp43/Hfvl7gZT88BRSXHaisVb5s3V5XgnEiTaLuIHMfYj2vbgebYyPzS03pKoel7cDKYcjcdQxi8nzH4/98tVdOViMhGlBhVk11iTflYIe9AN5vVyX3Iu6zO3oUbPvK/U84eSptWPJ/K32LYTrCO/8I0iKceTuD3Nb68lIyv8YTUz1hye5+NilRnhXx6+M3O/J0wmkuBa3K8o/glYxrszVwlNJWbUinVCumQjlVBzvEJ3qVrxOODJTibZJ1Zf+I8/RGT0kX/FGbTP9XyeU/yl626nv/D+fI0L3eb2PniCd0lezdlD+UaSLyZfUwhqeMW4rdXl9oB+pnq/eN+ZmXbm7rtqO25AZfUw8Yerx1Fvz2UHtO5fs+U5D7quBw1dEe1gs35NVJc2ptN6Vcf8PZPWz6d1qkW16t6MW1NJ3T4bVgnttM9o37vNx/49akitTP+ODkmtBe1gsv/uhfCCUD4TygVA+EMoH0rXyJ24+SC3YJrpGvv6eerKQd5eupBZsE4+G7jVdM0y+/+VLUyG9iP6ycKfJRL7+dcbe0FBqUb1ApX9Agu1tI6P9ZCNfpbGxEf5cJq042/EWF42FzpKZfB09+wd37qQWaCP61e6vrJir7zyZymc6C+UDQ/nAUD4wlA8M5YNy+vRZ+QVxy5kxDzqgoAAAAABJRU5ErkJggg==)
    
12.  If the migration needs to be scheduled for a specific date and time, that can be done on the settings page. We will keep it unchecked and click `Next`
    
    ![](/images/files-migration-plan12.35a0b082.png)
    

13.  On the Summary page, the plan can be Saved for later. We will `Save and Start` the migration.
    
    ![](/images/files-migration-plan13.e8fa4e88.png)