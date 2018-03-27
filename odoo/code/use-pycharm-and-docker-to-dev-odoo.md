This is written for MacOS, on Linux, things should be the same.

// Some explanation why docker, and this is gonna use docker-compose to make things simpler

Pycharm used 2017.3 Pro
Docker used 18.03 ce

First of all, create a directory in which your works will be located.

    mkdir ~/Projects/docker-odoo

Inside this, structure your directory like

    ~/Projects/docker-odoo
                  ├── addons/
                  ├── config
                  │     └── odoo
                  │          └── odoo.conf
                  ├── docker-compose.yml
                  └── odoo

I will explain a bit for this

- `addons` folder will store all your addons (developed by yourself or by a 3rd-party)
- `config/odoo/odoo.cnf` will be a configuration for odoo

      [options]
      addons_path = /mnt/extra-addons,/usr/lib/python3/dist-packages/odoo/addons
      data_dir = /var/lib/odoo
      db_host=192.168.1.50
      db_port=5432
      db_user=odoo
      db_password=odoo
      db_name = test-order-import-001
      update = mekongoo_barcode # anononyme this

- `docker-compose.xml` main docker compose file

      version: '3.1'

      services:
        odoo:
          image: odoo:11
          # restart: always
          ports:
            - 8069:8069
            - 8071:8071
          depends_on:
            - db
          environment:
            - HOST=192.168.1.50
            - USER=odoo
            - PASSWORD=odoo
          volumes:
            - odoo-web-data:/var/lib/odoo:cached
            - ./config/odoo:/etc/odoo:ro
            - ./addons:/mnt/extra-addons:cached
        db:
          image: postgres:9.4
          # restart: always
          environment:
            - POSTGRES_USER=odoo
            - POSTGRES_PASSWORD=odoo
            - PGDATA=/var/lib/postgresql/data/pgdata
          volumes:
            - odoo-db-data:/var/lib/postgresql/data/pgdata
          ports:
            - 5432:5432
      volumes:
        odoo-web-data:
        odoo-db-data:

- `odoo` an executable file

      #!/usr/bin/python3

      # set server timezone in UTC before time module imported
      __import__('os').environ['TZ'] = 'UTC'
      import odoo

      if __name__ == "__main__":
          odoo.cli.main()

Half way was done. Now change to pycharm configuration.

Project interpreter

--> Add remote (cliked on gear), choose Docker Compose

- confiruation file ./docker-compose.xml
- serviec odoo

This should show Remote Python 3.5.3 Docker Compose at ....

May mapping required

- /addons -> /mnt/extra/addons
- /odoo   -> /usr/bin/odoo

Run/debug configuration

Create new profile

- Name `odoo`
- script path `~/Projects/docker-odoo/odoo`
- parameters, e.g: if you want to update your module: `-u module_name`
- Python interpreter: Project default
- Working directory: ~/Projects/docker-odoo
- Path mapping: ~/Projects/docker-odoo/addons=/mnt/extra-addons
- Check on Add content roots to PYTHONPATH # not sure about 3 of this
- Check on Add source roots to PYTHONPATH
- Uncheck on Show command line afterwards

Open terminal and cd to ~/Projects/docker-compose

    docker-compose up -d db

Start the server. This should run your odoo container.

Access to Odoo backend on http://localhost:8069

GL HF
