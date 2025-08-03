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
