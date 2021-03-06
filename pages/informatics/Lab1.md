---
layout: page
title:  Лабораторная работа 1
published: true
description: Пояснения к первой лабораторной работе по курсу Информатика
---

# Задание

Составить блок-схему алгоритма и программу на языке Паскаль для вычисления значений функции  

$$
y = \frac{a^2 + \ln bx}{e^x + \cos (cx)}
$$

при заданном значении x, которое вводится с клавиатуры. Параметры функции и значение аргумента  приведены в таблице:

| Параметр | Значение |
| a | 1.5 |
| b | 2.1 |
| c | 3.22 |
| x | 1.83 |

# Текст программы

~~~pascal
program Lab1;

  var a, b, c, x, y: real;

begin
  a:=1.5;
  b:=2.1;
  c:=3.22;

  write('введите x=');
  readln(x);

  y:=(sqr(a)+ln(b*x))/(exp(x)+cos(c*x));

  writeln('при x=',x:5:2,' значение y=',y:5:2);
  readln;

end.
~~~

# Пояснения

Программа на языке Паскаль состоит из трёх основных разделов:

- заголовок
- раздел описаний
- тело программы или блок опрераторов

В рассматриваемой программе заголовок это первая строчка
~~~pascal
program Lab1;
~~~
которая говорит о том, что эта программа называется Lab1.

Следующий раздел это раздел описаний, где перечисляются используемые в программе переменные с указанием их типов (целые, вещественные, ...).
В примере это
~~~pascal
var a, b, c, x, y : real;
~~~
В программе используется 5 переменных, которым можно присвоить вещественные значения (тип real).

В разделе операторов, который начинается с ключевого слова begin и заканчивается словом end. (с точкой) записываются действия, которые выполняет программа.
В первых строчках раздела операторов, объявленным ранее переменным, присваиваются значения:
~~~pascal
a:=1.5;
b:=2.1;
c:=3.22;
~~~
Обратите внимание, что операция присвоения значения переменной записывается при помощи сиволов двоеточие и равно (a:=1.5), чтобы отличать это действие от операции сравнения значений переменных a=1.5, результатом которой будет истина или ложь.

Во второй строке вызывается функция write, которая выводит на экран значение переменной или текст, который передается этой функции в качестве аргумента:
~~~pascal
write('введите x=');
~~~
После этого вызывается функция readln(x), которая приводит к приостановке программы, пока пользователь не введет какое-то значение и не нажмет ENTER.
~~~pascal
readln(x);
~~~
Введённое пользователем значение запиcывается в переменную x.

После получения от пользователя программы значения x, можно вычислить значение функции и записать её в переменную y:
~~~pascal
y:=(sqr(a)+ln(b*x))/(exp(x)+cos(c*x));
~~~
В этой строке используются математические функции вычисления корня sqrt, натурального логарифма ln, косинуса cos и экспоненты exp. Аргументы этим функциям передаются в круглых скобках. Для тригонометрических функций предполагается, что аргумент задан в радианах.

Результат выводится на экран при помощи функции writeln, которая отличается от использованной ранее функции write тем, что после вывода на экран всех значениий, функция переводит крусор на новую строчку.
~~~pascal
writeln('при x=', x:5:2, ' значение y=', y:5:2);
~~~
Функции writeln передано четыре аргумента, которые она выведет на экран друг за другом. В начале выведется текст 'при x=' (он записан в кавычках, чтобы [транслятор](http://gos-it.wikia.com/wiki/%D0%A2%D1%80%D0%B0%D0%BD%D1%81%D0%BB%D1%8F%D1%82%D0%BE%D1%80_%D1%8F%D0%B7%D1%8B%D0%BA%D0%B0_%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F) языка Паскаль не рассматривал содержимое этой строки как часть кода порграммы. Далее выводится значение (содержимое) переменной x. Слева от переменной x через двоеточия указаны два числа -- 5 и 2, при помощи которых производится форматирование числового значения:

- для вывода используется пять позиций на экране;
- значение переменной x округляется до второго знака после запятой.

Например, если пользователь ввел значение x равное 1.15345687, то на экран будет выведено

|  | 1 | . | 1 | 5 |

Если рассматриваемую строку заменить на
~~~pascal
writeln('при x=', x, ' значение y=', y);
~~~
то на экран тоже будет выведен результат, но с большим количеством "лишних" цифр.
~~~pascal
при x= 1.1200000000000001E+000 значение y= 1.4303744323446415E+000
~~~

После значения x в той же строке выводится текст ' значение y=' и значение переменной y, с тем же форматом вывода и округления, который использовался для вывода переменной x.

Последняя функция приостанавливает выполнения программы, ожидая от пользователя нажатия кнопки ENTER, чтобы можно было прочитать результаты работы программы:
~~~pascal
readln;
~~~

Код этой же программы на языке Python 
~~~python
import math

a = 1.5
b = 2.1
c = 3.22

x = float(input('Введите значение x = '))

y=(a*a+math.log(b*x))/(math.exp(x)+math.cos(c*x))

print('при x= {} значение y = {}'.format(x,y))
~~~
