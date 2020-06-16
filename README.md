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

#### Create Table (Serverless Framework)

```yaml
Type: AWS::DynamoDB::Table
Properties:
  TableName: ${self:custom.dynamoTableName}
  AttributeDefinitions:
    - AttributeName: userId
      AttributeType: S
    - AttributeName: createdAt
      AttributeType: S
  KeySchema:
    - AttributeName: userId
      KeyType: HASH
    - AttributeName: createdAt
      KeyType: RANGE
  ProvisionedThroughput:
    ReadCapacityUnits: 1
    WriteCapacityUnits: 1
```

#### Initialize JavaScript SDK

```js
const AWS = require('aws-sdk');
AWS.config.update({ region: 'us-east-1' });
const dynamo = new AWS.DynamoDB.DocumentClient();
```

#### Query Operation

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

## API Gateway

1. REST API is the most common
1. To integrate with a Lambda, there are two types:
  1. `LAMBDA_PROXY`
    1. A lot of documentation assumes you're using this if you're routing to a Lambda
    1. Gives up some control in favor of ease of use and conventions like indicating statusCode 
    via the returned object: `{statusCode: 500, body: 'Internal Server Error'}`
  1. `LAMBDA`
    1. Gives the most control, allows some logic to be built into the API Gateway rather than in 
    client code, for example 

## Lambda

### Example `LAMBDA_PROXY` Event

<details>
  <summary>Click to expand</summary>
  

  ```json
  {
      "resource": "/activities",
      "path": "/activities",
      "httpMethod": "POST",
      "headers": {
          "Accept": "*/*",
          "Accept-Encoding": "gzip, deflate, br",
          "Authorization": "<REMOVED>",
          "Cache-Control": "no-cache",
          "CloudFront-Forwarded-Proto": "https",
          "CloudFront-Is-Desktop-Viewer": "true",
          "CloudFront-Is-Mobile-Viewer": "false",
          "CloudFront-Is-SmartTV-Viewer": "false",
          "CloudFront-Is-Tablet-Viewer": "false",
          "CloudFront-Viewer-Country": "US",
          "Content-Type": "text/plain",
          "Host": "<REMOVED>.execute-api.us-east-1.amazonaws.com",
          "Postman-Token": "<REMOVED>",
          "User-Agent": "PostmanRuntime/7.25.0",
          "Via": "1.1 ede80d7a8b8f3860f5bfc65271bbce47.cloudfront.net (CloudFront)",
          "X-Amz-Cf-Id": "<REMOVED>",
          "X-Amzn-Trace-Id": "<REMOVED>",
          "X-Forwarded-For": "<REMOVED>",
          "X-Forwarded-Port": "443",
          "X-Forwarded-Proto": "https"
      },
      "multiValueHeaders": {
          "Accept": [
              "*/*"
          ],
          "Accept-Encoding": [
              "gzip, deflate, br"
          ],
          "Authorization": [
              "<REMOVED>"
          ],
          "Cache-Control": [
              "no-cache"
          ],
          "CloudFront-Forwarded-Proto": [
              "https"
          ],
          "CloudFront-Is-Desktop-Viewer": [
              "true"
          ],
          "CloudFront-Is-Mobile-Viewer": [
              "false"
          ],
          "CloudFront-Is-SmartTV-Viewer": [
              "false"
          ],
          "CloudFront-Is-Tablet-Viewer": [
              "false"
          ],
          "CloudFront-Viewer-Country": [
              "US"
          ],
          "Content-Type": [
              "text/plain"
          ],
          "Host": [
              "<REMOVED>.execute-api.us-east-1.amazonaws.com"
          ],
          "Postman-Token": [
              "<REMOVED>"
          ],
          "User-Agent": [
              "PostmanRuntime/7.25.0"
          ],
          "Via": [
              "<REMOVED>"
          ],
          "X-Amz-Cf-Id": [
              "<REMOVED>"
          ],
          "X-Amzn-Trace-Id": [
              "<REMOVED>"
          ],
          "X-Forwarded-For": [
              "<REMOVED>"
          ],
          "X-Forwarded-Port": [
              "443"
          ],
          "X-Forwarded-Proto": [
              "https"
          ]
      },
      "queryStringParameters": null,
      "multiValueQueryStringParameters": null,
      "pathParameters": null,
      "stageVariables": null,
      "requestContext": {
          "resourceId": "ohbrd5",
          "authorizer": {
              "claims": {
                  "at_hash": "<REMOVED>",
                  "sub": "<REMOVED>",
                  "aud": "<REMOVED>",
                  "email_verified": "true",
                  "event_id": "<REMOVED>",
                  "token_use": "id",
                  "auth_time": "1592265376",
                  "iss": "https://cognito-idp.us-east-1.amazonaws.com/<REMOVED>",
                  "cognito:username": "<REMOVED>",
                  "exp": "Tue Jun 16 00:56:16 UTC 2020",
                  "iat": "Mon Jun 15 23:56:16 UTC 2020",
                  "email": "<REMOVED>"
              }
          },
          "resourcePath": "/activities",
          "httpMethod": "POST",
          "extendedRequestId": "<REMOVED>",
          "requestTime": "16/Jun/2020:00:30:22 +0000",
          "path": "/dev/activities",
          "accountId": "<REMOVED>",
          "protocol": "HTTP/1.1",
          "stage": "dev",
          "domainPrefix": "<REMOVED>",
          "requestTimeEpoch": 1592267422277,
          "requestId": "<REMOVED>",
          "identity": {
              "cognitoIdentityPoolId": null,
              "accountId": null,
              "cognitoIdentityId": null,
              "caller": null,
              "sourceIp": "<REMOVED>",
              "principalOrgId": null,
              "accessKey": null,
              "cognitoAuthenticationType": null,
              "cognitoAuthenticationProvider": null,
              "userArn": null,
              "userAgent": "PostmanRuntime/7.25.0",
              "user": null
          },
          "domainName": "<REMOVED>.execute-api.us-east-1.amazonaws.com",
          "apiId": "<REMOVED>"
      },
      "body": "{\r\n    \"createdAt\": \"2020-06-15T23:56:39.724Z\",\r\n    \"type\": \"EAT\"\r\n}",
      "isBase64Encoded": false
  }
  ```
</details>

### Example `LAMBDA` Integration Request Mapping

Uses DSL to construct even which is sent to the Lambda

Where this lives: Resources > Choose Resource > Integration Request
 > Mapping Templates > Select a Content Type

```json
#set($inputRoot = $input.path('$'))
{
  "operation": "put",
  "params": {
      "TableName" : "notes",
      "Item" : {
          "userId" : "$context.authorizer.claims.sub",
          "noteId" : "$inputRoot.noteId",
          "note" : "$inputRoot.note"
       }
  }
}
```
