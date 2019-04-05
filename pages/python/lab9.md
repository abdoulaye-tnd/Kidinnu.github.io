---
layout: page
title:  Лабораторная работа 9
published: true
---

## Задание 1

Напишие функцию, выполняющую вычисление следа квадратной матрицы. След матрицы -- это сумма произведений её диагональных элементов.

~~~python

import numpy as np

def trace(A):
    ...
    ...
    ...
    return tr


M1 = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12.0],[1,2,3,4]])

result = trace(M1)

print(result)

~~~

## Задание 2

Напишите функцию, вычисляющую произведение матрицы на вектор

~~~python

import numpy as np

def MatrixVec(M, v):
    ...
    ...
    ...
    return res


M = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12.0]])
a = np.array([1,2,3,4.0])

result = MatrixVec(M, a)

print(result)

~~~

Проверьте результат при помощи функции **dot** библиотеки **numpy**.


## Задание 3

Напишите функцию, вычисляющую произведение двух матриц

~~~python

import numpy as np

def MatrixMatrix(A, B):
    ...
    ...
    ...
    return res


M1 = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12.0]])
M2 = np.array([[1,2],[1,2],[1,2],[1,2]])

result = MatrixMatrix(M1, M2)

print(result)

~~~

Проверьте результат при помощи функции **dot** библиотеки **numpy**.

## Задание 4

Напишие функцию, выполняющую транспонирование квадратной матрицы без создания новой матрицы

~~~python

import numpy as np

def transpose(A):
    ...
    ...
    ...
    return A


M1 = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12.0],[1,2,3,4]])

result = transpose(M1)

print(result)

~~~



