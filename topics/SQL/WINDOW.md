## Как работает оконная функция. Пример в ClickHouse

Попробуем на примере посмотреть, как отрабатывает оконная функция в разных версиях.

### Создание примера
Для этого мы задаём в CTE пример


    with t1 as (
    select 'IT' as department, 'Ivanov' as name, 100 as salary, 1 as ord
    union all 
    select 'IT' as department, 'Petrov' as name, 200 as salary, 2 as ord
    union all 
    select 'IT' as department, 'Sidorov' as name, 300 as salary, 3 as ord
    union all 
    select 'Marketing' as department, 'Smirnova' as name, 100 as salary, 4 as ord
    union all 
    select 'Marketing' as department, 'Savelyeva' as name, 200 as salary, 5 as ord
    )
    select department, name, salary
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
