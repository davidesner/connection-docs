---
title: Working with files in development branch
permalink: /components/development-branches/files/in-branch/
---

* TOC
{:toc}

Create a new development branch. If you skipped the first part of the tutorial regarding tables, you can find [the instructions for creating a branch](http://localhost:4000/components/development-branches/tables/in-branch/#create-a-new-branch) there. In your development branch, let’s run the same transformation you ran in the production branch.

## Run the transformation in development branch 

In your development branch, go to **Transformations**, select the transformation *Testing dev/prod mode* and run it.

{: .image-popup}
![Screenshot - Run Transformation in Development Branch](/components/development-branches/11-option-edit-vars.png)

Notice that there is an option to edit the variables in the *Variables* dropdown box before you run
the configuration. But we don’t need this now.

## File Tags
When the job finishes, go to **Storage -- Files**. You can see that the `demoFile.txt` was created but is was not
assigned the tag **demoOutput**. It was assigned the tag **1189-demoOutput** instead. The prefix **1189** is the ID of
this branch. You can also see the ID in the URL. This is how the development branch files are distinguished from
the files that were created in the production environment.

{: .image-popup}
![Screenshot - Development Branch Output](/components/development-branches/12-dev-branch-output.png)

## Change Configuration
Now it’s time to make the changes to the transformation and test them. Let’s add a bit of code first. Go to
**Transformations** and in **Shared Codes**, select *Create a file*. Then click **Edit Code** and **Let’s Change
the Shared Code**.

{% highlight sql %}
f = open("out/files/{{ outFile }}", "a")
f.write("{{ outFileContent }}")
f.close()

print("Output written to {{ outFile }}")
{% endhighlight %}

{: .image-popup}
![Screenshot - Edit Shared Code](/components/development-branches/13-edit-shared-code-in-dev.png)

The transformation will do exactly the same as before, but it will add `Output written to demoFile.txt` to
the output. Save the shared code. Go back to **Transformations**, select the transformation *Testing dev/prod
mode*, and run the component again.

You can see in the job log that you used this new block.

{: .image-popup}
![Screenshot - Job Log](/components/development-branches/14-jobs-log.png)

The file was uploaded to Storage again.

Now change the name of the output file. To do that, go to your transformation and change the value of the variable
`outFile` from `demoFile.txt` to `demoBranchFile.txt`. Then click the little check mark icon on the right.

{: .image-popup}
![Screenshot - Edit Variable in Development Branch](/components/development-branches/15-edit-var-in-dev.png)
