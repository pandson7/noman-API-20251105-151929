# Product Specifications API

A serverless REST API built with AWS CDK that provides access to product specifications stored in DynamoDB with flexible JSON schema support.

## Architecture

- **API Gateway**: RESTful API endpoints with CORS support
- **AWS Lambda**: Node.js 22.x functions for business logic
- **DynamoDB**: NoSQL database with flexible JSON schema
- **CDK**: Infrastructure as Code for deployment

## API Endpoints

### GET /products
Retrieves all product specifications from the database.

**Response**: Array of product objects
**Status Codes**: 200 (Success), 500 (Internal Error)

**Example Response**:
```json
[
  {
    "productId": "ELEC001",
    "productName": "iPhone 15 Pro",
    "category": "Electronics",
    "brand": "Apple",
    "specifications": {
      "screen": "6.1 inch",
      "storage": "256GB",
      "color": "Natural Titanium",
      "camera": "48MP"
    },
    "price": 999.99,
    "availability": true,
    "createdAt": "2025-11-05T20:28:21.326Z",
    "updatedAt": "2025-11-05T20:28:21.329Z"
  }
]
```

### GET /products/{productId}
Retrieves a specific product by its ID.

**Parameters**: 
- `productId` (path parameter): The unique identifier for the product

**Response**: Single product object
**Status Codes**: 200 (Success), 404 (Not Found), 500 (Internal Error)

**Example Response**:
```json
{
  "productId": "ELEC001",
  "productName": "iPhone 15 Pro",
  "category": "Electronics",
  "brand": "Apple",
  "specifications": {
    "screen": "6.1 inch",
    "storage": "256GB",
    "color": "Natural Titanium",
    "camera": "48MP"
  },
  "price": 999.99,
  "availability": true,
  "createdAt": "2025-11-05T20:28:21.326Z",
  "updatedAt": "2025-11-05T20:28:21.329Z"
}
```

## Sample Data

The API comes pre-populated with sample products across different categories:

- **Electronics**: iPhone 15 Pro, MacBook Pro 14
- **Clothing**: Cotton T-Shirt
- **Home & Garden**: Office Chair
- **Books**: Clean Code

## Deployment

### Prerequisites
- Node.js 18+ installed
- AWS CLI configured with appropriate permissions
- AWS CDK CLI installed (`npm install -g aws-cdk`)

### Deploy the Stack
```bash
# Install dependencies
npm install

# Bootstrap CDK (first time only)
npx cdk bootstrap

# Deploy the stack
npm run deploy
```

### Outputs
After deployment, you'll receive:
- **API URL**: The base URL for your API Gateway
- **Table Name**: The DynamoDB table name

## Testing

### Using curl

Test all products:
```bash
curl -X GET "https://your-api-url/prod/products"
```

Test specific product:
```bash
curl -X GET "https://your-api-url/prod/products/ELEC001"
```

Test non-existent product:
```bash
curl -X GET "https://your-api-url/prod/products/INVALID123"
```

## AWS Resources Created

- **DynamoDB Table**: `ProductSpecifications20251105151929`
  - Partition Key: `productId` (String)
  - Provisioned billing with auto-scaling enabled
  - Read/Write capacity: 1-10 units

- **Lambda Functions**:
  - `GetProducts20251105151929`: Retrieves all products
  - `GetProductById20251105151929`: Retrieves specific product
  - `SeedDataProvider20251105151929`: Seeds sample data during deployment

- **API Gateway**: `ProductSpecificationsApi20251105151929`
  - REST API with CORS enabled
  - Two endpoints: `/products` and `/products/{productId}`

## Data Schema

Products support flexible JSON schema with the following common fields:

```json
{
  "productId": "string (required)",
  "productName": "string",
  "category": "string",
  "brand": "string",
  "specifications": {
    // Flexible object with product-specific attributes
  },
  "price": "number",
  "availability": "boolean",
  "createdAt": "string (ISO date)",
  "updatedAt": "string (ISO date)"
}
```

## Error Handling

The API returns appropriate HTTP status codes:
- **200**: Success
- **400**: Bad Request (missing productId)
- **404**: Product Not Found
- **500**: Internal Server Error

Error responses include descriptive messages:
```json
{
  "error": "Product not found"
}
```

## Security Features

- **CORS**: Enabled for cross-origin requests
- **IAM**: Least-privilege roles for Lambda functions
- **API Gateway**: Built-in throttling and monitoring

## Cleanup

To remove all resources:
```bash
npm run destroy
```

## Project Structure

```
├── src/
│   └── lambda/           # Lambda function code
├── lib/                  # CDK stack definition
├── cdk-app/             # CDK application entry point
├── package.json         # Dependencies and scripts
├── cdk.json            # CDK configuration
└── README.md           # This file
```

## API URL

Your deployed API is available at:
**https://y2cr0mrfwb.execute-api.us-east-1.amazonaws.com/prod/**
