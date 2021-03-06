---
published: false
layout: list
title: Формы уравнений относительного орбитального движения.
---

## Система координат

Рассматривается движение космического аппарата (КА) относительно орбитальной станции движущейся по круговой или эллиптической орбите. Положение КА относительно станции определяется в орбиальной подвижной системе координат $$Oxyz$$, связанной с центром масс станции. Ось $$Ox$$ направлена по направлению  радиус вектора станции относительно центра Земли, ось $$Oy$$  лежит в плоскости орбиты станции и направлена в направлении орбитального движения, ось $Oz$ дополняет систему координат $$Oxyz$$ до правой.

## Нелинейные уравнений

$$
\left\{
\begin{aligned}
& \ddot x - 2 \dot{\vartheta}_0 \dot y - \ddot{\vartheta}_0 y - \dot{\vartheta}^2_0 x = - \frac{\mu(r_0+x)}{[(r_0+x)^2+y^2+z^2]^{3/2}} + \frac{\mu}{r_0^2} \\
& \ddot y + \dot{\vartheta}_0 \dot x + \ddot{\vartheta}_0 x - \dot{\vartheta}^2_0 y = - \frac{\mu y}{[(r_0+x)^2+y^2+z^2]^{3/2}}\\
& \ddot z = - \frac{\mu z}{[(r_0+x)^2+y^2+z^2]^{3/2}}
\end{aligned}
\right.
$$

[Код на языке Python]

## Уравнения для круговой орбиты

При движении станции по круговой орбите $$\dot{\vartheta}_0 = n_0 = \text{const}, \, \ddot{\vartheta}_0  = 0, \, r_0 = a_0 = \text{const}$$

$$
\left\{
\begin{aligned}
& \ddot x - 2 n_0 \dot y - n^2_0 x = - \frac{\mu(a_0+x)}{[(a_0+x)^2+y^2+z^2]^{3/2}} + \frac{\mu}{a_0^2} \\
& \ddot y + n_0 \dot x - n^2_0 y = - \frac{\mu y}{[(a_0+x)^2+y^2+z^2]^{3/2}}\\
& \ddot z = - \frac{\mu z}{[(a_0+x)^2+y^2+z^2]^{3/2}}
\end{aligned}
\right.
$$

[Код на языке Python]

## Линеаризованные уравнения для круговой орбиты

Для малого в сравнении с радиусом орбиты расстояния между станцией и КА уравнения линеаризуются.

$$
-
$$

[Код на языке Python]

## Линеаризованные уравнения для круговой орбиты в безразмерной форме

В качестве независимой переменной можно использовать угол истинной аномалии $$\vartheta$$. Учитывая, что для круговой орбиты дифференциал времени и дифференциал угла истинной аномалии связаны отношением
$$
  d \vartheta = n dt
$$
где $$n$$ -- среднее движение.

[Код на языке Python]

## Линеаризованные уравнения для эллиптической орбиты

[Код на языке Python]
