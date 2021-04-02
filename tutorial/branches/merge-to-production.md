---
title: Merge to production
permalink: /tutorial/branches/merge-to-production/
---

In the previous section you learned about [conflicting changes and how to resolve them](/tutorial/branches/simultaneous-changes/). Now you're finally at the point where you merge the branch to production. You can either do full or partial merge. Let's start with a partial merge. 

## Partial merge

First, let's examine the project diff further. 

{: .image-popup}
![Screenshot - Match Change from Production](/tutorial/branches/figures/project-diff-after-reset.png)

You can see there are checkboxes to the left of each configuration in the list. This allows you to merge one a subset of the changes. So let's say we only want to merge the HTTP extractor change regarding bitcoin transactions. Uncheck all the checkboxes except the one near the **"Bitcoin" http extractor**.

*Note: **Don't click the merge button** when you click Merge to production in the next step unless you uncheck the checkbox. If you accidently do you'll need to recreate the whole branch.*  

Click **Merge to production** and in the modal uncheck the *Delete current development branch after merge.* checkbox. Only then click the **Merge** button.

{: .image-popup}
![Screenshot - Match Change from Production](/tutorial/branches/figures/partial-merge-dialog.png)

When you do a progress bar will show up informing you of the progress of the merge. When the merge is finished, you will see only 3 changed configurations in your branch.

{: .image-popup}
![Screenshot - Match Change from Production](/tutorial/branches/figures/partially-merged-branch.png)

Switch to production and examine the HTTP extractor configuration. Notice that a new version has been created with the merge message as change description. 

{: .image-popup}
![Screenshot - Match Change from Production](/tutorial/branches/figures/merged-http-ex-version.png)

If you go to **Storage**, you'll see that the bucket used by this configuration still only has the `bitcoin_price` table and the `bitcoin_transactions` table is missing, even though you had it in your branch and you merged the config. This is expected. Branch storage is completely isolated and no data are merged back to production. You need to run the extractor in production to get the data in production **Storage**.

While you're in the **Storage** section, examine that the branch buckets are still available if you toggle the switch to show development branch buckets. 

### Full merge

Switch back to your branch. Merge the remaining configurations, also keep the *Delete current development branch after merge.* checkbox checked. The merge will take slightly longer as the whole branch is being deleted. Afterwards you'll be redirected back to production.

{: .image-popup}
![Screenshot - Match Change from Production](/tutorial/branches/figures/branch-deleted.png)

Examine the merged configurations, try running them and examine the results. 

## Tada! You did it. 

This concludes the tutorial. You learned how to create branches and how tables and files work in branches. You learned about different scenarios when merging a branch and how to handle conflicting changes. 
