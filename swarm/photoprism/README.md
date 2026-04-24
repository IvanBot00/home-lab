# PhotoPrism Swarm Stack

## Prerequisites

- Docker Swarm with a node labelled `nas=synology-nas`
- `traefik-proxy` overlay network exists

## Setup

### 1. Create options.yml

`options.yml` is gitignored to keep credentials out of the repo. Copy the example and fill in your values:

```bash
cp options.yml.example options.yml
```

Then edit `options.yml` and set your admin password:

```yaml
AdminPassword: "yourpassword"
```

### 2. Create the swarm config

```bash
docker config create photoprism_options options.yml
```

To update the config later, you must remove and recreate it:

```bash
docker config rm photoprism_options
docker config create photoprism_options options.yml
docker service update --force photoprism_photoprism
```

### 3. Deploy the stack

```bash
docker stack deploy -c docker-compose.yml photoprism
```

## Volumes

| Host path | Container path | Purpose |
|---|---|---|
| `/appdata/photoprism/storage` | `/photoprism/storage` | Database, cache, sidecar files |
| `/mnt/source` | `/photoprism/originals` | Photo library (read-only) |
