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


Как выглядит пример: 

    
| department | name | salary |
| :--------------------: | :--------------------: |:--------------------: |
| IT | Ivanov | 100 |
| IT | Petrov | 200 |
| IT | Sidorov | 300 |
| Marketing | Smirnova | 100 |
| Marketing |  Savelyeva | 200 |

partition by и partition by + order by работают так:

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
    select 
    	department, 
    	name, 
    	salary,
    	sum(salary) over(partition by department) as dep_salary,
    	sum(salary) over(partition by department order by name) as dep_salary_v2
    from t1
    order by ord;

| department | name | salary | dep_salary | dep_salary_v2 |
| :--------------------: | :--------------------: |:--------------------: | :--------------------: |:--------------------: |
| IT | Ivanov | 100 | 600 | 100 |
| IT | Petrov | 200 | 600 | 300 |
| IT | Sidorov | 300 | 600 | 600 |
| Marketing | Smirnova | 100 | 300 | 300 |
| Marketing |  Savelyeva | 200 | 300 | 200 |
