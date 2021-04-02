---
title: Prepare transformations
permalink: /tutorial/branches/prepare-files/
---

* TOC
{:toc}

In the [previous section](/tutorial/branches/tables-in-branch/) you created a branch from configurations in production and you tested how tables behave in branches. In this section you'll learn how [files](/storage/files/) behave in development branches. First you'll create the production configurations, after that you'll edit it in a branch. 

## Prepare the production configurations

You'll create a production Python transformation with a simple code first. 

##  Create Transformation
In your testing project, create a new **Python** transformation and name it, for instance, *Testing dev/prod mode*.

{: .image-popup}
![Screenshot - Create Transformation](/tutorial/branches/figures/01-new-transformation.png)

## Create Shared Code
Then go back to **Transformations** and prepare the following [shared code](/transformations/variables/#shared-code)
in Python for the transformation. Name it *Create a file*.

*Name:* `Create a file`
<br> *Type:* `Python`
<br> *Shared code:*
{% highlight python %}
{% raw %}
f = open("out/files/{{ outFile }}", "a")
f.write("{{ outFileContent }}")
f.close ()
{% endraw %}
{% endhighlight %}

Finally, enter two out variables `outFile`, `outFileContent`, and click **Create Share Code**.

{: .image-popup}
![Screenshot - New Shared Code](/tutorial/branches/figures/02-new-shared-code.png)

## Add Shared Code to Transformation
To add the new shared code to your transformation, go to your new transformation, scroll down to
**Scripts -- Block 1**. Click **Select Shared Code** and select the code you have just created.
Click **Use as Shared Code**.

{: .image-popup}
![Screenshot - Add Shared Code](/tutorial/branches/figures/03-add-shared-code.png)

Now move up to the section **Variables** and prepare the variables for your shared code.
Click to **initialize the variables** with empty values.
<br> `outFile`: `demoFile.txt`
<br> `outFileContent`: `Hello world!`

{: .image-popup}
![Screenshot - Initialize Variables](/tutorial/branches/figures/04-initialize-variables.png)

## Set Output Mapping
Now go to the section **File Output Mapping** and click **New File Output**. And because the output of the
transformation will be the file `demoFile.txt`, let’s set it as *Source* and `demoOutput` as *Tags*. This means
that the output will be stored in Storage as `demoFile.txt` with the tag `demoOutput`. Click **Add File Output**.

{: .image-popup}
![Screenshot - Set Output Mapping](/tutorial/branches/figures/05-output-mapping.png)

## Run Transformation
Let’s run the component.

{: .image-popup}
![Screenshot - Run Transformation](/tutorial/branches/figures/06-run-component.png)

After the job is finished, go to **Storage -- Files**, where you can see the file `demoFile.txt` generated.

{: .image-popup}
![Screenshot - Generated File ](/tutorial/branches/figures/07-generated-file.png)

Now you have everything ready, you created production configuration using shared codes that creates files in storage. You can proceed to the next step - [working with files in a branch](/tutorial/branches/files-in-branch/). 
