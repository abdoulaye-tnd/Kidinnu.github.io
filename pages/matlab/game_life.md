---
layout: page
title:  Игра Жизнь
published: true
accent_image: /assets/img/Matlab_Logo.png
---


~~~

~~~
>>>>>>>+HEAD
айл-скриОсновной файл-скрипт (**main.m**)

~~~matlab

% Начальное состояние колонии
>>>>>>>+master
 1;
          3, 2;          
          10 10;
          10 11;
          10 12];

% Координаты вершин квадрата, изображающего клетку 
% (относительно центра клетки)
vert  = [-0.5,-0.5, 0.5,-0.5, 0.5,+0.5, -0.5,+0.5];

% границы области
xlim([-20,20]);
ylim([-20,20]);

% количество поколений
n_gen = 50;

% Для каждого поколения
for i=1:n_gen    
    % очистить изображение
    cla;
    % получить список клеток для следующего поколения
    cells = next_generation(cells);
    % сформировать список полигонов для функции patch 
    cells_vertices = repmat(cells,1,4)+repmat(vert,size(cells,1),1);
    x = (cells_vertices(:,1:2:end))';
    y = (cells_vertices(:,2:2:end))';    
<<<<<<< HEAD
    % нарисовать клетки
    patch(x,y,'r');
    % включить сетку
    grid on;
    % включить рамку
    box on;
    % сохранить кадр
    getframe;
    % подождать 0.2 с
    pause(0.2);
end
~~~

Файл-функция **next_generation.m**

Функция **next_generation** возвращает следующее поколение для **colony** 

~~~matlab
function next_gen = next_generation(colony)
    
    next_gen = [];
    area = get_colony_area(colony);
    for i=1:size(area,1)
        cell = area(i,:);
        n = count_cell_neighbours(cell, colony);
        if n == 3 || (n == 2 && cell_in_colony(cell, colony))
            next_gen = [next_gen; cell];
        end
        
    end
~~~

Файл-функция **get_colony_area.m**

Функция **get_colony_area**, возвращающая ареал колонии **colony**

~~~matlab
%
% Список клеток, принадлежащих колонии и смежных с клетками колонии
%
function cells = get_colony_area(colony)

    cells = colony;
    
    for i=1:size(colony,1)
        cells = union(cells, get_neighbours_cells( colony(i,:) ), 'rows');
    end
~~~

Файл-функция **count_cell_neighbours.m**

Функция **count_cell_neighbours** определяет количество соседей у клетки **cell** в колонии **colony** 

~~~matlab
%
% Количество соседей у клетки с координатами cell = [x, y]
%
function count = count_cell_neighbours(cell, colony)
    
    neighbours = get_neighbours_cells(cell);
    
    count = 0;
    
    for i=1:size(neighbours,1)
        
        if cell_in_colony(neighbours(i,:), colony)
            count = count + 1;
        end
        
    end
~~~

Файл-функция **get_neighbours_cells.m**

Функция **get_neighbours_cells** Координаты смежных (ближайших восьми) клеток для клетки  **cell**.

~~~matlab
%
% Координаты ближайших (смежных) клеток для клетки cell = [x, y]
%
function neighbours = get_neighbours_cells(cell)
    
    neighbours = [-1 -1; 0 -1; 1 -1;
        -1  0;       1  0;
        -1  1; 0  1; 1  1];
    
    neighbours = repmat(cell,8,1) + neighbours;
~~~   

Файл-функция **cell_in_colony.m**

Функция **cell_in_colony** определяет принадлежность клетки **cell** колонии **colony**. Для определения принадлежности клетки **cell** колонии **colony**, список пар координат **colony** рассматривается как множество пар координат и определяется пересечение этого множество с множеством, состоящем из одной пары координат -- клетки **cell**. Если результатом будет пустое множество, то клетки **cell** в колонии нет.

~~~matlab
%
% Принадлежит ли клетка cell колонии
%
function res = cell_in_colony(cell, colony)
    % Пересечение множества colony с множеством cell
    сс = intersect(cell,colony,'rows')
    % Если результат это пустое множество, 
    % то слетки cell в колонии colony нет
    res = ~isempty(сс);        
end
~~~