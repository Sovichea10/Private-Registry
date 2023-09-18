# Private-Registry

Private docker registry allows you to share your custom base images within your organization, keeping a consistent, private, and centralized source of truth for the building blocks of your architecture.

## Full setup private registry

Pull registry image from docker hub by using command:
```sh
docker pull registry:2
```
Create directory name:
```sh
mkdir docker-registry
```
Navigate to it
```sh
cd docker-registry
```
Create subdirectory called data, where your registry will store its images:
```sh
mkdir data
```
### Setting up Authentication

Installing the apache2-utils package (use apache2-utils to encrpyt password):
```sh
sudo apt install apache2-utils -y
```
You’ll store the authentication file with credentials under /docker-registry/auth. Create it by running:
```sh
mkdir auth
```
Create the first user, replacing username with the username you want to use. The -B flag orders the use of the bcrypt algorithm, which Docker requires:
```sh
htpasswd -Bc registry.password username
```

Create and open a file called docker-compose.yml by running:
```sh
nano docker-compose.yml
```
docker-compose.yml looks like this:
```sh
version: '3'

services:
  registry:
    image: registry:2
    ports:
    - "5000:5000"
    environment:
      REGISTRY_AUTH: htpasswd
      REGISTRY_AUTH_HTPASSWD_REALM: Registry
      REGISTRY_AUTH_HTPASSWD_PATH: /auth/registry.password
      REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY: /data
    volumes:
      - ./auth:/auth
      - ./data:/data
 ```
## Login to the Registry

Docker login is the default command for login to Docker Hub, which is a public docker registry provided by Docker.
But when we use shelf hosted private Registry we need to provide its endpoint to log in to the private registry.
### The command look like this 
```sh
docker login "ip server registry" 
```
### If you're in local of private registry, running:
```sh
docker login localhost:[port]
```
## Push image to registry
```sh
docker tag "name" "ip server registry"/[IMAGE]:[TAG]
```
```sh
docker push "ip server registry"/[IMAGE]:[TAG]
```
## Pull image from private registry
### Daemon.json
We need to create json file called daemon.json under /etc/docker because from any server access to private registry, it's detected by permission. so it looks like this:
```sh
{
  "insecure-registries": [
    "localhost:5000"
  ],
  "debug": true,
  "experimental": false,
  "registry-mirrors": []
}
```
Note: For Windows, open docker -> go settings -> you'll see Docker Engine and then configure by passing daemon.json script above to it.
### Login
```sh
docker login "ip server registry"
```
### Pull image
```sh
docker pull "ip server registry"/[IMAGE]:[TAG]
```
## Docker Registry API Request (CURL)
### Login
```sh
curl --user ${user}:${password} <registry_url>/v2
```
### List Repo
```sh
curl --user ${user}:${password} <registry_url>/v2/_catalog
```
### Get Tag
```sh
curl --user ${user}:${password} <registry_url>/v2/<repo>/tag/list
```

    
