# Домашнее задание к занятию "6.4. PostgreSQL"

## Задача 1

Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.

vagrant@vagrant:~$ sudo docker pull postgres:13

vagrant@vagrant:~$ sudo docker volume create vol1
vol1

vagrant@vagrant:~$sudo docker run --rm --name pg-netology -e POSTGRES_PASSWORD=vagrant -ti -p 5432:5432 -v vol1:/var/lib/postgresql/data postgres:13
328aa7a4634fec298214613bc3e67d023a7b27e2d138bab10c3e6aa1b2c372f5

Подключитесь к БД PostgreSQL используя `psql`.

vagrant@vagrant:~$ sudo docker exec -it pg-netology psql -U postgres
psql (13.7 (Debian 13.7-1.pgdg110+1))
Type "help" for help.

postgres=#

Воспользуйтесь командой `\?` для вывода подсказки по имеющимся в `psql` управляющим командам.

postgres=# \?
General
  \copyright             show PostgreSQL usage and distribution terms
  \crosstabview [COLUMNS] execute query and display results in crosstab
  \errverbose            show most recent error message at maximum verbosity
  \g [(OPTIONS)] [FILE]  execute query (and send results to file or |pipe);
                         \g with no arguments is equivalent to a semicolon
  \gdesc                 describe result of query, without executing it
  \gexec                 execute query, then execute each value in its result
  \gset [PREFIX]         execute query and store results in psql variables
  \gx [(OPTIONS)] [FILE] as \g, but forces expanded output mode
  \q                     quit psql
  \watch [SEC]           execute query every SEC seconds
  *
  *
  *

**Найдите и приведите** управляющие команды для:
- вывода списка БД
![image](https://user-images.githubusercontent.com/91233405/169825508-ac04a2c1-b7e8-4b6f-8b53-60521a053737.png)

- подключения к БД

![image](https://user-images.githubusercontent.com/91233405/169826483-5a58b416-42a2-4642-a177-ff7d8bac48ce.png)


- вывода списка таблиц

![image](https://user-images.githubusercontent.com/91233405/169826594-0098c7e8-0574-4d0a-a172-5c67364c8860.png)


- вывода описания содержимого таблиц

![image](https://user-images.githubusercontent.com/91233405/169826810-d366919d-8424-4227-8189-3b188788ee18.png)


- выхода из psql

![image](https://user-images.githubusercontent.com/91233405/169826865-0602df9a-5f39-42f1-a1ab-e7c5ad651218.png)


## Задача 2

Используя `psql` создайте БД `test_database`.

![image](https://user-images.githubusercontent.com/91233405/169832528-4aecfb3b-85fe-43a5-9b27-54a3d36bcd02.png)



Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/master/06-db-04-postgresql/test_data).

Восстановите бэкап БД в `test_database`.

![image](https://user-images.githubusercontent.com/91233405/169837750-14522166-c4a3-429d-bc4a-754923e8ec8e.png)


Перейдите в управляющую консоль `psql` внутри контейнера.



Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.

![image](https://user-images.githubusercontent.com/91233405/169838692-9b9265ce-214e-42c1-ae64-d559cde71230.png)


Используя таблицу [pg_stats](https://postgrespro.ru/docs/postgresql/12/view-pg-stats), найдите столбец таблицы `orders` 
с наибольшим средним значением размера элементов в байтах.

![image](https://user-images.githubusercontent.com/91233405/169838858-8abdd07f-c45c-4c1c-8271-9e69d672d5b8.png)


**Приведите в ответе** команду, которую вы использовали для вычисления и полученный результат.

## Задача 3

Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и
поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили
провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).

Предложите SQL-транзакцию для проведения данной операции.

![image](https://user-images.githubusercontent.com/91233405/169839697-6f50aa4d-31fa-4f9f-8bc1-b7e8fa6f5d88.png)


Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?

Если сразу создать секционированную таблицу

## Задача 4

Используя утилиту `pg_dump` создайте бекап БД `test_database`.

vagrant@vagrant:~$ sudo docker exec -it pg-netology bash

root@328aa7a4634f:/# cd /var/lib/postgresql

root@328aa7a4634f:/var/lib/postgresql# pg_dump -U postgres -d test_database >test_database_dump.sql

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца `title` для таблиц `test_database`?
Можно добавить индекс.

---

### Как cдавать задание

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
