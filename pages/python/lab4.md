---
layout: page
title:  Лабораторная работа 4
published: true
description: Задание для лабораторной работы 4.
---

Дана клетка с координатами x, y. Построить список пар координат, смежных клеток. Координаты смежных клеток отличаются от координат x, y не больше чем на 1.

~~~python

x, y = 2, 3

# Смещения 
dxdy = [ ( 1, 1), ( 0, 1), (-1, 1), 
         (-1, 0), (-1,-1), ( 0,-1),
         ( 1,-1), ( 1, 0) ]

# Пустой список, в который будут добалятся координаты смежных клеток
nearest = list()

# Для каждого элемента из списка смещений
for (dx,dy) in dxdy:
    nearest_cell = (x+dx, y+dy)
    nearest.append(nearest_cell)
    
print(nearest)
~~~

## Задание 4.1 

Для заданной колонии клеток, например

~~~python
colony = [ (1, 1), (1, 2), (1, 3) ]
~~~

и определенного при помощи предыдущей программы списка смежных клеток, получить список пар координат (кортежей) занятых клеток из числа смежных, т.е. определить список соседей клетки с координатами x и y  

~~~python
x, y = 2, 3

neighbours = list()

#
# Здесь ваш код
#
        
print(neighbours)
~~~

Как определить количество соседей?

## Задание 4.2

Для заданной колонии клеток, например

~~~python

colony = [ (1, 1), (1, 2), (1, 3) ]

~~~
определите список координат клеток (без повторений), составляющих ареал колонии. В ареал колонии входят все живые клетки колонии и клетки смежные живым клеткам колонии. 

~~~python

# В ареал входят все клетки колонии 
area = list(colony)

# и клетки смежные клеткам колонии 

for cell in colony:
    for (dx,dy) in dxdy:
        x = cell[0]
        y = cell[1]
        nearest_cell = (x+dx, y+dy)
        if nearest_cell not in area:
            # Ваш код
            # Ваш код
            
print('Колония ', colony)
print('Ареал колонии ', area)

~~~




