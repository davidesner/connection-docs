---
title: Working with tables in development branch
permalink: /components/development-branches/tables/in-branch/
---

* TOC
{:toc}

**Your task is following:**

> The amount of 5 top values is too low, it should be top 10. Also, you were given access to your company's bitcoin account history and you're tasked with converting the amounts of bitcoin to dolar amounts based on the value of bitcoin on the given day.

You're hesitant to do this on a live and business critical transformation in production. This is where
branches come in, giving you the option to test the changes safely before merging them to
production.

## Create a new branch

To create a new branch, click on your project’s name at the top of the screen (for the purposes of this tutorial,
the production project is called *Dev branches demo*). Then click on the green icon **New** displayed next to
your project’s name.

{: .image-popup}
![Screenshot - Create Development Branch](/components/development-branches/08-create-dev-branch.png)

Name your new development branch, for example, *My dev branch*, and click **Create Development Branch**  to open it.

{: .image-popup}
![Screenshot - Name Development Branch](/components/development-branches/09-name-dev-branch.png)

Your new branch will appear right below the name of your production project.

{: .image-popup}
![Screenshot - Created Development Branch](/components/development-branches/10-dev-branch-created.png)

## Change the transformation

Let's start with the easier task - we need to change the transformation so that it returns TOP 10 instead of TOP 5. In your branch, navigate to **Transformations**. You can see the transformation you created in production, but no worries - it's a development version that you can safely change.

Change the query limit from `LIMIT 5` to `LIMIT 10` and save.

You can now switch back to production and check that your production transformation still shows `LIMIT 5`. Cool, right?

Get back to your `My dev branch` branch and navigate back to the transformation. Try running it. Examine the **Mapping** section in the job detail.

{: .image-popup}
![Mapping in branch](/components/development-branches/mapping-in-branch.png)

Notice how in the **Input** section even though you did not run the extractor in this branch, the data are still loaded from the `bitcoin_price` table. What happens is that **Keboola checks whether a developement branch version of the table exists** and if it does not, it **falls back to read from production version**. 

Also notice that **Output** shows small yellow branch icon, and the name of the bucket is prefixed with a number - `1400-bitcoin`. When you are in branch and component wants to write to storage, Keboola **automatically creates a duplicate bucket prefixed with branch ID**, so it won't overwrite your production data. Examine the table and check that it indeed has 10 values as it should.

*Note: The number in bucket name will be different for you as you have different branch ID.*

This part of your task is done. Let's get to the second part.

## Download the transactions

Second part of your task is to produce a list of transaction showing the amount in both BTC and in USD. So you need to download the account data first. Again, we [prepared csv file](/components/development-branches/bitcoin_transactions.csv) for you.

In the branch navigate to your existing HTTP extractor configuration and add a new table named `bitcoin_transactions` and fill **Path** with `/transformations/dev-prod-mode/bitcoin_transactions.csv`. Save and run the extractor.

Examine the job outputs. There are tables with branch icon in a bucket prefixed with a number. As we've already shown, this signifies that the output was stored in branch context, keeping your production data intact.  

{: .image-popup}
![Extractor output](/components/development-branches/extractor-output.png)

If you navigate to **Storage Explorer** in your branch you'll see that the created buckets are shown there with an icon as well.

{: .image-popup}
![Extractor output](/components/development-branches/dev-branch-storage.png)

Also, when you switch back to production and navigate to production **Storage Explorer** you'll see a switch to show development buckets.

{: .image-popup}
![Storage explorer in production](/components/development-branches/storage-dev-buckets.png)

*Note: There is only one storage in your project, development buckets are created with a prefixed name, but are stored along normal buckets and are visible in production as well as in branch. You can see all buckets from all development branches in production.*

Let's get back to our transformation and run it again. What do you think will happen?

If you thought it will use the `bitcoin_prices` table from your branch as input, you were right.

{: .image-popup}
![Input mapping from branch data](/components/development-branches/input-mapping-from-branch.png)

Keboola checked whether development branch version of the bucket exists and as it did, it used it.

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
![Input mapping from branch data](/components/development-branches/transformation-branch-input-mapping.png)

Notice the branch icon next to the table name. You're adding your development version of this table. In the actual input mapping you'll see the production version though. This is so that when the configuration is merged, you'll get the production table in input mapping. Don't be alarmed by the UI saying that the table does not exist. The UI will be improved in future versions.  

{: .image-popup}
![Input mapping from branch data](/components/development-branches/transformation-branch-input-mapping-missing.png)

For the data to make it out of the transformation you need to add the created `dollar_btc_transactions` table to output mapping as well.

{: .image-popup}
![Input mapping from branch data](/components/development-branches/output-mapping-branch-transformation.png)

Run the updated transformation and examine the results.

{: .image-popup}
![Input mapping from branch data](/components/development-branches/transformation-branch-output.png)

As you can see, this run of the transformation accessed only branch buckets. It loaded the data from HTTP extractor from branch bucket as input and stored the output in two tables in another branch bucket. You can also examine the data in the tables to see that you indeed created a list of transactions with their dollar amounts.

This means that the second part of your task is done. All the work you did is now constrained in your branch, though. In production there is still the old code creating the TOP5 list. 

You can now choose. You can:

* take the next step and learn about [how files work in development branches](/components/development-branches/files/)
* skip ahead and learn about [how changes in a branch are merged back to production](/components/development-branches/diff-and-merge)
