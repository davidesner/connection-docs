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
