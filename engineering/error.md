# Handling Error

We use the following structure for our error message. It helps our system stay consistent, easier to present between components and also better for error tracking.

- To make it easy to build informative error messages.
- To make errors easy to understand for users.
- To make errors helpful as diagnostics for programmers.

## Library

- Go: https://github.com/dwarvesf/go-errx
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
```

- Elixir

``` elixir
```

## Errors across the wire

``` json
{

}
```

- Logging:

```

```

## Matching errors

## Replicating & handling error

A method exists for replicating the state of one environment in another (e.g. copy prod to QA to reproduce an error)


