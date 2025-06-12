# DeepShield-Registry Setup Guide

## 1. Configure DeepShield Docker Registry
### Tag and Push Images
```bash
# Tag the image with registry address
docker tag <image-name>:<tag> 47.97.67.233:5000/deepshield/<image-name>:<tag>

# Login to DeepShield registry
docker login 47.97.67.233:5000

# Push to DeepShield registry
docker push 47.97.67.233:5000/deepshield/<image-name>:<tag>
```
> Image naming format: `[REGISTRY_HOST:PORT]/REPOSITORY[:TAG]`

## 2. Client Configuration
### Edit Docker Daemon Configuration
Modify `/etc/docker/daemon.json`:
```json
{
  "insecure-registries": ["47.97.67.233:5000"]
}
```
> Required for HTTP connections to DeepShield registry (https://medium.com/@dataq/membuat-docker-private-registry-6efe534df4d5) (https://gist.github.com/paulgwebster-oe/b3eab23c5c369d659bf0f66f124f2715)

### Restart Docker Service
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 3. Pull Images from DeepShield Registry
```bash
docker pull 47.97.67.233:5000/deepshield/<image-name>:<tag>
```
> Verify with: `docker images | grep <image-name>`

## Troubleshooting
- **Error**: `http: server gave HTTP response to HTTPS client`  
  Solution: Double-check `insecure-registries` in `daemon.json` and restart docker
  
- **Image not found after push**:  
  Verify port mapping in registry container with:  
  `docker ps --filter "name=registry"`

> For secure production setups, configure TLS certificates instead of `insecure-registries` (https://docs.docker.com/engine/security/trust/)
