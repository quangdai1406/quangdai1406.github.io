---
layout: page
title: Future Improvement
---

```python
## Import Modulus
from __future__ import division # To prevent 1/2=0
from __future__ import print_function # Accomodate to python 2
import numpy as np #
import matplotlib.pyplot as plt
from scipy.integrate import odeint
%matplotlib inline

## Define Function
def f(y, t, params):
    y, theta, v_y, omega = y      # unpack current values of y

    ## Define Input Torque
    if t<= 0.5:
        tau= 8. # N*m: initial torque
    else:
        tau=0.
    
    m,a,k,I_d = params  # unpack parameters
    
    theta_dot = omega
    y_dot = v_y
    omega_dot=(-m*a*y*omega**2 -2*m*y*omega*v_y + k*a*y + tau)/(I_d+m*y**2)
    v_y_dot= -a*omega_dot + y*omega**2 -(k/m)*y
    export = [y_dot, theta_dot, v_y_dot, omega_dot]
    return export

## System Parameters When the Spring Stiffness is Very High
m_d=5 # kg: mass of the disk
R=0.5 # m:  radius of the disk
I_d=m_d*R**2/2 # kg*m**2: moment of inertia
m=2.5 # kg: mass of particle
fn=1 # Hz: spring-mass system frequency
k=m*(2*np.pi*fn)**2 # N/m: spring stiffness
a=0.4 # m: distance to the slot
params=[m,a,k,I_d] # Bundle all parameters
#print(params) # Testting outputs

## Initial Conditions
y_init=0
theta_init=0
v_y_init=0
omega_init=0
y_initial = [y_init, theta_init, v_y_init, omega_init] # Bundle all initial conditions
#print (y_initial) # Testing outputs

## Create Time Array
tStop = 4
tInc = 0.00001
t = np.arange(0., tStop, tInc)
    
## Solving ODE Function
psoln = odeint(f, y_initial, t, args =(params,)) 
#print(psoln) # Testing the output

plt.figure(1)
## Plotting of Angular Velocity VS Time When Spring Stiffness is Very High
plt.subplot(211)
plt.plot(t,psoln[:,3])
#plt.xlabel(r'Time (sec)')
plt.ylabel(r'Angular Velocity (rad/sec)')
#plt.axis([0, 4, 0, 5])
plt.grid(True)

## Plotting of Mass Particle Position VS Time When Spring Stiffness is Very High
plt.subplot(212)
plt.plot(t,psoln[:,0])
plt.xlabel(r'Time (sec)')
plt.ylabel(r'Mass Particle Position (m)')
#plt.axis([0, 5, -2*10**-7, 10**-7])
plt.grid(True)


#plt.title(r'Angular Velocity VS Time When Spring Stiffness is Very High')
#plt.text(-50, 1.5, r'$\theta_{init} = 45^o$')
#plt.text(-50, 1.4, r'$r_{init} = 1 m$')
#plt.axis([-60, 60, 1, 1.6])
plt.show()

```


![png](output_0_2.png)



```python

```
