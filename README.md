# Api module

This module provides a REST API interface for Feather CMS.

## Installation

You can use the Swift Package Manager to integrate this module.

```swift
// add to your dependencies 
.package(url: "https://github.com/FeatherCMS/api-module", from: "1.0.0-beta"),

// add to your target
.product(name: "ApiModule", package: "api-module"),
```

## Api module hooks

### public-api

You can register your own public API endpoints through this hook (everyone can access them publicly). 

```swift
app.hooks.register("public-api", use: (router as! SystemRouter).publicApiRoutesHook)

func publicApiRoutesHook(args: HookArguments) {
    let routes = args["routes"] as! RoutesBuilder

    /// register publicly available API routes here...
}
```

### api

You can register your own private (user authentication required) API endpoints through this hook. 

```swift
app.hooks.register("api", use: (router as! SystemRouter).apiRoutesHook)

func apiRoutesHook(args: HookArguments) {
    let routes = args["routes"] as! RoutesBuilder

    /// register private API routes here...
}
```

### api-auth-middlewares

You can return auth middlewares to provide the user authentication logic for the private API endpoints. 

The [User module](https://github.com/FeatherCMS/user-module) provides a token based authenticator middleware and a guard middleware for the `UserModel` type. 

```swift
app.hooks.register("api-auth-middlewares", use: apiAuthMiddlewaresHook)

func apiAuthMiddlewaresHook(args: HookArguments) -> [Middleware] {
    [UserTokenModel.authenticator(), UserModel.guardMiddleware()]
}
```


