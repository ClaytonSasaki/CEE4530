
```python
from aide_design.play import*
```

1)	Calculate the incremental volume of a 100 g/L red dye stock that would need to be added to 1 L of water to produce 0, 1, 2, 5, 10, 20, 30, 40, and 50 mg/L calibration points. Calculate a numpy array containing the cumulative volume of red dye required. Strip the units from the array using .magnitude. Then create a copy of the array with a zero appended (np.append) in front and the last element deleted (np.delete). Then use numpy subtract to get the different between the two arrays to calculate the incremental volume that you need to add.

```python
x = np.array([0, 1, 2, 5, 10, 20, 30, 40, 50 ])

y = x/100.
print(y)

cumulative = np.zeros([1,9])
cumulative[0,0] = y[0]

for i in range(len(y)):
  cumulative[0,i] = cumulative[0,i-1] + y[i]
print(cumulative)

new_cumulative = np.append(0,cumulative)
new_cumulative = new_cumulative[:-1]
print(new_cumulative)
incremental = np.subtract(cumulative,new_cumulative)
print(incremental)

```

2)	Calculate the change in hydraulic grade line between baffled sections of a reactor with a flow rate of 380 mL/min. The reactor baffles are perforated with 6 holes 1 mm in diameter. Is the flow through these orifices in series or in parallel? Do you multiply the head loss for one orifice by the number of orifices to get the total head loss? Use the pc.head_orifice function to calculate the head loss through an orifice. The vena contracta for the orifice can be found at exp.RATIO_VC_ORIFICE. Why would 6 holes 1 mm in diameter not be a good design for this reactor?

```python
Q = 6.33333333e-6
n= 6
k = 0.6
d = 0.001
g = 9.8

delta_h = (((4*Q)/(n*k*np.pi*d**2))**2)/(2*g)

#prints head loss for 6 holes 1 mm in diameter using equation
print(delta_h)

#prints head loss for 6 holes 1 mm in diameter using function
pc.head_orifice(0.001,exp.RATIO_VC_ORIFICE,6.33333333e-6)/36

```
The flow through the orifices is in parallel. You cannot multiply the head loss for one orifice by the number of orifices to get the total head loss because of they are in parallel and not series. They are not independent. 6 holes 1 mm in diameter is not a good design because you want energy loss (head loss) to slow down the flow. Adding more holes decreases the head loss, and therefore leads to a faster flow.

3)	On a single graph plot the exit age distribution (E(t*)) for a reactor that operates as a 1-dimensional advection-dispersion reactor with Peclet numbers of 1, 10, and 100 (there will be three plots on the graph and thus a legend is required). The x-axis should be t* from 0.0 to 3.0. Comment on the shapes of the curves as a function of the Peclet number.
```python

Pe = np.array([1,10,100])
t_star = np.linspace(0.00000001,3.0,1000)


for i in range(len(Pe)):
  E_t_star = np.sqrt(Pe[i]/(4*np.pi*t_star))*np.exp((-((1-t_star)**2)*Pe[i])/(4*t_star))
  plt.plot(t_star,E_t_star, label='Pe = '+str(Pe[i]))
plt.xlabel('t*')
plt.ylabel('(E(t*))')
plt.title('Exit Age Distributions for Varying Peclet Numbers')
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
plt.show()

```
