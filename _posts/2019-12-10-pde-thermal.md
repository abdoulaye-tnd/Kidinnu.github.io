---
published: true
layout: list
title: Плоская задача теплопроводности
tags: [matlab,термодинамика]
---

Использование модуля pde-toolbox для решения нестационарной задачи теплопроводности.

![2019-12-10-pde-thermal-f1.jpg]({{site.baseurl}}/assets/img/2019-12-10-pde-thermal-f1.jpg)

~~~matlab
% Нестационарная модель
thermalModelT = createpde('thermal','transient');

% Описание геометрии: Прямоугольник
g = decsg([3 4 -0.2 0.2 0.2 -0.2 0 0 .1 .1;
           3 4 -0.2 0.2 0.2 -0.2 0.1 0.1 .11 .11]');
% Модель
geometryFromEdges(thermalModelT,g);
generateMesh(thermalModelT,'Hmax',0.01);

% Свойства материала
k = 50;     % Теплопроводность, W/(m-degree C)
rho = 7800; % плотность, kg/m^3
cp = 468;   % удельная теплоёмкость, W-s/(kg-degree C)

thermalProperties(thermalModelT,'ThermalConductivity',k,'MassDensity',rho,'SpecificHeat',cp);

~~~

Для корректного определения граничных условий необходимо увидеть геометрию тела с идентификаторами ребер и областей. 

~~~matlab
pdegplot(thermalModelT,'EdgeLabels','on','SubdomainLabels','on');
axis equal
~~~

![2019-12-10-pde-thermal-f2.png]({{site.baseurl}}/assets/img/2019-12-10-pde-thermal-f2.png)

Идентификаторы ребер (E1, 2, 3, ... 7) и областей (F1, F2) используются при задании граничных условий.    

~~~matlab
% Постоянная Стефана-Больцмана
thermalModelT.StefanBoltzmannConstant = 5.670373E-8; 
% Излучение со всех ребер
% Степень черноты 0.1
% Температура среды 273 
thermalBC(thermalModelT,'Edge',[1 2 4 5 6 7],'Emissivity',0.1,'AmbientTemperature',273);
~~~

Внутренний источник тепла в тонком слое на верхней границе тела (F2). В этом слое генерируется $$10^6 \cdot 0.4 \times 0.01 = 4 $$ кДж тепла в секунду.

~~~matlab
internalHeatSource(thermalModelT,1e6,'Face',2);
~~~

Создаем сетку

~~~matlab
generateMesh(thermalModelT,'Hmax',0.03);
pdeplot(thermalModelT); 
axis equal
title 'Сетка'
~~~

![2019-12-10-pde-thermal-f3.png]({{site.baseurl}}/assets/img/2019-12-10-pde-thermal-f3.png)

Запускаем расчёт на интервале от 0 до 600 с с шагом 0,1 с. 

~~~matlab
tfinal = 600;
tlist = 0:0.1:tfinal;

% Начальная температура тела
thermalIC(thermalModelT,273);

% Вывести статистическую информацию решателя
thermalModelT.SolverOptions.ReportStatistics  = 'on';

% Решаем
result = solve(thermalModelT,tlist)
~~~

## Результаты

Карта температуры в момент t=600 c.

~~~matlab
pdeplot(thermalModelT,'XYData',result.Temperature(:,end),'Contour','on','ColorMap','hot');
~~~

График изменения температуры на верхней и нижней грани балки.

~~~matlab
probeTemp = cell2mat(arrayfun(@(i) interpolateTemperature(result,[0 0],[0.01 0],i)',(1:length(tlist))','UniformOutput',false));
subplot(3,1,3);
plot(tlist,probeTemp(:,1));
hold on;
plot(tlist,probeTemp(:,2));
hold off;
~~~

