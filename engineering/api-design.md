# API Design

## The Objectives
- Define consistent practices and patterns for all API endpoints accross the company
- Adhere as closely as possible to accepted REST/HTTP best practices in the industry at-large
- Allow for partners (developers, clients) to use these guidelines for their own REST endpoint design

## Table of Contents
- [1. Representational State Transfer (REST)](#representational-state-transfer-(REST))
- [2. API Design Principles](#api-design-principles)
- [3. Taxonomy](#taxonomy)
- [4. Consistency Fundamentals](#consistency-fundamentals)
- [5. Header](#header)
- [6. Response](#response)
- [7. CORS](#cors)
- [8. Collections](#collections)
- [9. References](#references)


## Representational State Transfer (REST)

### Definition
Representational state transfer (REST) is a **software architectural style that defines a set of constraints** to be used for creating Web services. 

Web services that conform to the REST architectural style, called RESTful Web services, provide interoperability between computer systems on the Internet. RESTful Web services allow the requesting systems to access and manipulate textual representations of Web resources by using a uniform and predefined set of stateless operations. Other kinds of Web services, such as SOAP Web services, expose their own arbitrary sets of operations

### Set of Contraints
Six guiding constraints define a RESTful system. These constraints restrict the ways that the server can process and respond to client requests so that, by operating within these constraints, the system gains desirable non-functional properties, such as **performance, scalability, simplicity, modifiability, visibility, portability, and reliability**. If a system violates any of the required constraints, it cannot be considered RESTful

[Dig yourself deeper at the dissertation about REST](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
  
## API Design Principles
- **Design with developer empathy**
When it comes to APIs, developers are your users. Perhaps the most important criteria to be mindful of is simplicity: as with any other product, people simply won’t use something if it is hard to use. Hard-to-use APIs create bad public policy outcomes.

- **Design granular, re-useable APIs**
A common misconception is that there is a one-to-one mapping between a service in the paper world and a corresponding API. In reality, this is almost never the case. APIs should be designed at the lowest practical level of granularity, can be re-used and combined in different ways.

- **Design APIs to provide full coverage**
When your API service is being integrated (or ‘mashed up’) into another service then it’s important to ensure you provide APIs that cover the full process life cycle. Failure to do so would seriously impact the user experience, in the consumer service, because users would need to jump between applications to complete a process

- **Use ubiquitous web standards** 
Just use the same standards and methods that are widely used on the web, following examples set by the leading cloud service platforms. This does not mean use the same technology for everything, but it does mean use the most universally accepted technology solution for each business pattern or requirement.

- **Provide useful error messages**
    - Error messages should provide a human-readable error message that is designed to be read and understood by the user
    - Error messages should include a diagnostic message that contains technical details for use by the developers/maintainers of the application that consumes the API

- **Provide appropriate stability and availability**
    - The availability of the API must be no less than that availability of the equivalent agency online service or website.
    - Changes to APIs must always be deployed as fully backwards compatible upgrades. If they are not backwards compatible, the old API version must be maintained alongside the new version for an appropriate period to allow all consumers to transition.
    - Alpha or Beta versions of APIs may have lower availability and stability but the service level should still be clearly specified.

## Taxonomy
### Errors

Are defined as a client passing invalid data to the service and the service correctly rejecting that data. These are generally "4xx" HTTP error codes and are the result of a client passing incorrect or invalid data

Example
``` create magazine:
POST /magazines HTTP/1.1
Host: example.gov.au
Accept: application/json, text/javascript

Status Code: 
    200 if success
    400 if receive bad payload 
```


### Faults

Are defined as the service failing to correctly return in response to a valid client request. These are generally "5xx" HTTP error codes

Example
``` create magazine:
POST /magazines HTTP/1.1
Host: example.gov.au
Accept: application/json, text/javascript

Status Code: 
    200 if success
    500 if an error came from server side
```

### Latency
Latency is defined as how long a particular API call takes to complete, measured as closely to the client as possible. This metric applies to both synchronous and asynchronous APIs in the same way. For long running calls, the latency is measured on the initial request and measures how long that call (not the overall operation) takes to complete

### Time to complete
Services that expose long operations MUST track "Time to Complete" metrics around those operations

## Client Guidance
### Ignore Rule
When the exact shape of the data is not known before the call, if the server returns something the client wasn't expecting, the client must safely ignore it.

Example
``` get all magazines:
GET /magazines HTTP/1.1
Host: example.gov.au
Accept: application/json, text/javascript
Response: {
"user": {
    "comment": "some comment"
    }
}
```
Client should ignore this bad result when client want to get back the magazines, but got user in response. Render empty magazines instead.

### Variable Order Rule
Client must not rely on the order in which data appears in JSON service responses.

Example
```
GET /magazines HTTP/1.1
Host: example.gov.au
Accept: application/json, text/javascript
Response: [
    {
        {
        "comment": "some comment",
        "created_at": "2019/02/01"
        },
        {
        "comment": "some comment",
        "created_at": "2019/02/03"
        },
        {
        "comment": "some comment",
        "created_at": "2019/02/02"
        },
    }
]
```
The Response not always like what Client expected to be. Therefore, in this case, Client should reorder the response by created_at value. 

### Silent Fail Rule
Client requesting OPTIONAL server functionality (such as optional headers) must be resilient to the server ignoring that particular functionality


## Consistency Fundamentals
### URL Structure
A URL identifies a resource. The following URL design patterns are considered REST best pratices:
- URLs should include nouns, not verbs
- Use plural nouns only for consistency (no singular nouns)
- Use HTTP methods (HTTP/1.1) to operate on these resources
- Use HTTP response status codes to represent the outcome of operations on resources.
- Humans SHOULD be able to easily read and construct URLs

Example

> Good URL
```
PUT /magazines HTTP/1.1
Host: example.gov.au
Accept: application/json, text/javascript
StatusCode: 
    200 if success
    400 if bad request from Client
    500 if fault from server
    
```

> Bad URL
```
PUT /magazines/change-order  HTTP/1.1
Host: example.gov.au
Accept: application/json, text/javascript
StatusCode: created
```

> Well-structred URL
```
https://api.contoso.com/v1.0/people/jdoe@contoso.com/inbox
```

> URL that is not friendly
```
https://api.contoso.com/EWS/OData/Users('jdoe@microsoft.com')/Folders('AAMkADdiYzI1MjUzLTk4MjQtNDQ1Yy05YjJkLWNlMzMzYmIzNTY0MwAuAAAAAACzMsPHYH6HQoSwfdpDx-2bAQCXhUk6PC1dS7AERFluCgBfAAABo58UAAA=')
```

### URL Length
- HTTP does not place a predenfined limit on the length of a request-line. A server that receives a request-target longer than any URI it wishes to parse must respond with a 414(URI Too Long) status code
- Services that can generate URLs longer that 2083 characters must make accommodations for the clients they wish to support


### Canonical Identifier
In addition to friendly URLs, resources that can be moved or be renamed SHOULD expose a URL that contains a unique stable identifier. The stable identifier is not required to be a GUID.

Example

```
https://api.contoso.com/v1.0/people/7011042402/inbox
```

### Versioning
- An API URL may contain a version number
- The format of version should follow: v1.2, v2.0(v{majorVersion}.{minorVersion}) and should not be: v-1

Example
```
GET api/v1/path/to/resource HTTP/1.1
Host: www.example.gov.au
Accept: application/json, text/javascript
```

### Supported Methods
Operations MUST use the proper HTTP methods whenever possible, and operation idempotency MUST be respected

Method  | Description                                                                                                                | Is Idempotent
------- | -------------------------------------------------------------------------------------------------------------------------- | -------------
GET     | Return the current value of an object                                                                                      | True
PUT     | Replace an object, or create a named object, when applicable                                                               | True
DELETE  | Delete an object                                                                                                           | True
POST    | Create a new object based on the data provided, or submit a command                                                        | False
HEAD    | Return metadata of an object for a GET response. Resources that support the GET method MAY support the HEAD method as well | True
PATCH   | Apply a partial update to an object                                                                                        | False
OPTIONS | Get information about a request; see below for details.                                                                    | True

## Header
### Request Header
- Authorization: Authorization header for the request as string, as the Oauth2 Standard, it need to have the 'Bearer' prefix to indicate that give the access to the bearer of this token.
- Accept: the requested content type for the response usually is application/json.
- Content-Type: the mine type of request body(PUT/PATCH/POST)

Example
```
GET api/v1/path/to/resource HTTP/1.1
Host: www.example.gov.au
Accept: application/json, text/javascript
Header: Authorization: Bearer JWT_token
```
## Response
- No values in keys – for example, {“125”: “Environment”} is bad, {“id”: “125”, “name”: “Environment”} is good. Note that in the first (bad) example, the key is “125” and the value is “Environment”. This is a problem because the key is supposed to be the name of the value. In the second example (good) the keys are descriptions of their coresponding values.
- No internal-specific names (for example, “node” and “taxonomy term”)
- Metadata should only contain direct properties of the response set, not properties of the members of the response set

Example
> Good Response
```
{
    "user": {
        "name": "some name",
        "age": 18
    }
}
```
> Bad Respose
```
{
        "some name",
        18
}
```
### Error Response
- The error response MUST be a single JSON object have a name/value pair named "error." The value MUST be a JSON object contain name/value pairs with the names "code" and "message" (and it MAY contain name/value pairs with the names "target," "details" and "innererror.")
    - The value for the "code" name/value pair is a language-independent string. Its value is a service-defined error code that SHOULD be human-readable. This code serves as a more specific indicator of the error than the HTTP error code specified in the response
    - The value for the "message" name/value pair MUST be a human-readable representation of the error. It is intended as an aid to developers and is not suitable for exposure to end users
    - The value for the "target" name/value pair is the target of the particular error (e.g., the name of the property in error)
    - The value for the "details" name/value pair MUST be an array of JSON objects that contain name/value pairs for "code" and "message," and MAY contain a name/value pair for "target," as described above. The objects in the "details" array usually represent distinct, related errors that occurred during the request
    - The value for the "innererror" name/value pair MUST be an object. The contents of this object are service-defined. Services wanting to return more specific errors than the root-level code MUST do so by including a name/value pair for "code" and a nested "innererror." Each nested "innererror" object represents a higher level of detail than its parent. When evaluating errors, clients MUST traverse through all of the nested "innererrors" and choose the deepest one that they understand

##### ErrorResponse : Object

Property | Type | Required | Description
-------- | ---- | -------- | -----------
`error` | Error | ✔ | The error object.

##### Error : Object

Property | Type | Required | Description
-------- | ---- | -------- | -----------
`code` | String | ✔ | One of a server-defined set of error codes.
`message` | String | ✔ | A human-readable representation of the error.
`target` | String |  | The target of the error.
`details` | Error[] |  | An array of details about specific errors that led to this reported error.
`innererror` | InnerError |  | An object containing more specific information than the current object about the error.

##### InnerError : Object

Property | Type | Required | Description
-------- | ---- | -------- | -----------
`code` | String |  | A more specific error code than was provided by the containing error.
`innererror` | InnerError |  | An object containing more specific information than the current object about the error.

Example

- Example of inner error
```
{
  "error": {
    "code": "BadArgument",
    "message": "Previous passwords may not be reused",
    "target": "password",
    "innererror": {
      "code": "PasswordError",
      "innererror": {
        "code": "PasswordDoesNotMeetPolicy",
        "minLength": "6",
        "maxLength": "64",
        "characterTypes": ["lowerCase","upperCase","number","symbol"],
        "minDistinctCharacterTypes": "2",
        "innererror": {
          "code": "PasswordReuseNotAllowed"
        }
      }
    }
  }
}
```

- Example of details
```
{
  "error": {
    "code": "BadArgument",
    "message": "Multiple errors in ContactInfo data",
    "target": "ContactInfo",
    "details": [
      {
        "code": "NullValue",
        "target": "PhoneNumber",
        "message": "Phone number must not be null"
      },
      {
        "code": "NullValue",
        "target": "LastName",
        "message": "Last name must not be null"
      },
      {
        "code": "MalformedValue",
        "target": "Address",
        "message": "Address is not valid"
      }
    ]
  }
}
```

## CORS
- Services compliant with the REST API Guidelines MUST support [CORS](https://www.w3.org/TR/access-control/)
- Services should suport an allowed origin of CORS and enforce authorization through valid Oauth tokens.

Example
```
    Access-Control-Allow-Origin: ["https://client-url.com"]
    Access-Control-Allow-Header: ["Authorization", "Content-Type"]
    Access-Control-Max-Age: 86400
    Access-Control-Request-Method: ["GET", "POST", "PUT", "PATCH", "OPTIONS"]
```    
### Avoiding Preflight
Because the CORS protocol can trigger preflight requests that add additional round trips to the server, performance-critical apps might be interested in avoiding them. The spirit behind CORS is to avoid preflight for any simple cross-domain requests that old non-CORS-capable browsers were able to make. All other requests require preflight.

A request is "simple" and avoids preflight if its method is GET, HEAD or POST, and if it doesn't contain any request headers besides Accept, Accept-Language and Content-Language. For POST requests, the Content-Type header is also allowed, but only if its value is "application/x-www-form-urlencoded," "multipart/form-data" or "text/plain." For any other headers or values, a preflight request will happen

## Collections
### Sorting
- The results of a collection query may be sorted based on the property values-determined by the value of the $orderBy query parameter. 
- The expression may include the suffix 'acs' for ascending or 'desc' for descending.

Example
```
 GET https://api.contoso.com/v1.0/people?$orderBy=name desc
```

### Filtering
- the $filter querystring parameter allows clients to filter a collection of resources that are addressed by a request URL.

Example
```
GET https://api.contoso.com/v1.0/products?$filter=price
```

### Pagination
- RESTful APIs that return collections may return partial sets. It expect partial result sets and crrectly page through to retrieve an entire set.
- the most commom way to due with Pagination is using SQL with LIMIT and OFFSET as a part of the SQL SELECT syntax.

Example
```
GET /items?limit=20&offset=20
```

# References
This is composed with <3 and engineering passion by Dwarves Foundation. We would love to thank these teams for references

- [Microsoft API Design Guidelines](https://github.com/microsoft/api-guidelines)
- [AusDTO API Design Docs](https://apiguide.readthedocs.io/)
- [Roy T. Fielding's REST Dissertion](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
