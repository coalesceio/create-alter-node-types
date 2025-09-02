## Create or Alter package

Create or Alter package includes:
* [Create or Alter table](#create-or-alter-table)
* [Create or Alter view](#create-or-alter-view)
* [Create or Alter Dimension](#create-or-alter-dimension)
* [Create or Alter Fact](#create-or-alter-fact)
* [Create or Alter Persistent Stage](#create-or-alter-persistent-stage)
* [Create or Alter Task](#create-or-alter-task)
* [Create or Alter DAG Root Task](#create-or-alter-dag-root-task)
* [Create DAG Root Resume Task](#Create-DAG-Root-Resume-Task)
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

### Redeployment with only metadata changes

Sometimes, changes to config can result in metadata changes from node edits, DML changes, or storage updates. A few cases are listed below:

1. Changes in business keys
2. Changes to change tracking keys
3. Changes in join clauses
4. Transformations made at column level
5. Changing DML options like DISTINCT, ORDER BY, GROUP BY ALL

And many more. Most of the time, specific ‘is’ and ‘was’ values will be displayed to specifically show what changed.

The following stages are executed:

| **Stage** | **Description** |
|-----------|----------------|
| **Metadata Update \| Business Keys \| Change Tracking \| Distinct \| Transformation \| Join** | A dummy statement would execute with specific changes listed in comments|

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

### Create Or Alter View Undeployment

If a Create or Alter view node are deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher level environment then the table in the target environment will be dropped.

This is executed in below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop View** | Removes the table or view from the environment |

----

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

### Redeployment with only metadata changes

Sometimes, changes to config can result in metadata changes from node edits, DML changes, or storage updates. A few cases are listed below:

1. Changes in business keys
2. Changes to change tracking keys
3. Changes in join clauses
4. Transformations made at column level
5. Changing DML options like DISTINCT, ORDER BY, GROUP BY ALL

And many more. Most of the time, specific ‘is’ and ‘was’ values will be displayed to specifically show what changed.

The following stages are executed:

| **Stage** | **Description** |
|-----------|----------------|
| **Metadata Update \| Business Keys \| Change Tracking \| Distinct \| Transformation \| Join** | A dummy statement would execute with specific changes listed in comments|

### Create Or Alter Tables Undeployment

If a Create or Alter table node of materialization type table/transient table are deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher level environment then the table in the target environment will be dropped.

This is executed in below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop table/transient table** | Removes the table from the environment |

----

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

### Redeployment with only metadata changes

Sometimes, changes to config can result in metadata changes from node edits, DML changes, or storage updates. A few cases are listed below:

1. Changes in business keys
2. Changes to change tracking keys
3. Changes in join clauses
4. Transformations made at column level
5. Changing DML options like DISTINCT, ORDER BY, GROUP BY ALL

And many more. Most of the time, specific ‘is’ and ‘was’ values will be displayed to specifically show what changed.

The following stages are executed:

| **Stage** | **Description** |
|-----------|----------------|
| **Metadata Update \| Business Keys \| Change Tracking \| Distinct \| Transformation \| Join** | A dummy statement would execute with specific changes listed in comments|

### Create Or Alter Tables Undeployment

If a Create or Alter table node of materialization type table/transient table are deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher level environment then the table in the target environment will be dropped.

This is executed in below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop table/transient table** | Removes the table from the environment |

----

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

### Redeployment with only metadata changes

Sometimes, changes to config can result in metadata changes from node edits, DML changes, or storage updates. A few cases are listed below:

1. Changes in business keys
2. Changes to change tracking keys
3. Changes in join clauses
4. Transformations made at column level
5. Changing DML options like DISTINCT, ORDER BY, GROUP BY ALL

And many more. Most of the time, specific ‘is’ and ‘was’ values will be displayed to specifically show what changed.

The following stages are executed:

| **Stage** | **Description** |
|-----------|----------------|
| **Metadata Update \| Business Keys \| Change Tracking \| Distinct \| Transformation \| Join** | A dummy statement would execute with specific changes listed in comments|

### Create Or Alter Tables Undeployment

If a Create or Alter table node of materialization type table/transient table are deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher level environment then the table in the target environment will be dropped.

This is executed in below stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop table/transient table** | Removes the table from the environment |

----

## Create Or Alter Task

The [Create or Alter Task](https://docs.snowflake.com/en/sql-reference/sql/create-task#create-or-alter-task) creates task if it doesn’t exist, or alters it according to the task definition. 

### Create or Alter Task Node

Create Or Alter Task node has two or three configuration groups depending on config options selected: 

* [Node Properties](#Create-or-Alter-Task-Node-Properties)
* [Create Table Options](#create-table-options-4)
* [Insert Data Options](#insert-data-options-4)
* [Scheduling Options](#Task-Scheduling-Options)

#### Create or Alter Task Node Properties

| **Property** | **Description** |
|--------------|-----------------|
| **Storage Location** | (Required) Storage Location where the Create Or Alter Task will be created |
| **Node Type** | (Required) Name of template used to create node objects |
| **Description** | A description of the node's purpose |
| **Deploy Enabled** | If TRUE the node will be deployed/redeployed when changes are detected<br/>If FALSE the node will not be deployed or will be dropped during redeployment |

#### Create Table Options

| **Options** | **Description** |
|-------------|-----------------|
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
| **Development Mode** | True / False toggle that determines whether a task will be created or if the SQL to be used in the task will execute as DML as a Run action.<br/>**True** - A table will be created and SQL will execute as a Run action<br/>**False** - After testing the SQL as a Run action, setting to false will wrap SQL in a task with specified Scheduling Options |
| **Multi Source** | Toggle: True/False<br/>Implementation of SQL UNIONs<br/>**True**: Combine multiple sources in a single node<br/>True Options:<br/>- **UNION**: Combines with duplicate elimination<br/>- **UNION ALL**: Combines without duplicate elimination<br/>**False**: Single source node or multiple sources combined using a join. |
| **Truncate Before** | Toggle: True/False<br/>This determines whether a table will be overwritten each time a task executes.<br/>**True**: Uses INSERT OVERWRITE<br/>**False**: Uses INSERT to append data |
| **Distinct** | Toggle: True/False<br/>**True**: Group by All is invisible.<br/>DISTINCT data is chosen for processing.<br/>**False**: Group by All is visible. |
| **Group by All** | Toggle: True/False<br/>**True**: DISTINCT is invisible<br/>Data grouped by all columns<br/>**False**: DISTINCT is visible |
| **Order By** | Toggle: True/False<br/>**True**: Sort column and sort order drop down are visible and are required to form order by clause.<br/>**False**: Sort options invisible |

#### Task Scheduling Options

| **Option** | **Description** |
|------------|----------------|
| **Scheduling Mode** | Choose compute type:<br/>- **Warehouse Task**: User managed warehouse executes tasks<br/>- **Serverless Task**: Uses serverless compute |
| **When Source Stream has Data Flag** | True/False toggle to check for stream data<br/>**True** - Only run task if source stream has capture change data<br/>**False** -  Run task on schedule regardless of whether the source stream has data. If the source is not a stream should set this to false. |
| **Multiple Stream has Data Logic** | AND/OR logic when multiple streams (visible if Stream has Data Flag is true)<br/>**AND** - If there are multiple streams task will run if all streams have data<br/>**OR** -  If there are multiple streams task will run if one or more streams has data |
| **Select Warehouse** | Visible if Scheduling Mode is set to Warehouse Task. Enter the name of the warehouse you want the task to run on without quotes.|
| **Select initial serverless size** | Visible when Scheduling Mode is set to Serverless Task.<br/> Select the initial compute size on which to run the task. Snowflake will adjust size from there based on target schedule and task run times. |
| **Task Schedule** | Choose schedule type:<br/>- **Minutes** - Specify interval in minutes. Enter a whole number from 1 to 11520 which represents the number of minutes between task runs.<br/>- **Cron** - Uses [Cron expressions](https://docs.coalesce.io/docs/reference/cron-reference/). Specifies a cron expression and time zone for periodically running the task. Supports a subset of standard cron utility syntax.<br/>- **Predecessor** - Specify dependent tasks |
| **Enter predecessor tasks separated by a comma**| Visible when Task Schedule is set to Predecessor. <br/>One or more task names that precede the task being created in the current node. Task names are case sensitive, should not be quoted and must exist in the same schema in which the current task is being created. If there are multiple predecessor task separate the task names using a comma and no spaces.|
| **Root task name** | Visible when Task Schedule is set to Predecessor.<br/> Name of the root task that controls scheduling for the DAG of tasks. Task names are case sensitive, should not be quoted and must exist in the same schema in which the current task is being created. If there are multiple predecessor task separate the task names using a comma and no spaces.|
| **User Task Timeout** | Specifies the time limit on a single run of the task before it times out (in milliseconds)<br/>Values: 0 - 604800000 (7 days). A value of 0 specifies that the maximum timeout value is enforced|

### Create or Alter Task Deployment

#### Create or Alter Task Deployment Parameters

The Create or Alter Task includes an environment parameter that allows you to specify a different warehouse used to run a task in different environments.

The parameter name is `targetTaskWarehouse` and the default value is `DEV ENVIRONMENT`.

When set to `DEV ENVIRONMENT` the value entered in the Scheduling Options config Select Warehouse on which to run the task will be used when creating the task.

```json
{
    "targetTaskWarehouse": "DEV ENVIRONMENT"
}
```

When set to any value other than `DEV ENVIRONMENT` the node will attempt to create the task using a Snowflake warehouse with the specified value.

For example, with the below setting for the parameter in a QA environment, the task will execute using a warehouse named `compute_wh`.

```json
{
    "targetTaskWarehouse": "compute_wh"
}
```

#### Create or Alter Task Initial Deployment

When deployed for the first time into an environment the Create or Alter Task node will execute the following stages:

For tasks without predecessors:

| **Stage** | **Description** |
|-----------|----------------|
| **Create Target Table** | Creates table that will be loaded by the task |
| **Create Task** | Creates task that will load the target table on schedule |
| **Resume Task** | Resumes the task so it runs on schedule |

For tasks with predecessors:

| **Stage** | **Description** |
|-----------|----------------|
| **Create Target Table** | Creates table that will be loaded by the task |
| **Suspend Root Task** | Suspends root task for DAG modification |
| **Create Task** | Creates task that will load the target table on schedule |

If a task is part of a DAG of tasks, the DAG needs to include a node type called "Task DAG Resume Root." This node will resume the root node once all the dependent tasks have been created as part of a deployment.

#### Create or Alter Task Redeployment

After initial deployment, changes in task schedule, warehouse, or scheduling options will result in a SUSPEND, CREATE or ALTER TASK, RESUME

For tasks without predecessors:

| **Stage** | **Description** |
|-----------|----------------|
| **Suspend Task** | Suspend task |
| **Create Task** | Create Or Alter task with new schedule |
| **Resume Task** | Resumes task with new schedule |

For tasks with predecessors:

| **Stage** | **Description** |
|-----------|----------------|
| **Suspend Root Task** | Suspends root task for DAG modification |
| **Create Task** | Create Or Alter task with new schedule |

#### Create or Alter Task Altering Tables

Subsequent deployments with changes in table like add or drop column and change in data type will result in an ALTER table statement followed by CREATE OR ALTER TASK AND RESUME TASK statements being issued.

| **Stage** | **Description** |
|-----------|----------------|
| **Change Column Attributes/Delete Column/Add Column/Change table description** | Create or Alter table statement is executed to perform the alter operation. |
| **Suspend Task** | Suspend task |
| **Create Task** | Create Or Alter task with new schedule |
| **Resume Task** | Resumes task with new schedule |

### Redeployment with only metadata changes

Sometimes, changes to config can result in metadata changes from node edits, DML changes, or storage updates when the **DEV_MODE** is **ON**. A few cases are listed below:

1. Changes in business keys
2. Changes to change tracking keys
3. Changes in join clauses
4. Transformations made at column level
5. Changing DML options like DISTINCT, ORDER BY, GROUP BY ALL

And many more. Most of the time, specific ‘is’ and ‘was’ values will be displayed to specifically show what changed.

The following stages are executed:

| **Stage** | **Description** |
|-----------|----------------|
| **Metadata Update \| Business Keys \| Change Tracking \| Distinct \| Transformation \| Join** | A dummy statement would execute with specific changes listed in comments|

### Create or Alter Task Undeployment

If a Create or Alter Task node is deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher level environment then all objects created by the node in the target environment will be dropped.

For tasks without predecessors:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop Table** |  Drop the table originally created to be loaded by the task. |
| **Drop Current Task** | Drop the task |

For tasks with predecessors:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop Table** | Drop the table |
| **Suspend Root Task** | Root task needs to be put into a suspended state. |
| **Drop Task** | Drops the task |

If a task is part of a DAG of tasks the DAG needs to include a node type called **Task Dag Resume Root**. This node will resume the root node once all the dependent tasks have been created as part of a deployment.

### Limitations of Create or Alter Task

* Collation specifications cannot be altered.
* Setting or unsetting an inline primary key changes the nullability of the column accordingly
* New columns can only be added to the end of the column list
* Columns cannot be renamed. If you attempt to rename a column, the column is dropped and a new column is added. When you rename a column in Coalesce mapping grid, ensure to move the same to the end of column list in mapping grid.
* A column's default value can only be set during its initial creation using a `CREATE OR ALTER` statement; re-running the statement with a different default value will result in an error and will not update/alter/drop the existing default or add a new default.
* All Task nodes, predecessor and root should be present in same schema.
* Renaming a task isn’t supported. Instead it drops old task and recreates the task with new name and schedule specified.

-----

## Create Or Alter DAG Root Task

The Coalesce Create Or Alter DAG Root Task UDN is a node that helps to create/alter a standalone root task.

The root task should have a defined schedule that initiates a run of the DAG. Each of the other tasks has at least one defined predecessor to link the tasks in the DAG. A child task runs only after all of its predecessor tasks run successfully to completion.

Tasks can also be used independently to generate periodic reports by inserting or merging rows into a report table or perform other periodic work.

More information about Tasks can be found in [Snowflake's Introduction to tasks](https://docs.snowflake.com/en/user-guide/tasks-intro).

### Create Or Alter DAG Root Task Node Configuration

The Create Or Alter DAG Root Task node has two configuration groups:

* [Node Properties](#Create-Or-Alter-DAG-Root-Task-Node-Properties)
* [Scheduling Options](#Create-Or-Alter-DAG-Root-Task-Scheduling-Options)

#### Create Or Alter DAG Root Task Node Properties

| **Property** | **Description** |
|-------------|-----------------|
| **Storage Location** | Storage Location where the Stream will be created |
| **Node Type** | Name of template used to create node objects |
| **Description** | A description of the node's purpose |
| **Deploy Enabled** | If TRUE the node will be deployed / redeployed when changes are detected<br/>If FALSE the node will not be deployed or will be dropped during redeployment |

#### Create Or Alter DAG Root Task Scheduling Options

| **Option** | **Description** |
|------------|----------------|
| **Scheduling Mode** | Choose compute type:<br/>- **Warehouse Task** - User managed warehouse executes tasks<br/>- **Serverless Task** - Uses serverless compute |
| **Select Warehouse** | Visible if Scheduling Mode is set to Warehouse Task. <br/> Name of warehouse to run task on without quotes |
| **Select initial serverless size** | Visible when Scheduling Mode is set to Serverless Task <br/> Initial compute size for serverless tasks. Snowflake will adjust size from there based on target schedule and task run times. |
| **Task Schedule** | Choose schedule type:<br/>- Minutes - Specify interval in minutes<br/>- Cron - Use cron expression |
| **Enter root task SQL** | The SQL statement to be run when a standalone root task executes |
| **User Task Timeout** | Specifies the time limit on a single run of the task before it times out (in milliseconds)<br/>Values: 0 - 604800000 (7 days). A value of 0 specifies that the maximum timeout value is enforced|

### Create Or Alter DAG Root Task Deployment

#### Create Or Alter DAG Root Task Deployment Parameters

The parameter name is `targetTaskWarehouse` and the default value is `DEV ENVIRONMENT`.

When set to `DEV ENVIRONMENT` the value entered in the Scheduling Options config Select Warehouse on which to run task will be used when creating the task.

```json
{
    "targetTaskWarehouse": "DEV ENVIRONMENT"
}
```

When set to any value other than `DEV ENVIRONMENT` the node will attempt to create the task using a Snowflake warehouse with the specified value.

For example, with the below setting for the parameter in a QA environment, the task will execute using a warehouse named `compute_wh`.

```json
{
    "targetTaskWarehouse": "compute_wh"
}
```

#### Create Or Alter DAG Root Task Initial Deployment

When deployed for the first time into an environment, the following stages execute:

| **Stage** | **Description** |
|-----------|----------------|
| **Create Root Task** | Creates task that will execute Root SQL on schedule |

If a task is part of a DAG of tasks, the DAG needs to include a node type called `Create DAG Root Resume Task`. This node will resume the root node once all the dependent tasks have been created as part of a deployment.

#### Create Or Alter DAG Root Task Redeployment

After the Task has deployed for the first time into a target environment, subsequent deployments will execute two stages:

| **Stage** | **Description** |
|-----------|----------------|
| **Suspend Root Task** | Suspends root task |
| **Create Root Task** | Recreates root task |

### Create Or Alter DAG Root Task Undeployment

When a Create Or Alter DAG Root Task node is deleted, two stages are executed:

| **Stage** | **Description** |
|-----------|----------------|
| **Suspend Root task** | Suspends root task |
| **Drop current task** | Removes the task |

----

## Create DAG Root Resume Task

The Coalesce Create DAG Root Resume Task UDN is a node type that helps to resume the root task and its dependents or child tasks. Recursively resumes all dependent tasks tied to a specified root task in a DAG using the root task name specified.

The root task should have a defined schedule that initiates a run of the DAG. Each of the other tasks has at least one defined predecessor to link the tasks in the DAG. A child task runs only after all of its predecessor tasks run successfully to completion.

Tasks can also be used independently to generate periodic reports by inserting or merging rows into a report table or perform other periodic work.

More information about Tasks can be found in [Snowflake's Introduction to tasks](https://docs.snowflake.com/en/user-guide/tasks-intro).

### Create DAG Root Resume Task Node Configuration

The Task DAG Resume Root node has two configuration groups:

* [Node Properties](#Create-DAG-Root-Resume-Task-Node-Properties)
* [Scheduling Options](#Create-DAG-Root-Resume-Task-scheduling-options)

#### Create DAG Root Resume Task Node Properties

| **Property** | **Description** |
|-------------|-----------------|
| **Storage Location** | Storage Location where the Stream will be created |
| **Node Type** | Name of template used to create node objects |
| **Description** | A description of the node's purpose |
| **Deploy Enabled** | If TRUE the node will be deployed / redeployed when changes are detected<br/>If FALSE the node will not be deployed or will be dropped during redeployment |

#### Create DAG Root Resume Task Scheduling Options

| **Option** | **Description** |
|------------|----------------|
| **Enter root task name** | Name of the root task to be resumed - recursively resumes all dependent tasks tied to this specified root task |

### Create DAG Root Resume Task Deployment

#### Create DAG Root Resume Task Initial Deployment

When deployed for the first time into an environment the following stage executes:

| **Stage** | **Description** |
|-----------|----------------|
| **Try Enable Root Task** | Resumes root task and all its dependents |

#### Create DAG Root Resume Task Undeployment

| **Stage** | **Description** |
|-----------|----------------|
| **Metadata Updates** | Metadata Updates |

------

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

### Create or Alter Task

| **Component** | **Link** |
|--------------|-----------|
| **Node definition** | [definition.yml](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateOrAlterTask-548/definition.yml) |
| **Create Template** | [create.sql.j2](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateOrAlterTask-548/create.sql.j2) |
| **Run Template** | [run.sql.j2](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateOrAlterTask-548/run.sql.j2)

### Create Or Alter DAG Root Task

| **Component** | **Link** |
|--------------|-----------|
| **Node definition** | [definition.yml](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateOrAlterDAGRootTask-549/definition.yml) |
| **Create Template** | [create.sql.j2](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateOrAlterDAGRootTask-549/create.sql.j2) |

### Create DAG Root Resume Task

| **Component** | **Link** |
|--------------|-----------|
| **Node definition** | [definition.yml](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateDAGRootResumeTask-550/definition.yml) |
| **Create Template** | [create.sql.j2](https://github.com/coalesceio/create-alter-node-types/blob/main/nodeTypes/CreateDAGRootResumeTask-550/create.sql.j2) |
