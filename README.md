# serverless-dice-roll-api
A dice roll API built using AWS Lambda + API Gateway

Steps to replicate this project can be found on the AWS Builder Center: https://builder.aws.com/content/3Bl1ZPShZdu8G0OfzFlOHXvfed9/build-a-serverless-dice-roll-api

# Using the API

To use the API, simply curl the endpoint:

```
curl "https://szgj6qbxjk.execute-api.us-east-1.amazonaws.com/Prod/roll"
```
# Parameters

Optionally, you can pass a parameter "sides" which will take in an integer to be the number of sides on the dice.

For example, roll?sides=12 will generate a pseudorandom number between 1 and 12.
