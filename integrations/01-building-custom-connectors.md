# Building Custom Connectors

**Date:** January 2026  
**Author:** Power Platform Journey  
**Category:** Integrations  
**Tags:** #custom-connectors #api #integration #rest-api

## Overview

Custom connectors enable Power Platform to communicate with any REST API. This guide walks through building and deploying custom connectors for external service integration.

## What are Custom Connectors?

Custom connectors are wrappers around REST APIs that make them available in:
- Power Automate (flows)
- Power Apps (canvas and model-driven)
- Logic Apps

### When to Use Custom Connectors

- Integrating with APIs without pre-built connectors
- Wrapping internal/private APIs
- Extending existing connector functionality
- Creating reusable integration components

## Prerequisites

- API endpoint URL and documentation
- Authentication details
- Power Platform environment with appropriate permissions
- API testing tool (Postman, Insomnia, etc.)

## Building Your First Custom Connector

### Step 1: Gather API Information

For this example, let's integrate with a hypothetical Weather API:

```
Base URL: https://api.weather.example.com/v1
Authentication: API Key in header
Endpoints:
  GET /current?city={city}
  GET /forecast?city={city}&days={days}
```

### Step 2: Create the Connector

1. Navigate to Power Automate or Power Apps
2. Go to Data → Custom Connectors
3. Click "+ New custom connector"
4. Choose "Create from blank"

### Step 3: Configure General Settings

```
Connector Name: Weather API
Description: Get current weather and forecasts
Host: api.weather.example.com
Base URL: /v1
```

### Step 4: Configure Security

**API Key Authentication:**
```
Authentication Type: API Key
Parameter label: API Key
Parameter name: X-API-Key
Parameter location: Header
```

**OAuth 2.0 Example:**
```
Authentication Type: OAuth 2.0
Identity Provider: Generic OAuth 2
Client ID: [Your Client ID]
Client Secret: [Your Client Secret]
Authorization URL: https://api.example.com/oauth/authorize
Token URL: https://api.example.com/oauth/token
Refresh URL: https://api.example.com/oauth/refresh
Scope: read write
```

### Step 5: Define Actions

**Action 1: Get Current Weather**

```
Summary: Get current weather
Description: Retrieve current weather for a city
Operation ID: GetCurrentWeather

Request:
  URL: /current
  Method: GET
  Parameters:
    - Name: city
      Type: string
      Required: Yes
      Location: query
      Description: City name

Response:
  Status Code: 200
  Body:
    {
      "city": "London",
      "temperature": 15,
      "condition": "Cloudy",
      "humidity": 65,
      "windSpeed": 10
    }
```

**Action 2: Get Forecast**

```
Summary: Get weather forecast
Description: Retrieve weather forecast for specified days
Operation ID: GetForecast

Request:
  URL: /forecast
  Method: GET
  Parameters:
    - Name: city
      Type: string
      Required: Yes
      Location: query
    - Name: days
      Type: integer
      Required: No
      Default: 5
      Location: query

Response: [Array of forecast objects]
```

### Step 6: Test the Connector

1. Create a new connection
2. Provide authentication details
3. Test each operation
4. Verify responses

### Step 7: Update from OpenAPI/Postman

**Import from OpenAPI (Swagger):**
```
1. Export your API's OpenAPI specification
2. In connector editor, choose "Import from OpenAPI"
3. Upload the OpenAPI file
4. Review and adjust imported definitions
```

**Import from Postman:**
```
1. Export Postman collection
2. Choose "Import from Postman"
3. Upload collection file
4. Configure authentication
```

## Advanced Features

### Dynamic Schema

For APIs with variable response structures:

```json
{
  "x-ms-dynamic-schema": {
    "operationId": "GetSchema",
    "parameters": {
      "type": {
        "parameter": "type"
      }
    },
    "value-path": "schema"
  }
}
```

### Dynamic Values (Dropdowns)

Populate dropdowns from API:

