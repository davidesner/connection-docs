---
title: Working with tables in branches
permalink: /components/development-branches/tables/
---

* TOC
{:toc}

In this section you will prepare [component configurations](tood) so that you can use them to create a [development Branch](todo).
The [following section](/components/development-branches/tables/in-branch/) then shows how to work with [Storage tables](todo) inside a Branch.

## Prepare the production configurations

We'll start with a data pipeline that pulls data about bitcoin prices, creates a list of TOP 5 dates when the price was the highest. Let's prepare the production configurations, so that we can try working with branches later.

First we need to pull the bitcoin data. To simplify this, we will download a [prepared csv file](/components/development-branches/bitcoin_price.csv) using HTTP extractor.

## Set up http extractor in production

Create new HTTP extractor configuration, fill in **Base URL** to `https://help.keboola.com`. Then add new table to the extractor, named `bitcoin_price` and fill the **Path** to `components/development-branches/bitcoin_price.csv`. **Table Name** should be `bitcoin_price`.

{: .image-popup}
![Prepared HTTP extractor](/components/development-branches/http-ex-prod-set-up.png)

Run the extractor and verify that a new table `in.c-keboola-ex-http-682373219.bitcoin_price` was created.

*Note: The number in your bucket name will be different.*

## Prepare a transformation in production

First create a new Snowflake transformation, named `Bitcoin`.

In the **Table Input Mapping** section fill in the `bitcoin_price` table that you created using HTTP extractor. In **Table Output Mapping** add `top5` table that will be created in the transformation.

Finally, add a new code to `Block 1` named `TOP` with following query.

```SQL
CREATE TABLE "top5" AS SELECT * FROM "bitcoin_price" ORDER BY PRICE DESC LIMIT 5;

ALTER TABLE "top5" DROP COLUMN "_timestamp";
```

{: .image-popup}
![Prepared HTTP extractor](/components/development-branches/transformation-prod-set-up.png)

Save the transformation and run it. Then verify that there is a new table `out.c-bitcoin.top5` containing 5 values from the source data - dates and amounts when bitcoin had the most value.

Now you have the production set up and we can give branches a test run in the next part of the tutorial - [Working with tables in development branch](/components/development-branches/tables/in-branch/).
