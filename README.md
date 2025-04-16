


# backup-restore-n8n-in-docker
How to backup workflows and credentials of n8n running with docker to another self host?

#in old self host backup to json files
```
cd ~
#backup workflow in old self host
docker exec -it n8n /bin/sh -c "n8n export:workflow --all" > export.json
#backup credentials
docker exec -it n8n /bin/sh -c "n8n export:credentials --all" > export_cre.json
```
#in new self host auto install n8n:

```
cd ~
apt update && sudo apt upgrade -y
bash -c "$(curl -fsSL https://raw.githubusercontent.com/thanhlongzzz/backup-restore-n8n-in-docker/refs/heads/main/auto_install_n8n_with_docker_and_ssl_domain.sh)"
```
#in new self host import

```
docker cp ~/export.json n8n:/home/node/.n8n
docker cp ~/export_cre.json n8n:/home/node/.n8
docker exec -it n8n /bin/sh -c "n8n import:credentials --input=/home/node/.n8n/export_cre.json"
docker exec -it n8n /bin/sh -c "n8n import:workflow --input=/home/node/.n8n/export.json"
```


#update n8n newer version in future

```
cd ~/n8n_data/
docker-compose down
docker-compose pull
docker-compose up -d
```
