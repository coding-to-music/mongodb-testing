# Connect with MongoDB via CLI Data API

https://docs.atlas.mongodb.com/api/data-api/

```java
wget https://downloads.mongodb.com/compass/mongodb-mongosh_1.1.4_amd64.deb

sudo apt update

sudo apt install ./mongodb-mongosh_1.1.4_amd64.deb
```

output
```java
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Note, selecting 'mongodb-mongosh' instead of './mongodb-mongosh_1.1.4_amd64.deb'
The following NEW packages will be installed:
  mongodb-mongosh
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 0 B/42.1 MB of archives.
After this operation, 225 MB of additional disk space will be used.
Get:1 /mnt/ap/ap/mongodb-testing/mongodb-mongosh_1.1.4_amd64.deb mongodb-mongosh amd64 1.1.4 [42.1 MB]
Selecting previously unselected package mongodb-mongosh.
(Reading database ... 156137 files and directories currently installed.)
Preparing to unpack .../mongodb-mongosh_1.1.4_amd64.deb ...
Unpacking mongodb-mongosh (1.1.4) ...
Setting up mongodb-mongosh (1.1.4) ...
Processing triggers for man-db (2.9.1-1) ...
```

# Read and Write with the Data API (Preview)
## Introduction
The MongoDB Data API lets you read and modify data in MongoDB Atlas over HTTP. You don't need any additional MongoDB drivers, libraries, or connection strings; just a standard HTTP client and a valid API key.

The Data API's endpoints expose actions that are similar to the standard query methods available in MongoDB drivers. You can call them to create, read, update, delete, or aggregate documents in your cluster.

## Get Started
Follow the steps below to set up the Data API and send your first requests.

### 1. Enable the Data API
The Data API is disabled by default. To use the API, you need to turn it on in the Atlas UI for one or more clusters.

Click Data API in the left navigation menu. On the following screen, select one or more clusters that you want to enable the API on from the dropdown menu and then click Enable the Data API.

### NOTE
You can enable or disable the Data API for a cluster at any time from the Data API screen.

### 2. Create a Data API Key
The Data API uses project-level API keys to manage access and prevent unauthorized requests. Every request must include a valid Data API key.

A Data API key grants full read and write access every collection in a cluster and can access any enabled cluster in the project.

### IMPORTANT
Data API keys are not the same as the programmatic API keys used to access the Atlas and Realm Administration APIs.

Click Create API Key, enter a unique name for the new key, and then click Create API Key.

You can now see and copy your new API key for the first and only time. Once you close the modal, Atlas will never expose the value again.

Copy the new API key and store it somewhere safe where you can reference it later. Data API keys are sensitive, so make sure not to hardcode them directly into user-facing apps or commit them to version control.

### TIP
You can delete a Data API key at any time. Any request that includes a deleted key will fail. You might delete a key to prevent an existing client from continuing to use the API or if you accidentally expose the key and need to replace it.

### 3. Send a Data API Request
You include your Data API key when you call action endpoints that read and write documents in MongoDB. For a list of all available actions & endpoints, see Data API Resources.

You can run the following commands in a shell to make sure everything works and then start exploring:

### TIP
Make sure to replace placeholder values before you run each request:

- <Data API App ID>: Your Data API App ID, which you can find in the URL Endpoint section of the UI.
- <Data API key>: The Data API key you just created.
- <cluster name>: The name of a cluster with the Data API enabled.
Insert a test document:



```java
curl --request POST \
  'https://data.mongodb-api.com/app/<Data API App ID>/endpoint/data/beta/action/insertOne' \
  --header 'Content-Type: application/json' \
  --header 'Access-Control-Request-Headers: *' \
  --header 'api-key: <Data API Key>' \
  --data-raw '{
      "dataSource": "<cluster name>",
      "database": "learn-data-api",
      "collection": "people",
      "document": {
        "name": "John Sample",
        "age": 42
      }
  }'
```

Then, find the test document that you just inserted:

```java
curl --request POST \
  'https://data.mongodb-api.com/app/<Data API App ID>/endpoint/data/beta/action/findOne' \
  --header 'Content-Type: application/json' \
  --header 'Access-Control-Request-Headers: *' \
  --header 'api-key: <Data API Key>' \
  --data-raw '{
      "dataSource": "<cluster name>",
      "database": "learn-data-api",
      "collection": "people",
      "filter": { "name": "John Sample" }
  }'
```

Finally, delete the test document:

```java
curl --request POST \
  'https://data.mongodb-api.com/app/<Data API App ID>/endpoint/data/beta/action/deleteOne' \
  --header 'Content-Type: application/json' \
  --header 'Access-Control-Request-Headers: *' \
  --header 'api-key: <Data API Key>' \
  --data-raw '{
      "dataSource": "<cluster name>",
      "database": "learn-data-api",
      "collection": "people",
      "filter": { "name": "John Sample" }
  }'
```

### Request Logs
The Data API logs all requests and stores the logs for 30 days.

You can view them on the Data API screen in the Logs tab.

### Billing
The Data API uses the same usage-based pricing as MongoDB Realm applications. For details, see MongoDB Realm Billing.

Data API usage is billed based on the following dimensions:

- Requests: Every action adds one request to your billed total regardless of whether or not the action is successful.
- Compute: Every action bills for time and memory that the server uses to process the action. The exact usage depends on your workload.
- Data Transfer: Every action bills for data returned to the caller in the response. The response may include the result set of the action or metadata that describes the result of the action.
