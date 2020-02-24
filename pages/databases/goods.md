---
layout: page
title: Пример базы данных сети магазинов бытовой техники
published: true
---

База данных продавца бытовой техники содержит информацию о продаваемых товарах, их категориях, производителях, филиалах (магазинах), поставках товаров, изменений цен на товары, покупателях и покупках. База данных состоит из 7 таблиц:
1. clients (покупатели)
1. products (товары)
1. purchases (покупки)
1. purchase_item (элемент покупки)
1. deliveries (поставки)
1. price_change (изменения цены товаров)
1. categories (категории товара)
1. manufacturers (производители товара)

[Скачать базу данных SQLite](https://drive.google.com/file/d/1hK8fEqvoLe-iY-bNqprB7l72kqoV41MN/view?usp=sharing)

## Таблицы

### Категории (categories)

| Идентификатор категории (PK) | Наименование категории |
|--------|:-----------|
|category_id|category_name|

### Производители товара (manufacturers)

В таблице содержаться идентификатор производителя товара и его наименование 

| Идентификатор производителя (PK)  |Наименование производителя| 
|--------|:-----------|
|  manufacturer_id |manufacturer_name |

### Товары (products)

В таблице содержатся идентификатор товара, его наименование, ссылка на идентификатор поставщика, ссылка на идентификатор категории, к которой принадлежит товар.

| Идентификатор товара (PK)  |Наименование товара| Идентификатор поставщика (FK) | Идентификатор категории (FK)  |
|--------|:----------:|:-----------------:|:---------------:|
|  product_id |product_name | manufacturer_id      | category_id   |

### Изменения цен на товары (price_change)

В таблице содержатся ссылка на товар, дата изменения его цены и новая цена.

| Идентификатор товара (PK) | Дата изменения цены (PK) | Новая цена  |
|--------|:----------:|:-----------------:|
|  product_id |date_price_change | new_price  |

### Филиалы (stores)

В таблице содержаться идентификаторы филиалов и их наименования.

| Идентификатор филиала (PK)  |Наименование филиала| 
|--------|:-----------|
|  store_id |store_name |

### Поставки (deliveries)

В таблице содержаться  идентификаторы поставленных товаров, филиал, куда бы поставлен товар, дата поставки и количество товара.

| Идентификатор товара (PK) |Идентификатор филиала (PK)| Дата поставки (PK) | Количество товара  |
|--------|:----------:|:-----------------:|:---------------:|
|  product_id |store_id | delivery_date      | product_count   |

### Клиенты (customers)

Таблица содержит идентификаторы и имена клиентов.

| Идентификатор клиента (PK) | ФИО клиента| 
|--------|:----------:|
|  product_id |store_id |

### Покупки (purchases)

Таблицу покупок можно представить как таблицу счетов на оплату товаров, покупаемых в рамках одной покупки (покупка характеризуется уникальным сочетанием: дата, покупатель, филиал). Таблица содержит идентификатор покупки, идентификатор покупателя, совершившего покупку, идентификатор филиала, где была совершена покупка, и дату покупки. 

| Идентификатор покупки | Идентификатор покупателя |Идентификатор филиала | Дата покупки |
|--------|:----------:|:-----------------:|:---------------:|
|  purchase_id | customer_id | store_id | purchase_date | 

### Запись в счете (purchase_item)

Таблица содержит информацию о товарах, купленных в рамках одной покупки (товары в счете на оплату). Для упрощения анализа информации о покупках в таблицу введено поле цена продукта, которое заполняется автоматически на основе цены товара на момент покупки.

| Идентификатор покупки |Идентификатор товара | Количество товара | Цена товара |
|--------|:----------:|:-----------------:|:---------------:|
|  purchase_id | product_id | product_count | product_price | 


## Схема связи таблиц базы данных

![database\_folder.png](/pages/databases/bd_goods.png)

## SQL-код для создания таблиц

~~~sql

DROP TABLE IF EXISTS categories;
DROP TABLE IF EXISTS manufacturers;
DROP TABLE IF EXISTS products;
DROP TABLE IF EXISTS price_change;
DROP TABLE IF EXISTS stores;
DROP TABLE IF EXISTS deliveries;
DROP TABLE IF EXISTS customers;
DROP TABLE IF EXISTS purchases;
DROP TABLE IF EXISTS purchase_items;

CREATE TABLE IF NOT EXISTS categories 
(
    category_id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, 
    category_name text  NOT NULL
);

CREATE TABLE IF NOT EXISTS manufacturers
(
    manufacturer_id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
    manufacturer_name TEXT NOT NULL
);

CREATE TABLE IF NOT EXISTS products
(
    product_id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
    product_name text  NOT NULL,
    manufacturer_id INTEGER  NOT NULL,    
    category_id INTEGER NOT NULL,
    FOREIGN KEY ([category_id]) REFERENCES "categories" ([category_id]) ON DELETE NO ACTION ON UPDATE NO ACTION,
    FOREIGN KEY ([manufacturer_id]) REFERENCES "manufacturers" ([manufacturer_id]) ON DELETE NO ACTION ON UPDATE NO ACTION
);

CREATE TABLE IF NOT EXISTS price_change
(
    product_id INTEGER NOT NULL,
    date_price_change integer NOT NULL,
    new_price REAL NOT NULL,      
    CONSTRAINT PK_PRICE_CHANGE PRIMARY KEY (product_id, date_price_change),  
    FOREIGN KEY ([product_id]) REFERENCES "products" ([product_id]) ON DELETE NO ACTION ON UPDATE NO ACTION   
);

CREATE TABLE IF NOT EXISTS stores
(
    store_id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
    store_name text  NOT NULL
);


CREATE TABLE IF NOT EXISTS deliveries
(    
    product_id  INTEGER NOT NULL,
    store_id INTEGER NOT NULL,
    delivery_date  INTEGER NOT NULL,
    product_count  INTEGER NOT NULL,    
    FOREIGN KEY ([product_id]) REFERENCES "products" ([product_id]) ON DELETE NO ACTION ON UPDATE NO ACTION,
    FOREIGN KEY ([store_id]) REFERENCES "stores" ([store_id]) ON DELETE NO ACTION ON UPDATE NO ACTION
);


CREATE TABLE IF NOT EXISTS customers
(
    customer_id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
    customer_name text  NOT NULL
);


CREATE TABLE IF NOT EXISTS purchases
(
    purchase_id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
    customer_id INTEGER NOT NULL,
    store_id INTEGER  NOT NULL,    
    purchase_date INTEGER NOT NULL,
    FOREIGN KEY ([customer_id]) REFERENCES "customers" ([customer_id]) ON DELETE NO ACTION ON UPDATE NO ACTION,
    FOREIGN KEY ([store_id]) REFERENCES "stores" ([store_id]) ON DELETE NO ACTION ON UPDATE NO ACTION
);

CREATE TABLE IF NOT EXISTS purchase_items
(
    purchase_id INTEGER NOT NULL,
    product_id INTEGER NOT NULL,
    product_count INTEGER NOT NULL,
    product_price REAL NOT NULL,
    CONSTRAINT PK_PURCHASE_ITEMS PRIMARY KEY (purchase_id, product_id),  
    FOREIGN KEY ([product_id]) REFERENCES "products" ([product_id]) ON DELETE NO ACTION ON UPDATE NO ACTION,
    FOREIGN KEY ([purchase_id]) REFERENCES "purchases" ([purchase_id]) ON DELETE NO ACTION ON UPDATE NO ACTION
);

~~~