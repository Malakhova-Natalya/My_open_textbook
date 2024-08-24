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
