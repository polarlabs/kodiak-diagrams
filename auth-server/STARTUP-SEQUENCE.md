# Startup sequence of Kodiak's auth server

WIP

```mermaid
---
title: Kodiak auth server 0.1.0
---
stateDiagram-v2
    [*] --> Init Logger

    state First {
        [*] --> Second

        state Second {
            [*] --> second
            second --> Third

            state Third {
                [*] --> third
                third --> [*]
            }
        }
    }
    
    First --> [*]
```