```json
{
  "x-ms-dynamic-values": {
    "operationId": "GetCities",
    "value-path": "id",
    "value-title": "name"
  }
}
```

### Paging

For APIs returning large datasets:

```json
{
  "x-ms-pageable": {
    "nextLinkName": "nextLink"
  }
}
```

## Real-World Example: CRM Integration

### Scenario
Connect Power Platform to internal CRM API.

### API Details
```
Base URL: https://crm.company.com/api/v2
Auth: OAuth 2.0
Operations:
  - GET /customers
  - POST /customers
  - GET /customers/{id}
  - PATCH /customers/{id}
  - GET /orders
  - POST /orders
```

### Implementation Steps

1. **Documented the API** - Gathered all endpoints, parameters, and responses
2. **Set up OAuth** - Registered app in CRM system
3. **Created connector** - Imported from OpenAPI spec
4. **Added policies** - Rate limiting and error handling
5. **Tested thoroughly** - Validated all operations
6. **Shared connector** - Distributed to team

### Challenges Faced

**Challenge**: API rate limiting (100 calls/minute)  
**Solution**: Implemented caching in Power Automate flows

**Challenge**: Complex nested responses  
**Solution**: Used Parse JSON actions to flatten data

**Challenge**: Authentication token expiry  
**Solution**: Configured proper refresh token handling

## Policy Definitions

Add custom policies for advanced scenarios:

### Set Request Headers
```xml
<policies>
  <set-header name="User-Agent" exists-action="override">
    <value>PowerPlatform/1.0</value>
  </set-header>
  <set-header name="X-Custom-Header" exists-action="override">
    <value>@parameters('customValue')</value>
  </set-header>
</policies>
```

### Transform Response
```xml
<policies>
  <forward-request />
  <set-body>
    @{
      var body = context.Response.Body.As<JObject>();
      body["timestamp"] = DateTime.UtcNow;
      return body.ToString();
    }
  </set-body>
</policies>
```

## Best Practices

1. **Clear Naming** - Use descriptive operation IDs and parameter names
2. **Complete Documentation** - Provide summaries and descriptions
3. **Error Handling** - Define expected error responses
4. **Validation** - Add parameter validation rules
5. **Versioning** - Plan for API version changes
6. **Security** - Never hardcode credentials
7. **Testing** - Test all operations thoroughly
8. **Performance** - Consider pagination and throttling

## Troubleshooting

### Common Issues

**401 Unauthorized**
- Verify authentication configuration
- Check token expiration
- Validate API key/credentials

**400 Bad Request**
- Review parameter names and types
- Check required vs optional fields
- Validate request body format

**Connection Timeout**
- Increase timeout settings
- Check API endpoint availability
- Review network connectivity

## Testing Strategies

### Unit Testing
- Test each operation individually
- Use known good data
- Verify response schemas

### Integration Testing
- Test in Power Automate flows
- Test in Power Apps
- Validate error handling

### Load Testing
- Simulate concurrent requests
- Monitor rate limits
- Test timeout scenarios

## Deployment and Sharing

### Connector Certification
For public distribution:
1. Complete Microsoft verification
2. Meet technical requirements
3. Provide support plan
4. Submit for review

### Team Distribution
1. Share in specific environment
2. Create connection for users
3. Document usage
4. Provide examples

## Monitoring and Maintenance

- **Analytics** - Monitor usage patterns
- **Errors** - Track failure rates
- **Updates** - Plan for API changes
- **Support** - Provide documentation

## Next Steps

- [Azure Integration Patterns](./02-azure-integration-patterns.md)
- [API Best Practices](./03-api-best-practices.md)

## Resources

- [Custom Connectors Documentation](https://learn.microsoft.com/en-us/connectors/custom-connectors/)
- [OpenAPI Specification](https://swagger.io/specification/)
- [Connector Certification](https://learn.microsoft.com/en-us/connectors/custom-connectors/submit-certification)

---

*Building custom connectors? Share your integration stories!*
