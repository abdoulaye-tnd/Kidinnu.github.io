---
published: true
layout: list
title: Плоская задача теплопроводности
description: |
  Использование модуля pde-toolbox для решения плоской нестационарной задачи теплопроводности. 
tags: [matlab,термодинамика]
---

Рассматривается нестационарная задача теплопроводности. Пластина со сторонами $$a = 0.4$$ м, $$b=0.05$$ м нагревается в тонком слое толщиной $$d=1$$ мм (на верхней грани). В слое в одну секунду генерируется 80 кДж тепла.   

![2019-12-10-pde-thermal-f1.png]({{site.baseurl}}/assets/img/2019-12-10-pde-thermal-f1.png)

Температура точек пластины $$T(x,y,t)$$ есть функция времени и координат. Эта зависимость определяется дифференциальным уравнением в частных производных:

$$
    \frac{\partial T}{\partial t} = k \left( \frac{\partial^2 T}{\partial x^2} + \frac{\partial^2 T}{\partial y^2} \right) + \frac{q}{\rho c_p}
$$

где $$k$$ -- теплопроводность [Вт/(м$$ \cdot $$К)], $$\rho$$ -- плотность [кг/м$$^3$$], $$c_p$$ -- удельная теплоёмкость [Дж/(кг$$ \cdot $$K)], $$q(x,y)$$ -- функция внутренних источников тепла [Вт/(м$$^3$$)]. 

Создаем модель, указывая тип задачи: нестационарная (transient) задача теплопроводности (thermal) 

~~~matlab
% Нестационарная модель
thermalModelT = createpde('thermal','transient');
~~~


~~~matlab
% Размеры пластины
a = 0.4;
b = 0.05;
% Толищина "горячего" слоя
d = 0.001;

