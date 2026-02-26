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
    ![different types of stages](https://github.com/user-attachments/assets/c99b83a7-92ef-4b18-898e-cdc91a0568e2)

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
  - Loading to multiple tables
  - Referred to with '@~'

#### 2. Table Stages
  - Automatically created with a table
  - Can only be accessed by one table
  - Cannot be altered or dropped
  - Load to one table
  - Referred to with  '@%TABLE_NAME'

#### 3. Named Stages
  - CREATE STAGE ..
  - Snowflake database object
  - Everyone with privileges can access it
  - Most flexible
  - Referred to with '@STAGE_NAME'

Example use case:
![use case](https://github.com/user-attachments/assets/eef7c328-36cf-4a01-b24f-9e9a143d705e)

### External Stage
![external stage](https://github.com/user-attachments/assets/21d71e8c-d7a4-4097-8650-94e9094fb5df)

![external stage - azure container](https://github.com/user-attachments/assets/e1ad89f0-b38e-4270-ac2e-720561bcefeb)

<img width="1000" height="405" alt="list stages" src="https://github.com/user-attachments/assets/f12d0fa8-436e-4ed5-8337-5ca9658b316a" />

##### List command - to list all files and additional properties
External stage/Internal named stage - LIST @STAGE_NAME;
User stage - LIST @~;
Table stage - LIST @%TABLE_STAGE_NAME;

##### Refereincing stage command
Copy FROM stage - COPY INTO TABLE_NAME FROM @STAGE_NAME;
Copy TO stage - COPY INTO @STAGE_NAME FROM TABLE_NAME;
Query from stage - SELECT * FROM @STAGE_NAME;
Table stage - SELECT $1, $2, $3 FROM @STAGE_NAME;

### Creating and Manaing Stages
- Go to worksheet -> Create a new worksheet -> rename as Stages

##### Show all named stages
 SHOW STAGES;
##### List files in user stage;
  LIST @~;

##### List files in user stage;
  LIST @%LOAN_PAYMENT;

#####  Database to manage stage objects, fileformats etc.
  CREATE OR REPLACE DATABASE manage_db;
  CREATE OR REPLACE SCHEMA external_stages;

##### Creating external stage
  CREATE OR REPLACE STAGE manage_db.external_stages.aws_stage
    url='s3://bucketsnowflakes3'
    credentials=(aws_key_id='ABCD_DUMMY_ID' aws_secret_key='1234abcd_key');

##### Description of external stage
  DESC STAGE manage_db.external_stages.aws_stage; 
    
##### Alter external stage   
  ALTER STAGE aws_stage
    SET credentials=(aws_key_id='XYZ_DUMMY_ID' aws_secret_key='987xyz');
    
##### Publicly accessible staging area    
  CREATE OR REPLACE STAGE MANAGE_DB.external_stages.aws_stage
    url='s3://bucketsnowflakes3';

##### List files in stage
  LIST @aws_stage;

### Data Loading
- Bulk loading - Manually executing the command
- Continous loading - Snowpipe

### Snowpipe
- Loads data immediately when file appears in a blob storage
- According to defined COPY statement
- If data needs to be available immediately for analysis
- Snowpipe uses serverless features instead of warehouses (no user created warehouse is needed)
  
### Snowpipe for Azure
![snowpipeline](https://github.com/user-attachments/assets/5989b19b-d261-4f6a-83e4-054aa2bb71b4)

*Create PIPE <name>
AUTO_INGEST = TRUE | FALSE
INTEGRATION = '<string>'
COMMENT = '<string_literal>'
AS <copy_statement>*









