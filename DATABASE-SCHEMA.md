# Database-Schema

Kodiak's database schema, still WIP.

```mermaid
---
title: NAMESPACE
---
erDiagram
    NAMESPACE {
        int    id   PK "NOT NULL, AUTOINCREMENT"
        string uuid UK "NOT NULL"
        string name    "NOT NULL"
    }
    NAMESPACE_LOG {
        int       id           PK "NOT NULL, AUTOINCREMENT"
        int       namespace_id FK "NOT NULL"
        int       user_id      FK "NOT NULL"
        int       action_id    FK "NOT NULL"
        string    timestamp       "NOT NULL"
        string    pre
        string    post
        string    note 
    }
    ACTION {
        int       id           PK "NOT NULL, AUTOINCREMENT"
        string    name            "NOT NULL"
    }
    NAMESPACE ||--o{ NAMESPACE_LOG               : has
    ACTION    ||--o{ NAMESPACE_LOG               : has
```

# Links

[ER diagrams with Mermaid](https://mermaid.js.org/syntax/entityRelationshipDiagram.html)
