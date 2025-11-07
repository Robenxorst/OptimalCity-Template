WITH filtering AS (
    SELECT *
    FROM v_xml_cdr
    WHERE
        (:domain_authorized = '' OR domain_name = :domain_authorized)
        AND start_stamp > :t_from
        AND start_stamp < :t_to
        AND duration > 3
        AND LENGTH(caller_id_number) > 4
),
last_records AS (
    SELECT f.*
    FROM filtering f
    INNER JOIN (
        SELECT var_1, MAX(id) AS max_id
        FROM filtering
        GROUP BY var_1
    ) AS last_ids
        ON f.id = last_ids.max_id
)
SELECT
    COUNT(DISTINCT f.var_1) AS "Всего обзвонили, шт.",
    COUNT(*) FILTER (WHERE dl_result IN ('Автосекретарь/Автоответчик', 'Автосекретарь', 'Автоответчик')) AS "Автосекретарь/Автоответчик , шт.",
    COUNT(*) FILTER (WHERE dl_result = 'Сброс') AS "Сброс , шт.",
    COUNT(*) FILTER (WHERE dl_result = 'Неэффективный') AS "Неэффективный, шт.",
    COUNT(*) FILTER (WHERE dl_result = 'Иное') AS "Иное, шт.",
    COUNT(*) FILTER (WHERE dl_result = 'Молчание') AS "Молчание, шт.",
    COUNT(*) FILTER (WHERE dl_result = 'Перезвон') AS "Перезвон, шт.",
    COUNT(*) FILTER (WHERE dl_result = 'Оповещен') AS "Оповещен, шт.",
    COUNT(*) FILTER (WHERE dl_result = 'Запись перенесена или отменена') AS "Запись перенесена или отменена, шт.",
    COUNT(*) FILTER (WHERE dl_result = 'Неопределившийся') AS "Неопределившийся, шт.",
    COUNT(*) FILTER (WHERE dl_result = 'Не звонить') AS "Не звонить, шт.",
    COUNT(*) FILTER (WHERE dl_result = 'Не записывался') AS "Не записывался, шт."
FROM last_records f
OFFSET :start LIMIT 1;
