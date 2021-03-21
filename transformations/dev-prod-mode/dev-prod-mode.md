---
title: Development/Production Mode Tutorial
permalink: /transformations/dev-prod-mode/
---

* TOC
{:toc}

### Show Project Diff
First, you want to check what are the things that you changed against the production. Go to the dashboard and click the button **Show Project Diff** on the right. There you can see the things you’ve changed. 

{: .image-popup}
![Screenshot - Project Diff](/transformations/dev-prod-mode/tables-project-diff.png)

To see a detailed diff of the changes, click the three dots on the right and then **Compare with production**.

{: .image-popup}
![Detailed diff of configuration change](/transformations/dev-prod-mode/tables-config-changes.png)

Note the message above the project diff. It says that your production storage will not be affected by that merge. This means that tables will not be created in production unless you run the configurations that created them and no data will be transfered from branch to production. You need to keep that in mind - for example if you were to drop a column on a table in the branch, you need to drop that column on the production table as well. 

### Conflicting changes

Switch back to production, navigate to the transformation and change the limit from 5 to 7 and save. This is as if you were working in your branch for some time and your coleague changed the production configuration in the meantime.

Now switch back to your branch and examine the project diff again.

{: .image-popup}
![Conflict in project diff](/transformations/dev-prod-mode/tables-project-diff-conflict.png)

In the diff detail you can see that you changed the limit to 10 but the limit in production changed to 7. 

{: .image-popup}
![Conflict in configuration diff](/transformations/dev-prod-mode/tables-diff-conflict.png)

### Merge to Production
Now click **MERGE TO PRODUCTION** and confirm. If the merge is successful, you will be taken to your production
project. The development branch is deleted when merged.

Examine your production configurations and see how they changed. New version with all the changes from your branch were created for each of them. 

## Working with files







### Switch Back to Production
Let’s go back to production to see that by changing the transformation in the development branch, we did not 
affect the production transformation in any way. 

Go to **Transformations** and check **Shared Codes** to see that there is still the original version of 
`Create a file` that we created at the beginning. It was not overwritten by the changes we made in the development 
branch.

{: .image-popup}
![Screenshot - Check Shared Code In Production](/components/development-branches/19-check-shared-code.png)

Then go to your production transformation and check **Block 1** to see that there, too, is the original short 
version of the shared code. 

{: .image-popup}
![Screenshot - Check Variable In Production](/components/development-branches/20-check-block1.png)

To make sure nothing has changed, run the transformation in production again. When it finishes, go to **Storage -- 
Files**. You can see that `demoFile.txt` was created. Because it ran in production mode, it was assigned the 
tag `demoOutput` without any prefix.

{: .image-popup}
![Screenshot - Storage File in Production](/components/development-branches/21-storage-files-prod.png)

###   Simultaneous Changes
Before we proceed to merging the development branch to the production project, there is one more scenario we need 
to mention here. While you are working in your development branch, someone else might be doing changes to your 
production project at the same time.

Let’s see what this might look like. Go to your production branch, to your transformation, and edit the variable 
`outFileContent`. Instead of `Hello world!` write `Hello tester!`, and then click on the blue check icon. 

{: .image-popup}
![Screenshot - Edit Variable in Production](/components/development-branches/22-edit-var-in-prod.png)

Now we’ve made configuration changes in both branches. Switch back to the development branch and look at the 
project diff. You’ll see a **warning message** saying that there is a conflict between the two configurations. 

{: .image-popup}
![Screenshot - Conflict Warning](/components/development-branches/23-conflict-warning.png)

Click **Compare with production** to see the differences.

{: .image-popup}
![Screenshot - Configuration Changes Differences](/components/development-branches/24-config-changes-diff.png)

The conflict here means that the production project changed while we were working in the development branch. 
`Hello world!` was changed to `Hello tester!`. If you merged the development project to production now, 
the development project would overwrite any changes made in production. In our case, `Hello tester!` would become
`Hello world!` again.

#### Resolve Conflicts
To resolve the conflict and merge the development branch to production safely without overwriting anything new, 
you need to manually replicate the production change in the development branch and turn `Hello world!` to `Hello 
tester!` too.

{: .image-popup}
![Screenshot - Match Change from Production](/components/development-branches/25-match-change-in-prod.png)

Don’t forget to click the blue check icon on the right. When done, go back to the dashboard and look 
at the project diff again. 

{: .image-popup}
![Screenshot - Remaining Changes Diff](/components/development-branches/26-config-remaining-changes-diff.png)

You will still see the configuration as in conflict because it was edited both in production as well 
as in the development branch. However, we had the option to edit the changes and make sure the remaining 
differences are only those that we really want. In this case, we only want the two following changes.

{: .image-popup}
![Screenshot - Config Changes - Variable](/components/development-branches/18-diff-config-changes-var.png)

and

{: .image-popup}
![Screenshot - Config Changes - Shared Code](/components/development-branches/17-diff-config-changes-shared-code.png)

### Merge to Production
Now click **MERGE TO PRODUCTION** and confirm. If the merge is successful, you will be taken to your production 
project. The development branch is deleted when merged. 

Go to your transformation in your production project and let’s see what did or did not change:

In `outFile`, there is still the `Hello tester!` as we changed it in production earlier; it was not overwritten by 
`Hello world!` that was originally in the development branch. The file `demoBranchFile.txt` was successfully 
copied from the development branch.

{: .image-popup}
![Screenshot - Check Production Changes](/components/development-branches/27-check-changes-in-prod-var.png)

The new part of the shared code that you added in the development branch was copied from the development branch as 
well.

{: .image-popup}
![Screenshot - Check Production Changes](/components/development-branches/28-check-changes-in-prod-code.png)

*Note: All events in the development branch are separated from the events in production. They are only displayed 
in the branch where they took place.*
