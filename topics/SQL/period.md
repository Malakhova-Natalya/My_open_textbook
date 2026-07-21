## Задача про начало и конец периода

Предположим, перед нами ежемесячный отчёт. В нём - клиенты и сегменты.

Создаём таблицу:
  
    SELECT '2025-01-31' AS date, 'A11111' AS client_id, 2 AS segment_id
    UNION ALL
    SELECT '2025-02-28', 'A11111', 2
    UNION ALL
    SELECT '2025-03-31', 'A11111', 1
    UNION ALL
    SELECT '2025-04-30', 'A11111', 1
    UNION ALL
    SELECT '2025-05-30', 'A11111', 2
    UNION ALL
    SELECT '2025-11-30', 'B22222', 1
    UNION ALL
    SELECT '2025-10-31', 'B22222', 1
    UNION ALL
    SELECT '2025-09-30', 'B22222', 3
    UNION ALL
    SELECT '2025-08-31', 'B22222', 1
    UNION ALL
    SELECT '2025-11-30', 'C33333', 1
    UNION ALL
    SELECT '2025-10-31', 'C33333', 1;

