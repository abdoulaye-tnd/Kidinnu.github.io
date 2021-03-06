---
layout: page
title: Продольные колебания стержня
published: true
---

## Стержень, как система материальных точек, связанных пружинами

Представим однородный стержень постоянного сечения $$A$$ массы $$M$$ в виде системы $$n$$ материальных точек, соединенных невесомыми линейно-упругими элементами -- пружинами. Масса материальных точек:

$$
  m_i = M/n
$$

Жесткость пружин:

$$
  c_i = n \cdot \frac{EA}{L}
$$

где $$E$$ -- модуль Юнга, $$L$$ -- длина стержня.

Уравнение движения массы $$i$$ имеет вид (в проекции на горизонтальную ось $$x$$ )

$$
  m_i \ddot{x}_i = -F_i + F_{i+1}, \quad i = 1, \ldots, n-1
$$

Для последней массы ($$i=n$$)

$$
  m_n \ddot{x}_n = -F_n
$$

Силы, действующие на массы вычисляются следующим образом

$$
  F_k = c_k (x_k - x_{k-1} - l_0), \quad k=2,\ldots,n
$$

Для первой точки

$$
  F_1 = c_1 (x_1 - l_0)
$$

где $$l_0$$ - свободная длина пружины

$$
  l_0 = \frac{L}{n}
$$  

![Image](spring.png)

### Программа (файл-скрипт)

~~~matlab
% Модуль Юнга Н/м2
E  = 1000000;
% Площадь поперечного сечения стержня м2
A  = 0.001;
% Погонная масса кг/м
mu = 10.0;
% Длина стержня м
L  = 1.0;
% Масса стержня
M   = L*mu;
% Количество материальных точек, на которые разбивается стержень
N   = 30;
% Начальное расстояние между точками (l0)
p.L = L/N;
% Масса материальной точки
p.m = M/N;
% Жесткость пружин
p.c = N*E*A/L;
%
% Начальное положение точек
x0  = (1:N)'/N*(L+0.1);
% Начальная скорость
vx0 = zeros(N,1);
% Вектор состояния
q0  = [x0;vx0];

% Интегрирование
[t, q] = ode113(@(t,q) dqdt(t,q,p),0:0.002:2,q0,odeset('RelTol',1e-8));

% Вектор деформаций пружин
dx = [q(:,1)-p.L (q(:,2:N) - q(:,1:N-1))-p.L];
% Максимальная деформация
dx_max = max(max(dx));
% Минимальная деформация
dx_min = -dx_max;
% Открываем файл для записи видео
v = VideoWriter('rod_d.avi');
open(v);
% Фигура
figure('Position',[100 100 1920 1080]);
axis([0 L*1.3,-0.5, 0.5]);
set(gca,'FontSize',20);
grid on; hold on;
% Палитра
colormap('jet');
% Для каждого момента времени из таблицы результатов интегрирования
for i=1:size(t,1)
    % Очищаем рисунок
    cla;
    % Массив деформаций пружин
    dydx = ([q(i,1) diff(q(i,1:N))] - repmat(p.L,1,N));
    % Вершины прямоугольников, изображающих пружины
    % Каждый столбец содержит 4-ку координат для прямоугольника
    % Координаты x   
    vertex_x  = [0 q(i,1) q(i,1) 0; q(i,1:N-1)' q(i,2:N)' q(i,2:N)' q(i,1:N-1)'];
    % Координаты y
    vertex_y  = repmat([0.1 0.1 -0.1 -0.1],N,1);     
    % Рисуем все прямоугольники сразу
    % Третий аргумент - деформация каждой пружины для раскраски прямоугольника
    patch(vertex_x', vertex_y', dydx,'FaceColor','flat');
    % Приведение диапазона деформаций к диапазону цветов
    caxis([dx_min dx_max]);
    colorbar;
    text(0.1,0.45,sprintf('T=%5.3f c. Длина стержня L=%5.3f м',t(i),q(i,N)),'FontSize',20);   
    % График деформаций
    fplot(@(x) interp1([0 q(i,1:N)],[0 dydx]*0.1/dx_max,x,'next'),[0 q(i,N)],'k-','LineWidth',1);
    frame = getframe(gcf);
    writeVideo(v,frame);
