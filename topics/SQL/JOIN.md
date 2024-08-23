## Виды JOIN. Пример в PostgreSQL

Попробуем на примере двух маленьких таблиц посчитать, сколько (и каких) строк получится при каждом из видов джойнов.

### Создание таблиц
Для этого мы создадим две маленькие таблицы t1 и t2. В каждой таблице - по одной колонке. В t1 - колонка a, в t2 - колонка b.


    CREATE TABLE t1 (
    a INTEGER
    );
    
    CREATE TABLE t2 (
    b INTEGER
    );

Теперь наполним их данными:
    
    INSERT INTO t1(a) VALUES(1);
    INSERT INTO t1(a) VALUES(2);
    INSERT INTO t1(a) VALUES(3);
    INSERT INTO t1(a) VALUES(4);
    INSERT INTO t1(a) VALUES(5);
    INSERT INTO t1(a) VALUES(6);
    INSERT INTO t1(a) VALUES(7);
    INSERT INTO t1(a) VALUES(NULL);
    
    INSERT INTO t2(b) VALUES(1);
    INSERT INTO t2(b) VALUES(1);
    INSERT INTO t2(b) VALUES(2);
    INSERT INTO t2(b) VALUES(2);
    INSERT INTO t2(b) VALUES(5);
    INSERT INTO t2(b) VALUES(9);
    INSERT INTO t2(b) VALUES(NULL);
    INSERT INTO t2(b) VALUES(NULL);

Посмотрим, как они выглядят, для наглядности поставим их рядом: 

    
| a |  | b |
| :--------------------: | :--------------------: | :--------------------: |
| 1 |      | 1 |
| 2 |      | 1 |
| 3 |      | 2 |
| 4 |      | 2 |
| 5 |      | 5 |
| 6 |      | 9 |
| 7 |      | NULL |
| NULL |      | NULL |

### INNER JOIN

    SELECT t1.a, t2.b
    FROM t1
    INNER JOIN t2 ON t1.a=t2.b


    a		b
    1		1
    1		1
    2		2
    2		2
    5		5

### LEFT JOIN

    SELECT t1.a, t2.b
    FROM t1
    LEFT JOIN t2 ON t1.a=t2.b


    a		b
    1		1
    1		1
    2		2
    2		2
    3		NULL
    4		NULL
    5		5
    6		NULL
    7		NULL
    NULL	NULL


### RIGHT JOIN

    SELECT t1.a, t2.b
    FROM t1
    RIGHT JOIN t2 ON t1.a=t2.b

    a		b
    1		1
    1		1
    2		2
    2		2
    5		5
    NULL	9
    NULL	NULL
    NULL	NULL

### FULL OUTER JOIN

    SELECT t1.a, t2.b
    FROM t1
    FULL JOIN t2 ON t1.a=t2.b

    a		b
    1		1
    1		1
    2		2
    2		2
    3		NULL
    4		NULL
    5		5
    6		NULL
    7		NULL
    NULL	NULL
NULL	9
NULL	NULL
NULL	NULL
