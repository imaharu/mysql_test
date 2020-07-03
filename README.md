## docker-compose.yml

```yml
version: '3.3'
services:
  db:
    image: mysql
    restart: always
    environment:
      MYSQL_DATABASE: test_db
      MYSQL_USER: user
      MYSQL_PASSWORD: password
      MYSQL_ROOT_PASSWORD: rootpassword
    ports:
      - "3314:3306"
```


```
$ docker-compose up -d
$ docker-compose exec db bash

# mysql -u user -ppassword
> use test_db
> create table users (id INT AUTO_INCREMENT NOT NULL PRIMARY KEY ,email VA
RCHAR(255) not null unique) ENGINE = INNODB;
> Insert into users (email) values ("imaharu@example.com");
> START TRANSACTION;
> Insert into users (email) values ("imaharu@example.com");
> Insert into users (email) values ("hoge@example.com");
> commit;
> select * from users;
```
