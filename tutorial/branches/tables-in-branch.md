---
title: Working with tables in development branch
permalink: /tutorial/branches/tables-in-branch/
---

* TOC
{:toc}

In the [previous section](/tutorial/branches/prepare-tables/) you prepared the configurations in production. In this section you'll manipulate the configurations, run them and learn about how tables behave in branches.  

Lets say that you have a second look at the [previously created configurations](/tutorial/branches/prepare-tables/), 
you realized that you need top 10 values instead of top 5 and you need to convert the amounts of bitcoin to dollar amounts 
based on the value of bitcoin on the given day.

You don't want to change the current transformation yet though. Perhaps it is a critical transformation in production,
perhaps you want to check the results first. In either case this is where
branches come in, giving you the option to test the changes safely before merging them to
production.

## Create a new branch

To create a new branch, click on your project’s name at the top of the screen (for the purposes of this tutorial,
the production project is called *Dev branches demo*). Then click on the green icon **New** displayed next to
your project’s name.

{: .image-popup}
![Screenshot - Create Development Branch](/tutorial/branches/figures/08-create-dev-branch.png)

Name your new development branch, for example, *My dev branch*, and click **Create Development Branch**  to open it.

{: .image-popup}
![Screenshot - Name Development Branch](/tutorial/branches/figures/09-name-dev-branch.png)

Your new branch will appear right below the name of your production project.

{: .image-popup}
![Screenshot - Created Development Branch](/tutorial/branches/figures/10-dev-branch-created.png)

## Change the transformation

Let's start with the easier task - we need to change the transformation so that it returns top 10 rows instead of top 5. In your branch, navigate to **Transformations**. You can see the previously created transformation, it is however a copy living in the branch.
That you are working with the branch copy you can verify in the page heading. 

When you are in a branch, change the query limit from `LIMIT 5` to `LIMIT 10` in the transformation code and save it.

Feel free to check the original transformation did not change. You can do so by switching back to the project in the top menu.
The production transformation still contains `LIMIT 5`. Now that you verified that branch transformation can be changed independently,
switch again to the branch you created (*My dev branch*).

Navigate back to the transformation. Try running it. Examine the **Mapping** section in the job detail.

{: .image-popup}
![Mapping in branch](/tutorial/branches/figures/mapping-in-branch.png)

Notice how in the **Input** section even though you did not run the extractor in this branch, the data are still loaded from the `bitcoin_price` table. What happens is that **Keboola Connection checks whether a branch version of the table exists** and if it does not, it **falls back to read from production version**. 

Also notice that **Output** shows small yellow branch icon, and the name of the bucket is prefixed with a number -- `1400-bitcoin`. When you run a component in a branch and the component wants to write to Storage, the outputs do not overwrite production data. Instead a **duplicate bucket prefixed with branch ID** is automatically created. Examine the table and check that it indeed has 10 values as it should.

*Note: The number in bucket name will be different for you as you have different branch ID.*

This part of your task is done. Let's get to the second part.

## Download the transactions

Second objective is to produce a list of transactions showing the amount in both BTC and in USD. So you need to extract the account data first. Use the [prepared CSV file](/tutorial/branches/bitcoin_transactions.csv) with sample transactions.

In the branch navigate to your existing HTTP extractor configuration and add a new table named `bitcoin_transactions` and fill **Path** with `/tutorial/branches/bitcoin_transactions.csv`. Save and run the extractor.

Examine the job outputs. There are tables with branch icon in a bucket prefixed with a number. As we've already shown, this signifies that the output was stored in branch context, keeping your production data intact.  

{: .image-popup}
![Extractor output](/tutorial/branches/figures/extractor-output.png)

If you navigate to **Storage Explorer** in the branch you'll see that the created buckets are shown there with an icon as well.

{: .image-popup}
![Extractor output](/tutorial/branches/figures/dev-branch-storage.png)

Also, when you switch back to production and navigate to the **Storage Explorer** you'll see a switch to show the branch buckets.

{: .image-popup}
![Storage explorer in production](/tutorial/branches/figures/storage-dev-buckets.png)

*Note: There is only one storage in your project, branch buckets are created with a prefixed name, but are stored along normal buckets and are visible in production as well as in branch. You can see all buckets from all development branches in production.*

Let's get back to the transformation and run it again. Check the input mapping and 
verify that the `bitcoin_prices` table from the branch was used as input. 

{: .image-popup}
![Input mapping from branch data](/tutorial/branches/figures/input-mapping-from-branch.png)

What happened is that Keboola Connection checked whether a branch version of the bucket existed and since it did (you ran the branch version of the HTTP extractor), it was used in the transformation.

## Extend the transformation

Now add a second code to `Block 1` named `Dollar values of transactions` and insert the following SQL

```sql
CREATE TABLE "dollar_btc_transactions" AS 
SELECT 
    BT.DATE, 
    BT.AMOUNT AS BTC_AMOUNT, 
    BP.PRICE AS RATE,  
    BT.AMOUNT * BP.PRICE AS USD_AMOUNT
FROM 
    "bitcoin_transactions" AS BT
LEFT JOIN 
    "bitcoin_price" AS BP
        ON BP.DATE = BT.DATE;
```

We're loading the data from `bitcoin_transactions` table, so we need to add the table to input mapping.

{: .image-popup}
![Input mapping from branch data](/tutorial/branches/figures/transformation-branch-input-mapping.png)

Notice the branch icon next to the table name. You're referring to adding the branch version of the table. Don't be alarmed by the UI saying that the table does not exist -- it exists only in the branch so far. The UI will be improved in future versions.

{: .image-popup}
![Input mapping from branch data](/tutorial/branches/figures/transformation-branch-input-mapping-missing.png)

For the data to make it out of the transformation you need to add the created `dollar_btc_transactions` table to output mapping as well.

{: .image-popup}
![Input mapping from branch data](/tutorial/branches/figures/output-mapping-branch-transformation.png)

Run the updated transformation and examine the results.

{: .image-popup}
![Input mapping from branch data](/tutorial/branches/figures/transformation-branch-output.png)

As you can see, this run of the transformation accessed only branch buckets. It loaded the data from HTTP extractor from branch bucket as input and stored the output in two tables in another branch bucket. You can also examine the data in the tables to see that you indeed created a list of transactions with their dollar amounts.

This means that the second part of your task is done. You changed the production configurations in a branch. You verified that the none of the changes affected the neither the original project configurations nor the project production data.

You can now choose to:

* either take the next step and learn about [how files work in branches](/tutorial/branches/prepare-files/)
* or skip ahead and learn about [how changes in a branch are merged back to production](/tutorial/branches/project-diff)
