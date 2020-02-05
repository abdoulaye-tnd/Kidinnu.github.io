---
layout: page
title: Основы работы c SQLite
published: true
description: Работа в Google Colab с базой данных SQLite, расположенной на диске Google Drive 
---

## Доступ к Google Drive для хранения базы данных

Для работы в Google Colab с файлами на диске Google Drive необходимо разрешить доступ. Для этого нужно выполнить следующий код 

~~~python
from google.colab import drive
drive.mount('/content/drive')
~~~

Google Colab предложит перейти по предложенной ссылке для разрешения доступа к диску 

![database_folder.png](/pages/databases/auth_gd_1.png)

После того, как вы разрешите доступ к диску Google Drive, появится окно с кодом, который нужно вставить в показанное выше поле "Enter your authorization code":

![database_folder.png](/pages/databases/auth_gd_2.png)

После ввода кода и нажатия Enter появится сообщение о том, что диск подключен "Mounted at /content/drive/".

![database_folder.png](/pages/databases/auth_gd_3.png)

## Подключение к базе данных

Подключаем модуль для работы с базой SQLite

~~~python
import sqlite3
~~~

## Подключение к базе данных

Создаем на диске Google каталог databases. Указываем путь к будущей базе данных.

![database_folder.png](/pages/databases/database_folder.png)

~~~python 
path = "./drive/My Drive/databases/mydatabase.db"
~~~

Подключаемся к базе mydatabase.db. Если этого файла нет в каталоге, то он будет создан файл с таким именем. 

~~~python
conn = sqlite3.connect(path)
~~~

## Создание таблицы

Создадим таблицу музыкальных альбомов (albums) с пятью столбцами:

| title           |artist      | release_date | publisher     | media_type |
|:----------------|:-----------|:-------------|:--------------|:-----------|
| Наименование альбома   | Исполнитель | Дата выхода  | Издатель      | Тип носителя |

~~~python
# Создаем объект типа cursor для доступа к данным
cursor = conn.cursor()
# Создание простейшей таблицы, все поля (столбцы) которой имеют тип text
cursor.execute("CREATE TABLE albums (title text, artist text, release_date text, publisher text, media_type text)")
# Подтверждаем изменения (обязательно)
conn.commit()
# Закрываем курсор
cursor.close()
# Закрываем соединение (рекомендуется)
conn.close()
~~~

## Добавление записей в таблицу

Добавление в таблицу двух строк:

~~~python
conn = sqlite3.connect(path)
cursor = conn.cursor()

sql  = "INSERT INTO albums VALUES (?, ?, ?, ?, ?)"
val1 = ("The Serpen't Egg", "Dead Can Dance", "1988", "4AD", "CD")
val2 = ("Everyday Is Christmas", "SIA", "2017", "Atlantic", "CD")
cursor.execute(sql, val1)
cursor.execute(sql, val2)
conn.commit()
cursor.close()
conn.close()
~~~

## Запрос данных из таблицы

~~~python
conn = sqlite3.connect(path)
cursor = conn.cursor()

sql  = "SELECT artist, title, release_date from albums"
cursor.execute(sql)

for i in cursor:
    print(i)

cursor.close()
conn.close()
~~~

Для наглядного представления табличных данных можно использовать библиотеку pandas:

~~~python
import pandas as pd

conn = sqlite3.connect(path)
cursor = conn.cursor()

sql  = "SELECT artist, title, release_date from albums"
cursor.execute(sql)

# Загружаем все результаты в список списков rows 
rows = cursor.fetchall()
cursor.close()
conn.close()
~~~

Создаем объект DataFrame на основе списка rows, указывая наименования столбцов (columns=...):

~~~python
pd.DataFrame( rows, columns=('Исполнитель', 'Альбом', 'Год') )
~~~

Результат будет выглядеть так:

![database_folder.png](/pages/databases/panda_table_res.png){:.lead data-width="200px"}

