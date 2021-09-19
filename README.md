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

вывод данных в таблицах

    test_db=# select * from orders;
     id |     наименование     | цена
    ----+----------------------+------
      1 | Шоколад              |   10
      2 | Принтер              | 3000
      3 | Книга                |  500
      4 | Монитор              | 7000
      5 | Гитара               | 4000
    (5 rows)


    create table clients
    (
	
	    id SERIAL PRIMARY KEY,
    	фамилия char(20),
	    страна_проживания char(20),
    	заказ integer,
    	FOREIGN KEY (заказ) REFERENCES  orders (id)
    );


    test_db=# select * from clients
    test_db-# ;
     id |       фамилия        |  страна_проживания   | заказ
    ----+----------------------+----------------------+-------
      2 | Петров Пётр Петрович | Canada               |
      3 | Иоган Себастьян Бах  | Japan                |
      4 | Ронни Джеймс Дио     | Russia               |
      5 | Rithie Blackmore     | Russia               |
      1 | Иванов Иван Иванович | USA                  |     3
        (5 rows)


    test_db=# select count (*) from orders;
     count
    -------
        5
    (1 row)

    test_db=# select count (*) from clients;
    count
    -------
         5
    (1 row)
    
    
## Задача 4



		test_db=# update clients1
		test_db-# set заказ = 3
		test_db-# where id = 1;
		UPDATE 1
		test_db=# select * from orders;
		 id |     наименование     | цена
		----+----------------------+------
		  1 | Шоколад              |   10
 		  2 | Принтер              | 3000
		  3 | Книга                |  500
 		  4 | Монитор              | 7000
 		  5 | Гитара               | 4000
		(5 rows)
		
		test_db=# select * from clients1
		test_db-# ;
		 id |       фамилия        |  страна_проживания   | заказ
		----+----------------------+----------------------+-------
 		  2 | Петров Пётр Петрович | Canada               |
		  3 | Иоган Себастьян Бах  | Japan                |
		  4 | Ронни Джеймс Дио     | Russia               |
		  5 | Rithie Blackmore     | Russia               |
 		  1 | Иванов Иван Иванович | USA                  |     3
		(5 rows)

		test_db=# update clients1
		set заказ = 4
		where id = 2;
		UPDATE 1
		test_db=# update clients1
		set заказ = 5
		where id = 3;
		UPDATE 1
		test_db=#
		test_db=#
		test_db=# select * from clients1
		;
		 id |       фамилия        |  страна_проживания   | заказ
		----+----------------------+----------------------+-------
		  4 | Ронни Джеймс Дио     | Russia               |
		  5 | Rithie Blackmore     | Russia               |
 		  1 | Иванов Иван Иванович | USA                  |     3
		  2 | Петров Пётр Петрович | Canada               |     4
		  3 | Иоган Себастьян Бах  | Japan                |     5
		(5 rows)
		
		
			test_db=# select *
		from clients1
		where exists
		(select *
		from orders
		where clients1.заказ = orders.id);
		 id |       фамилия        |  страна_проживания   | заказ
-		 ---+----------------------+----------------------+-------
		  1 | Иванов Иван Иванович | USA                  |     3
		  2 | Петров Пётр Петрович | Canada               |     4
 		  3 | Иоган Себастьян Бах  | Japan                |     5
		(3 rows)
		
		test_db=#
		
		
