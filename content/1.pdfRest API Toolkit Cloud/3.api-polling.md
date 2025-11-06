## Introduction

API Polling is a technique used to repeatedly check the status of a long-running process by making periodic requests to a specific endpoint. In pdfRest, the `/request-status` endpoint is designed for this purpose. This guide will walk you through the steps to implement and use API Polling effectively.

::alert{to="https://pdfrest.com/apitools?signup=true" target="_blank" icon="lucide:link"}
API Polling is exclusive to API Toolkit Cloud subscriptions, and not available on self-hosted installations of pdfRest. Click this link to create an account on pdfRest.com now!
::

## Prerequisites

Before you start, ensure you have:
- Access to the pdfRest API Toolkit Cloud.
- An API key for authentication.
- Basic knowledge of HTTP requests and responses.

## Step-by-Step Guide

::steps{level=4}

#### 1. Making the Initial Request

When you initiate a process (e.g., converting a document), you will make an initial request to the relevant endpoint. This request will return a unique ID that you will use for polling.

**Example Initial Request:**

```bash
curl -X POST "https://api.pdfrest.com/convert" \
     -H "Authorization: Bearer YOUR_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{
           "source": "https://example.com/document.pdf",
           "format": "docx"
         }'
```

**Response:**

```json
{
  "id": "12345",
  "status": "processing"
}
```

#### 2. Polling the `/request-status` Endpoint

Using the ID from the initial request, you will poll the `/request-status` endpoint to check the status of your request.

**Polling Request:**

```bash
curl -X GET "https://api.pdfrest.com/request-status/12345" \
     -H "Authorization: Bearer YOUR_API_KEY"
```

#### 3. Understanding Status Codes

The server will respond with different status codes indicating the state of your request:

- `202 Accepted`: The request is still being processed.
- `200 OK`: The request has been completed successfully.
- `4xx`: There was an error with your request.

**Example Response:**

```json
{
  "id": "12345",
  "status": "completed",
  "result": "https://example.com/converted-document.docx"
}
```

#### 4. Handling the Final Data

Once the request is completed (`200 OK`), you can retrieve the final data from the provided URL in the response.

**Example Final Data Retrieval:**

```bash
curl -X GET "https://example.com/converted-document.docx"
```
::


### Best Practices

- **Polling Interval**: Choose an appropriate polling interval (e.g., every 5 seconds) to avoid excessive requests that may lead to rate limiting.
- **Error Handling**: Implement robust error handling to manage different status codes and potential issues.
- **Timeouts**: Set reasonable timeouts for your polling requests to prevent indefinite waiting.

### Conclusion

API Polling is a powerful technique for managing long-running processes in pdfRest. By following this guide, you can effectively implement and use the `/request-status` endpoint to monitor and retrieve the status of your requests.

For more detailed information, refer to the <a href="/pdfrest-api-toolkit-cloud/api-reference-guide/" target="_blank" rel="noopener noreferrer">Cloud API Reference Guide</a> .
