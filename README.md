# orock-workshops 
This repo contains ansible playbooks for building out environments and systems for redhatgov.io workshops.

## Build using Docker/Podman 
To build the workshop with Docker or Podman switch to the container directory and build the image from the dockerfile

### Docker login
```
docker login registry.redhat.io
```

### Docker build
```
docker build -t ansible-workshops .
```
### Podman build
```
podman build -t ansible-workshops .
```

### Docker run
```
docker run -it -v <full path to workshop playbook directory>:/app -v <home directory>/.config/:/home/appuser/.config ansible-workshops
```

### Podman run
```
podman run -it -v <full path to workshop playbook directory>:/app -v <home directory>/.config/:/home/appuser/.config ansible-workshops
```