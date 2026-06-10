## Как работает оконная функция. Пример в ClickHouse

Попробуем на примере посмотреть, как отрабатывает оконная функция в разных версиях.

### Создание примера
Для этого мы задаём в CTE пример


    with t1 as (
    select null as id1, 1 as id2, 1 as ord
    union all 
    select 2 as id1, 2 as id2, 2 as ord
    union all 
    select 3 as id1, null as id2, 3 as ord
    union all 
    select null as id1, null as id2, 4 as ord
    union all 
    select 2 as id1, 5 as id2, 5 as ord
    )
    select id1, id2
    from t1
    order by ord;


Посмотрим, как они выглядят, для наглядности поставим их рядом: 

    
| department | name | salary |
| :--------------------: | :--------------------: |:--------------------: |
| IT | Ivanov | 100 |
| IT | Petrov | 200 |
| IT | Sidorov | 300 |
| Marketing | Smirnova | 100 |
| Marketing |  Savelyeva | 200 |
