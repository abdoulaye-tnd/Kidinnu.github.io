---
layout: page
title:  Лабораторная работа 8
published: true
---

Объявлен класс **Point** -- точка на плоскости

~~~python
class Point:
    def __init__(self, x, y):
        ''' Конструктор 
        x - координата x точки
        y - координата y точки
        '''
        self.x = x
        self.y = y
~~~

и класс **Shape** -- фигура

~~~python
class Shape:
    def __init__(self, x, y):
        ''' Конструктор 
        x - координата x центра окружности
        y - координата y центра окружности
        '''
        self.x = x
        self.y = y
~~~

Создайте класс **Rectangle** на основе класса **Shape**. Представитель класса **Rectangle** создаётся при помощи конструкутора с четырьмя параметрами, задающими положение центра прямоугольника его ширину и высоту. 

~~~python
class Rectangle(Shape):
    def __init__(self, x, y, a, b):
        super().__init__(x,y)
        self.a = a
        self.b = b

~~~

Добавьте к классу **Rectangle** метод **area**, который вычисляет и возвращает площадь прямоугольника. 

~~~python
class Rectangle(Shape):
    
    def __init__(self, x, y, a, b):
        super().__init__(x,y)
        self.a = a
        self.b = b
    
    def area(self):
        return self.a*self.b

R = Reactangel(0,0,3,2)
print(R.area())
~~~
~~~
> 6
~~~

Добавьте к классу **Rectangle** метод **perimeter**, который вычисляет и возвращает периметр прямоугольника. 

~~~python
R = Reactangel(0,0,3,2)
print(R.perimeter())
~~~
~~~
> 10
~~~

Создайте класс **Circle** на основе класса **Shape**. Параметры конструктора для создания экземляра класса **Circle** -- положение центра окружности и её радиус. 

~~~python
C = Circle(0,0,4)
~~~

Добавьте к классу **Circle** метод **area**, который вычисляет и возвращает площадь, ограниченную окружностью. 

~~~python
C = Circle(0,0,4)
print(C.area)
~~~

Добавьте к классу **Circle** метод **perimeter**, который вычисляет и возвращает длину окружности. 

~~~python
C = Circle(0,0,4)
print(C.perimeter)
~~~

Создайте класс **Ellipse** на основе класса **Shape**. Параметры конструктора для создания экземляра класса **Ellipse** -- положение центра, большая и малая полуось. 

~~~python
E = Ellipse(0,0,4,2)
~~~

Добавьте к классу **Ellipse** метод **area**, который вычисляет и возвращает площадь, ограниченную эллипсом. 

~~~python
E = Ellipse(0,0,4,2)
print(E.area())
~~~

Создайте класс отрезок -- **Line**. Параметры конструктора для создания экземляра класса **Line** -- две точки -- объекты типа **Point**. 

~~~python
L = Line( Point(0,0), Point(1,1) )
~~~

Добавьте к классу **Line** метод **length**, который вычисляет и возвращает длину отрезка. 

~~~python
L = Line( Point(0,0), Point(1,1) )
print(L.length())
~~~

Напишите функцию **intersect** с двумя аргументами типа **Line**. Функция должна возвращать пустой кортеж, если отрезки не пересекаются, или кортеж с одним элементом типа **Point** -- точкой пересечения отрезков.

~~~python
L1 = Line( Point(0, 0), Point(5,1) )
L2 = Line( Point(0,-3), Point(5,6) )

print( intesect(L1, L2) )
~~~

В классе **Rectangle** определите **get** и **set** методы для всех свойств: 
- **get_x**, **set_x**
- **get_y**, **set_y**
- **get_a**, **set_dx**, 
- **get_b**, **set_b**.

~~~python
R = Rectangle(0,0,1,3)
R.set_a(4)
print( R.get_a() )
~~~

Определите метод **get_area**, возвращающий площадь прямоугольника.

~~~python
R = Rectangle(0,0,1,3)
print( R.get_area() )
~~~

Определите метод **get_vertices**, который возвращает список пар координат вершин прямоугольника

~~~
    ((x1, y1), (x2, y2), (x3,y3), (x4, y4))
~~~

Перепишите класс **Rectangle**, защитив свойства (x, y, a, b) прямоугольника от прямого изменения.

~~~python
R = Rectangle(0,0,1,3)
R.a = 3
~~~

Перепишите класс **Rectangle**, используя синтаксис **property** для чтения и изменения свойств.

~~~python
R = Rectangle(0,0,1,3)
R.a = 2
print(R.area)
~~~

Перепишите класс **Rectangle**, используя синтаксис **@property** для чтения и изменения свойств, включая площадь прямоугольника.

~~~python
R = Rectangle(0,0,1,3)
R.a = 2
print(R.area)
~~~
