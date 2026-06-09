## Как работает COUNT. Пример в ClickHouse

Попробуем на примере посмотреть, как и почему отрабатывает COUNT в разных своих версиях.

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

    
| id1 | id2  |
| :--------------------: | :--------------------: |
|  NULL |  1 |
| 2 | 2 |
| 3 |  NULL |
|  NULL |  NULL |
| 2 |  5 |


### COUNT

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
    select 
    		count(*), 				--5
            count(1),				--5
            count(id1), 			--3
            count('id1'),			--5
            count(null),			--0
            count(distinct id1),	--2
            min(id1),				--2
            max(id1),				--3
            sum(id1),				--7
            sum(id1+id2),			--11
            avg(id1),				--2,3333333
            sum(1)					--5
    from t1

| id1 | id2  |
| :--------------------: | :--------------------: |
|  NULL |  1 |
| 2 | 2 |
| 3 |  NULL |
|  NULL |  NULL |
| 2 |  5 |

| функция | результат  |
| :--------------------: | :--------------------: |
| count(*)|  5 |
| count(1) | 5 |
| count(id1) |  3 |
| count('id1') |  5 |
| count(null) |  0 |
| count(distinct id1) | 2 |
| min(id1) |  2 |
| max(id1) | 3 |
| sum(id1) | 7 |
| sum(id1+id2) | 11 |
| avg(id1) | 2,3333333 |
| sum(1) | 5 |



