This is written for MacOS, on Linux, things should be the same.

// Some explanation why docker, and this is gonna use docker-compose to make things simpler

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
