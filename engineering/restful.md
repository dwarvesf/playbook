---
title: null
description: null
date: null
redirect:
  - /s/gtAXaw
---

# Restful API

An API is a user interface for developers. Put the effort in to ensure it's not just functional but pleasant to use.

## Terminology

The following terms are crucial when discussing REST APIs:

- **Resource**: An object or representation of something with associated data. Operations such as `delete`, `add`, and `update` can be performed on resources. Examples include Animals, schools, and employees.
- **Collection**: A group of resources, like the collection of Company resources under Companies.
- **URL (Uniform Resource Locator)**: A path used to locate a resource and perform actions on it.

## Conventions

- **Documentation**: Swagger is our chosen tool for REST API documentation.
- **Use nouns in URI**: Design REST APIs around resources. Instead of `/create-user`, use `/users`.
- **Plurals**: Prefer using plurals in URIs, but singular forms are also acceptable.
- **HTTP verbs**: Map actions to HTTP verbs. Avoid misusing safe methods. Match HTTP methods to the required action.
- **SSL everywhere**: Use SSL for all communications, without exceptions.
- **Pretty print & gzip**: Default to pretty-printing and ensure support for gzip compression.
- **API versioning**: Always version APIs. Indicate versions in the URL, not headers.
- **Depict Resource [Hierarchy](https://hackernoon.com/restful-api-designing-guidelines-the-best-practices-60e1d954e7c9)**: Represent resource hierarchy in URIs. If sub-resources exist, reflect this in the API. For instance, use `GET /users/123/posts/1` to fetch Post 1 by User 123.
- **Field name casing**: Maintain consistent casing across the application. If using JSON for requests or responses, follow [camelCase](https://en.wikipedia.org/wiki/Camel_case) convention.

## API versioning

Always version APIs for backward compatibility while introducing new features or updates.

- **URL**: Embed version in the URL, e.g., `POST /v2/users`.
- **Headers**:
  - **Custom header**: Use a custom `X-API-VERSION` header to route requests correctly.
  - **Accept header**: Specify version using the accept header.

Version via the URL, not via headers. Versioning APIs always helps to ensure backward compatibility of service while adding new features or updating existing functionality for new clients. The backend will support the old version for a specific amount of time.

## Filtering, searching, and sorting

Filtering, searching, and sorting are essential features for efficiently retrieving the desired data from your API. They enable clients to tailor their requests to specific criteria and arrange the results as needed.

### Supported types for filtering

You can filter by various data types to customize your API queries. Here are the supported types, their descriptions, and examples:

| Type   | Description                                       | Example                                                               |
| ------ | ------------------------------------------------- | --------------------------------------------------------------------- |
| int    | Integer value filter                              | GET /companies?size=10                                                |
| float  | Floating-point value filter                       | GET /products?price=20.99                                             |
| string | String value filter                               | GET /users?name=John                                                  |
| enum   | Enumeration value filter (limited predefined set) | GET /orders?status=completed                                          |
| list   | List of values filter                             | GET /events?location=nyc&location=la                                  |
| time   | Time range filter in ISO8601 format               | GET /logs?startTime=2023-08-01T00:00:00Z&endTime=2023-08-15T23:59:59Z |

### Pagination

Pagination is crucial when dealing with large datasets to ensure manageable response sizes. It allows clients to retrieve data in smaller chunks or pages. Include the following parameters:

- **page**: The page number you want to retrieve.
- **pageSize**: The number of items per page.

Example:

```
GET /companies?page=2&pageSize=20
```

### Sorting

Sorting helps arrange data in a specific order, making it easier for clients to analyze and display information. Use the following parameter:

- **sort**: Specify the field by which the data should be sorted. Add a minus sign (`-`) for descending order. You can also use multiple `sort` parameters for complex sorting.

Example:

```
GET /companies?sort=-createdAt&sort=updatedAt

```

### Filtering

Filtering allows clients to narrow down the results based on specific criteria. Use query parameters to achieve this:

- **category**: Filter by a specific category, e.g., `category=software`.
- **location**: Filter by one or multiple locations, e.g., `location=saigon&location=hanoi`.
- **startTime**: Filter data with a start time in ISO8601 format.
- **endTime**: Filter data with an end time in ISO8601 format.

Example:

```
GET /companies?category=software&location=saigon&location=hanoi&startTime=2023-08-01T00:00:00Z&endTime=2023-08-15T23:59:59Z

```

### Search

Searching enables clients to find specific keywords or phrases within the data. Use the following parameter:

- **search**: Include the search term to look for.

Example:

```
GET /companies?search=Mckinsey
```

### Cursor-based pagination

In addition to regular pagination, consider offering cursor-based pagination for a more efficient user experience. The cursor is a base64-encoded string that contains additional data, allowing users to easily navigate through pages and retrieve results more reliably.

- **cursor**: The base64-encoded string representing the cursor, containing necessary data.

Example:

```
GET /companies?cursor=base64encodedstring&pageSize=20
```

By providing these filtering, searching, and sorting options, you empower clients to retrieve the precise data they need while optimizing performance and user experience.

## Batch delete

Batch deletion presents challenges in adhering to a clean RESTful approach due to limitations with the HTTP `DELETE` method, which doesn't support request payloads. To overcome these limitations, you can employ a custom `POST` method for implementing batch delete functionality.

### Limitations of `DELETE` method

The HTTP `DELETE` method is designed to indicate that a resource should be removed. However, according to the HTTP specification, the `DELETE` method typically doesn't include request payloads. This can hinder attempts to include a list of IDs or other data required for batch deletion.

### Custom `POST` Method for Batch delete

To achieve batch deletion while working within the constraints of the HTTP methods, a custom `POST` method is recommended. This approach allows you to include a request payload containing the necessary information, such as a list of IDs to be deleted.

Here's an example of how to structure the batch delete request:

```
POST /companies/batch-delete

BODY: {
    "ids": ["id_1", "id_2"]
}

```

By using the custom `POST` method and including the `ids` array in the request body, you can effectively perform batch deletion operations.

Please note that the decision to use a custom `POST` method for batch deletion is influenced by the need to work around the limitations of the HTTP `DELETE` method. While it may not adhere to the strictest RESTful conventions, it offers a pragmatic solution for managing batch delete operations.

## Responses from POST, PATCH & PUT

Responses from POST, PUT, or PATCH should include the updated resource representation. POST success should return status 201 with the new resource's URI in the Location header.

## Stateless authentication & authorization

REST APIs must be stateless, with self-sufficient requests. Authenticate via credentials, avoiding cookies or sessions. Use token-based authentication, especially OAuth2 for delegation.

- For service-to-service communication, pass encrypted API keys in headers.
- For user authorization, use JWT with OAuth2.

## Rate limiting

To prevent abuse, apply rate-limiting. Include these headers:

- `X-Rate-Limit-Limit`: Allowed requests in the current period.
- `X-Rate-Limit-Remaining`: Remaining requests in the current period.
- `X-Rate-Limit-Reset`: Seconds left in the current period. Use [RFC 1123 date format](https://www.ietf.org/rfc/rfc1123.txt).

## Caching

HTTP provides a built-in caching framework. All you have to do is include some additional outbound response headers and do a little validation when you receive some inbound request headers.

- **[ETag](http://en.wikipedia.org/wiki/HTTP_ETag)**: When generating a response, include an HTTP header ETag containing a hash or checksum of the representation. This value should change whenever the output representation changes. If an inbound HTTP request includes a If-None-Match header with a matching ETag value, the API should return a 304 Not Modified status code instead of the resource's output representation.
- **[Last-Modified](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.29)**: This works like ETag, except that it uses timestamps. The response header Last-Modified contains a timestamp in RFC 1123 format, which is validated against If-Modified-Since. Note that the HTTP spec has had 3 different acceptable date formats, and the server should be prepared to accept any one of them.

## HTTP status codes

HTTP status codes indicate the outcome of a request. They help clients understand results and actions. Here's a list of common HTTP status codes and their descriptions:

- `200 OK`: Request succeeded. Can be used for GET, PUT, PATCH, and DELETE methods, indicating the action was successful. Response may include a representation.
- `201 Created`: POST request resulted in a new resource's creation. Should be combined with a Location header pointing to the location of the new resource.
- `202 Accepted`: The request has been accepted but not yet enacted. Used when the action will likely succeed but hasn't been completed yet.
- `204 No Content`: Request succeeded, and there's no additional response content. Useful for DELETE requests or successful updates where the client doesn't need data back.
- `304 Not Modified`: Used with conditional requests. Indicates that the resource hasn't changed since the last request, allowing for efficient caching.
- `400 bad request`: Request couldn't be understood or was malformed. Typically used when the server can't process the request due to client error.
- `401 Unauthorized`: Authentication credentials are missing or invalid. Used when authentication is required but not provided.
- `403 Forbidden`: Authenticated user lacks permission to access the resource. Different from `401`, as it's related to authorization, not authentication.
- `404 not found`: The requested resource doesn't exist on the server.
- `405 Method Not Allowed`: The requested HTTP method is not supported for the given resource.
- `409 Conflict`: Indicates a conflict with the current state of the target resource. Used in cases where the requested action cannot be completed due to a conflict.
- `410 Gone`: The requested resource is no longer available on the server. Can be used as a blanket response for old API versions.
- `422 Unprocessable Entity`: Used for validation errors when the server understands the content but cannot process it due to validation issues.
- `429 Too Many Requests`: The client has sent too many requests in a given amount of time, leading to rate limiting.
- `500 internal server error`: Indicates a server-side error that prevents the request from being fulfilled.

## Best practices for HTTP method response

- **GET**:
  - Use `200 OK` for successful retrieval of resources.
  - Use `404 not found` if the resource isn't available.
- **POST**:
  - Use `201 Created` for successful resource creation.
  - Use `400 bad request` for malformed requests.
- **PUT**:
  - Use `200 OK` or `204 No Content` for successful updates.
  - Use `404 not found` if the resource isn't available.
- **PATCH**:
  - Use `200 OK` for successful partial updates.
  - Use `404 not found` if the resource isn't available.
- **DELETE**:
  - Use `202 Accepted` if the action is accepted but not yet enacted.
  - Use `204 No Content` for successful deletion.
  - Use `404 not found` if the resource isn't available.
- Batch delete:
  - Use `200 OK` for successful partial delete.
  - Use `400 bad request` for malformed requests.
  - Use `404 not found` if the resource isn't available.

## Response formats

When designing your API responses, consistency is key. Using standardized response formats makes it easier for clients to understand and process the data. Here are the recommended response formats for single and multiple data entries:

### Single data entry response

```json
{
  "data": {
    "id": 1,
    "name": "Dwarves",
    "plug": "dwarvesf"
  }
}
```

### Multiple data entries or array

```json
{
  "data": [
    {
      "id": 1,
      "name": "Company A"
    },
    {
      "id": 2,
      "name": "Company B"
    }
    // ... more data entries
  ],
  "metadata": {
    "pageSize": 20,
    "page": 1,
    "totalPages": 15,
    "totalRecords": 295,
    "sort": ["+createdAt", "-name"],
    "hasNext": true
  }
}
```

### Multiple Data Entries with Cursor-based pagination

```json
{
  "data": [
    {
      "id": 1,
      "name": "Company A"
    },
    {
      "id": 2,
      "name": "Company B"
    }
    // ... more data entries
  ],
  "metadata": {
    "pageSize": 20,
    "cursor": "id",
    "sort": ["createdAt", "-name"],
    "hasNext": true
  }
}
```

In these response formats, the `data` field holds the actual resource data or an array of resources. The `metadata` field provides information about pagination and sorting, allowing clients to navigate and process the data effectively.

- **pageSize**: The number of items per page.
- **page**: The current page number (for non-cursor pagination).
- **totalPages**: The total number of pages available.
- **totalRecords**: The total number of records across all pages.
- **sort**: The sorting criteria applied to the data (optional).
- **cursor**: The cursor used for cursor-based pagination (optional).
- **hasNext**: Indicates if there are more results available.

By using these standardized response formats, you enhance the consistency and clarity of your API responses, making it easier for developers to work with your API.

## Error handling

Error handling is essential to ensure that both API providers and consumers have a clear understanding of the issues that might arise during API interactions. Well-structured error responses help developers diagnose and address problems more effectively.

### HTTP status codes and Error Responses

HTTP status codes provide a standardized way of indicating the outcome of API requests. Alongside these codes, error responses should include structured information to convey the specifics of the error. Here are some common scenarios:

### 400 bad request

Example:

```json
{
  "error": "field1: required, field2: greater than 10",
  "code": "CREATE_BODY_INVALID",
  "errors": [
    {
      "field": "field1",
      "error": "required"
    }
  ],
  "traceId": "<trace_id>"
}
```

### 404 not found

Example:

```json
{
  "error": "Entity not found",
  "code": "ENTITY_NOT_FOUND",
  "traceId": "<trace_id>"
}
```

### 500 internal server error

Example:

```json
{
  "error": "Internal server error",
  "code": "INTERNAL_CODE_001",
  "traceId": "<trace_id>"
}
```

### Error response structure

A well-structured error response should include the following fields:

- **error**: A human-readable error message.
- **code**: A unique error code that can be referenced for troubleshooting.
- **errors** (optional): An array of error objects, each containing a specific field and associated error message.
- **traceId**: A unique identifier for the error occurrence, helpful for debugging.

By consistently using this error response structure, you can facilitate efficient error handling and resolution across your API.

For more detailed information and best practices on error handling, please refer to [Error handling](/engineering/error.md).
