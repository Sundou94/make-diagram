# MSA Container Diagram

```mermaid
flowchart LR
    %% Zones
    subgraph Internet
        ext[External Users]
    end

    subgraph AWS[Cloud (AWS)]
        lb[Internet-facing Load Balancer]
        cicd[CI/CD Pipeline]
    end

    subgraph DMZ[DMZ]
        fe[FRONTEND]
        gw[GATEWAY]
    end

    subgraph Intranet[Company Intranet]
        subgraph Platform[Platform Services]
            disc[DISCOVERY]
            config[CONFIG]
            auth[AUTH]
            file[FILE]
            mail[MAIL]
            common[COMMON]
        end

        subgraph Business[Business Services]
            core[CORE]
            bpms[BPMS]
        end
    end

    %% Traffic flow
    ext --> lb
    lb --> fe
    lb --> gw
    fe --> gw

    gw --> disc
    gw --> auth
    gw --> core
    gw --> bpms

    disc --> core
    disc --> bpms

    config --> common
    config --> core
    config --> bpms

    auth --> core
    auth --> bpms

    file --> core
    mail --> common

    common --> core
    common --> bpms

    cicd -. deployments .-> fe
    cicd -. deployments .-> gw
    cicd -. deployments .-> disc
    cicd -. deployments .-> config
    cicd -. deployments .-> auth
    cicd -. deployments .-> file
    cicd -. deployments .-> mail
    cicd -. deployments .-> common
    cicd -. deployments .-> core
    cicd -. deployments .-> bpms
```
