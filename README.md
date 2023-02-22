# css
Листинг программы
Листинг rocket.m

function y = rocket(dy0)
global h te
[tv,yv] = euler2(h,0,te,0,dy0);
plot(tv,yv,'o-', 'LineWidth',2);
% invariant: tv(te/h+1)==te
y = yv(te/h+1);     % returns y at t=te
return;
    
function [tv,yv] = euler2(h,t0,tmax,y0,dy0)
    
global a;
g = 9.8;
y = y0;
dy = dy0;
tv = [t0];
yv = [y0];
for t = t0:h:tmax
y = y+dy*h; % метод Эйлера
dy = dy+(-g+a*y^2)*h;
tv = [tv; t+h];
yv = [yv; y];
end
return;

Листинг shoot.m

function ret = shoot(hh)
    
global a te ye h;    
a = 0;
te = 5;
ye = 40;    
h = hh;
 
clf;
hold on;
for dy = 20:10:50
y = rocket(dy);
text(te+.2,y,sprintf('y\047(0)=%g',dy), 'FontSize',15);
end
    
dy = secant(20,30,1e-4);
    
y = rocket(dy);
text(te+.2,y,sprintf('y\047(0)=%g',dy), 'Color','r', 'FontSize',15);
    
set(gca, 'FontSize', 16);       % for tick marks
line([te te],[-40 140], 'Color','k');
line([te te],[ye ye], 'Color','r', 'Marker','o', 'LineWidth',3);
xlabel('t', 'FontSize',20);
ylabel('y', 'FontSize',20);
title(sprintf('Метод стрельбы y\047\047=-g'), 'FontSize',20);
return;
    
function x = secant(x1,x2,tol)
global ye;
y1 = rocket(x1)-ye;
y2 = rocket(x2)-ye;
while abs(x2-x1)>tol
disp(sprintf('(%g,%g) (%g,%g)', x1, y1, x2, y2));
x3 = x2-y2*(x2-x1)/(y2-y1);
y3 = rocket(x3)-ye;
x1 = x2;
y1 = y2;
x2 = x3;
y2 = y3;
end
x = x2;
return;
