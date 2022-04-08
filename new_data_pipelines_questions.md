##### Questions to ask
- how can one distinguish between the transforms which needs to be done the spark way and others which can be ELT on data warehouse? 
    - new batch platform architecture? what happens to it?
    - are raw ingested data too big to store?
    - custom data science transforms? udfs?
- can we use a separate product to do metadata management to search columns, tables, look at lineage, enable versioning, etc.?How does an analyst know which table to use for a certain use case?
- what are the key expectations from a new data warehouse?
    - why snowflake, why not redshift-ra3, clickhouse and variants like firebolt, data lakes solutions like dremio, databricks lake house, etc.
    - about [benchmarking][1] and [comparison showdown][2]?
    - should workload pattern not be considered? e.g. what are the typical types of queries which hit the system, how many operators, relations, etc. do they have between them on avg, etc.?
    - what about query cost?
- how does one migrate?
    - what about impact of changing sql dialect?
    - what about existing integrations with firehose, lambda, athena, etc.?
- what about security? Would moving to a modern cloud data warehouse be a bottleneck, due to security concerns? Especially is data plane does not belong to Bidgely. How can that be validated?
- wouldn't dbt need something like an analytics engineer between  analysts and engineers?
- wouldn't pdt generation still be a problem with any other data warehouses?
- how much should we focus on cluster isolation between customers and products?

##### Other questions (8/April)
- what would migration look like and how difficult would it be with our current queries. Do we use a lot of vendor locking functions?

[1]: https://www.fivetran.com/blog/warehouse-benchmark
[2]: https://poplindata.com/data-warehouses/2021-database-showdown-bigquery-vs-redshift-vs-snowflake/