end
close(v);
~~~

### Файл-функция правых частей дифференциальных уравнений движения системы материальных точек

~~~matlab
function dq = dqdt(t, q, p)

n  = size(q,1)/2;
x  = q(1:n);

% Расстояние между точками для определения деформаций пружин
% x1 (x2-x1) (x3-x2) (x4-x3) ... (xn - x_n-1)
xdif = [x(1);diff(x)];

% Из расстояний между точками вычитается свободная длина пружины
% определяется деформация
% x1-l0 (x2-x1)-l0 (x3-x2)-l0 (x4-x3)-l0 ... (xn - x_n-1)-l0
dx   = xdif - repmat(p.L,n,1);

% По деформации определяется сила
% F = [-(x1-l0)*c -(x2-x1-l0)*c ...]
F    = -dx*p.c;
% На все точки, кроме последей, действуют еще силы со стороны соседней пружины справа
% с противоположным знаком. На последнюю точку действует только одна пружина,
% поэтому к действующей на нее силе добавляется ноль
F    = F - [F(2:end); 0];

% Ускорения точек
a    = F/p.m;
% Результат работы функции -- скорости и ускорения точек
dq   = [q(n+1:end);a];

end
~~~

<iframe width="560" height="315" src="https://www.youtube.com/embed/NxsJKNXhRRY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Стержень, как распределённая система

Бабаков И.М. Теория колебаний. 4-е изд., испр. - М.: Дрофа, 2004. - 591 с

Упругие свободные колебания однородного стержня постоянного сечения описываются линейным уравнением в частных производных с постоянными коэффициентами

$$
  \frac{\partial^2 y}{\partial t^2} - c^2  \frac{\partial^2 y}{\partial x^2} = 0
$$

где велчиина $$c$$ представляет собой скорость распространения звука в материале стержня, зависящая от модуля упругости $$E$$ материала и его плотности:

$$
  c^2 = \frac{E \cdot A}{\mu}
$$

В последней формуле плотность материала стержня определяется отношением погонной массы $$\mu$$ к площади поперечного сечения $$A$$.

Сложные малые колебания рассматриваемой системы с бесконечным числом степеней свободы представляются в виде суммы гармонических колебаний с частотами $$p_i$$ и формами колебаний $$d_i(x)$$, определяющих распределение деформаций по длине стержня     

$$
y_i(x,t) = d_i(x) \sin (p_i \cdot t + \alpha_i)
$$

где $$d_i(x)$$ -- $$i$$-я форма главного колебания  -- амлитудное значение деформации, как функция положения точки на стержне.

Подстановка $$y(x,t) = d(x) \sin (p \cdot t + \alpha)$$ в уравнение движения приводит к уравнению собственных форм колебаний:

$$
  d''(x) + a^2 d(x) = 0
$$

где

$$
a^2 = \frac{p^2 \mu}{EA}  
$$  

Решение полученного однородного дифференциального уравнения второго порядка  имеет следующий вид:

$$
d_i(x) = B \cos a_i x + C \sin a_i x
$$

Постоянные $$B$$ и $$C$$ определяются из граничных условий -- условий закрепления стержня. Например для закрпеленного с одной стороны стержня граничные условия будут иметь вид:

$$
  d(0) = 0
$$

$$
  d'(L) = 0
$$

Подставляя эти граничные условия в решение, получим

$$
  B = 0
$$

и

$$
  C a_i \cos a_i L = 0
$$

Нетривиальное решение этого уравнения имеет вид:

$$
  \cos a_i L  = 0 \quad a_i = \frac{1+2k}{2 L} \pi, \quad k=0,1,2,\ldots,\infty
$$

Таким образом, ряд сообственных частот продольных колебаний стержня c закреплённым ($$x=0$$) будет определяться следующим образом:

$$
  p_k = \sqrt{\frac{EA}{\mu}} \frac{1+2k}{2 L} \pi, \quad k=0,1,2,\ldots,\infty
$$  

Общее решение имеет следующий вид

