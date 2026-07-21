## Задача про начало и конец периода

Предположим, перед нами ежемесячный отчёт. В нём - срезы сегментов клиентов.
Нужно по каждому клиенту получить периоды действия каждого сегмента.

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
