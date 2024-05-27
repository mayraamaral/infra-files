## 1. Creating folder
```shell
mkdir portainer
```
## 2. Installing
```shell
curl -L https://downloads.portainer.io/ce2-19/portainer-agent-stack.yml -o portainer-agent-stack.yml
```
```shell
docker stack deploy -c portainer-agent-stack.yml portainer
```
## 3. After creating network for NPM (Nginx Proxy Manager), edit the ```yml``` file and insert the network created
## 4. Restart the Portainer stack
```shell
docker stack deploy -c portainer-agent-stack.yml portainer
```
## Credits
[Portainer Docs](https://docs.portainer.io/start/install-ce/server/swarm/linux)  
[Tutorial](https://www.youtube.com/watch?v=T_DMW3relvQ)