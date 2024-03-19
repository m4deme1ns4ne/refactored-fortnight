# Как развернуть MYSQL в PHPMYADMIN с помощью DOCKER

Для развёртывания phpMyAdmin с MySQL в Docker, вам нужно создать docker-compose.yml файл, который будет содержать конфигурацию для обоих контейнеров.

## Пункт 1

Для начала вам нужно иметь уже контейнер с развёрнутой базой данных mysql.

Чтобы его создать нужно ввести команду:

```bash
docker run --name mysql-test \
-v /Users/northbug/Development/testing/dbases:/var/lib/mysql \
-p 33060:3306 \
-e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:8.0.36
```

### Примечание 1

Вам нужно заменить путь к локальному каталогу, который вы мэпите внутрь контейнера с СУБД.
Можно заменить название mysql-test и пароль my-secret-pw.



### Примечание 2

Если контейнер не запустится, т.е. не будет выведен при запуске команды docker ps -a, то может потребоваться изменить порт, указываемый с помощью ключа -p. Например, так: -p 63306:3306



### Для просмотра работающих контейнеров:

```docker
docker ps
```



### Для просмотра всех контейнеров:

```docker
docker ps -a
```

Обращайте внимание на id, name.



### Для остановки контейнера:

```docker
docker stop <id или name контейнера>
```

id можно указывать частично (первые 3-4 символа).



### Для запуска контейнера:

```docker
docker start <id или name контейнера>
```

id можно указывать частично (первые 3-4 символа).

Эти же действия указываются через Docker Desktop (кнопка "Play" / "Stop")

Чтобы удалить работающий контейнер необходимо его остановить или вызвать удаление с ключом -f



### Например:

```docker
docker rm -f <id или name контейнера>
```

или

```docker
docker stop <id или name контейнера>
```



## Пункт 2

Далее нужно создать файл docker-compose.yml в проекте с содержанием:

```docker
version: '3'

services:
  db:
    image: mysql:latest
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: your_root_password
      MYSQL_DATABASE: your_database_name
      MYSQL_USER: your_username
      MYSQL_PASSWORD: your_password

  phpmyadmin:
    image: phpmyadmin/phpmyadmin:latest
    restart: always
    ports:
      - "8080:80"
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: your_root_password
```

Поменяйте your_root_password, your_database_name, your_username и your_password на ваши значения.

### Пример:

```docker
version: "3"

services:
  db:
    image: mysql:latest
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 1324
      MYSQL_DATABASE: hello_phpmyadmin
      MYSQL_USER: alexander
      MYSQL_PASSWORD: 1324

  phpmyadmin:
    image: phpmyadmin/phpmyadmin:latest
    restart: always
    ports:
      - "8080:80"
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: your_root_password

```



1. После создания docker-compose.yml файла, выполните следующие команды в директории с файлом:

   ```docker
   docker-compose up -d
   ```

2. Откройте браузер и перейдите по [http://localhost:8080](http://localhost:8080) или [http://your_server_ip:8080](http://your_server_ip:8080) для доступа к phpMyAdmin. Введите учётные данные для подключения к MySQL.


# How to deploy MYSQL to PHPMYADMIN using DOCKER

To deploy phpMyAdmin with MySQL in Docker, you need to create a docker-compose.yml file that will contain the configuration for both containers.

## Paragraph 1

First, you need to already have a container with a deployed mysql database.

To create it you need to enter the command:

```bash
docker run --name mysql-test\
-v /Users/northbug/Development/testing/dbases:/var/lib/mysql \
-p 33060:3306\
-e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:8.0.36
```

### Note 1

You need to replace the path to the local directory that you map inside the container with the DBMS.
You can replace the name mysql-test and the password my-secret-pw.



### Note 2

If the container does not start, i.e. will not be printed when you run the docker ps -a command, you may need to change the port specified using the -p switch. For example, like this: -p 63306:3306



### To view running containers:

```docker
docker ps
```



### To view all containers:

```docker
docker ps -a
```

Pay attention to id, name.



### To stop the container:

```docker
docker stop <container id or name>
```

id can be specified partially (the first 3-4 characters).



### To start a container:

```docker
docker start <container id or name>
```

id can be specified partially (the first 3-4 characters).

The same actions are specified via Docker Desktop ("Play" / "Stop" button)

To delete a running container, you need to stop it or call deletion with the -f switch



### For example:

```docker
docker rm -f <container id or name>
```

or

```docker
docker stop <container id or name>
```



## Point 2

Next you need to create a docker-compose.yml file in the project with the content:

```docker
version: '3'

services:
   db:
     image: mysql:latest
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: your_root_password
       MYSQL_DATABASE: your_database_name
       MYSQL_USER: your_username
       MYSQL_PASSWORD: your_password

   phpmyadmin:
     image: phpmyadmin/phpmyadmin:latest
     restart: always
     ports:
       - "8080:80"
     environment:
       PMA_HOST: db
       MYSQL_ROOT_PASSWORD: your_root_password
```

Change your_root_password, your_database_name, your_username and your_password to your values.

### Example:

```docker
version: "3"

services:
   db:
     image: mysql:latest
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: 1324
       MYSQL_DATABASE: hello_phpmyadmin
       MYSQL_USER: alexander
       MYSQL_PASSWORD: 1324

   phpmyadmin:
     image: phpmyadmin/phpmyadmin:latest
     restart: always
     ports:
       - "8080:80"
     environment:
       PMA_HOST: db
       MYSQL_ROOT_PASSWORD: your_root_password

```



1. After creating the docker-compose.yml file, run the following commands in the directory with the file:

    ``` docker
    docker-compose up -d
    ```

2. Open your browser and go to [http://localhost:8080](http://localhost:8080) or [http://your_server_ip:8080](http://your_server_ip:8080) to access phpMyAdmin. Enter your credentials to connect to MySQL.
