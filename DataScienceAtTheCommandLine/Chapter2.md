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

To add the /data directory which contains the data need it to reproduce the book's exercise, I **exit** the container, move to directory ```/CompBiologist/data``` and ran:
```
docker run --rm -it -v "$(pwd)":/data datasciencetoolbox/dsatcl2e
```

Now, I am ready to start learning!!!

## Combining Command-Line Tools

Each tool has three standard communication streams: 
1. Standard input (**stdin**)
2. Standard output (**stdout**)
3. Standard error (**stderr**)

### Pipe
```
curl -s "https://www.gutenberg.org/files/11/11-0.txt" | grep "CHAPTER"
```




