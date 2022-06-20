##### Questions to ask (4/April)
- how can one distinguish between the transforms which needs to be done the spark way and others which can be ELT on data warehouse? 
    - new batch platform architecture? what happens to it?
    - are raw ingested data too big to store?
    - custom data science transforms? UDFs (record by record)? disagg -> for a single user 11 months of data, compute current disagg (patterns, NN)
      - __TODO: Ask SF themselves?__
- can we use a separate product to do metadata management (data dictionary, searcheability, definition) to search columns, tables, look at _lineage_, enable versioning, etc.?How does an analyst know which table to use for a certain use case? We have Amundsen, not integrated with anything.
  - through DBT maybe possible. P: It would have a placeholder for all of these. Lineage (table level lineage, not column) comes out of the box.
  - P: SF has schema tables which helps as well.
  - How Amundsen integrates with SF, datalake and dbt.
- what are the key expectations from a new data warehouse?
    - why snowflake, why not reshift-serverless, redshift-ra3, BQ, clickhouse and variants like firebolt, data lakes solutions like dremio, databricks lake house, ~presto~, etc. P: Warehouse management is easy. Micropartitions. Security (even more than redshift?) yes: in terms of PII data, encryptions. Snowpipe for incremental load. DBA is not needed (no need to do vacuum), zero-copy for new environment. UI.
    - about [benchmarking][1] and [comparison showdown][2]?
    - should workload pattern not be considered? e.g. what are the typical types of queries which hit the system, how many operators, relations, etc. do they have between them on avg, etc.? => Generic question
    - what about query cost?
    - can we migrate easily?
      - P: roles and policies may need attention. security, access control. (DBT)
      - P: changing connections on looker. SQL dialect changes could impact, but low min.
- how does one migrate?
    - what about impact of changing sql dialect?
    - what about existing integrations with firehose, lambda, athena, etc.?
- what about security? Would moving to a modern cloud data warehouse be a bottleneck, due to security concerns? Especially is data plane does not belong to Bidgely. How can that be validated?
  - data plane might be managed by SF. TODO: We should have security discussion with them. Vivek, Sushovan to meet with the right folks.
- wouldn't dbt need something like an analytics engineer between  analysts and engineers?
- ~wouldn't pdt generation still be a problem with any other data warehouses?~ Answered below
- ~how much should we focus on cluster isolation between customers and products?~
  - warehousing isloation in SF

##### Other questions (8/April)
- what would migration look like and how difficult would it be with our current queries. Do we use a lot of vendor locking functions? Especially queries which are not creating with looker. (pgsql => snowflake_sql). Queries which are written by delivery team / AIG team directly on Redshift (not looker).
  - G: __TODO: Gaurav to answer this fully.__
- should we have an ease of use metrics too as the KPI? How we we test it?

##### Follow up (25/May)
- ~is it that most of the data is avista data? if so, how do we measure / benchmark / compare performance with current system?~
- appliance_profile_v2
    - why are we using `variant` column type .. e.g. `dataset` in `appliance_profile_v2`?
    - clustering by `pilot_id, source` .. but only data that exists in this table is from pilot avista
- raw_consumption_v4
    - cluster index of `LINEAR(pilot_id, uuid, time)`?? what is the query pattern?
- are we ok with maintaining cluster indices (read >> write)?
- are we also emulating incremental raw consumption update, as I don't see data after 14th May in the system .. how do we test snowpipe without working on incremental data?
- have we tested the linear scaling with increasing pilots?
  - P: __TODO: We can test this.__
- ~how much load would fall on the first time ingestion load?~
- cache maintained on waerhouse suspension?
- ~will looker pdts maintenance remain unchanged?~
  - P: monthly aggregation happening today (as well as final agg). Since that is not happening now, this should be reduced.

##### Follow up again (6/June)
- how do you create and maintain snowpipes? is there an admin panel for the same, or purely programmatic?
- same as before, but about transforms, without using DBT? is it through materialized views and if so, is there an admin panel for the same?
- are materialized views user by Bidgely / potentially useful? Would this mean anything in terms of which version suits us best, standard or enterprise snowflake? would dbt help here?
- are the ETLs performed currently not needed at all? Or will they be still in use and thus cost us additionally.
- can we orchestrate using DBT or SF, other custom operators like disagg. E.g. I could do things like `disagg-dataprep -> disagg -> appliance-aggr` .. here `disagg` itself need not be a transform on SF, but could be a separate service invocation.

[1]: https://www.fivetran.com/blog/warehouse-benchmark
[2]: https://poplindata.com/data-warehouses/2021-database-showdown-bigquery-vs-redshift-vs-snowflake/
