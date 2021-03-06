# CMaNGOS - Docker
## What is it?
These are docker-compose files for the CMaNGOS project for the WotLK variant.

## How to use it?
To use this project clone it using:
```
$ git clone https://github.com/korhaldragonir/cmangos-docker.git
```

Place the Data files (DBC, Maps, VMaps, MMaps) in the directory `/opt/cmangos-data` with the following structure:
```
/opt/cmangos-data
├── dbc
├── maps
├── mmaps
└── vmaps
```
I did not include these because they would make the repository far to big and may need re-extraction every once in a while.

Make sure you have docker and docker-compose installed on your system and run the following command in the `cmangos-docker` directory:
```
$ sudo docker-compose up -d
```

Now wait for a while because it will need to build the images (cmangos_mangosd, cmangos_realmd, cmangos_db) and will then start spinning up containers. (Depending on your system this can take between 30min up to 1 hour)

Afterwards you should be able to connect with mysql to 127.0.0.1:3306 and change the IP of the Realm if you are not planning on using it locally:
```
$ mysql -umangos -pmangos -h 127.0.0.1 -P 3306
mysql> use wotlkrealmd;
mysql> UPDATE `realmlist` SET `address` = '<your-ip>', `name` = '<realm-name>' WHERE `id` = '1' LIMIT 1;
mysql> quit;
```

## Running without Docker-Compose
If you want to run the individual containers manually and without docker-compose find an example in the `Dockerrun` or `docker-run.sh` files in the subdirectories.

## Environment Variables
### cmangos_mangosd
| Variable     | Description           | Default Value     |
|--------------|-----------------------|-------------------|
| TZ       | This variable is used set the correct timezone. | Europe/Amsterdam |
| CHARACTERS_DB | This variable is used to set the Characters Database during init. | wotlkcharacters |
| MANGOSD_DB | This variable is used to set the Wold Database during init. | wotlkmangos |
| REALMD_DB | This variable is used to set the Realm Database during init. | wotlkrealmd |
| DB_USER | This variable is used to set the Database user for the World and Characters Database during init. | mangos |
| DB_PASS | This variable is used to set the Database password for the World and Characters Database during init. | mangos |
| REALMD_USER | This variable is used to set the Database user for the Realm Database during init. | mangos |
| REALMD_PASS | This variable is used to set the Database password for the Realm Database during init. | mangos |
| DB_SERVER | This variable is used to set the Database Server (Host or IP) for the World and Characters Database during init. | database |
| REALMD_SERVER | This variable is used to set the Database Server (Host or IP) for the Realm Database during init. | database |

### cmangos_realmd
| Variable     | Description           | Default Value     |
|--------------|-----------------------|-------------------|
| TZ       | This variable is used set the correct timezone. | Europe/Amsterdam |
| REALMD_DB | This variable is used to set the Realm Database during init. | wotlkrealmd |
| DB_USER | This variable is used to set the Database user for the Realm Database during init. | mangos |
| DB_PASS | This variable is used to set the Database password for the Realm Database during init. | mangos |
| DB_SERVER | This variable is used to set the Database Server (Host or IP) for the Realm Database during init. | database |

### cmangos_db
| Variable     | Description           | Default Value     |
|--------------|-----------------------|-------------------|
| TZ       | This variable is used set the correct timezone. | Europe/Amsterdam |
| MYSQL_ROOT_PASSWORD       | This variable is used to set the MySQL/MariaDB root password on initial deploy. | mangos |

## Tips
If you want to re-build the CMaNGOS code within the images without bringing down the running containers you can run the following from within the `cmangos-docker` directory:
```
$ sudo docker-compose build --no-cache
```

Then after that was succesful you can run:
```
$ sudo docker-compose down
$ sudo docker-compose up -d
```
Now you should be running the new version with minimal downtime.

# Credits
Thanks to @vishnubob and contributors for the wait-for-it.sh script (https://github.com/vishnubob/wait-for-it).  
Thanks to CMaNGOS Community (https://github.com/cmangos) / (https://cmangos.net/)  