$$
  y(x, t) = \sum_{i=1}^{\infty} d_i(x) (M_i \cos p_i t + N_i \sin p_i t)
$$

Постоянные $$M_i$$, $$N_i$$ определяются из начальных условий -- распределения начальной деформации по длиней стержня:

$$
y(x,0) = f(x)
$$

и распределением начальных скоростей:

$$
\dot{y}(x,0) = \gamma(x)
$$

В общем случае константы $$M_i$$, $$N_i$$ определяются следующим образом:

$$
  M_i = \int_0^L d_i(x) f(x) dx
$$

$$
  N_i = \frac{1}{p_i} \int_0^L d_i(x)\gamma(x) dx
$$

~~~matlab
% Модуль Юнга Н/м2
E  = 1000000;
% Площадь поперечного сечения м2
A  = 0.001;
% Погонная масса kg/м
mu = 10.0;
% Длина стержная м
L  = 1.0;
% Количество учитываемых собственных форм
k  = 1:10;
a  = (2*k-1)*pi*0.5/L;
% Массив частот
p  = a*sqrt(E*A/mu); % рад/с
% Начальная деформация
% К концу стержня приложена растягивающая сила F
F  = 100; % Н
y0 = @(xx) interp1(linspace(0,L,10),F*linspace(0,L,10)/(E*A),xx);
% Постоянные, определяемые по начальным условиям (для частоты i)
M  = @(i)  (2/L)*integral(@(xx) y0(xx).*sin(a(i).*xx),0, L);
M  = @(i)  (2*F/(L*E*A))*sin(a(i)*L)/(a(i)^2.0);
% Начальная скорость деформаций равна нулю, поэтому (для частоты i)
N  = @(i) 0;
% Слагаемые общего решения
yk = @(i,t,x) (M(i)*cos(p(i)*t)+N(i)*sin(p(i)*t))*sin((2*i-1)*pi*x*0.5/L);
% Слагаемые деформации
ek = @(i,t,x) (M(i)*cos(p(i)*t)+N(i)*sin(p(i)*t))*cos((2*i-1)*pi*x*0.5/L)*(2*i-1)*pi*0.5/L;
% Общее решение y(t,x)
y   = @(t,x) sum(arrayfun(@(i) yk(i,t,x),k));
% Деформация
eks = @(t,x) sum(arrayfun(@(i) ek(i,t,x),k));
% ------------------------------------------------------------------------
% Рисуем
% ------------------------------------------------------------------------
% Видео
v = VideoWriter('rod_c.avi');
open(v);

% Разделяем балку на 30 частей (для раскраски)
x = linspace(0,L,30);
figure('Position',[100 100 900 600]);
axis([0 1.3 -0.5 0.5]);
hold on;
% Цветовая палитра JET
cmap = colormap(jet(128));
% Максимальная деформация
maxd =  0.2;
mind = -0.2;
def2index = @(d) floor((d-mind)/((maxd-mind)/size(cmap,1)));
for t=0:0.002:2
    cla;
    yx = arrayfun(@(xx) y(t,xx),x);    
    pos = x + yx;    
    defx = arrayfun(@(xx) eks(t,xx),x);
    for j = 1:size(x,2)
        col = cmap( def2index(defx(j)),:);
        prev_point = 0;
        if j ~= 1
            prev_point = pos(j-1);
        end
        patch([prev_point pos(j) pos(j) prev_point],...
              [0.1 0.1 -0.1 -0.1],col,'EdgeColor','none');            
    end
    plot(pos, arrayfun(@(xx) eks(t,xx),x));
    xlabel('x, м');ylabel('y, м');box('on');

    text(0.1,0.45,sprintf('T=%5.3f c. Длина стержня L=%5.3f м',t,yx(end)+L));    
    text(0.1,0.35,sprintf('Частоты (Гц): '));    
    text(0.1,0.31,sprintf('%3.1f | ',(2*pi./p).^-1));    

    frame = getframe;    
    writeVideo(v,frame);    
end
close(v);
~~~

<iframe width="640" height="360" src="https://www.youtube.com/embed/ZihqWpzX86A" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
