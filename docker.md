# DOCKER

## HELPFUL COMMANDS
```sh

# view what's happening	
docker ps


# clean up your mess
docker rm $(docker ps -qa)			remove all the stopped containers. follow with:
docker system prune (-v)			prune (-volumes)

# burn it with fire
docker kill gitlab-runner
docker rm gitlab-runner


```

## GENERAL DOCKER STUFF
* did you know that docker is an api service? so when you use the command line (or the GUI), it's just sending calls to the api -- you could just do it all from postman if that made sense for some reason
