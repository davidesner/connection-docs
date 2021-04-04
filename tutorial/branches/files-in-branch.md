---
title: Working with files in development branch
permalink: /tutorial/branches/files-in-branch/
---

* TOC
{:toc}

In the [previous section](/tutorial/branches/prepare-files/), you created production configurations that create files in storage. In this section, you'll create a new branch and examine how files behave in the context of branch. 

Create a [new branch](/tutorial/branches/tables-in-branch/#create-a-new-branch). In your development branch, let’s run the 
same Python transformation you ran in the production branch. 
*Note that changes made in production are not reflected in the branches. That is the reason why you need to create another branch now, because the branch [created previously](/tutorial/branches/tables-in-branch/) will not contain the 
[new transformation](/tutorial/branches/prepare-files/) working with files.*


## Run the transformation in development branch 

In your development branch, go to **Transformations**, select the transformation *Testing dev/prod mode* and run it.

{: .image-popup}
![Screenshot - Run Transformation in Development Branch](/tutorial/branches/figures/11-option-edit-vars.png)

Notice that there is an option to edit the variables in the *Variables* dropdown box before you run
the configuration. But we don’t need this now.

## File Tags
When the job finishes, go to **Storage -- Files**. You can see that the `demoFile.txt` was created but is was not
assigned the tag **demoOutput**. It was assigned the tag **1189-demoOutput** instead. The prefix **1189** is the ID of
this branch. You can also see the ID in the URL. This is how the development branch files are distinguished from
the files that were created in the production environment.

{: .image-popup}
![Screenshot - Development Branch Output](/tutorial/branches/figures/12-dev-branch-output.png)

## Change Configuration
Now it’s time to make the changes to the transformation and test them. Let’s add a bit of code first. Go to
**Transformations** and in **Shared Codes**, select *Create a file*. Then click **Edit Code** and **Let’s Change
the Shared Code**.

{% highlight python %}
{% raw %}
f = open("out/files/{{ outFile }}", "a")
f.write("{{ outFileContent }}")
f.close()

print("Output written to {{ outFile }}")
{% endraw %}
{% endhighlight %}

{: .image-popup}
![Screenshot - Edit Shared Code](/tutorial/branches/figures/13-edit-shared-code-in-dev.png)

The transformation will do exactly the same as before, but it will add `Output written to demoFile.txt` to
the output. Save the shared code. Go back to **Transformations**, select the transformation *Testing dev/prod
mode*, and run the component again.

You can see in the job log that you used this new block.

{: .image-popup}
![Screenshot - Job Log](/tutorial/branches/figures/14-jobs-log.png)

The file was uploaded to Storage again.

Now change the name of the output file. To do that, go to your transformation and change the value of the variable
`outFile` from `demoFile.txt` to `demoBranchFile.txt`. Then click the little check mark icon on the right. Don't forget to change the **File Output Mapping** of the transformation as well!  

{: .image-popup}
![Screenshot - Edit Variable in Development Branch](/tutorial/branches/figures/15-edit-var-in-dev.png)

## Switch Back to Production
Let’s go back to production to see that by changing the transformation in the development branch, we did not
affect the production transformation in any way.

Go to **Transformations** and check **Shared Codes** to see that there is still the original version of
`Create a file` that we created at the beginning. It was not overwritten by the changes we made in the development
branch.

{: .image-popup}
![Screenshot - Check Shared Code In Production](/tutorial/branches/figures/19-check-shared-code.png)

Then go to your production transformation and check **Block 1** to see that there, too, is the original short
version of the shared code.

{: .image-popup}
![Screenshot - Check Variable In Production](/tutorial/branches/figures/20-check-block1.png)

To make sure nothing has changed, run the transformation in production again. When it finishes, go to **Storage --
Files**. You can see that `demoFile.txt` was created. Because it ran in production, it was assigned the
tag `demoOutput` without any prefix.

{: .image-popup}
![Screenshot - Storage File in Production](/tutorial/branches/figures/21-storage-files-prod.png)

If you go back to your development and run the transformation, it will create a new file `branchDemoFile.txt` as expected. 

This concludes the file manipulation part of this tutorial. You examined how files and shared codes behave in branches. You can continue to the next part - [merging back to production](/tutorial/branches/project-diff). 
