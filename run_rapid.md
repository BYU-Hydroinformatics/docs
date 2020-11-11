# Run Rapid Docker

-   Navigate to the directory you have the files in: which will contain a folder `rapid-io/input` and `rapid-io/output`
-   Rename namelist file to `rapid_namelist` (if not already) and also update the paths in there to match the file location
-   Run Docker Container:

```sh
docker run --rm --name rapid -it -v $(pwd)/rapid-io/input/sam_atrato:/home/rapid/run -v $(pwd)/rapid-io/output/sam_atrato:/home/rapid/run/output chdavid/rapid
```

-   Symlink rapid executable to our run location:

```sh
ln -s /home/rapid/src/rapid /home/rapid/run/rapid
```

-   Run Rapid! : /home/rapid/src/rapid

```sh
/home/rapid/src/rapid
```
