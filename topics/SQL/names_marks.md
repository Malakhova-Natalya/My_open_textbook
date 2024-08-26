## Задача про имена и оценки

Предположим, перед нами классный журнал. В нём - имена и оценки.

Создаём таблицу:
  
    CREATE TABLE IF NOT EXISTS names_marks (
    Name VARCHAR(30),
    Mark INTEGER
    );

Наполняем её данными:
  
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 5); 
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 5);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 4);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 3);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 5);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 5);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 3);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 5);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 4);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 5);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 5);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 5);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 5);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 5);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 2);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 5);
    INSERT INTO names_marks(Name, Mark) VALUES('Иванов И.И.', 5);
    INSERT INTO names_marks(Name, Mark) VALUES('Петров П.П.', 5);
    INSERT INTO names_marks(Name, Mark) VALUES('Петров П.П.', 5);
    INSERT INTO names_marks(Name, Mark) VALUES('Петров П.П.', 2);

**Задача:**

Найти количество двоек для тех учеников, у которых 10 и больше пятёрок.

**Решение:**

1. На мой взгляд, сложную (или а-ля сложную) задачу лучше изначально разбить на части: озвучить логику шагов и затем уже написать код по шагам. Плюс можно сразу оформить код читабельно, с комментариями к шагам.


        --первый шаг: берём учеников, считаем количество пятёрок для каждого и оставляем тех, у кого набралось 10 и больше пятёрок
        WITH names_with_many_5_marks AS (  
        SELECT name, COUNT(mark) AS cnt_5
        FROM names_marks
        WHERE mark=5
        GROUP BY 1
        HAVING COUNT(mark) >=10
        )
        
        --второй шаг: берём имена тех учеников, которых отобрали на предыдущем шаге, и считаем количество их двоек
        SELECT name, COUNT(mark) as cnt_2
        FROM names_marks 
        WHERE name IN (SELECT name FROM names_with_many_5_marks) AND mark=2
        GROUP BY 1
        ORDER BY 1


2. Но, возможно, спрашивают именно в формате без доп.шагов/подзапросов. Тогда можно использовать FILTER в блоке SELECT (и в HAVING тоже)


        SELECT Name, COUNT(Mark) FILTER(where mark=2) AS cnt_2     -- выводим имя и количество двоек
        FROM names_marks
        WHERE mark=5 or mark=2                                     -- можно и без этого, но так мы сразу снижаем нагрузку на БД, отбирая только строки с нужными оценками
        GROUP BY 1
        HAVING (COUNT(Mark) FILTER(where mark=5))>=10              -- здесь реализуем подзапрос: отбираем тех, у кого 10 и больше пятёрок
        ORDER BY 1

Похожий пример (с FILTER): [здесь](https://github.com/Malakhova-Natalya/Snippets/tree/main/clickhouse/clickhouse_last_value)
