# Docker Service and Command Usage Guide

This document explains the differences between **system-level Docker** (`dockerd`) and **user-level Docker Desktop**, and also when to use `sudo` with Docker commands.

---

## 1. Starting Docker Services

### System-wide Docker Engine
```
sudo systemctl start docker
```

- Starts the Docker Engine (dockerd) as a system service.

- Runs with root privileges and affects all users.

- Commonly used on servers and Linux distributions where Docker Engine is installed directly.

### Example:

```
sudo systemctl start docker
docker ps
```

## User-level Docker Desktop

```
systemctl --user start docker-desktop
```

- Starts Docker Desktop as a user service.

- Runs inside your user session only.

- Does not require root to run Docker commands.

#### Example:

```
systemctl --user start docker-desktop
docker ps
```

| Command                                 | Scope        | Runs As | Use Case                         |
| --------------------------------------- | ------------ | ------- | -------------------------------- |
| `sudo systemctl start docker`           | System-wide  | Root    | Servers, multi-user environments |
| `systemctl --user start docker-desktop` | User session | User    | Development, Docker Desktop      |


## 2. Running Docker Commands
With `sudo`
```
sudo docker build -t myapp .
```

- Runs the Docker CLI with root privileges.

- Required if your user is not in the docker group.

- Example error without `sudo`:

Got permission denied while trying to connect to the Docker daemon socket

Without `sudo`
```
docker build -t myapp .
```

- Runs Docker as your regular user.

- Works if:

    - Your user is added to the docker group:

    ````
    sudo usermod -aG docker $USER
    ````

    - Or you are using Docker Desktop, which manages permissions automatically.

| Command               | Privileges | Requirement                      | Use Case                   |
| --------------------- | ---------- | -------------------------------- | -------------------------- |
| `sudo docker build .` | Root       | No setup                         | Quick use, but less secure |
| `docker build .`      | User       | `docker` group or Docker Desktop | Safer, preferred           |

### Example Workflow
### Using system Docker (dockerd):

```
# Start Docker Engine (system-wide)
sudo systemctl start docker  

# Build image (needs sudo if not in docker group)
sudo docker build -t node-app .

```
### Using Docker Desktop (user session):
```
# Start Docker Desktop
systemctl --user start docker-desktop  

# Build image (no sudo required)
docker build -t node-app .
```

## Summary

- Use system Docker (dockerd) for servers and production.

- Use Docker Desktop (user session) for local development.

- Add your user to the docker group for convenience and security.

- Prefer running Docker without sudo where possible.
