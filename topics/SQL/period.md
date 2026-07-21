## Задача про начало и конец периода

Предположим, перед нами ежемесячный отчёт. В нём - срезы сегментов клиентов.
Нужно по каждому клиенту получить периоды действия каждого сегмента.

Главное:
- делаем флаг начала нового периода (как 1 и 0)
- делаем условный id периода (суммируем с накоплением 1 из предыдущего шага)
- вычисляем старт и конец периода как min и max даты, группируя данные по id клиента, сегмента (то, что было в данных) и id периода (то, что вычислили на предыдущем шаге)

Создаём таблицу:
  
      with segment as (
    	select '2025-01-31' as date, 'A1' as client_id, 2 as segment_id
        union all
        select '2025-02-28', 'A1', 2
        union all
        select '2025-03-31', 'A1', 1
        union all
        select '2025-04-30', 'A1', 1
        union all
        select '2025-05-31', 'A1', 2
        union all
        select '2025-06-30', 'B2', 1
    )
    select *
    from segment
    order by 1,2

    
Данные выглядят следующим образом:
| date | client_id | segment_id |
| :--------------------: | :--------------------: |:--------------------: |
| 2025-01-31 | A1 | 2 | 
| 2025-02-28 | A1 | 2 |
| 2025-03-31 | A1 | 1 |
| 2025-04-30 | A1 | 1 |
| 2025-05-31 | A1 | 2 |
| 2025-06-30 | B2 | 1 |

Добавляем к этим данные значения из предыдущих строк

    with segment as (
    	select '2025-01-31' as date, 'A1' as client_id, 2 as segment_id
        union all
        select '2025-02-28', 'A1', 2
        union all
        select '2025-03-31', 'A1', 1
        union all
        select '2025-04-30', 'A1', 1
        union all
        select '2025-05-31', 'A1', 2
        union all
        select '2025-06-30', 'B2', 1
    ),
    prev_data as (
        select
        	date,
            client_id,
            segment_id,
            lagInFrame(date) over (partition by client_id order by date) as prev_date,
            lagInFrame(segment_id) over (partition by client_id order by date) as prev_segment_id
        from segment
    )
    select *
    from prev_data
    order by 1,2
Данные выглядят следующим образом:
| date | client_id | segment_id | prev_date | prev_segment_id |
| :----: | :----: | :----: | :----: | :----: |
| 2025-01-31 | A1 | 2 |            | 0 |
| 2025-02-28 | A1 | 2 | 2025-01-31 | 2 |
| 2025-03-31 | A1 | 1 | 2025-02-28 | 2 |
| 2025-04-30 | A1 | 1 | 2025-03-31 | 1 |
| 2025-05-31 | A1 | 2 | 2025-04-30 | 1 |
| 2025-06-30 | B2 | 1 |            | 0 |

Выделяем флаг старта нового периода:

    with segment as (
    	select '2025-01-31' as date, 'A1' as client_id, 2 as segment_id
        union all
        select '2025-02-28', 'A1', 2
        union all
        select '2025-03-31', 'A1', 1
        union all
        select '2025-04-30', 'A1', 1
        union all
        select '2025-05-31', 'A1', 2
        union all
        select '2025-06-30', 'B2', 1
    ),
    prev_data as (
        select
        	date,
            client_id,
            segment_id,
            lagInFrame(date) over (partition by client_id order by date) as prev_date,
            lagInFrame(segment_id) over (partition by client_id order by date) as prev_segment_id
        from segment
    ),
    period_start_flag as (
        select
            *,
            -- Начало нового периода, если:
            -- 1) это первая строка клиента, или
            -- 2) сегмент изменился
            or(
            	prev_date is null,
             	segment_id != prev_segment_id
            ) as new_period_flag
        from prev_data
    )
    select * 
    from period_start_flag
    order by 1,2
