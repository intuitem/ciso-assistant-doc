# Frequent questions



### Didn't get the prompt for the first user



If you didn't get the prompt to create the first user, or lost the password but you still have access to the infra level, you can trigger the `createsuperuser` command to fix that.\
\
In docker-based environment:\


`docker ps -a | grep backend` (this will get you the id of the Backend for CISO Assistant container, keep it for the next step)

`docker exec -it <the_container_id> poetry run python manage.py createsuperuser`\


and you should get a prompt now 😉

