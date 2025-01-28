## Create or Alter package

Create or Alter package includes:
* [Create or alter table](#create-or-alter-table)
* [Create or alter view](#create-or-alter-view)

# Create or alter table

The CREATE TABLE(https://docs.snowflake.com/en/sql-reference/sql/create-table#create-or-alter-table) command in Snowflake allows creating, 
replacing, or altering a table within a specified schema. Tables can include multiple columns with customizable attributes like data type, 
default values, constraints (e.g., NOT NULL, primary key, foreign key), and referential integrity.


### Create Or Alter Node properties

Create Or Alter has two configuration groups: 

*[Node Properties](#create-or-alter-node-properties)
*[General Options](#create-or-alter-general-options)

#### Create Or Alter Node Properties

| **Property** | **Description** |
|--------------|-----------------|
| **Storage Location** | (Required) Storage Location where the Create Or Alter Table will be created |
| **Node Type** | (Required) Name of template used to create node objects |
| **Description** | A description of the node's purpose |
| **Deploy Enabled** | If TRUE the node will be deployed/redeployed when changes 
are detected<br/>If FALSE the node will not be deployed or will be dropped during redeployment |

#### Create Or Alter General Options

| **Options** | **Description** |
|-------------|-----------------|
| **Create As** | Choose 'table' or 'transient table' |
| **Cluster key** | True/False toggle for clustering: **True**: Specify clustering 
column and optional expressions **False**: No clustering |
| **Change Tracking** | Toggle to enable or disable change tracking on the table. |
| **Enable Schema Evolution** | Toggle to enable or disable schema evolution. |
| **Inline Constraints** | Toggle to enable inline constraints. When enabled, 
specify column names and constraints |
| **Out-Of-Line Constraints** | Toggle to enable or disable out-of-line constraints. 
When enabled, you can define both primary and foreign keys. |
| **Data Retention Time** | Set the number of days for data retention for Time Travel actions. |
| **Default DDL Collation** | Set the default collation specification for the DDL operations. |

### Create Or Alter Deployment

#### Create Or Alter Initial Deployment Parameters

### Create Or Alter Initial Deployment

### Deploying a DAG of Create Or Alter Tables

#### Create Or Alter Table Redeployment

#### Altering Create Or Alter

### Changing Materialization Type From Create Or Alter Table to Transient Create Or Alter Table

### Changing Materialization Type From Transient Create Or Alter Table to Create Or Alter Table

### Recreating the Create Or Alter Table

### Redeploying a DAG of Create Or Alter Tables Work

### Create Or Alter Tables Undeployment

# Create or alter view



