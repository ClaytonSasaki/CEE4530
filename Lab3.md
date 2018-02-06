```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib
import aide_design
import aide_design.pipedatabase as pipe
from aide_design.units import unit_registry as u
from aide_design import physchem as pc
import aide_design.expert_inputs as exp
import aide_design.materials_database as mat
import aide_design.utility as ut
import aide_design.k_value_of_reductions_utility as k
import aide_design.pipeline_utility as pipeline
import warnings

```

# Laboratory 3: Acid Rain

## Team 1: Clayton Sasaki, Kristen Ajmo, Megan McEnaney

Data Analysis

1) Plot measured pH of the lake versus hydraulic residence time (t/θ).

```python

file= pd.read_csv('Lab3_Data1.csv')
array = np.array(file)
print(array[0,0])

V = 4.027*u.L
Q = 0.0048*u.L/u.s
theta = V/Q

t = array[:,0]*24*60*u.s
#this represents time of the day in minutes
HRT = t/theta
pH = array[:,1]
print(pH[1])
print(HRT[1])


plt.figure('ax',(10,8))
plt.plot(HRT,pH)
# put in your x and y variables
plt.xlabel('Hydraulic Residence Time', fontsize=15)
plt.ylabel('PH of Lake', fontsize=15)
plt.title('pH vs Hydraulic Redidence Time (sec)', fontsize=15)
plt.show()

```

2) Calculate α0, α1, and α2 based on the pH measured at each time point. Recall that each α represents the fraction of a particular carbonate system species at that pH. As such, the sum of the alphas at each measured pH should be 1.0!

```python

K1 = 10**-6.3
K2 = 10**-10.3

#concentration of H+
H = 10**-pH*u.g/u.L

a0_bottom = 1 + (K1/H) + (K1*K2/H**2)
a0 = 1/a0_bottom

a1_bottom = (H/K1) + 1 + (K2/H)
a1 = 1/a1_bottom

a2_bottom = (H**2/(K1*K2)) + (H/K2) + 1
a2 = 1/a2_bottom
print(a0 + a1+a2)
#checking

```

3) Assuming that the lake can be modeled as a completely mixed flow reactor and that ANC is a conservative parameter, equation 1.21 can be used to calculate the expected ANC in the lake effluent as the experiment proceeds. Graph the expected ANC in the lake effluent versus the hydraulic residence time (t/θ) based on the completely mixed flow reactor equation with the plot labeled (in the legend) as “conservative ANC.”

```python

ANC_in = -H
ANC_0 = 0.624/4.027*u.g/u.L #convert to moles?
print(ANC_0)
#ANC_out = ANC_in*(1-exp(-t/theta))+ ANC_0*exp(-t/theta)
ANC_out = ANC_in*(1-np.exp(-t/theta)) + ANC_0*np.exp(-t/theta)

plt.plot(HRT,ANC_out)
plt.legend(handles=[line_up, line_down])
plt.show()

```

4) If we assume that there are no carbonates exchanged with the atmosphere during the experiment, then we can calculate ANC in the lake effluent by using equation 1.11 describing the ANC of a closed system. Calculate the ANC under the assumption of a closed system and plot it on the same graph produced in answering question #3 with the plot labeled (in the legend) as “closed ANC.”

```python

ANC = C_T (a1 + 2*a2)

```

5) If we assume that there is exchange with the atmosphere and that carbonates are at equilibrium with the atmosphere, then we can calculate ANC in the lake effluent by using equation 1.15 describing the ANC of an open system. Calculate the ANC under the assumption of an open system and plot it on the same graph produced in answering question #3 with the plot labeled (in the legend) as “open ANC.”

```python

```

6) Derive an equation for C_T (the concentration of carbonate species) as a function of time based on the input of NaHCO3 and its dilution in the completely mixed lake assuming that no carbonate species are lost or gained to the atmosphere (the equation will be the same form as equation 1.21). Graph C_T versus the hydraulic residence time (t/θ) with the plot labeled (in the legend) as “conservative C_T.”

```python

#C_T_out = C_T_in(1- np.exp(-t/theta)) + C_T_0*(np.exp(it/theta))
C_T_out = -np.ln(C_T_0)/Q

```

7) Derive an equation for C_T as a function of ANC and pH based on equation 1.11. Use the CMFR model to obtain ANC as a function of time (the results from question 3). Plot this measured CT versus hydraulic residence time (t/θ) on the same graph produced in answering question #6 with the plot labeled (in the legend) as “closed C_T.”

Since there is no ${C_T}_{in}$, using the CMFR model we get,

$$-QC_{T}=V\frac{dC_{T}}{dt}$$

$$\frac{dC_{T}}{-QC_{T}}=\frac{dt}{V}$$

$$-\int_{C_{T_{0}}}^{C_{T}} \frac{dC{_{T}}^{'}}{C{_{T}}^{'}} = \int_{0}^{t} \frac{Q}{V} dt^{'}$$

$$ln(\frac{C_{T}}{C_{T_{0}}}) = - \frac{Q}{V} dt $$

$$\frac{C_{T}}{C_{T_{0}}} = e^{-\frac{Q}{V}t}$$

$$C_{T} = C_{T_{0}}e^{-\frac{t}{\theta}}$$

```python

Kw = 10**-14
ANC = C_T(a1 + 2a2) + (Kw/H) - H

```

8) Plot the equilibrium concentration of C_T (a function of pH) versus hydraulic residence time (t/θ) on the same graph produced in answering question 6 with the plot labeled (in the legend) as “equilibrium C_T”

```python

```

9) Compare the two plots and determine whether the lake is best modeled as a volatile or non volatile system. What changes could be made to the lake to bring the lake into equilibrium with atmospheric CO_2?

```python

```

10) Analyze the data from the 2nd experiment and graph the data appropriately. What did you learn from the 2nd experiment?


In the second experiment, we did not use any stirring until 10 minutes in.. We saw that without the stirring, the pH actually decreased rapidly, as the sodium bicarbonate stayed undissolved at the bottom of the lake. This showed how important stirring can be, especially in real life situations. When we started stirring again, the pH slowly increased for some time before it started decreasing again.


In the second experiment, we did not use any stirring until 10 minutes in.. We saw that without the stirring, the pH actually decreased rapidly, as the sodium bicarbonate stayed undissolved at the bottom of the lake. This showed how important stirring can be, especially in real life situations. When we started stirring again, the pH slowly increased for some time before it started decreasing again.

```python

file2= pd.read_csv('Lab3_Data2.csv')
array2 = np.array(file)

V= 4.027*u.L #ASK QUESTION WHY UNIT NO WORK
Q= 0.0048*u.mL/u.s
theta= V/Q

t = array2[:,0]*24*60
#this represents time of the day in minutes
HRT = t/theta
pH =array2[:,1]

plt.figure('ax',(10,8))
plt.plot(HRT,pH)
# put in your x and y variables
plt.xlabel('Hydraulic Residence Time', fontsize=15)
plt.ylabel('pH of Lake', fontsize=15)
plt.title('pH vs Hydraulic Residence Time, No Stirring Until Ten Minutes', fontsize=15)
plt.show()

```

Summary
