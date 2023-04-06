# System architecture

Kodiak's system architecture, still WIP.

```mermaid
---
title: Kodiak's system architecture
---
flowchart TB
  direction TB
  subgraph user-web[ ]
    direction LR
    web-user((user)) --> web-client[[web-client]]
    web-client --o web-client.lib.common[lib.common]
    web-client --o web-client.lib.taxonomy[lib.taxonomy]
    web-client --o web-client.lib.validator[lib.validator]
  end
  subgraph user-cli[ ]
    direction RL
    cli-user((user)) --> cli-client[[cli-client]]
    cli-client --o cli-client.lib.common[lib.common]
    cli-client --o cli-client.lib.validator[lib.validator]
  end
  subgraph file-server[ ]
    direction TB
    file-server.api[[file-server]]
  end
  subgraph web-server[ ]
    direction TB
    web-server.api[[web-server]] --o web-server.lib.common[lib.common]
    web-server.api[[web-server]] --o web-server.lib.db[lib.db]
    web-server.api[[web-server]] --o web-server.lib.taxonomy[lib.taxonomy]
    web-server.api[[web-server]] --o web-server.lib.taxonomy[lib.validator]
  end
  subgraph app-server[ ]
    direction TB
    app-server.api[[app-server]] --o app-server.lib.common[lib.common]
    app-server.api[[app-server]] --o app-server.lib.db[lib.db]
  end
  web-client --> web-server.api
  cli-client --> web-server.api
  web-server.api --> file-server.api
  web-server.api --> app-server.api
  file-server --> filesystem[(filesystem)]
  web-server --> database[(database)]
  app-server --> database[(database)]
  filesystem ~~~ database
```
