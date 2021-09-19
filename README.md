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
   
   

    
