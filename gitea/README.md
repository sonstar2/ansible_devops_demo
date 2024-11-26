# Using GiTea

To run gitea as a local podman instance:

`podman network create gitea-net`

`podman container run --replace --detach --tty --env MARIADB_RANDOM_ROOT_PASSWORD=yes --env MARIADB_DATABASE=gitea --env MARIADB_USER=gitea --env MARIADB_PASSWORD=Password1 --volume gitea-db-volume:/var/lib/mysql/:Z --network gitea-net --name gitea-db docker.io/library/mariadb:10`

`podman container run --replace --detach --tty --env DB_TYPE=mysql --env DB_HOST=gitea-db:3306 --env DB_NAME=gitea --env DB_USER=gitea --env DB_PASSWORD=Password1 --volume gitea-data-volume:/var/lib/gitea:Z --volume gitea-config-volume:/etc/gitea:Z --network gitea-net --publish 2222:2222 --publish 3000:3000 --name gitea-app docker.io/gitea/gitea:1-rootless`

Then access http://aap.localdomain:3000/ for fun and games

To get a PAT for use in a "Source Control" credential: User menu -> Settings -> Applications -> Generate New Token
