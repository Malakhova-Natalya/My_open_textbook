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

На мой взгляд, сложную (или а-ля сложную) задачу лучше изначально разбить на части и решить по шагам.

    -- первый шаг: берём учеников, считаем количество пятёрок для каждого и оставляем тех, у кого набралось 10 и больше пятёрок
    WITH names_with_many_5_marks AS (  
    SELECT name, COUNT(mark) AS cnt_5
    FROM names_marks
    WHERE mark=5
    GROUP BY 1
    HAVING COUNT(mark) >=10
    )
    
    -- второй шаг: берём имена тех учеников, которых отобрали на предыдущем шаге, и считаем количество их двоек
    SELECT name, COUNT(mark) as cnt_2
    FROM names_marks 
    WHERE name IN (SELECT name FROM names_with_many_5_marks) AND mark=2
    GROUP BY 1
    ORDER BY 1
