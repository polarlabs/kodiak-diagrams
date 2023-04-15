# Database-Schema

Kodiak's database schema, still WIP.

Conventions:

* Naming:
  * table names: nouns, singular, all capital letters
  * FK: always use the name of the target table + _ + id (all lowercase letters)
* Ordering of columns:
  * first: Primary Key (PK)
  * second (optional): Unique Key (UK) 
  * following (optional): Foreign Key(s) (FK) (sorted alphabetically)
  * data columns: use sensible order, not necessarily alphabetically

```mermaid
---
title: NAMESPACE, NAMESPACE_LOG, ACTION and ENTITY
---
erDiagram
    NAMESPACE {
        int    id   PK "NOT NULL, AUTOINCREMENT"
        string uuid UK "NOT NULL"
        string name    "NOT NULL"
    }
    NAMESPACE_LOG {
        int    id           PK "NOT NULL, AUTOINCREMENT"
        int    action_id    FK "NOT NULL"
        int    entity_id    FK "NOT NULL | links to entities of Concept User"
        int    namespace_id FK "NOT NULL"
        string timestamp       "NOT NULL"
        string prev
        string post
        string note 
    }
    ACTION {
        int    id           PK "NOT NULL, AUTOINCREMENT"
        string name            "NOT NULL | one of created, changed, archived, deleted, restored"
    }
    ENTITY {
        int    id           PK "NOT NULL, AUTOINCREMENT"
        string uuid         UK "NOT NULL"
        int    concept_id   FK "NOT NULL"
        int    namespace_id FK "NOT NULL"
    }
    NAMESPACE ||--o{ NAMESPACE_LOG               : has
    ACTION    ||--o{ NAMESPACE_LOG               : describes
    ENTITY    ||--o{ NAMESPACE_LOG               : "carries out"
```

# Links

[ER diagrams with Mermaid](https://mermaid.js.org/syntax/entityRelationshipDiagram.html)
