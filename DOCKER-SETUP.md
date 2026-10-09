# GhostTrack Docker setup

GhostTrack is an interactive terminal application, not a web server.
Docker Compose runs it as a non-root user with a read-only filesystem and
no published ports. It needs outgoing HTTPS access for public lookup APIs.

## Build and run

```sh
cd /home/ubuntu/greg/Projects/GhostTrack
docker compose build
docker compose run --rm ghosttrack
```

Select a menu item (1-4), or enter 0 to quit.
For one-off scripted lookups:

```sh
docker compose run --rm ghosttrack ip 8.8.8.8 --json
docker compose run --rm ghosttrack phone +60123456789 --region MY
docker compose run --rm ghosttrack username octocat
```

## Docker without Compose

```sh
docker build -t ghosttrack:local .
docker run --rm -it --read-only --tmpfs /tmp:rw,noexec,nosuid,size=16m \
  --cap-drop=ALL --security-opt=no-new-privileges --pids-limit=64 \
  --memory=256m ghosttrack:local
```

## Verification note

The connected Linux_VM coding shell may be running in an existing container
without a Docker binary or mounted daemon socket. In that case Docker builds
and runs must be performed on the **Docker-enabled Ubuntu host**.
Run `docker compose config` and `docker compose build` there.

## Scope and accuracy

Phone data is metadata from public numbering plans, not a live device
location. IP geolocation is approximate, not street-level tracking.
GitHub and GitLab username status uses public APIs; links to other
platforms are unverified and should not be treated as found accounts.
