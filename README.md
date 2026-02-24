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

### Internal Stage
![internal stages](https://github.com/user-attachments/assets/4d495424-fab2-4cd9-b5c3-d6c9b5d808d9)
 - When we want to copy data into the Table, we first upload the file into the Internal Stage. We do this by using "PUT" Snow sql command. If we use the "PUT" command, the data will be automatically compressed (.gz file ending) by default. If we dunt use anything, the data "AUTO_COMPRESS = TRUE " and compressed with "gzip" by default.
 - Then use "COPY INTO" to load the data into the Table
 ![internal stage out](https://github.com/user-attachments/assets/aac9094c-506a-4732-8d93-27fa526b7b83)
 - Also we can unload data (from Table to Internal Stage) using "COPY INTO" command. Then use "GET" command to download the files to the local computer.

#### 1. User Stages
  - Tied to a user
  - Cannot be accessed by other users
  - Every user has default stage
  - Cannot be altered or dropped
  - Put files to that stage before loading
  - Explicitly remove files again

#### 2. Table Stages
  - Automatically created with a table
  - Can only be accessed by one table
  - Cannot be altered or dropped
  - Load to one table
  - Referred to with #### '@%TABLE_NAME'

