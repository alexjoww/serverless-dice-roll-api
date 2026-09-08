# serverless-dice-roll-api
A dice roll API built using AWS Lambda + API Gateway

Steps to replicate this project can be found on the AWS Builder Center (copy and paste URL): https://builder.aws.com/content/3Bl1ZPShZdu8G0OfzFlOHXvfed9/build-a-serverless-dice-roll-api

# AWS Lambda Function

This function will generate pseudorandom number and return HTTP response headers:

```
export const handler = async (event) => {
  // API Gateway passes query string params (?sides=20) through the event object.
  const params = event.queryStringParameters || {};
  // Default to a standard 6-sided die if we don't get a value for sides.
  const sides = parseInt(params.sides) || 6; 
  // A 1-sided die isn't a die, and anything more than 100 sides is just chaos. We'll return a client error. 
  if (sides < 2 || sides > 100) {
    return {
      statusCode: 400,
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        error: "Sides must be between 2 and 100.",
      }),
    };
  }

  // Calculate a random integer between 1 and sides.
  const result = Math.floor(Math.random() * sides) + 1;

  // Lambda functions behind API Gateway return this shape:
  // statusCode, headers, and a stringified body
  return {
    statusCode: 200,
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      sides: sides,
      roll: result,
    }),
  };
};
```

# Infrastructure as Code (IaC)

This project utilizes infrastructure as code to spin up the Lambda function and API Gateway. Below is the YAML file that will create all the necessary services within AWS.

```
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: Dice Roll API - A serverless API that rolls dice

Resources:
  DiceRollFunction:
    Type: AWS::Serverless::Function
    Properties:
      Handler: index.handler
      Runtime: nodejs24.x
      CodeUri: lambda/
      Timeout: 5
      MemorySize: 128
      Events:
        RollDice:
          Type: Api
          Properties:
            Path: /roll
            Method: get

Outputs:
  DiceRollApi:
    Description: API Gateway endpoint URL
    Value: !Sub "https://${ServerlessRestApi}.execute-api.${AWS::Region}.amazonaws.com/Prod/roll"
```

# Using the API

To use the API, simply curl the endpoint:

```
curl "https://szgj6qbxjk.execute-api.us-east-1.amazonaws.com/Prod/roll"
```
# Parameters

Optionally, you can pass a parameter "sides" which will take in an integer to be the number of sides on the dice.

For example, roll?sides=12 will generate a pseudorandom number between 1 and 12.

```
curl "https://szgj6qbxjk.execute-api.us-east-1.amazonaws.com/Prod/roll?sides=12"
```

<img width="1396" height="132" alt="image" src="https://github.com/user-attachments/assets/108fb397-0050-488d-a20e-3094544531f7" />

