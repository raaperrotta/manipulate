# Manipulate
MATLAB interactive function caller

Similar to the Mathematica function Manipulate, this function creates a slider that calls a function whenever its value is updated. Great for interactive plotting.

`manipulate(@(ARGS)FUNC(ARGS),{LABEL,ARG_MIN,ARG_MAX,[DEFAULT]}...)`
calls the function `func(ARGS)` and allows the user to control the
values of the arguments via sliders. If `DEFAULT` is not set, the value
is initially set to `ARG_MIN`.
 
manipulate expects one cell input per argument. All arguments must be
scalars. Cells can also be of the form `{LABEL,VALUES,[DEFAULT]}` where
`VALUES` is a 1D array containing allowed values of the argument.
 
`H = manipulate(...)` returns the the slider handles.
 
`manipulate(...,'UpdateMode',MODE)` allows the user to set the frequency
with which the function is called.
  `'Low'`    Refresh only when slider is released (after drag) or
           clicked, or when edit field is altered.
  `'High'`   Also refresh continuously as slider is dragged. [Default]
  `'Manual'` Refresh only when user presses button.
 
`manipulate(...,'SliderStep',[SMALL BIG])` allows the user to set the
step size of incremental slider increases and decreases. See `UICONTROL`
for details.
  
## Examples:

```matlab
  x = 1:.001:10;
  gca, L = plot(1,1,1,1);
  myfunc = @(i,f,theta)set(L(i),'XData',x,'YData',sin(10*x)+sin(f*x+theta));
  h=manipulate(@(f,theta)myfunc(1,f,theta),{'frequency1',10,20},{'phase1',0,2*pi});
  manipulate(h,@(f,theta)myfunc(2,f,theta),{'frequency2',10,20},{'phase2',0,2*pi})
 ```
 
Plots the interference of two sine waves and allows the user to
manipulate the frequency and phase offset of the second wave. Then does
the same again, adding the slider controls to the existing slider
figure.
 
 ```matlab
  x = linspace(-10,10,1e4);
  fig = figure('Visible','off');
  ax = [subplot(2,1,1),subplot(2,1,2)];
  f1 = @(u,s) plot(ax(1),x,exp(-1/2*((x-u)/s).^2)/sqrt(2*pi)/s,'r');
  f2 = @(u,s) title(ax(1),sprintf('N(%.2f,%.2f)',u,s^2));
  f3 = @(u,s) plot(ax(2),x,erf((x-u)/s/sqrt(2))/2+1/2,'r');
  myfunc = @(u,s) {f1(u,s),f2(u,s),f3(u,s)};
  manipulate(fig,myfunc,{'Mean',-5,5,0},{'Standard Deviation',.01,5,1})
  linkaxes(ax,'x'), set(ax,'NextPlot','replacechildren')
  movegui(fig,'onscreen')
  set(fig,'Visible','on')
 ```
 
Plots the normal distribution and its cumuliative distribution on
separate subplots and allows the user to manipulate the mean and
standard deviation. The mean is initially set to 0 and the standard
deviation to 1. The slider controls appear in the same figure as the
subplots.
 
Created by:
  Robert Perrotta