% Описание геометрии: Прямоугольник
g = decsg([3 4 -a/2 a/2 a/2 -a/2 0.00 0.00 b b;
           3 4 -a/2 a/2 a/2 -a/2 b b b+d b+d]');

% Модель
geometryFromEdges(thermalModelT,g);
~~~

Свойства материала пластины

~~~matlab
k   = 50;   % теплопроводность
rho = 7800; % плотность
cp  = 468;  % удельная теплоёмкость
thermalProperties(thermalModelT,'ThermalConductivity',k,'MassDensity',rho,'SpecificHeat',cp);
~~~

## Граничные условия

Для корректного определения граничных условий необходимо "видеть" геометрию тела с идентификаторами ребер и областей. 

~~~matlab
pdegplot(thermalModelT,'EdgeLabels','on','SubdomainLabels','on');
axis equal
~~~

![2019-12-10-pde-thermal-f2.png]({{site.baseurl}}/assets/img/2019-12-10-pde-thermal-f2.png)

Идентификаторы ребер (E1, 2, 3, ... 7) и областей (F1, F2) используются при задании граничных условий.    
Пластина излучает со всех своих ребер, степень черноты тела принята равной 0.8. Температура среды 4 К.

~~~matlab
% Постоянная Стефана-Больцмана
thermalModelT.StefanBoltzmannConstant = 5.670373E-8; 
% Излучение со всех ребер
% Степень черноты 0.8
% Температура среды 273 
thermalBC(thermalModelT,'Edge',[1 2 4 5 6 7],'Emissivity',0.8,'AmbientTemperature', 4);
~~~

Внутренний источник тепла в тонком слое на верхней границе тела (F2). В этом слое генерируется $$2 \cdot 10^7 \cdot 0.4 \times 0.001 = 8 $$ кДж в секунду.

~~~matlab
internalHeatSource(thermalModelT,2e7,'Face',2);
~~~

## Создание сетки

~~~matlab
generateMesh(thermalModelT,'Hmax',0.03);
pdeplot(thermalModelT); 
axis equal
title 'Сетка'
~~~

![2019-12-10-pde-thermal-f3.png]({{site.baseurl}}/assets/img/2019-12-10-pde-thermal-f3.png)

## Расчёт

Запускаем расчёт на интервале от 0 до 3600 с с шагом 1 с. 

~~~matlab
tfinal = 3600;
tlist = 0:1:tfinal;

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
pdeplot(thermalModelT,'XYData',result.Temperature(:,numel(tlist)),'Contour','on','ColorMap','hot'); hold on;
plot(0,0.000,'bo'); 
plot(0,0.051,'bo'); 
text(0,-0.010,'T2','FontSize',18);
text(0, 0.060,'T1','FontSize',18);
hold off;
set(gca,'FontSize',14,'LineWidth',1);grid on;
xlim([-0.25 0.25]);
ylim([-0.05 0.15]);
~~~

![2019-12-10-pde-thermal-f4.png]({{site.baseurl}}/assets/img/2019-12-10-pde-thermal-f4.png)

График изменения температуры на верхней и нижней грани балки.

~~~matlab
probeTemp = cell2mat(arrayfun(@(i) interpolateTemperature(result,[0.000 0.000],[0.051 0.000],i)',(1:length(tlist))','UniformOutput',false));
subplot(1,2,1);
plot(tlist,probeTemp(:,1),'r-','LineWidth',2); hold on;
plot(tlist,probeTemp(:,2),'b-','LineWidth',2); hold off;
%plot(tlist,probeTemp(:,2)-probeTemp(:,1),'b-','LineWidth',2);
set(gca,'FontSize',14,'LineWidth',1);grid on;
legend('T1','T2');
xlabel('t, c');
ylabel('Температура, K');
xlim([0,100]);
subplot(1,2,2);
plot(tlist,probeTemp(:,1),'r-','LineWidth',2); hold on;
plot(tlist,probeTemp(:,2),'b-','LineWidth',2); hold off;
%plot(tlist,probeTemp(:,2)-probeTemp(:,1),'b-','LineWidth',2);
set(gca,'FontSize',14,'LineWidth',1);grid on;
legend('T1','T2');
xlabel('t, c');
ylabel('Температура, K');
~~~

![2019-12-10-pde-thermal-f5.png]({{site.baseurl}}/assets/img/2019-12-10-pde-thermal-f5.png)

Ниже на видео показана динамика изменения температуры. Стрелками показано направление тепловых потоков. Показана температура на верхней грани, нижней грани и разница температур.


<iframe width="560" height="315" src="https://www.youtube.com/embed/_BERXTfAqos" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


Для построения видео использовался следующий код

~~~matlab
v = VideoWriter('temperature.avi');
open(v);
% Фигура
figure('Position',[100 100 1920 1080]);
hold on;
% Для каждого момента времени из таблицы результатов интегрирования
[qTx,qTy] = evaluateHeatFlux(result);
for i=1:5:size(tlist,2)
    % Очищаем рисунок
    cla;
    %[qTx,qTy] = evaluateHeatFlux(result,0.2,0.05,1:length(tlist));    
    pdeplot(thermalModelT,'XYData',result.Temperature(:,i),'Contour','on', ...
                     'FlowData',[qTx(:,i) qTy(:,i)], ...
                     'ColorMap','hot','Levels',3);    
    axis([-0.25 0.25,-0.05, 0.15]);
    Temp = interpolateTemperature(result,[0 0],[b+d 0],i);
    text(0,-0.010,sprintf('%5.1f',Temp(2)),'FontSize',20);
    text(0,+0.060,sprintf('%5.1f',Temp(1)),'FontSize',20);
    
    text(0,-0.03,sprintf('dT = %5.1f',Temp(1)-Temp(2)),'FontSize',20);
    text(-0.2, 0.12,sprintf('t = %5.1f', tlist(i)),'FontSize',20);
    set(gca,'FontSize',20);
    grid on;   
    frame = getframe(gcf);    
    writeVideo(v,frame);
end
close(v);
~~~

