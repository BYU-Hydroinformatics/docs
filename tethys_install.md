
# Deploying multiple tethys installations


- Create a new directory to host this tethys installation
- Clone the repo: `https://github.com/rfun/tethys.git` and checkout branch `release_docker`
- You now have to make a few changes to the files for this project

In the file : `/docker/docker-compose.py` , change the lines marked below

```yml
version: "3.4"
services:
  tethys:
    container_name: tethys-test2 //CHANGE THIS
    image: tethys-test2 //CHANGE THIS TO MATCH THE NAME ABOVE
    build:
      context: ../
      target: app
      args:
        TETHYSBUILD_DB_USERNAME: tethys_admin
        TETHYSBUILD_DB_PASSWORD: tethys123 //MAKE SURE TO UPDATE THIS IF YOU CHANGE THE PASSWORD BELOW
        TETHYSBUILD_DB_HOST: db
        TETHYSBUILD_DB_PORT: 5432
    ports:
      - "8000:80" // Change this to expose a port that isn't being used. So something like "8002:80"
      //Note the second part of the above ports will always remain 80
    environment:
      CONDA_HOME: /opt/conda
      ALLOWED_HOSTS: '"[''127.0.0.1'', ''localhost'']"'
      TETHYS_SUPER_USER: "admin"
      TETHYS_SUPER_USER_PASS: "pass"
    links:
      - db
    depends_on:
      - db
  db:
    image: postgres
    container_name: tethys-test2-db //CHANGE THIS NAME
    restart: always
    environment:
      POSTGRES_PASSWORD: tethys123 //I WOULD RECOMMEND CHANGING THIS
      POSTGRES_USER: tethys_admin 

```

Next, in the file `/tethys_apps/cli/gen_templates/settings` find the entry `STATIC_URL` and replace it as follows (if you were running the app on a subpath of `tethys1`):
```py
LOGIN_URL = "/tethys1/accounts/login"
STATIC_URL = "/tethys1/static/"
```

Next, we need to update our NGINX settings so that once this container spins up, we can route to it properly. Lets start building the container first, and while that is happening we can update NGINX. To start the build process, switch to the docker directory and run the following command (replace `tethys1` with the name of this project, ex: `columbia`): 

```sh
cd ./docker
docker-compose -p tethys1 up --build -d
```

Finally, while that is building, update your nginx settings and add the following to your nginx settings on your host. Not in the containers.  (after doing the appropriate replacements)

```nginx

#Replace tethys1 with the path you are hosting this at
location /tethys1 {
            rewrite ^/tethys1/(.*)$ /$1 break;
            proxy_pass http://127.0.0.1:8000/$uri$is_args$args;  #REPLACE THE PORT IN THIS LINE
            proxy_set_header Host $host;
            proxy_set_header X-Script-Name /tethys1;
            proxy_cookie_path / /tethys1;
        }

```
That should be it. You should be able to go to your host/tethys1 and see the portal come up. To enter the docker container and make changes: 

```sh
docker exec -it <container_name> bash
```

From there most things will work the same way as they do on a local machine. 