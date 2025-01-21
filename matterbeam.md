# Matterbeam
Matterbeam is a flexible, powerful way to avoid having to build pipelines all the time - get all your data flowing in one place, and then pull it to wherever you need it. Join datasets from 
 You can also replay data from a given point in time

## Vocabulary
* **collectors**: stream data from dbs, SaaS products, s3 buckets, or even the web. produces one or more datasets in MB. MB intuits the schema (shape) of the data and turns it into a MB format. this is not batched, it's done as live as possible. current day collectors include http, s3, salesforce -- eventually shopify, GA, google ads, postgres polling
* **emitters**: stream data to target systems (db, SaaS, or even web/APIs), could stream same data to multiple destinations. current day emitters include postgres, myspx, api -- eventually redshift and kaftka
* **transforms**: applies logic and creates a modified dataset to create new derived datasets. for example, rename fields, flatten, filter, join against other datasets, etc. transforms always create new data and never modify existing source. derived datasets can be used the same as source datasets, and can be emitted and transformed. transforms are written in Python
* **dataset**: one data flow or stream -- usually a table, object or event, e.g. a salesforce user object. datasets are created by collectors or as the output of a transform. once created, it can be streamed to other transforms or emitters
* **catalog**: a view where you can browse and search available datasets and other MB components
* **studio**: a view where you can see how data flows - a friendly way of seeing how data is moving through your system. you can also edit and create MB components and explore data lineage
* datasets are **materialized** into tables -- simple transforms can be applied during this process (is this accurate?)


## Navigation
* ctrl-j: toggle dark mode


## Tips
* limit data set to 5 cos that's all you'll be able to see in the UI
* will need pip
* will need a token - your token in on the matterbeam site under settings
* you can push data to matterbeam from your computer



