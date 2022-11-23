## SETUP
- We need to add the line "10.9.0.5 ww.seed-server.com" inside the file /etc/hosts.<br>
- Afterwards we just need to build the container and run it.
- Now we need to find tge ID of the container with `dockps` and once we know the ID we run `docksh ID` to open a shell inside the container.

### Task 1
- On the containers shell we will start using all databases associated with the user root `mysql -u root -pdees`.
- Now that we're using mysql we'll need to load the sqllab_users database with `use sqllab_users`.
- To show the database we can run `show tables`.
Insert image

ctf semana 8 desafio 1

depois de ver o codigo do index.php reparamos que a query de login estava insegura e que seria por ali a entrada maliciosa no website. 

username = admin';--
admin para entrar com a conta dele, ' para fechar o parametro do php, ; para acabar a linha de php e -- para comentar o codigo sql
