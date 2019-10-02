---
layout: page
title: Движение наноспутника внутри пускового контейнера
published: true
accent_image: /assets/img/Matlab_Logo.png
---

Наноспутник типоразмера 3U массой $$m$$ (от 2 до 6 кг) выталкивается из транспортно-пускового контейнера при помощи пружинного толкателя с известным начальным усилием $$P_0$$, конечными усилием $$P_k$$ и ходом $$h$$. 

1. Напишите MATLAB-функцию, которая определяет для заданной массы наноспутника, начального, конечного усилия пружины и её хода:
- скорость отделения наноспутника от носителя;
- продолжительной движения наноспутника внутри контейнера от момента начала движения до окончания работы пружины.
2. Постройте график перемещения наноспутника от времени. 
3. Постройте график изменения скорости наноспутника от времени.

Шаблон функции:

~~~matlab
function [dv dt] = get_v_t(...)	
	...
end 
~~~

Наноспутник выталкивается из контейнера при помощи пружины, усилие которой зависит от положения наноспутника внутри контейнера. Ход пружины обычно равен длине наноспутника. Наноспутник типоразмера 3U имеет длину 340.5 мм.

<iframe width="560" height="315" src="https://www.youtube.com/embed/THv4-rbR3-g" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Сила пружины $P$ толкателя изменяется по линейному закону: 

$$
	P = P_0 - c x, \quad 0 \leq x \leq h, 
$$

где $$P$$ -- начальное усилие пружины, $$c$$ -- жёсткость пружины: 
$$
	c = \frac{P_0 - P_k}{h}
$$

$$P_k$$ -- конечное усилие пружины. Работа пружины определяется выражением:

$$
	A = \frac{P_0 + P_k}{2} h 
$$

Если масса носителя, от которого отделяется наноспутник, значительно больше массы наноспутника, то можно принять допущение о том, что вся потенциальная энергия толкателя расходуется на приращение скорости наноспутника. В этом случае изменение кинетической энергии наноспутника равно работе пружинного толкателя:

$$
	T_k - T_0 = A
$$

где $$T_0=0$$ начальная кинетическая энергия наноспутника, $$A$$ -- работа пружинны, $$T_k$$ -- конечная кинетическая энергия наноспутника:

$$
	T_k = \frac{m V^2}{2}
$$

Скорость наноспутника определяется выражением:

$$
	V = \sqrt{\frac{2 A}{m}}. 
$$

Уравнение движения наноспутника под действием силы $$P$$ имеет вид:

$$
	m \ddot x = P_0 - c x.
$$

Это линейное неоднородное дифференциальное уравнение второго порядка с постоянными коэффициентами. Решение уравнения для нулевых начальных условий $$x(0)=0$$, $$\dot{x}(0) = 0$$ имеет вид:

$$
	x = \frac{P_0}{c}\left[1-\cos \left(\sqrt{\frac{c}{m}}t\right)\right].
$$

Время движения наноспутника до выхода из пускового контейнера $$x(t) = h$$ определяется формулой:

$$
	t = \sqrt{\frac{m}{c}} \arccos \frac{P_k}{P_0}.
$$

