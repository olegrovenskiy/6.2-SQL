# HomeWork 6.2-SQL

## Задача 1

  Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, 
в который будут складываться данные БД и бэкапы.

Загрузка образа

    c:\Oleg\data>docker pull postgres
    c:\Oleg\data>docker images
    REPOSITORY                     TAG       IMAGE ID       CREATED        SIZE
    ponysays-al                    latest    4be9438282ac   5 days ago     469MB
    olegrovenskiy/ponysays-01.v1   latest    4be9438282ac   5 days ago     469MB
    test                           latest    9073c1ec0d2c   5 days ago     469MB
    <none>                         <none>    281a4bc6dd9f   5 days ago     461MB
    postgres                       latest    346c7820a8fb   2 weeks ago    315MB
    archlinux                      latest    1d6f90387c13   2 weeks ago    381MB
    docker/getting-started         latest    083d7564d904   3 months ago   28MB

Запуск контейнера:

    docker run --name pg-test -e POSTGRES_PASSWORD=oleg -e POSTGRES_USER=oleg -e POSTGRES_DB=netology -d -p 5432:5432 -v c:\oleg\data1:/var/lib/postgresql/data -v        c:\oleg\backup:/etc/postgresql/ postgres
    
  С созданием БД - netology, пользователь oleg
  
    c:\Oleg\data>docker exec -it pg-test psql -U oleg -d netology
    psql (13.4 (Debian 13.4-1.pgdg100+1))
    Type "help" for help.

     netology=#
     netology=#
     netology=#
 Подключены два  voleme - один для данных, второй для бекапов
 
 ## Задача 2

создание БД

    CREATE DATABASE test_db;

    test_db=# \l
                             List of databases
       Name    | Owner | Encoding |  Collate   |   Ctype    | Access privileges
    -----------+-------+----------+------------+------------+-------------------
     netology  | oleg  | UTF8     | en_US.utf8 | en_US.utf8 |
     postgres  | oleg  | UTF8     | en_US.utf8 | en_US.utf8 |
     template0 | oleg  | UTF8     | en_US.utf8 | en_US.utf8 | =c/oleg          +
               |       |          |            |            | oleg=CTc/oleg
     template1 | oleg  | UTF8     | en_US.utf8 | en_US.utf8 | =c/oleg          +
               |       |          |            |            | oleg=CTc/oleg
     test_db   | oleg  | UTF8     | en_US.utf8 | en_US.utf8 |
    (5 rows)
    
    
       test_db=# \dt
            List of relations
     Schema |  Name   | Type  | Owner
    --------+---------+-------+-------
     public | clients | table | oleg
     public | orders  | table | oleg
    (2 rows)
    
    Создана БД и требуемые таблицы
    
    
       GRANT ALL PRIVILEGES ON DATABASE "test_db" to test_admin_user;

       GRANT SELECT, UPDATE, INSERT, DELETE ON ALL TABLES IN SCHEMA public TO "test_simple_user";


        test_db=# select * from pg_user;
         usename      | usesysid | usecreatedb | usesuper | userepl | usebypassrls |  passwd  | valuntil | useconfig
    ------------------+----------+-------------+----------+---------+--------------+----------+----------+-----------
     oleg             |       10 | t           | t        | t       | t            | ******** |          |
     test_admin_user  |    16385 | f           | f        | f       | f            | ******** |          |
     test_simple_user |    16418 | f           | f        | f       | f            | ******** |          |
    (3 rows)

    test_db=#

Созданы пользователи с требуемыми правами


## Задача 3


    
