# Log

Installed

- nextcloud (pacman hook)
- Conf php (extensions)
- php-intl
- mariadb (mariadb install init cmd) mysql_secure_installation (enable, start service), create db, user, grant...
- redis, php-redis (conf) (enable, start)
- nginx
- php-fpm (enable, start)

Tune

Add indices for Calendar and Contact apps

    cd /usr/share/webapps/nextcloud
    sudo -u http php occ db:add-missing-indices
