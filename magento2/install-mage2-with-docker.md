## Installation

1. Clone https://github.com/alexcheng1982/docker-magento2

2. Change `mysql` to `mariadb:latest`

3. Remove `phpmyadmin` as we won't need it.

4. Adapt your `env` file.

5. Then

        $ docker-compose up -d
        $ docker-compose exec web bash

        $ # inside the container
        $ install-magento
        $ install-sampledata

## Notes

- If your `env` file has some mistakes, then docker-compose down, and up again to get new env, need to find better way than this.

- You may want to adapt your /etc/hosts file.
