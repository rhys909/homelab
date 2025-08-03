# Homelab Project

My attempt to become more organised with my horribly unorganised homelab that is a mix of containers, VMs and Proxmox. 

## Next steps


## Diagrams
Planned setup
graph TD
    Internet((Internet))
    Router[Router]
    MeshAP[Wireless Mesh Node / AP]
    NinasPC["Nina's PC (Wireless)"]
    WiredPC["Desktop PC (Wired to Mesh Node)"]
    Switch[8-Port Switch]
    ThinClient1["Thin Client 1 (K8s Node)"]
    ThinClient2["Thin Client 2 (K8s Node)"]
    ThinClient3["Thin Client 3 (K8s Node)"]
    PiHole1["Raspberry Pi 1 (PiHole)"]
    PiHole2["Raspberry Pi 2 (PiHole)"]

    Internet --> Router
    Router --> MeshAP
    Router --> Switch

    MeshAP --> NinasPC
    MeshAP --> WiredPC

    Switch --> ThinClient1
    Switch --> ThinClient2
    Switch --> ThinClient3
    Switch --> PiHole1
    Switch --> PiHole2


Planned interactions
```mermaid
graph TD
    subgraph Kubernetes Cluster
        plex[Plex Pod]
        grafana[Grafana Pod]
        svcPlex[Plex Service]
        svcGrafana[Grafana Service]

        plex --> svcPlex
        grafana --> svcGrafana

        plex -- "/config\n/data/tv\n/data/movies" --> cifsPlex[(CIFS Plex Storage)]
        grafana -- "/var/lib/grafana" --> cifsGrafana[(CIFS Grafana Storage)]
    end

    userPC[User Browser] --> svcGrafana
    userPC --> svcPlex

    cifsPlex -.-> smbServer[(CIFS/SMB Server)]
    cifsGrafana -.-> smbServer