Данные выглядят следующим образом:
| date | client_id | segment_id | prev_date | prev_segment_id | new_period_flag |
| :----: | :----: | :----: | :----: | :----: | :----: |
| 2025-01-31 | A1 | 2 |            | 0 | 1 |
| 2025-02-28 | A1 | 2 | 2025-01-31 | 2 | 0 |
| 2025-03-31 | A1 | 1 | 2025-02-28 | 2 | 1 |
| 2025-04-30 | A1 | 1 | 2025-03-31 | 1 | 0 |
| 2025-05-31 | A1 | 2 | 2025-04-30 | 1 | 1 |
| 2025-06-30 | B2 | 1 |            | 0 | 1 |

Добавляем параметр периода:

    with segment as (
    	select '2025-01-31' as date, 'A1' as client_id, 2 as segment_id
        union all
        select '2025-02-28', 'A1', 2
        union all
        select '2025-03-31', 'A1', 1
        union all
        select '2025-04-30', 'A1', 1
        union all
        select '2025-05-31', 'A1', 2
        union all
        select '2025-06-30', 'B2', 1
    ),
    prev_data as (
        select
        	date,
            client_id,
            segment_id,
            lagInFrame(date) over (partition by client_id order by date) as prev_date,
            lagInFrame(segment_id) over (partition by client_id order by date) as prev_segment_id
        from segment
    ),
    period_start_flag as (
        select
            *,
            -- Начало нового периода, если:
            -- 1) это первая строка клиента, или
            -- 2) сегмент изменился
            or(
            	prev_date is null,
             	segment_id != prev_segment_id
            ) as new_period_flag
        from prev_data
    ),
    period_table as (
        select
            *,
            sum(new_period_flag) over (partition by client_id order by date
            ) as period_id
        from period_start_flag 
    ) 
    select * 
    from period_table
    order by 1,2
Данные выглядят следующим образом:
| date | client_id | segment_id | prev_date | prev_segment_id | new_period_flag | period_id |
| :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| 2025-01-31 | A1 | 2 |            | 0 | 1 | 1 |
| 2025-02-28 | A1 | 2 | 2025-01-31 | 2 | 0 | 1 |
| 2025-03-31 | A1 | 1 | 2025-02-28 | 2 | 1 | 2 |
| 2025-04-30 | A1 | 1 | 2025-03-31 | 1 | 0 | 2 |
| 2025-05-31 | A1 | 2 | 2025-04-30 | 1 | 1 | 3 |
| 2025-06-30 | B2 | 1 |            | 0 | 1 | 1 |

Вычисляем начало и конец периода, группируя данные по:
- клиенту
- сегменту
- периоду

      with segment as (
      	select '2025-01-31' as date, 'A1' as client_id, 2 as segment_id
          union all
          select '2025-02-28', 'A1', 2
          union all
          select '2025-03-31', 'A1', 1
          union all
          select '2025-04-30', 'A1', 1
          union all
          select '2025-05-31', 'A1', 2
          union all
          select '2025-06-30', 'B2', 1
      ),
      prev_data as (
          select
          	date,
              client_id,
              segment_id,
              lagInFrame(date) over (partition by client_id order by date) as prev_date,
              lagInFrame(segment_id) over (partition by client_id order by date) as prev_segment_id
          from segment
      ),
      period_start_flag as (
          select
              *,
              -- Начало нового периода, если:
              -- 1) это первая строка клиента, или
              -- 2) сегмент изменился
              or(
              	prev_date is null,
               	segment_id != prev_segment_id
              ) as new_period_flag
          from prev_data
      ),
      period_table as (
          select
              *,
              sum(new_period_flag) over (partition by client_id order by date
              ) as period_id
          from period_start_flag 
      ) 
      select 
          client_id,
          segment_id,
          min(date) as start_date,
          max(date) as end_date
      from period_table
      group by
          client_id,
          segment_id,
          period_id
      order by 1,3

Данные выглядят следующим образом:
| client_id | segment_id | start_date | end_date |
| :----: | :----: | :----: | :----: |
| A1 |	2	 | 2025-01-31	 | 2025-02-28 |
| A1 |	1	 | 2025-03-31	 | 2025-04-30 |
| A1 |	2	 | 2025-05-31	 | 2025-05-31 |
| B2 |	1	 | 2025-06-30	 | 2025-06-30 |
