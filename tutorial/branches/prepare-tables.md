---
title: Prepare tables
permalink: /tutorial/branches/prepare-tables/
---

* TOC
{:toc}

In this section you will prepare [component configurations](/components/) so that you can use them to create a [development Branch](/tutorial/branches/).
The [following section](/tutorial/branches/tables-in-branch/) then shows how to work with [Storage tables](/storage/tables/) inside a Branch.

You can create a configuration of any component in a branch and the following steps are not strictly necessary -- you
can use branches in a completely empty project. However, the tutorial follows the typical situation when you're using 
branches in a non-empty project.

## Prepare the production configurations
You'll start with a data pipeline that pulls data about bitcoin prices and creates a list of top 5 days when the price was the highest. Let's prepare the production configurations, so that you can try working with branches later.

First we need to pull the bitcoin data. To simplify this, we will download a [prepared csv file](/tutorial/branches/bitcoin_price.csv) using [HTTP extractor](/components/extractors/storage/http/).

### Set up HTTP extractor
Create a new [HTTP extractor](/components/extractors/storage/http/) configuration, fill in **Base URL** to `https://help.keboola.com`. Then add a new table to the extractor, named `bitcoin_price` and fill the **Path** to `components/development-branches/bitcoin_price.csv`. **Table Name** should be `bitcoin_price`.

{: .image-popup}
![Prepared HTTP extractor](/tutorial/branches/figures/http-ex-prod-set-up.png)

Run the extractor and verify that a new table `in.c-keboola-ex-http-682373219.bitcoin_price` was created.

*Note: The number in your bucket name will be different.*

### Set up transformation 
First create a new [Snowflake transformation](/transformations/snowflake-plain/), named `Bitcoin`.

In the **Table Input Mapping** section fill in the `bitcoin_price` table that you created using HTTP extractor. In **Table Output Mapping** add `top5` table that will be created in the transformation.

Finally, add a new code to `Block 1` named `TOP` with following query.

```SQL
CREATE TABLE "top5" AS SELECT * FROM "bitcoin_price" ORDER BY PRICE DESC LIMIT 5;

ALTER TABLE "top5" DROP COLUMN "_timestamp";
```

{: .image-popup}
![Prepared HTTP extractor](/tutorial/branches/figures/transformation-prod-set-up.png)

Save the transformation and run it. Then verify that there is a new table `out.c-bitcoin.top5` containing 5 values from the 
source data -- dates and amounts when bitcoin had the most value.

Now you have the production configurations ready and you can start working with branches in the 
[next part of the tutorial](/tutorial/branches/tables-in-branch/).
