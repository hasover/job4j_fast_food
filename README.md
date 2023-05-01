# job4j_fast_food
[![Build Status](https://app.travis-ci.com/hasover/job4j_fast_food.svg?branch=master)](https://app.travis-ci.com/hasover/job4j_fast_food)

* [Описание](#описание)
* [Технологии](#технологии)
* [Функционал](#функционал)
* [Запуск](#запуск)
* [Контакты](#контакты)

## Описание
Проект по заказу и доставке еды, который состоит из нескольких сервисов. 
Каждый сервис представляет собой отдельное Spring boot приложение.
Доменные модели, используемые в проекте, находятся [здесь](https://github.com/hasover/job4j_fast_food_domain).
Сервисы проекта job4j_fast_food:
- Admin - админка. Представляет собой web-приложение, где можно составлять каталог блюд, редактировать наименования и цены.
[Подробнее](https://github.com/hasover/job4j_fast_food_admin)
- Delivery - сервис доставки.Rest-сервис, через который курьеры получают заказы для доставки.
[Подробнее](https://github.com/hasover/job4j_fast_food_delivery)
- Dish - сервис блюд. Rest-сервис, через который можно добавлять новые позиции в меню, редактировать и удалять.
[Подробнее](https://github.com/hasover/job4j_fast_food_dish)
- Kitchen - сервис кухни. Приложение, которое обрабатывает созданные заказы. В случае успеха отправляет заказ в сервис доставки, в случае неудачи отменяет созданный заказ.
[Подробнее](https://github.com/hasover/job4j_fast_food_kitchen)
- Notification - сервис уведомлений. Приложение, которое обрабатывает все приходящие уведомления.
[Подробнее](https://github.com/hasover/job4j_fast_food_notification)
- Order - сервис заказов.Rest-сервис, через который можно сделать заказ, проверять/обновлять статус.
[Подробнее](https://github.com/hasover/job4j_fast_food_order)  
- Payment - сервис платежей. Rest-сервис по обработке платежей.
[Подробнее](https://github.com/hasover/job4j_fast_food_payment)

В качестве обратного прокси используется nginx:
- Запросы на http://localhost/admin перенаправляются на сервис admin
- Запросы на http://localhost/orders перенаправляются на сервис order
- Запросы на http://localhost/deliveries перенаправляются на сервис delivery

Архитектура проекта:
![alt text](https://github.com/hasover/job4j_fast_food/blob/master/images/overview.PNG)

## Технологии
* Spring Boot (Web, Data)
* Apache Kafka
* PostgreSQL
* Liquibase
* Docker
* Nginx
* Maven
* Travis CI

## Функционал

- Для оформления заказа делается post запрос на сервис order http://localhost/orders, указываются id блюд и их количество, 
customerId клиента и его счет accountId.
![alt text](https://github.com/hasover/job4j_fast_food/blob/master/images/create.PNG)
- Платежный сервис payment фиксирует покупку и обновляет баланс счета клиента.
![alt text](https://github.com/hasover/job4j_fast_food/blob/master/images/payment.PNG)
![alt text](https://github.com/hasover/job4j_fast_food/blob/master/images/balance.PNG)
- В случае успешного платежа данные отправляются в topic orders, откуда обрабатываются сервисом kitchen.
Если заказ можно приготовить, сервис kitchen отправляет данные в topic cooked_orders, которые получит сервис delivery.
![alt text](https://github.com/hasover/job4j_fast_food/blob/master/images/kitchen.PNG)
- Через сервис delivery http://localhost/deliveries курьеры могут видеть список ожидающих доставки заказов, брать заказ в работу,
а также обновлять статус заказа.
![alt text](https://github.com/hasover/job4j_fast_food/blob/master/images/delivery.PNG)
![alt text](https://github.com/hasover/job4j_fast_food/blob/master/images/delivery2.PNG)
![alt text](https://github.com/hasover/job4j_fast_food/blob/master/images/delivery3.PNG)
- Клиенты могут видеть статус заказа через сервис order
![alt text](https://github.com/hasover/job4j_fast_food/blob/master/images/status.PNG)
- Сервис notification читает входящие сообщения из topic notifications и обрабатывает их
![alt text](https://github.com/hasover/job4j_fast_food/blob/master/images/notification.PNG)

## Запуск
Приложение будет запускаться в docker. Настройки контейнеров находятся в файле `docker-compose.yml` в корне проекта.
Для запуска приложение выполните команду `docker compose up -d`.

## Контакты
telegram: @hasover 

