# strange_attractors
:monkey: monkeying around with chaos equations :monkey:



#### Rössler system
The Rössler system is given by the equations:

<font color='black'>
$$
\begin{align}
\dot{x} = -y - z \\
\dot{y} = x + ay \\
\dot{z} = b + z(x-c)
\end{align}
$$
</font>

which for certain parameter values of $$a$$, $$b$$ and $$c$$ will exhibit chaotic behavior.
I have chosen $$(a,b,c) = (0.1,0.1,14)$$.
The following code simulates the dynamics for the system.
First we import some packages and define the Rössler equations.

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# define equation
def rossler_attractor(x, y, z, a=0.1, b=0.1, c=14):
	x_dot = - y - z
	y_dot = x + a*y
	z_dot = b + z*(x - c)

	return x_dot, y_dot, z_dot
```

Then we define some basic parameters such as how many steps we want to make and the size of each step.


```python
# basic parameters
step_size = 0.01 
steps = 100000

# initialize solutions arrays (+1 for initial conditions)
xx = np.empty((steps + 1))
yy = np.empty((steps + 1))
zz = np.empty((steps + 1))

# fill in initial conditions
xx[0], yy[0], zz[0] = (0.1, 0., 0.1)
```

It is possible to solve the equation system using [scipy](https://docs.scipy.org/doc/scipy-0.18.1/reference/integrate.html), but since this is a simple system we will just do it using numpy. 

```python
# solve equation system
for i in range(steps):
    # Calculate derivatives
    x_dot, y_dot, z_dot = rossler_attractor(x_s[i], y_s[i], z_s[i])
    
    xx[i + 1] = xx[i] + (x_dot * delta_t)
    yy[i + 1] = yy[i] + (y_dot * delta_t)
    zz[i + 1] = zz[i] + (z_dot * delta_t)
```

From here its easy to use matplotlib to plot the solution in 3D.

```python
fig = plt.figure(figsize=(5,5))
ax = fig.gca(projection='3d')
plt.gca().patch.set_facecolor('black')

plt.plot(xx[:i], yy[:i], zz[:i],'-',color='white',lw=0.1)

# set limits
ax.set_xlim(-20,25)
ax.set_ylim(-25,20)
ax.set_zlim(0,50)

# remove ticks
ax.set_xticks([])
ax.set_yticks([])
ax.set_zticks([])

# make pane's have the same colors as background
ax.w_xaxis.set_pane_color((0.0, 0.0, 0.0, 1.0))
ax.w_yaxis.set_pane_color((0.0, 0.0, 0.0, 1.0))
ax.w_zaxis.set_pane_color((0.0, 0.0, 0.0, 1.0))

# set angle to view the figure from 
ax.view_init(30, angle)

# save fig
plt.savefig('rossler.png', dpi = 300, pad_inches = 0, bbox_inches = 'tight')
plt.close()
```
