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
  end
  subgraph user-cli[ ]
    direction RL
    cli-user((user)) --> cli-client[[cli-client]]
  end
  subgraph server[ ]
    direction TB
    file-server.api[[file-server]]
    web-server.api[[web-server]] --o web-server.db-lib[db-lib]
    app-server.api[[app-server]] --o app-server.db-lib[db-lib]
  end
  web-client --> web-server.api
  cli-client --> web-server.api
  web-server.api --> file-server.api
  web-server.api --> app-server.api
  file-server.api --> filesystem[(filesystem)]
  web-server.api --> database[(database)]
  app-server.api --> database[(database)]
```

Target picture:

```mermaid
---
title: Kodiak's system architecture
---
flowchart TB
  direction LR
  subgraph user-web
    user((User)) --> web-client[[web-client]]
  end
  direction RL
  subgraph user-cli
    user((User)) --> cli-client[[cli-client]]
  end
  web-client --> web-server
  cli-client --> web-server
  direction LR
  subgraph server
    file-server <--> web-server
    app-server[[app-server, db]] <--> web-server[[web-server, db]]
  end
  file-server --> file-system[(filesystem)]
  web-server --> database[(database)]
  app-server --> database[(database)]
```
