# Batches

## The batch

A batch is the result of an asynchronous operation (e.g. commissioning devices) that may take longer than a few seconds to complete. This enables any client to avoid blocking while waiting for the operation to complete. Below are its attributes:

|Attributes||
|----------|---------|
|batchId| Batch ID (int) |
|batchProgressMessage| Message describing the status of the batch |
|batchProgressValue | Percentage of completion of the batch. May be equal to -1 if the info is not available yet |
|batchRunning | Indicates whether the batch has completed or not (boolean)|
|cancelRequested| Indicates whether a cancel order has been issued for the batch (boolean)|
|cancelled| Indicates whether the batch has been cancelled (boolean)|
|errorCode| Error code |
|errorCodeLabel | Description of the error |
|message| Message describing the status of the batch |
|status|Status of the batch (`OK` or `ERROR`)|
|statusError|Indicates whether any error occured processing this batch (boolean)|
|statusOk|Indicates whether any error occured processing this batch (boolean)|
|value|Data structure provided by the underlying process|


## Retrieve the status of a batch

> Retrieving the status of batch ID 1481016600181

```python
import requests

payload = {
  'batchId': 1481016600181,
  'ser': 'json'
}

r = requests.get(SLV_URL + "/api/batch/getBatchResult", params=payload, auth=('USERNAME', 'PASSWORD'))
```

> Sample response

```python
{
  "batchId" : "1481016600181",
  "batchProgressMessage" : "Commissioning process finished.",
  "batchProgressValue" : 100,
  "batchRunning" : false,
  "cancelRequested" : false,
  "cancelled" : false,
  "errorCode" : "0",
  "errorCodeLabel" : null,
  "message" : null,
  "status" : "OK",
  "statusError" : false,
  "statusOk" : true,
  "value" :
    {
      "errorMessagesCount" : 0,
      "infoMessagesCount" : 4,
      "login" : "admin",
      "status" : 0,
      "stepResults" :
        [
          {
            "errorMessages" :
              [
              ],
            "errorMessagesCount" : 0,
            "infoMessages" :
              [
                {
                  "status" : 0,
                  "statusText" : "OK",
                  "text" : "Inventory has been checked in the database and is consistent"
                }
              ],
            "infoMessagesCount" : 1,
            "label" : "Check database configuration",
            "messages" :
              [
                {
                  "status" : 0,
                  "statusText" : "OK",
                  "text" : "Inventory has been checked in the database and is consistent"
                }
              ],
            "status" : 0,
            "stepLabel" : "Check database configuration",
            "stepName" : "check",
            "warningMessages" :
              [
              ],
            "warningMessagesCount" : 0
          },
          {
            "errorMessages" :
              [
              ],
            "errorMessagesCount" : 0,
            "infoMessages" :
              [
                {
                  "status" : 0,
                  "statusText" : "OK",
                  "text" : "Devices configs are pushed using mode 'LIST'."
                },
                {
                  "status" : 0,
                  "statusText" : "OK",
                  "text" : "2 devices in list. 2 were pushed. 0 were ignored and 0 were not pushed because not found on controller."
                },
                {
                  "status" : 0,
                  "statusText" : "OK",
                  "text" : "All messages and configuration data have been sent and accepted by the controller"
                }
              ],
            "infoMessagesCount" : 3,
            "label" : "Commissioning",
            "messages" :
              [
                {
                  "status" : 0,
                  "statusText" : "OK",
                  "text" : "Devices configs are pushed using mode 'LIST'."
                },
                {
                  "status" : 0,
                  "statusText" : "OK",
                  "text" : "2 devices in list. 2 were pushed. 0 were ignored and 0 were not pushed because not found on controller."
                },
                {
                  "status" : 0,
                  "statusText" : "OK",
                  "text" : "All messages and configuration data have been sent and accepted by the controller"
                }
              ],
            "status" : 0,
            "stepLabel" : "Commissioning",
            "stepName" : "commissioning",
            "warningMessages" :
              [
              ],
            "warningMessagesCount" : 0
          }
        ],
      "warningMessagesCount" : 0
    }
}

```

To retrieve the status of a batch, use the method `getBatchResult` available at `/api/batch`. 

|Arguments||
|----------|---------|
|batchId (required) | Batch ID (int)|