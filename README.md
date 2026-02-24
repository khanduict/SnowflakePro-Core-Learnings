# SnowflakePro-Core-Learnings

## Data Loading and Unloading
### Stages
  - Stages in snowflake are locations used to store data
  - Whenever we want to load data into the Table, we need to have the files first available in the Stage.
  - We also use Stage to off load files from the Table. We can put those files into the stage. This is called unloading
  - Note: this is not to be confused with dataware house stages
#### Two types of Stage
1. Internal Stages
   - Managed by snowflake
     - 3 different tyes
        - User stages
        - Table stages
        - Internal Named stages
2. External Stages
   - Managed by external cloud provider
     - AWS S3 (AWS)
     -  Google cloud storage (GCP)
     -  Azure container (Azure)
