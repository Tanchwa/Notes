- fully managed Petabyte Scale, low cost data warehouse
- Input through streaming or batch ingest
	- through any of Googles services
	- free bulk loading in from an external source
- batch and export is free
- can use it for real time analytics
- standard SQL querying
- integrates with Apache big data ecosystem
- billing
	- storage
	- and queries
		- on demand - only number of bytes processed (read) from the table
		- flat rate
	- not based on bytes returned
-
- you can `--dry_run` a big query query to get how much data it will run
- ![dryrunbq](https://cloud.google.com/static/bigquery/images/estimate.png)
- then, plug this into the [[GCP Pricing Calculator]] and see how much it will cost
-
- BigQuery supports querying Cloud Storage data in the following formats:
	- Comma-separated values (CSV)
	- JSON (newline-delimited)
	- Avro
	- ORC
	- Parquet
	- [[GCP Datastore]] exports
	- [[GCP Firestore]] exports