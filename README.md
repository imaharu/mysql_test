# MySQL

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

# Postgres


## docker-compose.yml

```
version: '3.1'

services:
  db:
    image: postgres
    restart: always
    environment:
      POSTGRES_PASSWORD: example

  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
```

```
root@51f0c7cff0d2:/# psql -U postgres
psql (12.3 (Debian 12.3-1.pgdg100+1))
Type "help" for help.

postgres=# select * from users;
 email 
-------
(0 rows)

postgres=# Insert into users (email) values ("imaharu@example.com");
ERROR:  column "imaharu@example.com" does not exist
LINE 1: Insert into users (email) values ("imaharu@example.com");
                                          ^
postgres=# Insert into users (email) values ('imaharu@example.com');
INSERT 0 1
postgres=# select * from users;
        email        
---------------------
 imaharu@example.com
(1 row)

postgres=# BEGIN;
BEGIN
postgres=# Insert into users (email) values ('imaharu@example.com');
ERROR:  duplicate key value violates unique constraint "users_email_key"
DETAIL:  Key (email)=(imaharu@example.com) already exists.
postgres=# Insert into users (email) values ('hoge@example.com');
ERROR:  current transaction is aborted, commands ignored until end of transaction block
postgres=# COMMIT;
ROLLBACK
postgres=# select * from users;
        email        
---------------------
 imaharu@example.com
(1 row)

postgres=# 
```
