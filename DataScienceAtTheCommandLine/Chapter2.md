# Getting started
First, I installed the Docker image using:
```
docker pull datasciencetoolbox/dsatcl2e
```

Then, I ran the Docker image:
```
docker run --rm -it datasciencetoolbox/dsatcl2e
```

To confirm that I am inside the docker container, I ran:
```
cowsay "Let's moove\!"
 ______________
< Let's moove! >
 --------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

To add the /data directory which contains the data need it to reproduce the book's exercise, I **exit** the container, move to directory ```/Users/danieldavila/OneDrive - University of Calgary/CompBiologist/data``` and ran:
```
docker run --rm -it -v "$(pwd)":/data datasciencetoolbox/dsatcl2e
```

Now, I am ready to start learning!!!





