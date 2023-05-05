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
title: NAMESPACE, CONCEPT, FEATURE, DATATYPE and ENTITY
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
    ENTITY {
        int    id           PK "NOT NULL, AUTOINCREMENT"
        string uuid         UK "NOT NULL"
        int    namespace_id FK "NOT NULL"
        int    concept_id   FK "NOT NULL"
    }
    ENTITY_FEATURE {
        string label
        int    namespace_id FK     "NOT NULL"
        int    entity_id    PK, FK "NOT NULL"
        int    feature_id   PK, FK "NOT NULL"
    }
    DATA_IDENTITY {
        int    id           PK "NOT NULL, AUTOINCREMENT"
        string i               "NOT NULL"
        int    namespace_id FK "NOT NULL | UK(namespace_id, entity_id, feature_id)" 
        int    entity_id    FK "NOT NULL | FK(entity_id, feature_id) -> ENTITY_FEATURE"
        int    feature_id   FK "NOT NULL"
    }
    DATA_CONCEPT_REFERENCE {
        int    id           PK "NOT NULL, AUTOINCREMENT"
        string reference       "NOT NULL"
        int    namespace_id FK "NOT NULL | UK(namespace_id, entity_id, feature_id)"
        int    entity_id    FK "NOT NULL | FK(entity_id, feature_id) -> ENTITY_FEATURE"
        int    feature_id   FK "NOT NULL"
    }
    DATA_CRON_EXPRESSION {
        int    id              PK "NOT NULL, AUTOINCREMENT"
        string cron_expression    "NOT NULL"
        int    namespace_id    FK "NOT NULL | UK(namespace_id, entity_id, feature_id)"
        int    entity_id       FK "NOT NULL | FK(entity_id, feature_id) -> ENTITY_FEATURE"
        int    feature_id      FK "NOT NULL"
    }
    DATA_TASK {
        int    id              PK "NOT NULL, AUTOINCREMENT"
        string n                  "NOT NULL"
        int    namespace_id    FK "NOT NULL"
        int    entity_id       FK "NOT NULL | FK(entity_id, feature_id) -> ENTITY_FEATURE"
        int    feature_id      FK "NOT NULL"
    }
    NAMESPACE      ||--o{ CONCEPT                : contains
    NAMESPACE      ||--o{ FEATURE                : contains
    NAMESPACE      ||--o{ CONCEPT_FEATURE        : contains
    NAMESPACE      ||--o{ ENTITY                 : contains
    CONCEPT        ||..o{ CONCEPT_FEATURE        : has
    FEATURE        ||..o{ CONCEPT_FEATURE        : has
    FEATURE        }o--|| DATATYPE               : has
    CONCEPT        ||--o{ ENTITY                 : has
    ENTITY         ||..o{ ENTITY_FEATURE         : has
    FEATURE        ||..o{ ENTITY_FEATURE         : has
    ENTITY_FEATURE ||--o{ DATA_IDENTITY          : provides
    ENTITY_FEATURE ||--o{ DATA_CONCEPT_REFERENCE : provides
    ENTITY_FEATURE ||--o{ DATA_CRON_EXPRESSION   : provides
    ENTITY_FEATURE ||--o{ DATA_TASK              : provides
```

```mermaid
---
title: NAMESPACE, NAMESPACE_LOG, ENTITY and ACTION
---
erDiagram
    NAMESPACE {
        int    id   PK "NOT NULL, AUTOINCREMENT"
        string uuid UK "NOT NULL"
        string n       "NOT NULL"
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
    ENTITY {
        int    id           PK "NOT NULL, AUTOINCREMENT"
        string uuid         UK "NOT NULL"
        int    namespace_id FK "NOT NULL"
        int    concept_id   FK "NOT NULL"
    }
    ACTION {
        int    id PK "NOT NULL, AUTOINCREMENT"
        string n  UK "NOT NULL | one of created, changed, archived, deleted, restored"
    }
    NAMESPACE     ||--o{ NAMESPACE_LOG : has
    NAMESPACE_LOG }o--|| ENTITY        : "carried out by"
    NAMESPACE_LOG }o--|| ACTION        : "triggered by"
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
