# DevOpsNotes

## Docker Compose Practical Notes

This README summarizes the Docker Compose commands and behaviors shown in the classroom session using PowerShell on Windows.

### Environment
- Windows PowerShell
- Docker Compose version: `v5.0.2`
- Docker Desktop uses WSL2 backend
- Sample compose project name: `asus`

---

## Key Commands and Notes

### `docker-compose --version`
- Checks the installed Docker Compose version.
- Confirms Compose CLI is available and usable.

### `docker-compose config`
- Validates and prints the final Compose configuration.
- Warning shown:
  - `version` attribute is obsolete and ignored in Compose v5
- Output shows:
  - Service `web` using `nginx` image
  - Exposed port `8080` mapped to container port `80`
  - Default network `asus_default`

### `docker-compose up`
- Starts services defined in `docker-compose.yml`.
- Without `-d`, attaches to container logs in the terminal.
- Output shows Nginx startup logs and container entrypoint scripts.
- Typical lifecycle:
  - Create network
  - Create container
  - Start service

### `docker-compose up -d`
- Runs containers in detached mode.
- Returns control to terminal immediately.
- Useful for running services in the background.

### `docker-compose down`
- Stops and removes containers, networks, and default resources created by `up`.
- Repeated `down` after resources are already removed is harmless.
- Useful for cleanup after a test run.

### `docker-compose start`
- Starts existing stopped containers without recreating them.
- In this session, `docker-compose start` failed because no container existed to start.
- This indicates `down` had already removed the service container.

### `docker-compose --build`
- Not supported by Docker Compose v5 CLI as shown by the error:
  - `unknown flag: --build`
- In Compose v5, build is handled via `docker compose up --build` (note the space and new command syntax).

---

## Practical Observations

- Docker Compose v5 treats the top-level `version:` field as obsolete.
  - Remove `version` from `docker-compose.yml` to avoid warnings.
- `docker-compose config` is useful for confirming the final interpreted Compose settings.
- `docker-compose up` without `-d` is best for observing startup logs and debugging.
- `docker-compose down` is the proper cleanup command after tests.

---

## Example `docker-compose.yml`

```yaml
services:
  web:
    image: nginx
    ports:
      - "8080:80"
```

> Note: This simplified example uses Compose v5 syntax without `version:`.

---

## Recommendations

1. Remove the obsolete `version:` field from Compose files.
2. Use `docker-compose up -d` for detached service runs.
3. Use `docker-compose down` to remove created containers and networks.
4. Use `docker-compose config` to validate Compose file syntax and resulting configuration.
5. If building images, use `docker compose up --build` with the newer Compose command syntax.
