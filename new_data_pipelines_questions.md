##### Questions to ask (4/April)
- how can one distinguish between the transforms which needs to be done the spark way and others which can be ELT on data warehouse? 
    - new batch platform architecture? what happens to it?
    - are raw ingested data too big to store?
    - custom data science transforms? udfs?
- can we use a separate product to do metadata management to search columns, tables, look at lineage, enable versioning, etc.?How does an analyst know which table to use for a certain use case?
- what are the key expectations from a new data warehouse?
    - why snowflake, why not redshift-ra3, clickhouse and variants like firebolt, data lakes solutions like dremio, databricks lake house, presto, etc.
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
- what would migration look like and how difficult would it be with our current queries. Do we use a lot of vendor locking functions? Especially queries which are not creating with looker.
- should we have an ease of use metrics too as the KPI? How we we test it?

##### Follow up (25/May)
- is it that most of the data is avista data? if so, how do we measure / benchmark / compare performance with current system?
- appliance_profile_v2
    - why are we using `variant` column type .. e.g. `dataset` in `appliance_profile_v2`?
    - clustering by `pilot_id, source` .. but only data that exists in this table is from pilot avista
- raw_consumption_v4
    - cluster index of `LINEAR(pilot_id, uuid, time)`?? what is the query pattern?
- are we ok with maintaining cluster indices (read >> write)?
- are we also emulating incremental raw consumption update, as I don't see data after 14th May in the system .. how do we test snowpipe without working on incremental data?
- have we tested the linear scaling with increasing pilots?
- how much load would fall on the first time load?
- cache maintained on waerhouse suspension?

[1]: https://www.fivetran.com/blog/warehouse-benchmark
[2]: https://poplindata.com/data-warehouses/2021-database-showdown-bigquery-vs-redshift-vs-snowflake/
