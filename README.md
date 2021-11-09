# Дневник Стажировки 
## _День 1._
 - [X] Изучила http протоколы 
 - [X] Прочитала про репозитории кода 
 - [X] Прочитала про API
##  _День 2._ 
- [X] Пробую создать локальный репозиторий 
- [X] закоммитила 4 изменения 2 способами: 
   + через программу vs code 
   + через git bash
- [X] Создание новой ветки на локальном проекте 
- [X] Изучила базовые команды в гите  `git add`  
  `git commit`  `git status` `git rm`  `git config`
##  _День 3._ 
- [X] Синхронизировала локальный репозиторий с удаленным на GitHub
- [X] Изучила новую команду `-push`
##  _День 4._ 
- [X] Работа с GitHub  
:technologist: [super-engine](https://github.com/anzhelikapim/super-engine.git)
##  _День 5._
- [X] Установка Docker для локальной раскатки проекта :whale:
- [X] Изучение программы :mag:
##  _День 6._ 
- [X] Загрузка удаленного репозитория pimpay на пк
- [X] новая команда `git clone`
##  _День 7-9._
- [ ] Раскатка проекта локально через убунту 
- [X] Погружаюсь в проект `ПочтаРу`  
- [X] Попробовала создать коллекцию в постман для [ПочтаРу](https://pochtaru.pp-task-2896.stage.pp.ppdev.ru) 
##  _День 10._ 
- [X] Попробовала отправить запросы в Postman на стейдж [ПочтаРу](https://pochtaru.pp-task-2896.stage.pp.ppdev.ru)
- [X] создание клиента [пример запроса](https://pochtaru.pp-task-2896.stage.pp.ppdev.ru/customer/QA-ANZHELIKA-TEST-1)
##  _День 11._ 
- [X] Изучала отправленный запрос 
- [Customers: Заведение клиента](https://pochtaru.pp-task-2896.stage.pp.ppdev.ru/customer/QA-ANZHELIKA-TEST-1)
- [Customers: Передача информации об истории отправок за последние два месяца](https://pochtaru.pp-task-2896.stage.pp.ppdev.ru/customer/QA-ANZHELIKA-TEST-2/history)
- [Customer: Получить статус клиента (результат финансрования)](https://pochtaru.pp-task-2896.stage.pp.ppdev.ru/customer/QA-ANZHELIKA-TEST-2)
- [Request: Согласие с финансированием](https://pochtaru.pp-task-2896.stage.pp.ppdev.ru/customer/QA-ANZHELIKA-TEST-2/request)
- [Request: Получить список запросов на финансирование по клиенту](https://pochtaru.pp-task-2896.stage.pp.ppdev.ru/customer/QA-ANZHELIKA-TEST-2/request)
- [Request: Получить статус запроса на финансирование](https://pochtaru.pp-task-2896.stage.pp.ppdev.ru/customer/QA-ANZHELIKA-TEST-2/request/QA-ANZHELIKA-LN-TEST-2)
##  _День 12-14._ 
- [X] Раскатка проекта через гит баш по [документации](http://dev-docs.pimpay.ru/dev-process/local-provision.html#)
- [X] Раскатка локально `PochtaRu` `litedb` 
- [X] Рапортовали баги по проекту `PochtaRu`
##  _День 15._  
- [X] Скачала DataGrip
- [X] Изучила прогу
- [X] Написала первые SQL запросы `SELECT` `FROM` `WHERE` `ORDER BY/DESC` `GROUP BY`
##  _День 16._ 
- [X] SQL 
  - научилась смотреть схемы связанных таблиц 
  - историю их создания
##  _День 17._
- [X] Изучаем `JOIN` `SUM` `COUNT` 
- [X] Создаю таблицу на `stage1` (почта.ру) используя `JOIN` и ранее изученные комманды
```SQL
SELECT tin, 
       count(balance_money_request_id) AS "CountBalance", sum(balance_money_request_id) AS "SumBalance", 
       count(bank_account_money_request_id) AS "CountBank", sum(bank_account_money_request_id) AS "SumBank" 
FROM pochta_ru.tbl_pochta_ru_loan_request 
INNER JOIN pochta_ru.tbl_pochta_ru_customer 
    on tbl_pochta_ru_loan_request.pochta_ru_customer_id = tbl_pochta_ru_customer.id 
GROUP BY tin
```
(не правильно выбрали параметр для суммы, далее исправленная таблица)
##  __День 18.__ 
- [X] Разбираю новое задание, где нужно джойнить еще одну таблицу 
- [X] Разбираю по схеме проекта `pimpay` по какому параметру это делать
- [X] Готовая таблица 
```SQL
SELECT tin, 
       count(balance_money_request_id) AS "CountBalance", SUM (total_to_pay) AS "SumBalance" 
FROM pochta_ru.tbl_pochta_ru_loan_request 
INNER JOIN pochta_ru.tbl_pochta_ru_customer 
    ON tbl_pochta_ru_loan_request.pochta_ru_customer_id = tbl_pochta_ru_customer.id 
INNER JOIN tbl_money_request 
ON tbl_pochta_ru_loan_request.balance_money_request_id = tbl_money_request.id 
GROUP BY  tin 
ORDER BY "SumBalance" DESC
```
##  _День 19._
- [X] Изучаем `CTE` `COALESCE` 
- [X] С их помощью делаю новое задание на добавления 3-й таблицы `pochta_ru_repayment`
- [X] Готовая таблица 
```SQL
WITH 
balance_request AS ( 
SELECT tin, 
       COUNT(balance_money_request_id) AS CountBalance, SUM (total_to_pay) AS SumBalance 
FROM pochta_ru.tbl_pochta_ru_loan_request AS loan 
INNER JOIN pochta_ru.tbl_pochta_ru_customer AS ct ON loan.pochta_ru_customer_id = loan.id 
INNER JOIN tbl_money_request AS mr ON loan.balance_money_request_id = mr.id 
GROUP BY tin 
) 
, bank_request AS ( 
SELECT tin, 
       COUNT(bank_account_money_request_id) as CountBank, SUM (total_to_pay) as SumBank 
FROM pochta_ru.tbl_pochta_ru_loan_request AS loan 
INNER JOIN pochta_ru.tbl_pochta_ru_customer AS ct ON loan.pochta_ru_customer_id = ct.id 
INNER JOIN tbl_money_request AS mr 
ON loan.bank_account_money_request_id = mr.id 
GROUP BY tin 
) 
, totals AS ( 
SELECT COALESCE(balance_request.tin, bank_request.tin) AS tin 
, COALESCE(SumBalance, 0) + COALESCE(SumBank, 0)  AS total 
FROM balance_request 
FULL JOIN bank_request USING (tin) 
ORDER BY total DESC 
) 
, repayment_pochta AS ( 
SELECT 
tin, 
SUM (sum) AS SumRepayment 
FROM pochta_ru.tbl_pochta_ru_repayment AS rp 
INNER JOIN pochta_ru.tbl_pochta_ru_customer AS ct ON rp.pochta_ru_customer_id = ct.id 
GROUP BY tin 
) 
SELECT 
  COALESCE(balance_request.tin, bank_request.tin) AS tin 
, COALESCE(SumRepayment, 0) AS SumRepayment 
, COALESCE(SumBalance, 0) + COALESCE(SumBank, 0)  AS total 
, COALESCE(SumBalance, 0) + COALESCE(SumBank, 0) - COALESCE(SumRepayment, 0) AS difference 
FROM balance_request 
FULL JOIN bank_request USING (tin) 
FULL JOIN repayment_pochta USING (tin) 
ORDER BY total DESC
```
##  _День 20._ 
- [X] Изучаем новую комманду `WHEN THEN ELSE`
- [X] Пробую использовать в готовых таблицах
```SQL
WITH orders AS (
    SELECT t.id, t.weight, t.status
    FROM (VALUES
                   (1, 100, 'delivered')
                 , (2, 200, 'intransit')
                 , (3, 300, 'prepared')
                 , (4, 400, 'stored')
                 , (5, 500, 'intransit')
         ) AS t(id, weight, status)
)
-- тяжелый вёс это когда > 250 граммов
SELECT
       id
     , weight
     , CASE
         WHEN weight <= 200 THEN 'light'
         WHEN weight <= 400 THEN 'medium'
         ELSE 'heavy'
       END AS weight_type
     , CASE status
         WHEN 'delivered' THEN 'Доставлен'
         WHEN 'stored' THEN 'В ПВЗ'
         WHEN 'intransit' THEN 'В пути'
         ELSE status || ' (нет перевода)'
       END AS StatusRus
FROM orders
```
##  _День 20._
- [X] Погружаюсь в новый проект `BlackList` изучаем api и иную документацию
##  _День 20._
- [X] Создаю READMY.md файл ["Дневник стажера"](https://github.com/anzhelikapim/TheInternDiary.git)
## _День 21._
- [X] Завожу [клиента](https://api.checkclient.ru/v11/recipient) по проетку `BlackList`
## _День 22._
- [X] Создаю раздел по проекту `PochtaRu` в [документации](http://dev-docs.pimpay.ru/index.html) локально 
- [X] Просмотрела API Boxberry (задание от Лены)
## _День 23._
- [X] Создаю новую ветку `TASK-2896-docs` на https://gitlab.pimpay.ru/
- [X] Просмотрела API (задание от Лены)
   - Pickpoint
   - TopDelivery 
   - DRH
