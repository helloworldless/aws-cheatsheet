# AWS Cheatsheet

Cheatsheet for all things AWS

## Basic

1. SDKs paramters are case sensitive

## DynamoDB

1. Beware of discrepancies between SDKs and documents, for example, `put` vs. `putItem`
1. Primary Key is referred to as the `HASH`
1. Sort key is referred to as the `RANGE`
1. DynamoDB is schemaless, so at table creation time, you only need to define the `HASH` 
parameter and optionally the `RANGE` parameter

### Example

```js
const AWS = require('aws-sdk');
AWS.config.update({ region: 'us-east-1' });
const dynamo = new AWS.DynamoDB.DocumentClient();
```

```js
const params = {
    TableName: 'notes',
    KeyConditionExpression: 'userId = :userId',
    ExpressionAttributeValues: {
        ':userId': '037f49db-c102-4f02-b7f1-99b254655171',
    },
};
const result = await dynamo.query(params).promise();
```


