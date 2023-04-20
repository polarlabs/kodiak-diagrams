# User authentication of Kodiak's auth server

```mermaid
---
title: User Authentication (existing Kodiak User)
---
sequenceDiagram
    actor       U as User
    participant S as Auth Server
    participant D as Database
    U ->> S: LoginRequest /login<br/>POST(username, api_token)
    critical connects to database
        S ->>   D: connect
        D -->>  S: Result<connected, Error>
        alt connected
           S ->>   D: match username + API token
           D -->>  S: Result<matched, Error>
        else Error
           S -->>  S: log error
           S -->>  U: HTTP 500 INTERNAL SERVER ERROR(ServerError)
        end
    end
    alt matched
        S -->>  U: HTTP 200 OK(LoginResponse<Jwt>)
    else Error
        S -->>  U: HTTP 403 FORBIDDEN(LoginError)
    end
```

Thoughts:

- Kodiak uses HTTP 403 to indicate that a login request failed as opposed to 401 because `LoginRequest` is a POST request 
  to the server without any HTTP headers involved, neither in request nor in response.

# Links

[RFC9110 Client Error 4xx - 401 UNAUTHORIZED](https://www.rfc-editor.org/rfc/rfc9110#name-401-unauthorized)

[RFC9110 Client Error 4xx - 403 FORBIDDEN](https://www.rfc-editor.org/rfc/rfc9110#name-403-forbidden)

[RFC9110 Server Error 5xx - 500 INTERNAL SERVER ERROR](https://www.rfc-editor.org/rfc/rfc9110#name-500-internal-server-error)
