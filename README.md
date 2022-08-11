# Private-Registry
Private docker registry allows you to share your custom base images within your organization, keeping a consistent, private, and centralized source of truth for the building blocks of your architecture.

# Full setup registry
1. Pull 'registry' image from docker hub
2. Go to https://www.digitalocean.com/community/tutorials/how-to-set-up-a-private-docker-registry-on-ubuntu-20-04 

##Login to the Registry

Docker login is the default command for login to Docker Hub, which is a public docker registry provided by Docker.
But when we use shelf hosted private Registry we need to provid its endpoint to log in the private registry.
### The command look like this
```sh
docker login "ip server registry" 
```
 
2. docker tag "name" "ip server registry"/[IMAGE]:[TAG] : tag image push to private registry
3. docker push "ip server registry"/[IMAGE]:[TAG] : push to private registry
4. From another server, pull image from private registry :
   4.1. docker login 
