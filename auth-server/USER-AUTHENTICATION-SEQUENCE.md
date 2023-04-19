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
        alt connected
           S ->>  D: match username + API token
        else failed
           S ->>  S: log error
        end
    end
    alt matched
        S ->> U: JWT
    else failed
        S ->> U: HTTP ERROR
    end
```
