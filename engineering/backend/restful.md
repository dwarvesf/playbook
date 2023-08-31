# RESTful API

An API is a user interface for developers. Put the effort in to ensure it's not just functional but pleasant to use.

## Terminologies

The following are the most important terms related to REST APIs.
- Resource is an object or representation of something, which has some associated data with it, and there can be a set of methods to operate on it. E.g., Animals, schools, and employees are resources, and `delete, add, update` are the operations to be performed on these resources.
- Collections are set of resources, e.g., Companies is the collection of Company resource.
- URL (Uniform Resource Locator) is a path through which a resource can be located, and some actions can be performed on it.

## Convention

- We prefer Swagger for REST API documentation.
- Use Nouns in URI: REST API should be designed for resources. For example, instead of `/createUser` use `/users`.
- We prefer to use plurals, but there is no hard rule that one can't use the singular for the resource name.
- Let the HTTP verb define action. Don't misuse safe methods. Use HTTP methods according to the action which needs to be performed.
- Use SSL everywhere, no exceptions.
- Pretty print by default & ensure gzip is supported.
- Always version your API. Version via the URL, not via headers.
- Depict resource [hierarchy](https://hackernoon.com/restful-api-designing-guidelines-the-best-practices-60e1d954e7c9) through URI. If a resource contains sub-resources, make sure to depict this in the API to make it more explicit. `GET /users/123/posts/1`, which will retrieve Post with id 1 by user with id 123.
- Field name casing: make sure the casing convention is consistent across the application. If the request body or response type is JSON, please follow [camelCase](https://en.wikipedia.org/wiki/Camel_case) to maintain the consistency.

## API Version

Always version your API. Version via the URL, not via headers. Versioning APIs always helps to ensure backward compatibility of service while adding new features or updating existing functionality for new clients. The backend will support the old version for a specific amount of time.

- URL: Embed the version in the URL such as `POST /v2/users`.
- Header
    - Custom header: Adding a custom X-API-VERSION header key by the client can be used by a service to route a request to the correct endpoint.
    - Accept header: Using the accept header to specify your version.

## Filter, Search and Sort
Use query parameters for advanced filtering, sorting & searching.

###Pagination

**Offset pagination:**

- `page` - current page wanted to fetch
- `size` - number of items per page

- The API should return `totalItems` and `totalPages`

Example:
```
GET /companies?page=23&size=50
```

**Cursor pagination**

- `cursor` - is the reference element in the list. The best practice for choosing the cursor is only sorting by a unique sequential column, and creating a new concatenate column if you have to order by multiple none unique columns.
- `size` - number of items per page

- The API should return `previousCursor` and `nextCursor`

```
GET /companies?cursor=12&size=50&cursorDirection=
```

### Sort:

- Validate all sort able fields
- By default sort in ascending order
- Use  `minus(-)` prefix to indicate sort in descending order

Example
```
# sort by created_at in descending order
GET /companies?sort=-created_at

# sort by created_at in ascending order
GET /companies?sort=created_at
```

### Filter:

- Validate all enums values
- Use repeated query keys for multiple values
- Use RFC339 time format or unix timestamp for date time filter 

```
#### filter by category
GET /companies?category=software

#### filter by locations
GET /companies?location=saigon&location=hanoi

#### Filter by time range
GET /companies?start=1662873449&end=1662873449
GET /companies?start=2022-09-10T05:10:55.308Z&end=2022-09-11T05:10:55.308Z

#### Use specific field name when case we have multiple time range filter
GET /companies?created_at_from=2022-09-10T05:10:55.308Z&created_at_to=2022-09-11T05:10:55.308Z&updated_at_from=2022-09-10T05:10:55.308Z&updated_at_to=2022-09-11T05:10:55.308Z
```

### Search:
```
GET /companies?search=Mckinsey
```

## Batch Delete
- There is no clean `Restful way` to delete a collection of id without any limitation:
    - DELETE method with list of ID in query params is limit by its length (2048 characters)
    - DELETE method doesn't require a BODY, so most of Gateway services ignore the body when direct the request to servers.

- We use a custom `POST` methods to achieve a batch delete functionality:
```
POST /companies/batch_delete

BODY: {
    ids: ["id_1", "id_2"]
}
```

## Auto loading related resource representations

There are many cases where an API consumer needs to load data related to (or referenced from) the resource being requested.

In this case, embed would be a separated list of fields to be embedded. Dot-notation could be used to refer to sub-fields.

```
GET /companies/12?embed=lead.name|assigned_user
```

## Return something useful from POST, PATCH & PUT requests

POST, PUT, or PATCH methods, used to create a resource or update fields in a resource, should always return updated resource representation as a response with appropriate status code as described in further points.

POST, if successful in adding a new resource, should return HTTP status code 201 along with the URI of the newly created resource in the Location header (as per HTTP specification)

## Stateless Authentication & Authorization

REST APIs should be stateless. Every request should be self-sufficient and must be fulfilled without knowledge of the prior request. This means that request authentication should not depend on cookies or sessions. Instead, each request should come with some sort of authentication credentials.

Previously, developers stored user information in server-side sessions, which is not a scalable approach. Use token-based authentication, transported over OAuth2 where delegation is needed.

- For service-to-service communication, try to have the encrypted API-key passed in the header.
- For user authorization, JWT with OAuth2 provides a way to go.

## Rate limiting

To prevent abuse, it is standard practice to add some rate-limiting to an API. At a minimum, include the following headers:

- `X-Rate-Limit-Limit` - The number of allowed requests in the current period
- `X-Rate-Limit-Remaining` - The number of remaining requests in the current period
- `X-Rate-Limit-Reset` - The number of seconds left in the current period

Some APIs use a UNIX timestamp (seconds since epoch) for X-Rate-Limit-Reset. Don't do this! Use [RFC 1123 date formats](https://www.ietf.org/rfc/rfc1123.txt) instead.

## Caching

HTTP provides a built-in caching framework. All you have to do is include some additional outbound response headers and do a little validation when you receive some inbound request headers.

There are 2 approaches: [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) and [Last-Modified](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.29)

- ETag: When generating a response, include an HTTP header ETag containing a hash or checksum of the representation. This value should change whenever the output representation changes. If an inbound HTTP request includes a If-None-Match header with a matching ETag value, the API should return a 304 Not Modified status code instead of the resource's output representation.

- Last-Modified: This works like ETag, except that it uses timestamps. The response header Last-Modified contains a timestamp in RFC 1123 format, which is validated against If-Modified-Since. Note that the HTTP spec has had 3 different acceptable date formats, and the server should be prepared to accept any one of them.

## HTTP status codes

HTTP defines a bunch of meaningful [status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) that can be returned from your API. These can be leveraged to help the API consumers route their responses accordingly. I've curated a shortlist of the ones that you definitely should be using:

- `200 OK` - Response to a successful GET, PUT, PATCH or DELETE. It can also be used for a POST that doesn't result in creation.
- `201 Created` - Response to a POST that results in a creation. Should be combined with a Location header pointing to the location of the new resource
- `204 No Content` - Response to a successful request that won't be returning a body (like a DELETE request)
- `304 Not Modified` - Used when HTTP caching headers are in play
- `400 Bad Request` - The request is malformed, such as if the body does not parse or failed validation
- `401 Unauthorized` - When no or invalid authentication details are provided. Also useful to trigger an auth popup if the API is used from a browser
- `403 Forbidden` - When authentication succeeded but the authenticated user doesn't have access to the resource
- `404 Not Found` - When a non-existent resource is requested
- `405 Method Not Allowed` - When an HTTP method is being requested that isn't allowed for the authenticated user
- `410 Gone` - Indicates that the resource at this endpoint is no longer available. Useful as a blanket response for old API versions
- `415 Unsupported Media Type` - If the incorrect content type was provided as part of the request
- `422 Unprocessable Entity` - Used for validation errors
- `429 Too Many Requests` - When a request is rejected due to rate limiting
- `500 Internal Server Error` - The server has encountered a situation it does not know how to handle. This can be database error, 3rd party service error, etc.
- `500 Internal Server Error` - The server has encountered a situation it does not know how to handle. This can be database error, 3rd party service error, etc.

## Response

Single data entry response
``` json
{
    "data": {
        "id": 1,
        "name": "Dwarves",
        "plug": "dwarvesf"
    }
}
```

Multi data entries or array
``` json
{
    "data": [],
    "metadata": {
        "pageSize": 20,
        "currentPage": 2,
        "totalPages": 15,
        "totalCount": 295,
        "hasNextPage": true
    }
}
```

Error response
``` json
{
    "data": null,
    "error": "the error message",
    "errors": "Array<{field: string, msg: string, [x: string]: any}> List of detail error message"
}
```

## Error

A JSON error body should provide a few things for the developer - a useful error message, a unique error code (that can be looked up for more details in the docs), and possibly detailed description. For this part, please check out [Error Handling](/engineering/error.md).
