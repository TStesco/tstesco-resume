

# Airflow

Operated airflow DEF / INTG/ PROD environment for $10+ Billion pricing system

issues and solutions:
1. GCS file R/W is slow:
    - feature engineering wrote 10's of thousands for files on each run, took 30+ minutes
    - possible solution: use Network File System (NFS) instead of bare GCS buckets
    - implemented: partitioning of feature files more efficiently and using BiqQuery
        - 100 kB files instead of 1k kB files

2. Client developers or myself break DEV
    - DAG monorepo makes easy to find bug commit
    - client engineers were commiting directly to `dev` before testing locally, ironned out new workflow with `feature` branches that went through CI, but were not merged into `dev` until tested.
3. External upstream ETL failue and timing:
    - upstream job failed to produce ETL job inputs
    - coordinated with client to add a back-up run in case of failure that would capture critical data sources
    - schedule ETL to start after this job finished in worst case, a airflow hook to check for file existance would be a better long term solution

