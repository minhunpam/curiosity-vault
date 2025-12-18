- Check if there are any docker images
- Check if there are any docker containers
```bash
sudo docker images
sudo docker ps
```

## What is the role of `docker-compose.yml`
- Is a configuration file that tells Docker how to run multipler containers together as a single applicaton
- It defines:
	- which containers to run
	- which Docker images they use
	- which commands to run
	- which ports to expose
	- which environment variables to pass
	- which volumes to mount
	- which networks to join
