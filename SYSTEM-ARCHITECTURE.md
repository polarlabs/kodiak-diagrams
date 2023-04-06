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
  subgraph file-server[ ]
    direction TB
    file-server.pro[[file-server]]
  end
  subgraph web-server[ ]
    direction TB
    web-server.pro[[web-server]] --o web-server.db-lib[db-lib]
  end
  subgraph app-server[ ]
    direction TB
    app-server.pro[[app-server]] --o app-server.db-lib[db-lib]
  end
  file-server ~~~ web-server
  web-client --> web-server.pro
  cli-client --> web-server.pro
  web-server.pro --> file-server.pro
  web-server.pro --> app-server.pro
  file-server --> filesystem[(filesystem)]
  web-server --> database[(database)]
  app-server --> database[(database)]
  filesystem ~~~ database
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
