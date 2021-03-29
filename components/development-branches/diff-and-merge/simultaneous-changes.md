---
title: Handling simultaneous changes and conflicts
permalink: /components/development-branches/diff-and-merge/simultaneous-changes/
---

### Simultaneous Changes

Before we proceed to merging the development branch to the production project, there is one more scenario we need
to mention here. While you are working in your development branch, someone else might be doing changes to your
production project at the same time.

Let’s see what this might look like. Go to your production branch, to your transformation, and edit the variable
`outFileContent`. Instead of `Hello world!` write `Hello tester!`, and then click on the blue check icon.

*Note: If you skipped the [file manipulation part](/components/development-branches/files) of the tutorial, you'll need to change your Snowflake transformation instead of the Python transformation.*

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
