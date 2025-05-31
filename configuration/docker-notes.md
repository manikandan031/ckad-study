## Docker
```
# build image
docker build . -t <name>:<tag>

# run image and bind a host port to container port
docker run --name <container-name> -d -p <host-port>:<container-port> <image>:<tag>

# identify base operating system of the image
docker run -it <image>:<tag> cat /etc/os-release 

```
### Docker file CMD and Entrypoint
```
FROM ubuntu
CMD ["sleep", "5"]

# build image
docker build . -t ubuntu-sleeper
# below run sleeps for 5 seconds and exits
docker run ubuntu-sleeper
# below run sleeps for 10 seconds and exits
docker run ubuntu-sleeper sleep 10 

FROM ubuntu
ENTRYPOINT ["sleep"]

# With Entrypoint the arguments are appended to the `sleep` command not overwritten
docker run ubuntu-sleeper 10

# combining Entrypoint and CMD to have default behaviour which can be overridden
FROM ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"]

# sleeps 5 seconds
docker run ubuntu-sleeper

# sleeps 10 seconds
docker run ubuntu-sleeper 10
```
