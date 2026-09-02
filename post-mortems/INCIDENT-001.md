Incident #001 — Docker Storage Corruption / RWLayer Failure

Impact:

    Containers (grafana, cadvisor, prometheus, etc.) failed to start.

    Monitoring stack and game server nodes temporarily offline due to a daemon metadata inconsistency.

Detection:

    Error returned by Docker daemon: RWLayer of container ... is unexpectedly nil.

    Manual container start failure via docker compose start.

Investigation:

    Checked container metadata and daemon logs:
    Bash

    sudo docker inspect <container_id>
    sudo journalctl -u docker.service -b 0

    Verified underlying storage driver (overlay2) integrity on Arch Linux.

Root Cause:

    Corruption in Docker's internal metadata reference to the container's Read-Write layer (often triggered by an abrupt shutdown, storage desync, or unclean daemon state).

Resolution:

    Removed the corrupted container references cleanly without touching persistent volumes:
    Bash

    sudo docker rm -f <container_id>

    Re-instantiated the declarative stack using Docker Compose:
    Bash

    sudo docker compose up -d

Verification:

    Containers successfully recreated with fresh RW layers.

    Services responding, metrics flowing into Prometheus/Grafana, and players able to reconnect to the Minecraft nodes (bds-2, bds-3).
