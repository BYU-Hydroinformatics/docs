
# Cloning existing tethys portal


### Copy existing Files

- Create a new directory for your installation (Ex: test_copy)
- `cd` into that directory
- Assuming this directory is created at the same level as the existing `geoglows` directory, copy the folders `tethys_files` and `db_files_snapshot` and the `docker-compose` file
    ```sh
    cp -R ../geoglows/db_files_snapshot ./
    cp -R ../geoglows/tethys_files/ ./
    cp ../geoglows/docker-compose.yml ./
    ```

### Update Docker Compose and Other Tethys Setting files


```yml
version: "3.4"
services:
  tethys:
    container_name: test_copy #Change the name here
    image: geoglows:latest
    volumes:
      - ../geoglows/thredds:/usr/lib/thredds #Change this line if needed to represent the correct thredds path
      - ./tethys_files:/usr/lib/tethys
    ports:
      - "8004:80" #CHANGE the port on which this portal is being hosted
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
    image: geoglows_db:latest
    container_name: test_copy_db #Change the name here
    restart: always
    volumes:
      - ./db_files_snapshot:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: pass123
      POSTGRES_USER: tethys_admin
```

Also Update the settings.py in your current tethys_files directory : 
```py
LOGIN_URL = "/test_copy/accounts/login"
STATIC_URL = '/test_copy/static/'
```

### Update Nginx
Please follow the instructions in the deploying [tethys portal docs](https://github.com/BYU-Hydroinformatics/docs/blob/master/tethys_install.md) to do this. 

Restart nginx : 

```sh
sudo service nginx restart
```

### Run New Portal

Remember NOT to run this with the `--build` tag
```sh
docker-compose -p test_copy up -d
```

That should be it. You should be able to go to your host/test_copy and see the portal come up. 