# Create-a-CRUD-HTTP-API-with-Lambda-and-DynamoDB
A serverless CRUD API built with AWS Lambda and DynamoDB that enables managing items through RESTful endpoints.  This project demonstrates how to create, read, update and delete items from a DynamoDB table using API Gateway to route requests to a Lambda function.

# Serverless CRUD API with Lambda and DynamoDB

A serverless API implementation that performs Create, Read, Update, and Delete (CRUD) operations on a DynamoDB table using AWS Lambda and API Gateway.

## Architecture
![Architecture Diagram] ![CRUD HTTP API ](https://github.com/user-attachments/assets/ee0613d5-7c2f-4deb-93d0-33a8360593e4)


## Features
- Serverless architecture using AWS services
- RESTful API endpoints
- DynamoDB for data persistence
- Lambda function for business logic
- API Gateway for request handling

## Prerequisites
- AWS Account
- AWS CLI installed and configured
- Node.js and npm (for Lambda function development)

## API Endpoints
- GET /items - Retrieve all items
- GET /items/{id} - Retrieve a specific item by ID
- PUT /items - Create or update an item
- DELETE /items/{id} - Delete an item by ID

## Implementation Steps

### 1. DynamoDB Setup
1. Create a new DynamoDB table
   
   aws dynamodb create-table --table-name Items --attribute-definitions AttributeName=ID,AttributeType=S --key-schema AttributeName=ID,KeyType=HASH --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5
   
2. Configure 'ID' as the partition key

### 2. Lambda Function
1. Create a new Lambda function
   
   aws lambda create-function --function-name ItemsCRUD --runtime nodejs14.x --handler index.handler --role arn:aws:iam::YOUR_ACCOUNT_ID:role/YOUR_LAMBDA_ROLE --zip-file fileb://function.zip
   
2. Attach necessary IAM role with DynamoDB permissions
3. Implement CRUD operation handlers

### 3. API Gateway Configuration
1. Create a new HTTP API
   
   aws apigatewayv2 create-api --name ItemsAPI --protocol-type HTTP --target arn:aws:lambda:REGION:ACCOUNT_ID:function:ItemsCRUD
   
2. Configure routes for all CRUD operations
3. Create Lambda integration
4. Attach integration to all routes

## Deployment
1. Package Lambda function:
   
   zip -r function.zip .
   
2. Update Lambda function:
   
   aws lambda update-function-code --function-name ItemsCRUD --zip-file fileb://function.zip
   

## Testing
Use Postman or curl to test the API endpoints:

### Sample Requests

1. Create/Update Item:

curl -X "PUT" -H "Content-Type: application/json" -d "{\"id\": \"123\", \"price\": 12345, \"name\": \"myitem\"}" https://your-api-url/items


2. Get All Items:

curl https://your-api-url/items


3. Get Specific Item:

curl https://your-api-url/items/123


4. Delete Item:

curl -X "DELETE" https://your-api-url/items/123


### Sample Responses

1. Successful Item Creation:

{
  "message": "Item created successfully",
  "item": {
    "id": "123",
    "price": 12345,
    "name": "myitem"
  }
}


2. Get All Items:

{
  "items": [
    {
      "id": "123",
      "price": 12345,
      "name": "myitem"
    },
    {
      "id": "456",
      "price": 67890,
      "name": "anotheritem"
    }
  ]
}


## Troubleshooting
- If you encounter "Access Denied" errors, check your IAM roles and policies
- For "Function not found" errors, ensure your Lambda function is in the same region as your API Gateway

## Cleanup
To avoid incurring future charges, delete the resources:

1. Delete the API Gateway:
   
   aws apigatewayv2 delete-api --api-id YOUR_API_ID
   
2. Delete the Lambda function:
   
   aws lambda delete-function --function-name ItemsCRUD
   
3. Delete the DynamoDB table:
   
   aws dynamodb delete-table --table-name Items
   

## Security Considerations
- Use AWS IAM roles and policies to restrict access
- Implement API key for your API Gateway
- Use encryption at rest for your DynamoDB table

## Contributing
1. Fork the repository
2. Create your feature branch (git checkout -b feature/AmazingFeature)
3. Commit your changes (git commit -m 'Add some AmazingFeature')
4. Push to the branch (git push origin feature/AmazingFeature)
5. Open a Pull Request

## Related Resources
- [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
- [Amazon DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)
- [Amazon API Gateway Developer Guide](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)
