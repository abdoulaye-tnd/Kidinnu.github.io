---
layout: page
title:  Pascal
published: true
description: Структура программы, ключевые слова, типы данных
---

## Ключевые cлова

|and | end | nil | set |
|array |	file	| not |	then |
|begin	|for	|of|	to|
|const	|goto|	packed	|until|
|case|	function	|or|	type|
|div|	if|	procedure|	var|
|do|	in|	program|	while|
|downto|	label|	record|	with|
|else|	mod|	repeat|


## Структура программы

~~~pascal
Program MyFirstProgram;

  <раздел описаний>

begin

  <оператор 1>;
  <оператор 2>;
  . . . . . . .
  <оператор N>;

end.
~~~

Программа, которая выводит на экран сообщение "Здравствуй Мир!"

~~~pascal
Program HelloWorld;
Begin
   writeln('Здравствуй Мир!');
End.
~~~
