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
    alt succeeds
        S ->> U: JWT
    else fails
        S ->> U: HTTP ERROR
    end
```
