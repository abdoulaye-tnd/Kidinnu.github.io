---
layout: page
title:  Игра Жизнь
published: true
accent_image: /assets/img/Matlab_Logo.png
---


~~~
cells = [ 1, 1; 
          1, 2;
          1, 3;
          2, 1;
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
