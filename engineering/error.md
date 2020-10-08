# Handling Error

We use the following structure for our error message. It helps our system stay consistent, easier to present between components and also better for error tracking.

- To make it easy to build informative error messages.
- To make errors easy to understand for users.
- To make errors helpful as diagnostics for programmers.

## Library

- Go: https://github.com/dwarvesf/gerr
- Elixir: https://github.com/dwarvesf/error.ex
- Swift: https://github.com/dwarvesf/error.swift

## Remote Error Tracking

We use [Sentry](sentry.io) for both backend and frontend apps. Starting a new project, we usually set up and hook the error message to our project management system.

<img>

## Error Structure

```
- type: list of common or pre-defined error type
- message: error message
- errors: actual error value in details
- trace_id: 
```

## Constructing an Error

- Go

``` go
// init error code constant
const (
    ErrAttrInvalidCode = itoa + gerr.BusinessCodeCustomStart
    ErrAttrNotFoundCode
)

// init error variable
var (
    ErrAttrInvalid = gerr.New(ErrAttrInvalidCode, "message error for attr")
)

// error usage
func handle(req Request) (*Response, error) {
    if res, err := processRequest(req); err != nil {
        return nil, ErrAttrInvalid.Err()
    }

    return res, nil
}
```

- Elixir

``` elixir
```

## Errors across the wire

``` json
{
    "message": "client error message",
    "trace_id": "vldlCdkR3vAoupWkiENI",
    "code": 4001,
    "errors": {
        "attribute_1": "error message for attribute 1",
        "items": {
            "0": {
                "attr1": "error message for items[0].attr1"
            },
            "1": {
                "attr2": "error message for items[1].attr2"
            }
        }
    }
}
```

- Logging:

```js
{ time="2020-10-08T12:10:57+07:00", level="error", env="dev", ip="115.73.208.232", method="POST", path="/orders", service="example-be", statusCode="500", traceId="vldlCdkR3vAoupWkiENI", userAgent="insomnia/2020.4.1" }
Internal Server Error
 - at pkg/handler/order.go:34 (Handler.CreateOrder)
Trace: call service is failed
         - at pkg/handler/order.go:51 (Handler.doCreateOrder)
        Caused by: error message for items[0].attr1
         - at pkg/handler/order.go:60 (Handler.doValidateCreateOrder)
        Caused by: error message for items[1].attr2
         - at pkg/handler/order.go:70 (Handler.doValidateCreateOrder)
```

## Matching errors

## Replicating & handling error

A method exists for replicating the state of one environment in another (e.g. copy prod to QA to reproduce an error)


