---
layout: page
title:  Кратчайший путь
published: true
---

1. Дорожная сеть описывается графом. Вершины графа -- населенные пункты, дуги -- дороги, сединяющие населенные пункты. Для каждой дуги графа задан её вес (целое положительное число) или путь между двумя вершинами (городами), которые соединяет дуга (дорога). Написать программу поиска кратчайшего пути из вершины $$i$$ графа к вершине $$j$$.

2. Исходные данные о сети дорог должны загружаться из текстового файла, в котором должно содержаться не менее 20 населенных пунктов. 

~~~

# Пункты
1 Москва
2 Самара
3 Уфа
...

# Дороги
1-2 1000
1-3 458
...

~~~

3. Начальная и конечная точки маршрута должны задаваться в диалоговом режиме

~~
Введите пункт отправления: Самара
Введите пункт назначения: Владивосток 
~~

4. Результат должен выводится на экран

~~~
Самара-Уфа-...Владивосток : **** км.
~~~