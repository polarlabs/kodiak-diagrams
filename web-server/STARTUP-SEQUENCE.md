# Startup sequence of Kodiak web server

WIP

```mermaid
---
title: Kodiak web server 0.1.0
---
stateDiagram-v2
    [*] --> First

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
