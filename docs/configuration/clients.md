---
layout: docs-default
---

#Clients

The `Client` class models an OpenID Connect or OAuth2 client - e.g. a native application, a web application or a JS-based application ([link](https://github.com/thinktecture/Thinktecture.IdentityServer.v3/blob/master/source/Core/Models/Client.cs)).

* `Enabled`
    * Specifies if client is enabled. Defaults to `true`.
* `ClientId`
    * Unique ID of the client
* `ClientSecrets`
    * List of Client secrets - only relevant for flows that require a secret
* `ClientName`
    * Client display name (used for logging and consent screen)
* `ClientUri`
    * URI to further information about client (used on consent screen)
* `LogoUri`
    * URI to client logo (used on consent screen)
* `RequireConsent`
    * Specifies whether a consent screen is required. Defaults to `true`.
* `AllowRememberConsent`
    * Specifies whether user can choose to store consent decisions. Defaults to `true`.
* `Flow`
    * Specifies allowed flow for client (either `AuthorizationCode`, `Implicit`, `Hybrid`, `ResourceOwner`, `ClientCredentials` or `Custom`). Defaults to `Implicit`.
* `RedirectUris`
    * Specifies the allowed URIs to return tokens or authorization codes to
* `PostLogoutRedirectUris`
    * Specifies allowed URIs to redirect to after logout
* `ScopeRestrictions`
    * Specifies the scopes that the client is allowed to request. If empty, the client can request all scopes. Defaults to an empty collection.
* `IdentityTokenLifetime`
    * Lifetime to identity token in seconds (defaults to 300 seconds / 5 minutes)
* `AccessTokenLifetime`
    * Lifetime of access token in seconds (defaults to 3600 seconds / 1 hour)
* `AuthorizationCodeLifetime`
    * Lifetime of authorization code in seconds (defaults to 300 seconds / 5 minutes)
* `AbsoluteRefreshTokenLifetime`
    * Maximum lifetime of a refresh token in seconds. Defaults to 2592000 seconds / 30 days
* `SlidingRefreshTokenLifetime`
    * Sliding lifetime of a refresh token in seconds. Defaults to 1296000 seconds / 15 days
* `RefreshTokenUsage`
    * `ReUse`: the refresh token handle will stay the same when refreshing tokens
    * `OneTime`: the refresh token handle will be updated when refreshing tokens
* `RefreshTokenExpiration`
    * `Absolute`: the refresh token will expire on a fixed point in time (specified by the AbsoluteRefreshTokenLifetime)
    * `Sliding`: when refreshing the token, the lifetime of the refresh token will be renewed (by the amount specified in SlidingRefreshTokenLifetime). The lifetime will not exceed 
* `AccessTokenType`
    * Specifies whether the access token is a reference token or a self contained JWT token (defaults to `Jwt`).
* `EnableLocalLogin`
    * Specifies if this client can use local accounts, or external IdPs only. Defaults to `true`.
* `IdentityProviderRestrictions`
    * Specifies which external IdPs can be used with this client (if list is empty all IdPs are allowed). Defaults to empty.
* `CustomGrantTypeRestrictions`
    * Specifies which custom grant types the client can use (if the flow is set to `Custom`). Defaults to empty.
* `IncludeJwtId`
    * Specifies whether JWT access tokens should have an embedded unique ID (via the `jti` claim).
* `Claims`
    * Allows settings claims for the client (will be included in the access token).
* `AlwaysSendClientClaims`
    * If set, the client claims will be send for every flow. If not, only for client credentials flow (default is `false`)
* `PrefixClientClaims`
    * If set, all client claims will be prefixed with `client_` to make sure they don't accidentally collide with user claims. Default is `true`.
* `CustomGrantTypeRestrictions`
    * List of allowed custom grant types when Flow is set to `Custom`. If the list is empty, all custom grant types are allowed. Defaults to empty.

In addition there are a number of settings controlling the behavior of refresh tokens - see [here](advanced/refreshTokens.html)

##Example: Configure a client for implicit flow

```csharp
var client = new Client
{
    ClientName = "JS Client",
    Enabled = true,

    ClientId = "implicitclient",
    Flow = Flows.Implicit,

    RequireConsent = true,
    AllowRememberConsent = true,

    RedirectUris = new List<string>
    {
        "https://myapp/callback.html",
    },

    PostLogoutRedirectUris = new List<string>
    {
        "http://localhost:23453/index.html",
    }
}
```

##Example: Configure a client for resource owner flow

```csharp
var client = new Client
{
    ClientName = "Legacy Client",
    Enabled = true,

    ClientId = "legacy",
    ClientSecrets = new List<ClientSecret>
    {
        new ClientSecret("4C701024-0770-4794-B93D-52B5EB6487A0".Sha256())
    },

    Flow = Flows.ResourceOwner,

    AbsoluteRefreshTokenLifetime = 86400,
    SlidingRefreshTokenLifetime = 43200,
    RefreshTokenUsage = TokenUsage.OneTimeOnly,
    RefreshTokenExpiration = TokenExpiration.Sliding
}
```
