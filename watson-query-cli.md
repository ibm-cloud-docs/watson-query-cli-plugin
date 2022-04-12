---
 
copyright:
  years: 2022
lastupdated: "2022-01-10"

subcollection: _your-cli-subcollection_

keywords: Watson Query CLI, Watson Query command line , Watson Query terminal, Watson Query shell, Watson Query, Data Virtualization

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}


# Watson Query CLI
{: #CLI-name}

The {{site.data.keyword.cloud}} command-line interface (CLI) provides extra capabilities for service offerings. You can use {{site.data.keyword.cloud_notm}} CLI to manage service instance and virtualizations.
{: shortdesc} 

## Prerequisites
{: #<cli_name>-cli-prereq}

* Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started).
* Install the <CLI_name> CLI by running the following command:

   ```sh
   ibmcloud plugin install watson-query
   ```
   {: pre}

* Use [ibmcloud login command](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login) for logging in to your IBM Cloud account.
* Set IBM Cloudant service URL. For more information, see Service configuration.

You're notified on the command line when updates to the {{site.data.keyword.cloud_notm}} CLI and plug-ins are available. Be sure to keep your CLI up to date so that you can use the latest commands. You can view the current version of all installed plug-ins by running `ibmcloud plugin list`.
{: tip}

## Service configuration

When you make a server resource request, you can either set CRN environment variable or set --instance-id in each sub command to identify instance. To set environment variabel, firstly get CRN of service instance by
   ```sh
   ibmcloud resource service-instances --service-name data-virtualization --long
   ```

Then set the `DATA_VIRTUALIZATION_CRN` environment variable. You can define this variable two ways:

Export them as environment variables for example,export `DATA_VIRTUALIZATION_CRN=....`
Store them in a [credentials file](https://cloud.ibm.com/apidocs/cloudant?code=go#authentication-with-external-configuration).
As an alternative to `ibmcloud login`, you can set the environment variable `DATA_VIRTUALIZATION_APIKEY` to an IAM API key.
{: tip}

{{site.data.keyword.cloud_notm}} CLI requires Java&trade 1.8.0. You can download the CLI from {{site.data.keyword.cloud_notm}} to use on your local system as a complement to the {{site.data.keyword.cloud_notm}} console.
{: note}

## Data sources
{: #watson-query-data-sources-cli}

Connect data sources to the watson query service.

### ibmcloud watson-query datasource-connections
{: #watson-query-cli-datasource-connections-command}

Gets all data source connections that are connected to the service.

```sh
ibmcloud watson-query datasource-connections 
```


#### Examples
{: #watson-query-datasource-connections-cli-examples}

Get data connections

```sh
ibmcloud watson-query datasource-connections
```
{: pre}

#### Example output
{: #watson-query-datasource-connections-cli-output}

```json
{
  "datasource_connections" : [ {
    "agent_class" : "F",
    "dscount" : "0",
    "hostname" : "dv-0.dv.tns.svc.cluster.local",
    "is_docker" : "N",
    "node_name" : "AdminNode",
    "node_description" : "Not specified",
    "port" : "6414",
    "os_user" : "bigsql",
    "data_sources" : [ {
      "cid" : "MSSQL10000",
      "dbname" : "mssql2014db1",
      "connection_id" : "75e4d01b-7417-4abc-b267-8ffb393fb970",
      "srchostname" : "example.ibm.com",
      "srcport" : "1433",
      "srctype" : "MSSQLServer",
      "status" : "string",
      "usr" : "DV-user",
      "uri" : "example.ibm.com:1433/"
    } ]
  } ]
}
```
{: screen}

### ibmcloud watson-query datasource-connection-add
{: #watson-query-cli-datasource-connection-add-command}

Adds a data source connection to the watson query service.

```sh
ibmcloud watson-query datasource-connection-add --datasource-type DATASOURCE-TYPE --name NAME --origin-country ORIGIN-COUNTRY --properties PROPERTIES [--asset-category ASSET-CATEGORY] 
```


#### Command options
{: #watson-query-datasource-connection-add-cli-options}

--datasource-type (string)
:   The type of data source that you want to add. Required.

--name (string)
:   The name of data source. Required.

--origin-country (string)
:   The country of data source that you want to add which data originated from ISO 3166 Country Codes. Required.

--properties ([PostDatasourceConnectionParametersProperties](#cli-post-datasource-connection-parameters-properties-example-schema))
:   Database information. Example: "{\"database\":\"db1\",\"host\":\"databases.example.com\", \"password\":\"adminpassword\", \"port\":\"31365\", \"ssl\":\"true\", \"username\":\"admin\"}". Required.

--asset-category (string)
:   The asset category. Allowable values: [user,system].

#### Examples
{: #watson-query-datasource-connection-add-cli-examples}

Add datasource connections

```sh
ibmcloud watson-query datasource-connection-add --datasource-type MongoDB --name mongo1  --origin-country us  --properties  "{\"database\":\"db1\",\"host\":\"databases.example.com\", \"password"\:\"adminpassword\", \"port\":\"31365\", \"ssl\":\"true\", \"username\":\"admin\"}"
```
{: pre}

### ibmcloud watson-query datasource-connection-delete
{: #watson-query-cli-datasource-connection-delete-command}

Deletes a data source connection from the watson query service.

```sh
ibmcloud watson-query datasource-connection-delete --connection-id CONNECTION-ID --cid CID 
```


#### Command options
{: #watson-query-datasource-connection-delete-cli-options}

--connection-id (string)
:   The connection identifier for the platform.. Required.

--cid (string)
:   The identifier of the connection for the watson query.. Required.

#### Examples
{: #watson-query-datasource-connection-delete-cli-examples}

Delete datasource connection

```sh
ibmcloud watson-query datasource-connection-delete --connection-id 3ba0b656-bbb0-4f1c-8228-e6e800d3b2fa
```
{: pre}

## Users
{: #watson-query-users-cli}

Manage user access to virtualized table.

### ibmcloud watson-query virtualized-table-user-grant
{: #watson-query-cli-virtualized-table-user-grant-command}

Grant a user access to a specific virtualized table.

```sh
ibmcloud watson-query virtualized-table-user-grant --table-name TABLE-NAME --table-schema TABLE-SCHEMA --user USER 
```


#### Command options
{: #watson-query-virtualized-table-user-grant-cli-options}

--table-name (string)
:   The name of the virtualized table. Required.
    The minimum length is `1` character.

--table-schema (string)
:   The schema of the virtualized table. Required.
    The minimum length is `1` character.

--user (string)
:   The identifier of the authorization, if grant access to all users, the value is PUBLIC, othervise the value is the watson query username. Required.
    The minimum length is `1` character.

#### Examples
{: #watson-query-virtualized-table-user-grant-cli-examples}

Grant a user access to a specific virtualized table

```sh
ibmcloud watson-query virtualized-table-user-grant --table-name TABLE1 --table-schema DV_IBMID_270001PD8Q --user user1@ibm.com
```
{: pre}

### ibmcloud watson-query virtualized-table-user-revoke
{: #watson-query-cli-virtualized-table-user-revoke-command}

Revoke user access to the virtualized table.

```sh
ibmcloud watson-query virtualized-table-user-revoke --user USER --table-name TABLE-NAME --table-schema TABLE-SCHEMA 
```


#### Command options
{: #watson-query-virtualized-table-user-revoke-cli-options}

--user (string)
:   The watson query user name, if the value is PUBLIC, it means revoke access privilege from all watson query users. Required.

--table-name (string)
:   The virtualized table's name. Required.

--table-schema (string)
:   The virtualized table's schema name. Required.

#### Examples
{: #watson-query-virtualized-table-user-revoke-cli-examples}

Revoke user access to the virtualized table

```sh
ibmcloud watson-query virtualized-table-user-revoke --table-name TABLE1 --table-schema DV_IBMID_270001PD8Q --user user1@ibm.com
```
{: pre}

## Roles
{: #watson-query-roles-cli}

Manage service roles for users and virtualized tables.

### ibmcloud watson-query virtualized-table-role-grant
{: #watson-query-cli-virtualized-table-role-grant-command}

Grant a user role access to a specific virtualized table.

```sh
ibmcloud watson-query virtualized-table-role-grant --table-name TABLE-NAME --table-schema TABLE-SCHEMA --role ROLE 
```


#### Command options
{: #watson-query-virtualized-table-role-grant-cli-options}

--table-name (string)
:   The name of the virtualized table. Required.
    The minimum length is `1` character.

--table-schema (string)
:   The schema of the virtualized table. Required.
    The minimum length is `1` character.

--role (string)
:   The identifier of the authorization, if grant access to all users, the value is PUBLIC, othervise the value is the watson query username. Required.
    The minimum length is `1` character.

#### Examples
{: #watson-query-virtualized-table-role-grant-cli-examples}

Grants a user role access to a specific virtualized table

```sh
ibmcloud watson-query virtualized-table-role-grant --table-name TABLE1 --table-schema DV_IBMID_270001PD8Q --role DV_ENGINEER
```
{: pre}

### ibmcloud watson-query virtualized-table-role-revoke
{: #watson-query-cli-virtualized-table-role-revoke-command}

Revoke roles access to a virtualized table.

```sh
ibmcloud watson-query virtualized-table-role-revoke --role ROLE --table-name TABLE-NAME --table-schema TABLE-SCHEMA 
```


#### Command options
{: #watson-query-virtualized-table-role-revoke-cli-options}

--role (string)
:   The watson query role type. Values can be DV_ADMIN, DV_ENGINEER, DV_STEWARD, or DV_WORKER, which correspond to MANAGER, ENGINEER, STEWARD, and USER roles in the user interface. Required.

--table-name (string)
:   The virtualized table's name. Required.

--table-schema (string)
:   The virtualized table's schema name. Required.

#### Examples
{: #watson-query-virtualized-table-role-revoke-cli-examples}

Revoke roles access to a virtualized table

```sh
ibmcloud watson-query virtualized-table-role-revoke --table-name TABLE1 --table-schema DV_IBMID_270001PD8Q --role DV_ENGINEER
```
{: pre}

### ibmcloud watson-query tables-for-role
{: #watson-query-cli-tables-for-role-command}

Retrieves the list of virtualized tables that have a specific role.

```sh
ibmcloud watson-query tables-for-role --role ROLE 
```


#### Command options
{: #watson-query-tables-for-role-cli-options}

--role (string)
:   Watson Query has four roles: MANAGER, STEWARD, ENGINEER and USER The value of rolename should be one of them. Required.

#### Examples
{: #watson-query-tables-for-role-cli-examples}

Get virtualized tables by role

```sh
ibmcloud watson-query tables-for-role --role DV_ENGINEER
```
{: pre}

#### Example output
{: #watson-query-tables-for-role-cli-output}

```json
{
  "objects" : [ {
    "table_name" : "TEST_TABLE",
    "table_schema" : "ADMIN"
  } ]
}
```
{: screen}

## Securities
{: #watson-query-securities-cli}

Manage Wason Knowledge Catalog(WKC) policy enforcement status.

### ibmcloud watson-query policy-status-update
{: #watson-query-cli-policy-status-update-command}

Turn on WKC policy enforcement status.

```sh
ibmcloud watson-query policy-status-update --status STATUS 
```


#### Command options
{: #watson-query-policy-status-update-cli-options}

--status (string)
:   Set the status of WKC policy - can be 'enable' or 'disable'. Required.

#### Examples
{: #watson-query-policy-status-update-cli-examples}

Turn on or off WKC policy enforcement status

```sh
ibmcloud watson-query policy-status-update --enable
```
{: pre}

### ibmcloud watson-query policy-status
{: #watson-query-cli-policy-status-command}

Get WKC policy enforcement status, return enabled or disabled.

```sh
ibmcloud watson-query policy-status 
```


#### Examples
{: #watson-query-policy-status-cli-examples}

Get WKC policy enforcement status

```sh
ibmcloud watson-query policy-status
```
{: pre}

## Virtualization
{: #watson-query-virtualization-cli}

Create virtualized table.

### ibmcloud watson-query virtualized-table-create
{: #watson-query-cli-virtualized-table-create-command}

Transform a given data source table into a virtualized table.

```sh
ibmcloud watson-query virtualized-table-create --source-table-name SOURCE-NAME --source-table-def-file SOURCE-TABLE-DEF-FILE --sources SOURCES --virtualized-table-name VIRTUALIZED-NAME --virtualized-schema VIRTUALIZED-SCHEMA --virtualized-table-def-file VIRTUALIZED-TABLE-DEF-FILE [--is-included-columns IS-INCLUDED-COLUMNS] [--replace REPLACE] 
```


#### Command options
{: #watson-query-virtualized-table-create-cli-options}

--source-table-name (string)
:   The name of the source table. Required.

--source-table-def-file ([VirtualizeTableSourceTableDefFile](#cli-virtualize-table-parameter-source-table-def-item-example-schema))
:   &nbsp; Required.

--sources ([]string)
:   The name of data source. Required.

--virtualized-table-name (string)
:   The name of the table that will be virtualized. Required.

--virtualized-schema (string)
:   The schema of the table that will be virtualized. Required.

--virtualized-table-def-file ([VirtualizeTableVirtualTableDefFile](#cli-virtualize-table-parameter-virtual-table-def-item-example-schema))
:   &nbsp; Required.

--is-included-columns (string)
:   The columns that are included in the source table.

--replace (bool)
:   Determines whether to replace columns in the virtualized table.

#### Examples
{: #watson-query-virtualized-table-create-cli-examples}

Virtualize table

```sh
ibmcloud watson-query  virtualized-table-create --source-table-name table1 --source-table-def-file source_tabel_def.json   --virtualized-schema  DV_IBMID_270001PD8Q --sources CONN1:TABLE1 --virtualized-table-name TABLE1  --virtualized-table-def-file virtualized_table_def.json. The json file content example: "[{"column_name":"COL1","column_type":"VARCHAR"}]"
```
{: pre}

### ibmcloud watson-query virtualized-table-delete
{: #watson-query-cli-virtualized-table-delete-command}

Remove specified virtualized table. You must specify the schema and table name.

```sh
ibmcloud watson-query virtualized-table-delete --virtualized-schema VIRTUALIZED-SCHEMA --virtualized-name VIRTUALIZED-NAME 
```


#### Command options
{: #watson-query-virtualized-table-delete-cli-options}

--virtualized-schema (string)
:   The schema of virtualized table to be deleted. Required.

--virtualized-name (string)
:   The name of virtualized table to be deleted. Required.

#### Examples
{: #watson-query-virtualized-table-delete-cli-examples}

Delete virtualized table

```sh
ibmcloud watson-query virtualized-table-delete --virtualized-schema DV_IBMID_270001PD8Q --virtualized-name TABLE1
```
{: pre}

## Primary catalog
{: #watson-query-primary-catalog-cli}

Manage the primary WKC catalog information in watson query console.

### ibmcloud watson-query primary-catalog
{: #watson-query-cli-primary-catalog-command}

Get primary catalog ID from the table.

```sh
ibmcloud watson-query primary-catalog 
```


#### Examples
{: #watson-query-primary-catalog-cli-examples}

Get primary catalog ID from the table DVSYS.INSTANCE_INFO

```sh
ibmcloud watson-query primary-catalog
```
{: pre}

### ibmcloud watson-query primary-catalog-set
{: #watson-query-cli-primary-catalog-set-command}

Insert primary catalog ID into table DVSYS.INSTANCE_INFO.

```sh
ibmcloud watson-query primary-catalog-set --guid GUID 
```


#### Command options
{: #watson-query-primary-catalog-set-cli-options}

--guid (string)
:   Primary catalog ID. Required.

#### Examples
{: #watson-query-primary-catalog-set-cli-examples}

Insert primary catalog ID into table DVSYS.INSTANCE_INFO

```sh
ibmcloud watson-query primary-catalog-set --guid d77fc432-9b1a-4938-a2a5-9f37e08041f6
```
{: pre}

### ibmcloud watson-query primary-catalog-delete
{: #watson-query-cli-primary-catalog-delete-command}

Remove the setting of the primary catalog for enforced publication.

```sh
ibmcloud watson-query primary-catalog-delete --guid GUID 
```


#### Command options
{: #watson-query-primary-catalog-delete-cli-options}

--guid (string)
:   The watson query user name, if the value is PUBLIC, it means revoke access privilege from all watson query users. Required.

## Publish objects
{: #watson-query-publish-objects-cli}

Publish virtualized table to WKC.

### ibmcloud watson-query virtualized-table-publish
{: #watson-query-cli-virtualized-table-publish-command}

Publish virtualized tables to WKC.

```sh
ibmcloud watson-query virtualized-table-publish --catalog-id CATALOG-ID --allow-duplicates ALLOW-DUPLICATES --assets ASSETS 
```


#### Command options
{: #watson-query-virtualized-table-publish-cli-options}

--catalog-id (string)
:   Catalog ID. Required.

--allow-duplicates (bool)
:   Whether duplicated asset allowd. Required.

--assets ([CatalogPublishParametersAssetsItem[]](#cli-catalog-publish-parameters-assets-item-example-schema))
:   Asset description. Example: "[{\"schema\": \"db2inst1\",\"table\": \"employee\"}]". Required.

#### Examples
{: #watson-query-virtualized-table-publish-cli-examples}

Publish virtualized tables to WKC

```sh
ibmcloud watson-query virtualized-table-publish --catalog-id 12c60f7e-c366-4cda-ba3a-bfbb577a5f56 --allow-duplicates true --virtualized-schema DV_IBMID_6610020D12 --virtualized-table EMPLOYEE
```
{: pre}

## Schema examples
{: #watson-query-schema-examples}

The following schema examples represent the data that you need to specify for a command option. These examples model the data structure and include placeholder values for the expected value type. When you run a command, replace these values with the values that apply to your environment as appropriate.

### CatalogPublishParametersAssetsItem[]
{: #cli-catalog-publish-parameters-assets-item-example-schema}

The following example shows the format of the CatalogPublishParametersAssetsItem[] object.

```json

[ {
  "schema" : "db2inst1",
  "table" : "EMPLOYEE"
} ]
```
{: codeblock}

### PostDatasourceConnectionParametersProperties
{: #cli-post-datasource-connection-parameters-properties-example-schema}

The following example shows the format of the PostDatasourceConnectionParametersProperties object.

```json

{
  "access_token" : "exampleString",
  "account_name" : "exampleString",
  "api_key" : "exampleString",
  "auth_type" : "exampleString",
  "client_id" : "exampleString",
  "client_secret" : "exampleString",
  "collection" : "exampleString",
  "credentials" : "exampleString",
  "database" : "TPCDS",
  "host" : "192.168.0.1",
  "http_path" : "exampleString",
  "jar_uris" : "exampleString",
  "jdbc_driver" : "exampleString",
  "jdbc_url" : "exampleString",
  "password" : "password",
  "port" : "50000",
  "project_id" : "exampleString",
  "properties" : "exampleString",
  "refresh_token" : "exampleString",
  "role" : "exampleString",
  "sap_gateway_url" : "exampleString",
  "server" : "exampleString",
  "service_name" : "exampleString",
  "sid" : "exampleString",
  "ssl" : "false",
  "ssl_certificate" : "exampleString",
  "ssl_certificate_host" : "exampleString",
  "ssl_certificate_validation" : "exampleString",
  "username" : "db2inst1",
  "warehouse" : "exampleString"
}
```
{: codeblock}

### VirtualizeTableSourceTableDefFile
{: #cli-virtualize-table-parameter-source-table-def-item-example-schema}

The following example shows the format of the VirtualizeTableSourceTableDefFile object.

```json

[ {
  "column_name" : "Column1",
  "column_type" : "INTEGER"
} ]
```
{: codeblock}

### VirtualizeTableVirtualTableDefFile
{: #cli-virtualize-table-parameter-virtual-table-def-item-example-schema}

The following example shows the format of the VirtualizeTableVirtualTableDefFile object.

```json

[ {
  "column_name" : "Column_1",
  "column_type" : "INTEGER"
} ]
```
{: codeblock}
