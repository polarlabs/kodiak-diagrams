# User authentication of Kodiak's auth server

```mermaid
---
title: User Authentication (existing Kodiak User)
---
sequenceDiagram
    actor       U as User
    participant S as Auth Server
    participant D as Database
    U ->> S: POST Username and API token
    critical connects to database
        S ->>  D: connect
        D ->>  S: Result<connected, Error>
        alt connected
           S ->>  D: match username + API token
           D ->>  S: Result<matched, Error>
        else Error
           S ->>  S: log error
        end
    end
    alt matched
        S ->> U: Result<JWT>
    else Error
        S ->> U: Result<HTTP ERROR>
    end
```
