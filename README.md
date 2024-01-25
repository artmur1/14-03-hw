# Домашнее задание к занятию 3 "`MySQL`" - `Мурчин Артем`

## Задача 1

Используя Docker, поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.

Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/virt-11/06-db-03-mysql/test_data) и 
восстановитесь из него.

Перейдите в управляющую консоль `mysql` внутри контейнера.

Используя команду `\h`, получите список управляющих команд.

Найдите команду для выдачи статуса БД и **приведите в ответе** из её вывода версию сервера БД.

Подключитесь к восстановленной БД и получите список таблиц из этой БД.

**Приведите в ответе** количество записей с `price` > 300.

В следующих заданиях мы будем продолжать работу с этим контейнером.

### Решение 1

Версия сервера БД: 8.0.36 MySQL Community Server - GPL

![alt text](https://github.com/artmur1/14-03-hw/blob/main/14-03-hw-01-1.png)

Количество записей с `price` > 300 равно 1.

![alt text](https://github.com/artmur1/14-03-hw/blob/main/14-03-hw-01-2.png)

## Задача 2

Создайте пользователя test в БД c паролем test-pass, используя:

- плагин авторизации mysql_native_password
- срок истечения пароля — 180 дней 
- количество попыток авторизации — 3 
- максимальное количество запросов в час — 100
- аттрибуты пользователя:
    - Фамилия "Pretty"
    - Имя "James".

Предоставьте привелегии пользователю `test` на операции SELECT базы `test_db`.
    
Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES, получите данные по пользователю `test` и 
**приведите в ответе к задаче**.

### Решение 2

    CREATE USER 'test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'test-pass';
    ALTER USER 'test'@'localhost' PASSWORD EXPIRE INTERVAL 180 DAY;
    ALTER USER 'test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'test-pass' FAILED_LOGIN_ATTEMPTS 3;
    ALTER USER 'test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'test-pass' WITH MAX_QUERIES_PER_HOUR 20;
    ALTER USER 'test'@'localhost' ATTRIBUTE '{"fname": "Pretty", "lname": "James"}';

![alt text](https://github.com/artmur1/14-03-hw/blob/main/14-03-hw-02-1.png)

## Задача 3

Установите профилирование `SET profiling = 1`.
Изучите вывод профилирования команд `SHOW PROFILES;`.

Исследуйте, какой `engine` используется в таблице БД `test_db` и **приведите в ответе**.

Измените `engine` и **приведите время выполнения и запрос на изменения из профайлера в ответе**:
- на `MyISAM`,
- на `InnoDB`.

### Решение 3

В таблице БД test_db используется engine InnoDB.

![alt text](https://github.com/artmur1/14-03-hw/blob/main/14-03-hw-03-1.png)

Поменял engine на MyISAM

![alt text](https://github.com/artmur1/14-03-hw/blob/main/14-03-hw-03-2.png)

![alt text](https://github.com/artmur1/14-03-hw/blob/main/14-03-hw-03-3.png)

![alt text](https://github.com/artmur1/14-03-hw/blob/main/14-03-hw-03-4.png)

При выполнении одного и того же EXPLAIN ANALIZE стоимость запроса cost получается разной. При engine MyISAM cost=1.18, а при engine InnoDB cost=0.917.
Получается, что когда у таблицы engine = InnoDB база будет работать быстрее.

## Задача 4 

Изучите файл `my.cnf` в директории /etc/mysql.

Измените его согласно ТЗ (движок InnoDB):

- скорость IO важнее сохранности данных;
- нужна компрессия таблиц для экономии места на диске;
- размер буффера с незакомиченными транзакциями 1 Мб;
- буффер кеширования 30% от ОЗУ;
- размер файла логов операций 100 Мб.

Приведите в ответе изменённый файл `my.cnf`.

### Решение 4

    [mysqld]
    
    innodb_read_io_threads = 8
    innodb_write_io_threads = 8
    innodb_file_per_table = ON
    innodb_log_buffer_size = 1M
    innodb_buffer_pool_size = 614M
    innodb_log_file_size = 100M
    
    datadir=/var/lib/mysql
    socket=/var/lib/mysql/mysql.sock
    
    bind-address=0.0.0.0
    server-id=1
    log_bin=/var/log/mysql/mybin.log
    
    log-error=/var/log/mysql/mysqld.log
    pid-file=/var/run/mysqld/mysqld.pid


---

### Как оформить ДЗ

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