## Задача 5
	
		explain select *
		from clients
		where exists
		(select *
		from orders
		where clients1.заказ = orders.id);
		
		
		test_db=#
		test_db=# explain select *
		test_db-# from clients
		test_db-# where exists
		test_db-# (select *
		test_db(# from orders
		test_db(# where clients.заказ = orders.id);
      		                       QUERY PLAN
		---------------------------------------------------------------------
		 Hash Join  (cost=25.30..40.36 rows=400 width=176)
  		 Hash Cond: (clients1."заказ" = orders.id)
 		  ->  Seq Scan on clients1  (cost=0.00..14.00 rows=400 width=176)
		  ->  Hash  (cost=16.80..16.80 rows=680 width=4)
      		  ->  Seq Scan on orders  (cost=0.00..16.80 rows=680 width=4)
		(5 rows)
		
	Структура плана запроса представляет собой дерево узлов плана. Узлы на нижнем уровне дерева — это узлы сканирования, которые возвращают необработанные данные таблицы. Разным типам доступа к таблице соответствуют разные узлы: последовательное сканирование, сканирование индекса и сканирование битовой карты. Источниками строк могут быть не только таблицы, но и например, предложения VALUES и функции, возвращающие множества во FROM, и они представляются отдельными типами узлов сканирования. Если запрос требует объединения, агрегатных вычислений, сортировки или других операций с исходными строками, над узлами сканирования появляются узлы, обозначающие эти операции. 

cost - Приблизительная стоимость запуска. Это время, которое проходит, прежде чем начнётся этап вывода данных, например для сортирующего узла это время сортировки.

rows - Ожидаемое число строк, которое должен вывести этот узел плана. При этом так же предполагается, что узел выполняется до конца.

weidth - Ожидаемый средний размер строк, выводимых этим узлом плана (в байтах).

## Задача 6

Для создания бекапа подключаемся к контейнеру с базой в тепминале bash

		c:\Oleg\data>docker exec -it pg-test bash

		root@06382f9d4a43:/etc/postgresql#
		root@06382f9d4a43:/etc/postgresql# pg_dump -U oleg test_db > test_db.dump
		
		root@06382f9d4a43:/etc/postgresql# dir
		test_db.dump
		root@06382f9d4a43:/etc/postgresql#
		
Бекап тпкже появился на диске хостовой машины
		
		c:\Oleg>cd c:/oleg/backup
		
		c:\Oleg\backup>dir
 		Том в устройстве C не имеет метки.
		 Серийный номер тома: DE8F-3CD9
		
 		Содержимое папки c:\Oleg\backup
		
		19.09.2021  12:29    <DIR>          .
		19.09.2021  12:29    <DIR>          ..
		19.09.2021  12:32             6 128 test_db.dump
 		              1 файлов          6 128 байт
 		              2 папок  69 357 105 152 байт свободно
			 

Далее останавливаю текущий контейнер с базой и запускаю новый

контейнер pg-test1, база с тем же именеим test_db, волуме для данных с другой директроии хостовой машины

		c:\Oleg>docker run --name pg-test1 -e POSTGRES_PASSWORD=oleg -e POSTGRES_USER=oleg -e POSTGRES_DB=test_db -d -p 5432:5432 -v 					c:\oleg\data1:/var/lib/postgresql/data -v c:\oleg\backup:/etc/postgresql/ postgres
		23e96795e95952aba50938bf248b20af638a4c7a72f76e3b8587924f75cde80e

		c:\Oleg>
		c:\Oleg>
		c:\Oleg>docker ps -a
		CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS                      PORTS                                       NAMES
		23e96795e959   postgres                 "docker-entrypoint.s…"   8 seconds ago   Up 6 seconds                0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   pg-		test1
		06382f9d4a43   postgres                 "docker-entrypoint.s…"   19 hours ago    Exited (0) 14 minutes ago                                               pg-		test
		0bb3ac6f8d19   test                     "/usr/bin/ponysay /b…"   5 days ago      Exited (0) 5 days ago                                                   pony1
		ba7167c6c0d3   archlinux                "/usr/bin/bash"          7 days ago      Exited (255) 4 days ago                                                 arch
		64327697e578   docker/getting-started   "/docker-entrypoint.…"   7 days ago      Exited (255) 4 days ago     0.0.0.0:80->80/tcp, :::80->80/tcp           		musing_boyd
		
		c:\Oleg>
		c:\Oleg>
		c:\Oleg>docker exec -it pg-test1 psql -U oleg -d test_db
		psql: error: could not connect to server: No such file or directory
     		   Is the server running locally and accepting
     		   connections on Unix domain socket "/var/run/postgresql/.s.PGSQL.5432"?
		
		c:\Oleg>
		c:\Oleg>
		c:\Oleg>
		c:\Oleg>docker ps -a
		CONTAINER ID   IMAGE                    COMMAND                  CREATED              STATUS                      PORTS                                       		NAMES
		23e96795e959   postgres                 "docker-entrypoint.s…"   About a minute ago   Up About a minute           0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   		pg-test1
		06382f9d4a43   postgres                 "docker-entrypoint.s…"   19 hours ago         Exited (0) 15 minutes ago                                               		pg-test
		0bb3ac6f8d19   test                     "/usr/bin/ponysay /b…"   5 days ago           Exited (0) 5 days ago                                                   		pony1
		ba7167c6c0d3   archlinux                "/usr/bin/bash"          7 days ago           Exited (255) 4 days ago                                                 		arch
		64327697e578   docker/getting-started   "/docker-entrypoint.…"   7 days ago           Exited (255) 4 days ago     0.0.0.0:80->80/tcp, :::80->80/tcp           		musing_boyd
		
		c:\Oleg>
		c:\Oleg>
		c:\Oleg>
		c:\Oleg>docker exec -it pg-test1 psql -U oleg -d test_db
		psql (13.4 (Debian 13.4-1.pgdg100+1))
		Type "help" for help.
		
		test_db=#
		test_db=#
		test_db=#
		test_db=#
		test_db=# /l
		test_db-# /
		test_db-# \l
   		                          List of databases
 		  Name    | Owner | Encoding |  Collate   |   Ctype    | Access privileges
	       -----------+-------+----------+------------+------------+-------------------
 		postgres  | oleg  | UTF8     | en_US.utf8 | en_US.utf8 |
	        template0 | oleg  | UTF8     | en_US.utf8 | en_US.utf8 | =c/oleg          +
         		  |       |          |            |            | oleg=CTc/oleg
 		template1 | oleg  | UTF8     | en_US.utf8 | en_US.utf8 | =c/oleg          +
   		          |       |          |            |            | oleg=CTc/oleg
			test_db   | oleg  | UTF8     | en_US.utf8 | en_US.utf8 |
		(4 rows)

Проверка, что база пустая, нет таблиц
		test_db-#
		test_db-# \dt
		Did not find any relations.
		test_db-#


Востановка из бекапа


		root@23e96795e959:/etc/postgresql# psql -U oleg test_db < test_db.dump
		SET
		SET
		SET
		SET
		SET
		 set_config
		------------
		
		(1 row)
		
		SET
		SET
		SET
		SET
		SET
		SET
		CREATE TABLE
		ALTER TABLE
		CREATE TABLE
		ALTER TABLE
		CREATE SEQUENCE
		ALTER TABLE
		ALTER SEQUENCE
		CREATE SEQUENCE
		ALTER TABLE
		ALTER SEQUENCE
		CREATE TABLE
		ALTER TABLE
		CREATE SEQUENCE
		ALTER TABLE
		ALTER SEQUENCE
		ALTER TABLE
		ALTER TABLE
		ALTER TABLE
		COPY 5
		COPY 5
		COPY 5
		
		 setval
		--------
		      1
		(1 row)
		
 		setval
		--------
   		   1
		(1 row)
		
		 setval
		--------
		      1
		(1 row)
		
		ALTER TABLE
		ALTER TABLE
		ALTER TABLE
		ALTER TABLE
		ALTER TABLE
		
		root@23e96795e959:/etc/postgresql#
		
Подключаюсь к БД и проверяю таблицы

		docker exec -it pg-test1 psql -U oleg -d test_db
		
		test_db=# \dt
   		      List of relations
		 Schema |   Name   | Type  | Owner
		--------+----------+-------+-------
		 public | clients  | table | oleg
		 public | clients1 | table | oleg
		 public | orders   | table | oleg
		(3 rows)
		
		test_db=# select * from clients1
		test_db-# ;
		 id |       фамилия        |  страна_проживания   | заказ
		----+----------------------+----------------------+-------
 		  4 | Ронни Джеймс Дио     | Russia               |
		  5 | Rithie Blackmore     | Russia               |
		  1 | Иванов Иван Иванович | USA                  |     3
		  2 | Петров Пётр Петрович | Canada               |     4
 		  3 | Иоган Себастьян Бах  | Japan                |     5
		(5 rows)

База востановлена из бекапа


    
