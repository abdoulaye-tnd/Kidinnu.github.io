---
published: true
layout: list
title: Плоская задача теплопроводности
tags: [matlab,термодинамика]
---

Использование модуля pde-toolbox для решения плоской нестационарной задачи теплопроводности. 

![2019-12-10-pde-thermal-f1.jpg]({{site.baseurl}}/assets/img/2019-12-10-pde-thermal-f1.jpg)

Температура точек $$T(x,y,t)$$ однородной пластины определяется дифференциальным уравнением в частных производных

$$
    \frac{\partial T}{\partial t} = k \left( \frac{\partial^2 T}{\partial x^2} + \frac{\partial^2 T}{\partial y^2} \right) + \frac{q}{\rho c_p}
$$

где $$k$$ -- теплопроводность [Вт/(м$$ \cdot $$К)], $$\rho$$ -- плотность [кг/м$$^3$$], $$c_p$$ -- удельная теплоёмкость [Дж/(кг$$ \cdot $$K)], $$q(x,y)$$ -- функция внутренних источников тепла [Вт/(м$$^3$$)]. 


~~~matlab
% Нестационарная модель
thermalModelT = createpde('thermal','transient');

% Описание геометрии: Прямоугольник
% 3 4 x1 x2 x3 x4 y1 y2 y3 y4
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

Внутренний источник тепла в тонком слое на верхней границе тела (F2). В этом слое генерируется $$2 \cdot 10^7 \cdot 0.4 \times 0.01 = 80 $$ кДж в секунду.

~~~matlab
internalHeatSource(thermalModelT,2e7,'Face',2);
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
pdeplot(thermalModelT,'XYData',result.Temperature(:,end),'Contour','on','ColorMap','hot'); hold on;
plot(0,0.00,'bo'); 
plot(0,0.11,'bo'); 
text(0,-0.01,'T2','FontSize',18);
text(0, 0.12,'T1','FontSize',18);
hold off;
set(gca,'FontSize',14,'LineWidth',1);grid on;
xlim([-0.25 0.25]);
ylim([-0.05 0.15]);
~~~

![2019-12-10-pde-thermal-f4.png]({{site.baseurl}}/assets/img/2019-12-10-pde-thermal-f4.png)

График изменения температуры на верхней и нижней грани балки.

~~~matlab
probeTemp = cell2mat(arrayfun(@(i) interpolateTemperature(result,[0 0],[0.11 0],i)',(1:length(tlist))','UniformOutput',false));
plot(tlist,probeTemp(:,1),'r-','LineWidth',2); hold on;
plot(tlist,probeTemp(:,2),'b-','LineWidth',2); hold off;
set(gca,'FontSize',14,'LineWidth',1);grid on;
legend('T1','T2');
xlabel('t, c');
ylabel('Температура, K');
~~~

![2019-12-10-pde-thermal-f5.png]({{site.baseurl}}/assets/img/2019-12-10-pde-thermal-f5.png)

Ниже на видео показана динамика изменения температуры. Стрелками показано направление тепловых потоков. Показана температура на верхней грани, нижней грани и разница температур.

<iframe width="640" height="360" src="https://www.youtube.com/embed/C9y7O58UQQQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>