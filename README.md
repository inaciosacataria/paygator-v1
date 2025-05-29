# paygator-v1

# PayGator - Payment Middleware

PayGator is a robust payment middleware that integrates delivery platforms with E2Payments, supporting eMola and M-Pesa mobile wallets in Mozambique.

## Features

- Process payments through eMola and M-Pesa
- Automatic provider detection based on phone number prefixes
- Secure webhook handling with signature verification
- Comprehensive transaction management and tracking
- Refund processing
- Rate limiting and security features
- Detailed logging and monitoring

## Prerequisites

- Node.js 16 or higher
- MongoDB 4.4 or higher
- Redis (optional, for caching)
- RabbitMQ (optional, for message queuing)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/paygator.git
cd paygator
```

2. Install dependencies:
```bash
npm install
```

3. Copy the environment file and configure your variables:
```bash
cp .env.example .env
```

4. Build the project:
```bash
npm run build
```

5. Start the server:
```bash
npm start
```

For development:
```bash
npm run dev
```

## API Documentation

### Authentication

All API requests must include an API key in the `X-API-Key` header:

```
X-API-Key: your-api-key-here
```

### Endpoints

#### Create Payment

```http
POST /api/v1/payments
Content-Type: application/json

{
  "amount": 100.00,
  "currency": "MZN",
  "phone": "841234567",
  "reference": "ORDER123",
  "description": "Payment for Order #123",
  "metadata": {
    "customer_id": "12345"
  },
  "callback_url": "https://your-platform.com/webhooks/payments"
}
```

#### Get Payment

```http
GET /api/v1/payments/{payment_id}
```

#### List Payments

```http
GET /api/v1/payments?startDate=2024-01-01&endDate=2024-12-31&status=completed&provider=mpesa
```

#### Initiate Refund

```http
POST /api/v1/payments/{payment_id}/refunds
Content-Type: application/json

{
  "amount": 100.00,
  "reason": "Customer request"
}
```

### Webhooks

Your platform will receive webhook notifications at the specified `callback_url` with the following headers:

```
X-Webhook-Signature: computed-signature
X-Webhook-Timestamp: iso-timestamp
```

Webhook payload example:

```json
{
  "event_type": "payment.completed",
  "payment_id": "payment_123",
  "provider": "mpesa",
  "reference": "ORDER123",
  "amount": 100.00,
  "status": "completed",
  "timestamp": "2024-04-29T14:28:43.511Z",
  "metadata": {
    "customer_id": "12345"
  }
}
```

## Error Handling

The API uses standard HTTP status codes and returns errors in the following format:

```json
{
  "error": "Error message here"
}
```

Common status codes:
- 200: Success
- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error

## Development

### Running Tests

```bash
npm test
```

### Linting

```bash
npm run lint
```

### Type Checking

```bash
npm run typecheck
```

## Security Considerations

- All API endpoints are protected by API key authentication
- Webhooks are verified using HMAC signatures
- Rate limiting is enabled to prevent abuse
- Input validation is performed on all requests
- Sensitive data is never logged
- All communications use TLS

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a new Pull Request

## License

This project is licensed under the ISC License. 
