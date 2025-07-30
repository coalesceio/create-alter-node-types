## Create or Alter package

Create or Alter package includes:
* [Create or Alter table](#create-or-alter-table)
* [Create or Alter view](#create-or-alter-view)
* [Create or Alter Dimension](#create-or-alter-dimension)
* [Create or Alter Fact](#create-or-alter-fact)
* [Create or Alter Persistent Stage](#create-or-alter-persistent-stage)
* [Code](#code)

# Create or Alter table

The [Create or alter table](https://docs.snowflake.com/en/sql-reference/sql/create-table#create-or-alter-table) creates table if it doesn’t exist, or alters it according to the table definition. 

### Create or Alter Node properties

Create Or Alter has two configuration groups: 

* [Node Properties](#create-or-alter-node-properties)
* [Create table Options](#create-table-options)
* [Insert data Options](#insert-data-options)

#### Create or Alter Node Properties

| **Property** | **Description** |
|--------------|-----------------|
| **Storage Location** | (Required) Storage Location where the Create Or Alter Table will be created |
| **Node Type** | (Required) Name of template used to create node objects |
| **Description** | A description of the node's purpose |
| **Deploy Enabled** | If TRUE the node will be deployed/redeployed when changes 
are detected<br/>If FALSE the node will not be deployed or will be dropped during redeployment |

#### Create table Options

| **Options** | **Description** |
|-------------|-----------------|
| **Create As** | Choose 'table' or 'transient table' |
| **Cluster key** | True/False toggle for clustering: **True**: Specify clustering 
column and optional expressions **False**: No clustering |
| **Change Tracking** | Toggle to enable or disable change tracking on the table. |
| **Enable Schema Evolution** | Toggle to enable or disable schema evolution. |
| **Inline Constraints** | Toggle to enable inline constraints:**True**: Specify column name and constraint specification **False**: No constraints |
| **Out-Of-Line Constraints** | Toggle to enable or disable out-of-line constraints. 
When enabled, you can define both primary and foreign keys. |
| **Data Retention Time** | Set the number of days for data retention for Time Travel actions. |
| **Default DDL Collation** | Set the default collation specification for the DDL operations. |

#### Insert data Options

| **Options** | **Description** |
|-------------|-----------------|
| **Multi Source** | Toggle: True/False<br/>Implementation of SQL UNIONs<br/>**True**: Combine multiple sources in a single node<br/>True Options:<br/>- **UNION**: Combines with duplicate elimination<br/>- **UNION ALL**: Combines without duplicate elimination<br/>- **INSERT**: Individual insert for each source<br/>**False**: Single source node or multiple sources combined using a join. |
| **Truncate Before** | Toggle: True/False<br/>This determines whether a table will be overwritten each time a task executes. **True**: Uses INSERT OVERWRITE<br/>**False**: Uses INSERT to append data |
| **Enable tests** | Toggle: True/False<br/>Determines if tests are enabled |
| **Distinct** | Toggle: True/False<br/>**True**: Group by All is invisible. DISTINCT data is chosen for processing.<br/>**False**: Group by All is visible. |
| **Group by All** | Toggle: True/False<br/>**True**: DISTINCT is invisible, data grouped by all columns<br/>**False**: DISTINCT is visible |
| **Order By** | Toggle: True/False<br/>**True**: Sort column and sort order drop down are visible and are required to form order by clause. <br/>**False**: Sort options invisible |

### Limitations of Create or Alter Table

* Collation specifications cannot be altered.
* Setting or unsetting an inline primary key changes the nullability of the column accordingly
* New columns can only be added to the end of the column list
* Columns cannot be renamed. If you attempt to rename a column, the column is dropped and a new column is added.WHen you rename a column in Coalesce mapping grid.Ensure to move the same to the end of column list in mapping grid.

### Create Or Alter Table Deployment

### Create Or Alter Table Initial Deployment
When deployed for the first time into an environment the Create or Alter table node of materialization type table or transient table will execute the below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Create/Alter table/transient table** | This will execute a CREATE OR ALTER statement and create a table in the target environment |

#### Create Or Alter Table Redeployment

#### Altering the Tables and Transient Tables

When redeployed with change in target location or change in node name,results in alter of the table

| **Stage** | **Description** |
|-----------|----------------|
| **Rename/Move Table** | Alter table statement is executed to perform the alter operation |

### Changing Materialization Type 

| **Change** | **Stages Executed** |
|------------|-------------------|
| **Table to transient table or vice versa** |  Drop table/transient table<br/> Create or Alter table/transient table |

### Recreating the Create Or Alter Table

When the Create or Alter table node is redeployed with any changes in table or config changes result in re-creating table

| **Stage** | **Description** |
|-----------|----------------|
| **Create/Alter table/transient table** | This will execute a CREATE OR ALTER statement and create a table in the target environment |

### Redeployment with no changes 

If the nodes are redeployed with no changes compared to previous deployment,then no stages are executed

### Create Or Alter Tables Undeployment

If a Create or Alter table node of materialization type table/transient table are deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher level environment then the table in the target environment will be dropped.

This is executed in below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop table/transient table** | Removes the table from the environment |


# Create or alter view

A [create or alter view](https://docs.snowflake.com/en/sql-reference/sql/create-view#label-create-or-alter-view-syntax) node creates a new view if it doesn’t already exist, or updates the properties of an existing view to match those defined in the statement.

### Create or Alter View Node properties

Create Or Alter has two configuration groups: 

* [Node Properties](#create-or-alter-view-node-properties)
* [Create view Options](#create-view-options)

#### Create or Alter View Node Properties

| **Property** | **Description** |
|--------------|-----------------|
| **Storage Location** | (Required) Storage Location where the Create Or Alter Table will be created |
| **Node Type** | (Required) Name of template used to create node objects |
| **Description** | A description of the node's purpose |
| **Deploy Enabled** | If TRUE the node will be deployed/redeployed when changes 
are detected<br/>If FALSE the node will not be deployed or will be dropped during redeployment |

#### Create view Options

| **Options** | **Description** |
|-------------|-----------------|
| **Secure** | True/False toggle for secure view: **True**:creates a secure view **False**: Normal view |
| **Change Tracking** | Toggle to enable or disable change tracking on the table. |
| **Distinct** | Toggle: True/False<br/>**True**: Group by All is invisible. DISTINCT data is chosen for processing.<br/>**False**: Group by All is visible. |
| **Group by All** | Toggle: True/False<br/>**True**: DISTINCT is invisible, data grouped by all columns<br/>**False**: DISTINCT is visible |
| **Multi Source** | Toggle: True/False<br/>Implementation of SQL UNIONs<br/>**True**: Combine multiple sources in a single node<br/>True Options:<br/>- **UNION**: Combines with duplicate elimination<br/>- **UNION ALL**: Combines without duplicate elimination<br/>- **INSERT**: Individual insert for each source<br/>**False**: Single source node or multiple sources combined using a join. |

### Limitations of Create or Alter View

* The CREATE OR ALTER VIEW command doesn’t support changing a view definition once a view is created. In Coalesce,it is supported by dropping and recreating the view during redeployment.

### Create Or Alter View Deployment

### Create Or Alter View Initial Deployment
When deployed for the first time into an environment the Create or Alter view node will execute the below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Create/Alter view** | This will execute a CREATE OR ALTER statement and create a view in the target environment |

#### Create Or Alter View Redeployment

#### Altering the View

When redeployed with change in target location or change in node name,results in alter of the view

| **Stage** | **Description** |
|-----------|----------------|
| **Rename/Move View** | Alter view statement is executed to perform the alter operation |

### Recreating the Create Or Alter View

When the Create or Alter view node is redeployed with any changes in secure,change_tracking or multi-source config options,create or alter view is executed again.

| **Stage** | **Description** |
|-----------|----------------|
| **Create/Alter View** | This will execute a CREATE OR ALTER statement and create a view in the target environment |

### Change in view definition

Change in view definition like change in columns,add or drop columns,change in data type ,adding distinct or group by all results in below stages

| **Stage** | **Description** |
|-----------|----------------|
| **Create/Alter View** | This will execute a CREATE OR ALTER statement and create a view in the target environment |

### Redeployment with no changes 

If the nodes are redeployed with no changes compared to previous deployment,then no stages are executed

### Create Or Alter Tables Undeployment

If a Create or Alter view node are deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher level environment then the table in the target environment will be dropped.

This is executed in below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop View** | Removes the table or view from the environment |

## Create or Alter Dimension

The [Create or alter](https://docs.snowflake.com/en/sql-reference/sql/create-table#create-or-alter-table) creates dimension table if it doesn’t exist, or alters it according to the table definition. 

### Create or Alter Dimension Node

Create Or Alter has two configuration groups: 

* [Node Properties](#Create-or-Alter-Dimension-Node-Properties)
* [Create Table Options](#create-table-options-1)
* [Insert Data Options](#insert-data-options-1)

#### Create or Alter Dimension Node Properties

| **Property** | **Description** |
|--------------|-----------------|
| **Storage Location** | (Required) Storage Location where the Create Or Alter Dimension Table will be created |
| **Node Type** | (Required) Name of template used to create node objects |
| **Description** | A description of the node's purpose |
| **Deploy Enabled** | If TRUE the node will be deployed/redeployed when changes are detected<br/>If FALSE the node will not be deployed or will be dropped during redeployment |

#### Create Table Options

| **Options** | **Description** |
|-------------|-----------------|
| **Create As** | Choose 'table' or 'transient table' |
| **Cluster key** | True/False toggle for clustering<br/>**True**: Specify clustering column and optional expressions<br/>**False**: No clustering |
| **Change Tracking** | Toggle to enable or disable change tracking on the table. |
| **Enable Schema Evolution** | Toggle to enable or disable schema evolution. |
| **Inline Constraints** | Toggle to enable inline constraints<br/>**True**: Specify column name and constraint specification<br/>**False**: No constraints |
| **Out-Of-Line Constraints** | Toggle to enable or disable out-of-line constraints<br/>When enabled, you can define both primary and foreign keys. |
| **Data Retention Time** | Set the number of days for data retention for Time Travel actions.<br/>Available only for materialization type - table |
| **Maximum Data Extension time** | Set the number of days for max data extension for Historical data access.<br/>Available only for materialization type - table |
| **Default DDL Collation** | Set the default collation specification for the DDL operations. |

#### Insert data Options

| **Options** | **Description** |
|-------------|-----------------|
| **Multi Source** | Toggle: True/False<br/>Implementation of SQL UNIONs<br/>**True**: Combine multiple sources in a single node<br/>True Options:<br/>- **UNION**: Combines with duplicate elimination<br/>- **UNION ALL**: Combines without duplicate elimination<br/>**False**: Single source node or multiple sources combined using a join. |
| **Truncate Before** | Toggle: True/False<br/>This determines whether a table will be overwritten each time a task executes.<br/>**True**: Uses INSERT OVERWRITE<br/>**False**: Uses INSERT to append data |
| **Enable tests** | Toggle: True/False<br/>Determines if tests are enabled |
| **Business key** | Required column for both Type 1 and Type 2 Dimensions<br/>Note: Geometry and Geography data type columns are not supported as business key columns. |
| **Change tracking** | Required column for Type 2. If not chosen, treated as type 1|
| **Distinct** | Toggle: True/False<br/>**True**: Group by All is invisible.<br/>DISTINCT data is chosen for processing.<br/>**False**: Group by All is visible. |
| **Group by All** | Toggle: True/False<br/>**True**: DISTINCT is invisible<br/>Data grouped by all columns<br/>**False**: DISTINCT is visible |
| **Order By** | Toggle: True/False<br/>**True**: Sort column and sort order drop down are visible and are required to form order by clause.<br/>**False**: Sort options invisible |
| **Insert Zero Key Record** | Toggle: True/False<br/>Insert Zero Key Record to Dimension<br/>**True**: Zero Key Record Options enabled.<br/>**False**: Zero Key Record not added |
| **Zero Key Record Options** | Add custom zero key record values for :<br/>-Default Surrogate Key <br/>-Default String Value<br/>-Default Date Value (Date Format DD-MM-YYYY)<br/>-Default Timestamp Value (Timestamp Format YYYY-MM-DD HH24:MI:SS.FF)<br/>-Default Boolean Value |
| **Advanced Zero Key Record Options** | Toggle: True/False<br/>**True**: Select Columns and the default value of the column for zero key record<br/>**False**: Advanced Zero Key Record Options not enabled |

### Limitations of Create or Alter Dimension Table

* Collation specifications cannot be altered.
* Setting or unsetting an inline primary key changes the nullability of the column accordingly
* New columns can only be added to the end of the column list
* Columns cannot be renamed. If you attempt to rename a column, the column is dropped and a new column is added. When you rename a column in Coalesce mapping grid, ensure to move the same to the end of column list in mapping grid.
* A column's default value can only be set during its initial creation using a `CREATE OR ALTER` statement; re-running the statement with a different default value will result in an error and will not update/alter/drop the existing default or add a new default.

### Create Or Alter Dimension Table Deployment

### Create Or Alter Dimension Table Initial Deployment
When deployed for the first time into an environment the Create or Alter table node of materialization type table or transient table will execute the below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Create/Alter table/transient table** | This will execute a CREATE OR ALTER statement and create a table/transient table in the target environment |

#### Create Or Alter Dimension Table Redeployment

#### Altering the Tables and Transient Tables

When redeployed with change in target location or change in node name, results in alter of the table

| **Stage** | **Description** |
|-----------|----------------|
| **Rename/Move Table** | Alter table statement is executed to perform the alter operation |

### Changing Materialization Type 

| **Change** | **Stages Executed** |
|------------|-------------------|
| **Table to transient table or vice versa** |  Drop table/transient table<br/> Create or Alter table/transient table |

### Recreating the Create Or Alter Table

When the Create or Alter table node is redeployed with any changes in table or config changes result in re-creating table

| **Stage** | **Description** |
|-----------|----------------|
| **Create/Alter table/transient table** | This will execute a CREATE OR ALTER statement and create a table in the target environment |

### Redeployment with no changes 

If the nodes are redeployed with no changes compared to previous deployment, then no stages are executed

### Create Or Alter Tables Undeployment

If a Create or Alter table node of materialization type table/transient table are deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher level environment then the table in the target environment will be dropped.

This is executed in below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop table/transient table** | Removes the table from the environment |

## Create or Alter Fact

The [Create or alter](https://docs.snowflake.com/en/sql-reference/sql/create-table#create-or-alter-table) creates fact table if it doesn’t exist, or alters it according to the table definition. 

### Create or Alter Fact Node

Create Or Alter has two configuration groups: 

* [Node Properties](#Create-or-Alter-Fact-Node-Properties)
* [Create Table Options](#create-table-options-2)
* [Insert Data Options](#insert-data-options-2)

#### Create or Alter Fact Node Properties

| **Property** | **Description** |
|--------------|-----------------|
| **Storage Location** | (Required) Storage Location where the Create Or Alter fact Table will be created |
| **Node Type** | (Required) Name of template used to create node objects |
| **Description** | A description of the node's purpose |
| **Deploy Enabled** | If TRUE the node will be deployed/redeployed when changes are detected<br/>If FALSE the node will not be deployed or will be dropped during redeployment |

#### Create Table Options

| **Options** | **Description** |
|-------------|-----------------|
| **Create As** | Choose 'table' or 'transient table' |
| **Cluster key** | True/False toggle for clustering<br/>**True**: Specify clustering column and optional expressions<br/>**False**: No clustering |
| **Change Tracking** | Toggle to enable or disable change tracking on the table. |
| **Enable Schema Evolution** | Toggle to enable or disable schema evolution. |
| **Inline Constraints** | Toggle to enable inline constraints<br/>**True**: Specify column name and constraint specification<br/>**False**: No constraints |
| **Out-Of-Line Constraints** | Toggle to enable or disable out-of-line constraints<br/>When enabled, you can define both primary and foreign keys. |
| **Data Retention Time** | Set the number of days for data retention for Time Travel actions.<br/>Available only for materialization type - table |
| **Maximum Data Extension time** | Set the number of days for max data extension for Historical data access.<br/>Available only for materialization type - table |
| **Default DDL Collation** | Set the default collation specification for the DDL operations. |

#### Insert Data Options

| **Options** | **Description** |
|-------------|-----------------|
| **Multi Source** | Toggle: True/False<br/>Implementation of SQL UNIONs<br/>**True**: Combine multiple sources in a single node<br/>True Options:<br/>- **UNION**: Combines with duplicate elimination<br/>- **UNION ALL**: Combines without duplicate elimination<br/>**False**: Single source node or multiple sources combined using a join. |
| **Truncate Before** | Toggle: True/False<br/>This determines whether a table will be overwritten each time a task executes.<br/>**True**: Uses INSERT OVERWRITE<br/>**False**: Uses INSERT to append data |
| **Enable tests** | Toggle: True/False<br/>Determines if tests are enabled |
| **Business key** | Required column for Fact table creation.<br/>Note: Geometry and Geography data type columns are not supported as business key columns. |
| **Distinct** | Toggle: True/False<br/>**True**: Group by All is invisible.<br/>DISTINCT data is chosen for processing.<br/>**False**: Group by All is visible. |
| **Group by All** | Toggle: True/False<br/>**True**: DISTINCT is invisible<br/>Data grouped by all columns<br/>**False**: DISTINCT is visible |
| **Order By** | Toggle: True/False<br/>**True**: Sort column and sort order drop down are visible and are required to form order by clause.<br/>**False**: Sort options invisible |

### Limitations of Create or Alter Fact Table

* Collation specifications cannot be altered.
* Setting or unsetting an inline primary key changes the nullability of the column accordingly
* New columns can only be added to the end of the column list
* Columns cannot be renamed. If you attempt to rename a column, the column is dropped and a new column is added. When you rename a column in Coalesce mapping grid, ensure to move the same to the end of column list in mapping grid.
* A column's default value can only be set during its initial creation using a `CREATE OR ALTER` statement; re-running the statement with a different default value will result in an error and will not update/alter/drop the existing default or add a new default.

### Create Or Alter Fact Table Deployment

### Create Or Alter Fact Table Initial Deployment
When deployed for the first time into an environment the Create or Alter table node of materialization type table or transient table will execute the below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Create/Alter table/transient table** | This will execute a CREATE OR ALTER statement and create a table/transient table in the target environment |

#### Create Or Alter Fact Table Redeployment

#### Altering the Tables and Transient Tables

When redeployed with change in target location or change in node name, results in alter of the table

| **Stage** | **Description** |
|-----------|----------------|
| **Rename/Move Table** | Alter table statement is executed to perform the alter operation |

### Changing Materialization Type 

| **Change** | **Stages Executed** |
|------------|-------------------|
| **Table to transient table or vice versa** |  Drop table/transient table<br/> Create or Alter table/transient table |

### Recreating the Create Or Alter Table

When the Create or Alter table node is redeployed with any changes in table or config changes result in re-creating table

| **Stage** | **Description** |
|-----------|----------------|
| **Create/Alter table/transient table** | This will execute a CREATE OR ALTER statement and create a table in the target environment |

### Redeployment with no changes 

If the nodes are redeployed with no changes compared to previous deployment, then no stages are executed

### Create Or Alter Tables Undeployment

If a Create or Alter table node of materialization type table/transient table are deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher level environment then the table in the target environment will be dropped.

This is executed in below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop table/transient table** | Removes the table from the environment |

## Create or Alter Persistent Stage

The [Create or alter](https://docs.snowflake.com/en/sql-reference/sql/create-table#create-or-alter-table) creates Persistent Stage table if it doesn’t exist, or alters it according to the table definition. 

### Create or Alter Persistent Stage Node

Create Or Alter has two configuration groups: 

* [Node Properties](#Create-or-Alter-Persistent-Stage-Node-Properties)
* [Create Table Options](#create-table-options-3)
* [Insert Data Options](#insert-data-options-3)

#### Create or Alter Persistent Stage Node Properties

| **Property** | **Description** |
|--------------|-----------------|
| **Storage Location** | (Required) Storage Location where the Create Or Alter Persistent Stage Table will be created |
| **Node Type** | (Required) Name of template used to create node objects |
| **Description** | A description of the node's purpose |
| **Deploy Enabled** | If TRUE the node will be deployed/redeployed when changes are detected<br/>If FALSE the node will not be deployed or will be dropped during redeployment |

#### Create Table Options

| **Options** | **Description** |
|-------------|-----------------|
| **Create As** | Choose 'table' or 'transient table' |
| **Cluster key** | True/False toggle for clustering<br/>**True**: Specify clustering column and optional expressions<br/>**False**: No clustering |
| **Change Tracking** | Toggle to enable or disable change tracking on the table. |
| **Enable Schema Evolution** | Toggle to enable or disable schema evolution. |
| **Inline Constraints** | Toggle to enable inline constraints<br/>**True**: Specify column name and constraint specification<br/>**False**: No constraints |
| **Out-Of-Line Constraints** | Toggle to enable or disable out-of-line constraints<br/>When enabled, you can define both primary and foreign keys. |
| **Data Retention Time** | Set the number of days for data retention for Time Travel actions.<br/>Available only for materialization type - table |
| **Maximum Data Extension time** | Set the number of days for max data extension for Historical data access.<br/>Available only for materialization type - table |
| **Default DDL Collation** | Set the default collation specification for the DDL operations. |

#### Insert Data Options

| **Options** | **Description** |
|-------------|-----------------|
| **Multi Source** | Toggle: True/False<br/>Implementation of SQL UNIONs<br/>**True**: Combine multiple sources in a single node<br/>True Options:<br/>- **UNION**: Combines with duplicate elimination<br/>- **UNION ALL**: Combines without duplicate elimination<br/>**False**: Single source node or multiple sources combined using a join. |
| **Truncate Before** | Toggle: True/False<br/>This determines whether a table will be overwritten each time a task executes.<br/>**True**: Uses INSERT OVERWRITE<br/>**False**: Uses INSERT to append data |
| **Enable tests** | Toggle: True/False<br/>Determines if tests are enabled |
| **Business key** | Required column for both Type 1 and Type 2.<br/>Note: Geometry and Geography data type columns are not supported as business key columns. |
| **Change tracking** | Required column for Type 2. If not chosen, treated as type 1|
| **Distinct** | Toggle: True/False<br/>**True**: Group by All is invisible.<br/>DISTINCT data is chosen for processing.<br/>**False**: Group by All is visible. |
| **Group by All** | Toggle: True/False<br/>**True**: DISTINCT is invisible<br/>Data grouped by all columns<br/>**False**: DISTINCT is visible |
| **Order By** | Toggle: True/False<br/>**True**: Sort column and sort order drop down are visible and are required to form order by clause.<br/>**False**: Sort options invisible |

### Limitations of Create or Alter Persistent Stage Table

* Collation specifications cannot be altered.
* Setting or unsetting an inline primary key changes the nullability of the column accordingly
* New columns can only be added to the end of the column list
* Columns cannot be renamed. If you attempt to rename a column, the column is dropped and a new column is added. When you rename a column in Coalesce mapping grid, ensure to move the same to the end of column list in mapping grid.
* A column's default value can only be set during its initial creation using a `CREATE OR ALTER` statement; re-running the statement with a different default value will result in an error and will not update/alter/drop the existing default or add a new default.

### Create Or Alter Persistent Stage Table Deployment

### Create Or Alter Persistent Stage Table Initial Deployment
When deployed for the first time into an environment the Create or Alter table node of materialization type table or transient table will execute the below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Create/Alter table/transient table** | This will execute a CREATE OR ALTER statement and create a table/transient table in the target environment |

#### Create Or Alter Persistent Stage Table Redeployment

#### Altering the Tables and Transient Tables

When redeployed with change in target location or change in node name, results in alter of the table

| **Stage** | **Description** |
|-----------|----------------|
| **Rename/Move Table** | Alter table statement is executed to perform the alter operation |

### Changing Materialization Type 

| **Change** | **Stages Executed** |
|------------|-------------------|
| **Table to transient table or vice versa** |  Drop table/transient table<br/> Create or Alter table/transient table |

### Recreating the Create Or Alter Table

When the Create or Alter table node is redeployed with any changes in table or config changes result in re-creating table

| **Stage** | **Description** |
|-----------|----------------|
| **Create/Alter table/transient table** | This will execute a CREATE OR ALTER statement and create a table in the target environment |

### Redeployment with no changes 

If the nodes are redeployed with no changes compared to previous deployment, then no stages are executed

### Create Or Alter Tables Undeployment

If a Create or Alter table node of materialization type table/transient table are deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher level environment then the table in the target environment will be dropped.

This is executed in below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop table/transient table** | Removes the table from the environment |

## Code

### Create or Alter table Code

| **Component** | **Link** |
|--------------|-----------|
| **Node definition** | [definition.yml](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateorAlterTable-345/definition.yml) |
| **Create Template** | [create.sql.j2](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateorAlterTable-345/create.sql.j2) |
| **Run Template** | [run.sql.j2](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateorAlterTable-345/run.sql.j2)

### Create or Alter view Code

| **Component** | **Link** |
|--------------|-----------|
| **Node definition** | [definition.yml](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateorAlterView-419/definition.yml) |
| **Create Template** | [create.sql.j2](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateorAlterView-419/create.sql.j2) |
| **Run Template** | [run.sql.j2](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateorAlterView-419/run.sql.j2)

### Create or Alter Dimension Code

| **Component** | **Link** |
|--------------|-----------|
| **Node definition** | [definition.yml](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateOrAlterDimension-535/definition.yml) |
| **Create Template** | [create.sql.j2](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateOrAlterDimension-535/create.sql.j2) |
| **Run Template** | [run.sql.j2](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateOrAlterDimension-535/run.sql.j2)

### Create or Alter Fact Code

| **Component** | **Link** |
|--------------|-----------|
| **Node definition** | [definition.yml](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateOrAlterFact-534/definition.yml) |
| **Create Template** | [create.sql.j2](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateOrAlterFact-534/create.sql.j2) |
| **Run Template** | [run.sql.j2](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateOrAlterFact-534/run.sql.j2)

### Create or Alter Persistent Stage Code

| **Component** | **Link** |
|--------------|-----------|
| **Node definition** | [definition.yml](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateOrAlterPersistentStage-533/definition.yml) |
| **Create Template** | [create.sql.j2](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateOrAlterPersistentStage-533/create.sql.j2) |
| **Run Template** | [run.sql.j2](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateOrAlterPersistentStage-533/run.sql.j2)

