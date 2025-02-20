---
uid: PIAdapterForAzureEventHubsDataSourceConfiguration
---

# PI Adapter for Azure Event Hubs data source configuration

To use the adapter, you must configure the data source from which it polls data.

## Configure Azure Event Hubs data source

Complete the following steps to configure an Azure Event Hubs data source. Use the `PUT` method in conjunction with the `api/v1/configuration/<ComponentId>/DataSource` REST endpoint to initialize the configuration.

1. Using a text editor, create an empty text file.

2. Copy and paste an example configuration for an Azure Event Hubs data source into the file.

    For sample JSON, see [Azure Event Hubs data source examples](#azure-event-hubs-data-source-examples).

3. Update the example JSON parameters for your environment.

    For a table of all available parameters, see [Azure Event Hubs data source parameters](#azure-event-hubs-data-source-parameters).

4. Save the file. For example, as `ConfigureDataSource.json`.

5. Open a command line session. Change directory to the location of `ConfigureDataSource.json`.

6. Enter the following cURL command (which uses the `PUT` method) to initialize the data source configuration.

    ```bash
    curl -d "@ConfigureDataSource.json" -H "Content-Type: application/json" -X PUT "http://localhost:5590/api/v1/configuration/AzureEventHubs1/DataSource"
    ```

    **Notes:**
  
    * If you installed the adapter to listen on a non-default port, update `5590` to the port number in use.
    * If you use a component ID other than `AzureEventHubs1`, update the endpoint with your chosen component ID.
    * For a list of other REST operations you can perform, like updating or deleting a data source configuration, see [REST URLs](#rest-urls).
    <br/>
    <br/>

7. Configure data selection.

    For more information, see [PI Adapter for Azure Event Hubs data selection configuration](xref:PIAdapterForAzureEventHubsDataSelectionConfiguration)

## Azure Event Hubs data source schema

The full schema definition for the Azure Event Hubs data source configuration is in the `EventHubs_DataSource_schema.json` file located in one of the following folders:

Windows: `%ProgramFiles%\OSIsoft\Adapters\EventHubs\Schemas`

Linux: `/opt/OSIsoft/Adapters/EventHubs/Schemas`

## Azure Event Hubs data source parameters

The following parameters are available for configuring an Azure Event Hubs data source:

| Parameter                     | Required | Type      | Description |
|-------------------------------|----------|-----------|-------------|
| **StreamIdPrefix**            | Optional         | `string`  | Specifies what prefix is used for Stream IDs. The naming convention is `{StreamIdPrefix}{StreamId}`. An empty string means no prefix will be added to the Stream IDs and names. A `null` value defaults to _ComponentID_ followed by a period.<br><br>Example: `Azure Event Hubs1.{Topic}.{ValueField}`<br><br>**Note:** Every time you change the StreamIdPrefix of a configured adapter, for example when you delete and add a data source, you need to restart the adapter for the changes to take place. New streams are created on adapter restart and pre-existing streams are no longer updated. <br><br>Allowed value: any string <br> Default value: `null`            |
| **DefaultStreamIdPattern**    | Optional         | `string`  |  Specifies the default stream Id pattern to use. Possible parameters: `{Topic}`, `{ValueField}`, `{DataType}`. <br><br>Allowed value: any string<br>Default value: `{Topic}.{ValueField}`           |
| **EventHubNamespaceConnectionString** | Required | `string`| The connection string for the Event Hub namespace.<br><br>Allowed value:  The primary or secondary connection string as copied from the Azure portal.<br>Default value: `null`|
| **ConsumerGroupName** | Required | `string` | The name of the consumer group as defined in the Azure Portal for event hub<br><br>Allowed value: Maximum of 256 characters per Azure limits.<br>Default value: `"$Default"` |
| **BlobStorageConnectionString** | Required | `string` | The connection string for the Azure Blob Storage account which is used to store event hub checkpoints <br><br>Allowed value: The primary or secondary connection string as copied from the Azure portal.<br>Default value: `null`|
| **CheckpointBlobContainerName** | Required | `string` | The name of the container in the Blob Storage account used to store event hub checkpoints. <br><br>Allowed value:  3-63 characters long; contains only letters, numbers, and dash characters per Azure restrictions.|
| **TimeZone** | Optional | `string` | The time zone associated with the Event Hub namespace.<br><br>Allowed value: IANA format, for example "America/Los_Angeles". For the complete list, see [iana Time Zone Database](https://www.iana.org/time-zones).

## Azure Event Hubs data source examples

The following are examples of valid Azure Event Hubs data source configurations:

### Minimal data source configuration

```json
{
  "EventHubNamespaceConnectionString": "<Azure Event Hub Namespace connection string>",
  "BlobStorageConnectionString": "<Azure Storage Account connection string>",
  "ConsumerGroupName": "$Default",
  "CheckpointBlobContainerName": "<checkpoint container>"
}
```

### Complete data source configuration

```json
{
  "StreamIdPrefix": "EventHubs1.",
  "DefaultStreamIdPattern": "{EventHubName}.{ValueField}",
  "EventHubNamespaceConnectionString": "<Azure Event Hub Namespace connection string>",
  "BlobStorageConnectionString": "<Azure Storage Account connection string>",
  "ConsumerGroupName": "$Default",
  "CheckpointBlobContainerName": "<checkpoint container>",
  "TimeZone": "America/Los_Angeles"
}
```

## REST URLs

| Relative URL | HTTP verb | Action |
| ------------ | --------- | ------ |
| api/v1/configuration/_ComponentId_/DataSource  | GET | Retrieves the Azure Event Hubs data source configuration |
| api/v1/configuration/_ComponentId_/DataSource  | POST | Creates the Azure Event Hubs data source configuration |
| api/v1/configuration/_ComponentId_/DataSource  | PUT | Configures or updates the Azure Event Hubs data source configuration |
| api/v1/configuration/_ComponentId_/DataSource | DELETE | Deletes the Azure Event Hubs data source configuration |

**Note:** Replace _ComponentId_ with the Id of your Azure Event Hubs component, for example AzureEventHubs1.
