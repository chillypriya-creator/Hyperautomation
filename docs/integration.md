# Hyperautomation Integration Guide

## API Overview

The Hyperautomation API provides RESTful endpoints for integrating with external systems.

### Base URL
```
https://api.hyperautomation.dev/v1
```

## Authentication

### API Key Authentication
```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://api.hyperautomation.dev/v1/processes
```

### OAuth 2.0
```bash
POST /auth/token
Content-Type: application/json

{
  "client_id": "your_client_id",
  "client_secret": "your_client_secret",
  "grant_type": "client_credentials"
}
```

## Core Endpoints

### Processes

#### List Processes
```bash
GET /processes
```

Response:
```json
{
  "data": [
    {
      "id": "proc_123",
      "name": "Invoice Processing",
      "status": "active",
      "created_at": "2024-01-10T10:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "per_page": 20,
    "total": 100
  }
}
```

#### Create Process
```bash
POST /processes
Content-Type: application/json

{
  "name": "Invoice Processing",
  "description": "Automated invoice processing workflow",
  "workflow": {
    "steps": [
      {
        "id": "step1",
        "type": "trigger",
        "config": {
          "event": "file_upload"
        }
      },
      {
        "id": "step2",
        "type": "action",
        "config": {
          "service": "ocr",
          "params": {}
        }
      }
    ]
  }
}
```

#### Execute Process
```bash
POST /processes/{process_id}/execute
Content-Type: application/json

{
  "input_data": {
    "document_url": "https://example.com/invoice.pdf"
  }
}
```

Response:
```json
{
  "execution_id": "exec_456",
  "status": "running",
  "created_at": "2024-01-10T10:05:00Z"
}
```

### Executions

#### Get Execution Status
```bash
GET /executions/{execution_id}
```

Response:
```json
{
  "id": "exec_456",
  "process_id": "proc_123",
  "status": "completed",
  "output_data": {
    "invoice_number": "INV-2024-001",
    "amount": 1000.00,
    "vendor": "ACME Corp"
  },
  "completed_at": "2024-01-10T10:06:30Z"
}
```

## Pre-built Connectors

### Enterprise Systems
- SAP
- Oracle ERP
- Microsoft Dynamics
- NetSuite

### Cloud Applications
- Salesforce
- HubSpot
- Slack
- Microsoft Teams
- Google Workspace

### Databases
- MySQL
- PostgreSQL
- Oracle Database
- MongoDB
- SQL Server

### File Formats
- PDF
- Excel
- Word
- CSV
- JSON
- XML

## Webhook Configuration

### Register Webhook
```bash
POST /webhooks
Content-Type: application/json

{
  "event": "process.completed",
  "url": "https://your-domain.com/webhook",
  "active": true
}
```

### Webhook Payload
```json
{
  "event": "process.completed",
  "timestamp": "2024-01-10T10:06:30Z",
  "data": {
    "execution_id": "exec_456",
    "process_id": "proc_123",
    "status": "completed",
    "output_data": {}
  }
}
```

## Error Handling

### Error Response Format
```json
{
  "error": {
    "code": "INVALID_REQUEST",
    "message": "Invalid input parameters",
    "details": {
      "field": "workflow",
      "reason": "Missing required field"
    }
  }
}
```

### Common Error Codes
- `INVALID_REQUEST` - Invalid input
- `UNAUTHORIZED` - Authentication failed
- `FORBIDDEN` - Access denied
- `NOT_FOUND` - Resource not found
- `CONFLICT` - Resource conflict
- `RATE_LIMIT` - Rate limit exceeded
- `SERVER_ERROR` - Internal server error

## Rate Limiting

- **Free Plan**: 100 requests/minute
- **Pro Plan**: 1,000 requests/minute
- **Enterprise**: Unlimited

Headers:
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1641738400
```
