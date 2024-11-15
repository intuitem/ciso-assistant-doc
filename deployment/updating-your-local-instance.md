# Updating your local instance

All docker images are available on ghcr with the specific versions matching the repo tags. The `latest` tag points to the most recent release for both back and front.



**Backup your db file outside of the repo folder, always a good practice**

`cp db/ciso-assistant.sqlite3 ../ciso-assistant-backup.sqlite3`



**Stop the containers**

`docker compose stop`



**Delete the containers instances (you will need to confirm)**

`docker compose rm`&#x20;



**Clean up the previous docker images**

`docker rmi ghcr.io/intuitem/ciso-assistant-community/backend:latest ghcr.io/intuitem/ciso-assistant-community/frontend:latest 2> /dev/null`



**Trigger compose up to refresh the images**

`docker compose up -d`

