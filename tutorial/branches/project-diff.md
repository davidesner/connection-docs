---
title: Project diff
permalink: /tutorial/branches/project-diff/
---

In the previous parts you tested how [files](/tutorial/branches/files-in-branch/) and [tables](/tutorial/branches/tables-in-branch/) behave in branches. Now it's time to complete the branch lifecycle and merge the development branch back to production. 

### Show Project Diff
First, you want to check what are the things that you changed against the production. Go to the dashboard and click the button **Show Project Diff** on the right. There you can see the things you’ve changed.

{: .image-popup}
![Screenshot - Project Diff](/tutorial/branches/figures/project-diff.png)

To see a detailed diff of the changes to `Variables definition for keboola.python-transformation-v2` configuration, click the three dots on the right and then **Compare with production**.

{: .image-popup}
![Detailed diff of configuration change](/tutorial/branches/figures/18-diff-config-changes-var.png)

Note the message above the project diff. It says that your production storage will not be affected by that merge. This means that tables will not be created in production unless you run the configurations that created them and no data will be transferred from branch to production. 
For example, the table `bitcoin_transactions` which you [created in branch](/tutorial/branches/tables-in-branch/#extend-the-transformation) will not be created in production and if the branch is deleted after merge, the table will be discarded as well. 

You need to keep that in mind - for example if you were to drop a column on a table in the branch, you need to drop that column on the production table as well.

In the next part we'll look at a fairly common scenario of [simultaneous or conflicting changes between production and a branch](/tutorial/branches/simultaneous-changes).
