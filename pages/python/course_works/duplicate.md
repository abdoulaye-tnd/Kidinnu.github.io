---
layout: page
title:  Дубликаты файлов
published: true
---


Написать программу, которая сканирует заданный каталог и определяет дубли файлов изображений с расширением jpg, анализируя значения хэш-функции MD5 от содержимого файла. Каталог поиска задается параметром командной строки, например:

~~~
find_duplicates c:\MyFolder
~~~

Результат должен выводится в текстовый файл. В текстовом файле результата должны выводиться имена файлов-дублей с указанием их длины в килобайтах.

~~~
c:\dir1\file1.jpg : 100 Кб = c:\dir1\file2.jpg : 100 Кб = c:\dir1\file7.jpg : 100 Кб
c:\dir1\file3.jpg : 150 Кб = c:\dir1\file4.jpg : 150 Кб
~~~

Для вычисления хэш-функции использовать библиотеку hashlib

[Список заданий](list.md)