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
        string n       "NOT NULL"
    }
    CONCEPT {
        int    id           PK "NOT NULL, AUTOINCREMENT"
        string uuid         UK "NOT NULL"
        string n               "NOT NULL"
        int    namespace_id FK "NOT NULL"
    }
    FEATURE {
        int    id           PK "NOT NULL, AUTOINCREMENT"
        string uuid         UK "NOT NULL"
        string n               "NOT NULL"
        int    namespace_id FK "NOT NULL"
        int    datatype_id  FK "NOT NULL"
    }
    CONCEPT_FEATURE {
        string label
        int    namespace_id FK     "NOT NULL"
        int    concept_id   PK, FK "NOT NULL"
        int    feature_id   PK, FK "NOT NULL"
    }
    DATATYPE {
        int    id PK "NOT NULL, AUTOINCREMENT"
        string n  UK "NOT NULL | one of Identity, Concept Reference, Cron Expression"
    }
    NAMESPACE_LOG {
        int      id           PK "NOT NULL, AUTOINCREMENT"
        int      action_id    FK "NOT NULL"
        int      entity_id    FK "NOT NULL | links to entities of Concept User"
        int      namespace_id FK "NOT NULL"
        datetime tstamp        "NOT NULL"
        string   prev
        string   post
        string   note 
    }
    ACTION {
        int    id PK "NOT NULL, AUTOINCREMENT"
        string n  UK "NOT NULL | one of created, changed, archived, deleted, restored"
    }
    ENTITY {
        int    id           PK "NOT NULL, AUTOINCREMENT"
        string uuid         UK "NOT NULL"
        int    concept_id   FK "NOT NULL"
        int    namespace_id FK "NOT NULL"
    }
    NAMESPACE ||--o{ NAMESPACE_LOG : has
    NAMESPACE ||--o{ CONCEPT       : contains
    NAMESPACE ||--o{ FEATURE       : contains
    NAMESPACE ||--o{ ENTITY        : contains
    CONCEPT   }o..o{ FEATURE       : has
    CONCEPT   ||--o{ ENTITY        : has
    FEATURE   }o--|| DATATYPE      : has
    ACTION    ||--o{ NAMESPACE_LOG : describes
    ENTITY    ||--o{ NAMESPACE_LOG : "carries out"
```

```mermaid
---
title: MIGRATION_STATUS and MIGRATION_HISTORY
---
erDiagram
    MIGRATION_STATUS {
        int      id        PK "NOT NULL, AUTOINCREMENT"
        datetime tstamp       "NOT NULL"
        string   file_name    "NOT NULL"
        string   v            "NOT NULL"
        string   direction    "NOT NULL"
        string   progress     "NOT NULL"
        string   state        "NOT NULL"
    }
    MIGRATION_HISTORY {
        int      id        PK "NOT NULL, AUTOINCREMENT"
        datetime tstamp       "NOT NULL"
        string   script       "NOT NULL"
        string   event        "NOT NULL"
    }
```

# Links

[ER diagrams with Mermaid](https://mermaid.js.org/syntax/entityRelationshipDiagram.html)
