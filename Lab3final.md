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

V = 4.027*u.L
Q = 0.0048*u.L/u.s
theta = V/Q
t = np.arange(1181)*u.s
#this represents time of the day in seconds
HRT = t/theta
pH = array[:,1]

plt.figure('ax',(10,8))
plt.plot(HRT,pH)
# put in your x and y variables
plt.xlabel('Hydraulic Residence Time (sec)', fontsize=15)
plt.ylabel('PH of Lake', fontsize=15)
plt.title('pH vs Hydraulic Redidence Time', fontsize=15)
plt.show()

```

2) Calculate α0, α1, and α2 based on the pH measured at each time point. Recall that each α represents the fraction of a particular carbonate system species at that pH. As such, the sum of the alphas at each measured pH should be 1.0!

```python

K1 = 10**-6.3
K2 = 10**-10.3

#concentration of H+
H = 10**-pH

a0_bottom = 1 + (K1/H) + (K1*K2/H**2)
a0 = 1/a0_bottom

a1_bottom = (H/K1) + 1 + (K2/H)
a1 = 1/a1_bottom

a2_bottom = (H**2/(K1*K2)) + (H/K2) + 1
a2 = 1/a2_bottom


```

3) Assuming that the lake can be modeled as a completely mixed flow reactor and that ANC is a conservative parameter, equation 1.21 can be used to calculate the expected ANC in the lake effluent as the experiment proceeds. Graph the expected ANC in the lake effluent versus the hydraulic residence time (t/θ) based on the completely mixed flow reactor equation with the plot labeled (in the legend) as “conservative ANC.”

```python

ANC_in = 10**-3
print(ANC_in)
#ANC_in represents the acid rain influent, which only contains hydrogen ions

ANC_0 = 0.624/84.00661/4.027
print(ANC_0)
#initial ANC is HCO3 from the NaHCO3 input, as discussed in class

#took units off of ANC_0 because ANC_out formula was not executing due to unit discrepancy
ANC_out = ANC_in*(1-np.exp(-t/theta)) + ANC_0*np.exp(-t/theta)

plt.figure('ax',(10,8))
plt.plot(HRT,ANC_out, label='Conservative ANC')
plt.xlabel('Hydraulic Residence Time (cycle)', fontsize=15)
plt.ylabel('ANC (eq/L)', fontsize=15)
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
plt.title('ANC vs Hydraulic Redidence Time', fontsize=15)
plt.show()

```

4) If we assume that there are no carbonates exchanged with the atmosphere during the experiment, then we can calculate ANC in the lake effluent by using equation 1.11 describing the ANC of a closed system. Calculate the ANC under the assumption of a closed system and plot it on the same graph produced in answering question #3 with the plot labeled (in the legend) as “closed ANC.”

```python
C_T_0 = .001855/a1
Kw = (10**-14)
ANC_nv = C_T_0*(a1 + 2*a2) + Kw/H - H
ANC_nv[1000]

plt.plot(HRT,ANC_out, label='Conservative ANC')
plt.plot(HRT,ANC_nv, label='closed ANC')
plt.xlabel('Hydraulic Residence Time (cycle)', fontsize=15)
plt.ylabel('ANC (eq/L)', fontsize=15)
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
plt.title('ANC vs Hydraulic Residence Time', fontsize=15)
plt.show()

```

5) If we assume that there is exchange with the atmosphere and that carbonates are at equilibrium with the atmosphere, then we can calculate ANC in the lake effluent by using equation 1.15 describing the ANC of an open system. Calculate the ANC under the assumption of an open system and plot it on the same graph produced in answering question #3 with the plot labeled (in the legend) as “open ANC.”

```python
PCO2 = (10**-3.5)
KH = (10**-1.5)
Kw = (10**-14)
#known constants

#equation 1.15
ANC_v = ((PCO2 * KH * (a1 + 2*a2)))/a0 + (Kw/H) - H
plt.figure('ax',(10,8))
plt.plot(HRT,ANC_out,label='ANC_out')
plt.plot(HRT,ANC_nv, label='ANC_nv')
plt.plot(HRT,ANC_v, label='ANC_v')
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
plt.xlabel('Hydraulic Residence Time (cycle)', fontsize=15)
plt.ylabel('ANC (eq/L)', fontsize=15)
plt.title('ANC vs Hydraulic Residence Time', fontsize=15)
plt.show()
```

6) Derive an equation for C_T (the concentration of carbonate species) as a function of time based on the input of NaHCO3 and its dilution in the completely mixed lake assuming that no carbonate species are lost or gained to the atmosphere (the equation will be the same form as equation 1.21). Graph C_T versus the hydraulic residence time (t/θ) with the plot labeled (in the legend) as “conservative C_T.”

```python

#C_T= C_T_in(1- math.exp(-t/theta)) + C_T_0*(math.exp(t/theta))
#Assume C_T_in = 0 because there are no carbonates in the acid rain influent
C_T_out = C_T_0*(np.exp(-t/theta))

plt.figure('ax',(10,8))
plt.plot(HRT, C_T_out, label='conservative C_T')
plt.xlabel('Hydraulic Residence Time', fontsize= 15)
plt.ylabel('Concentration of Carbonate Species', fontsize=15)
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
plt.title('Concentration of Carbonate Species vs. HRT')
plt.show()
```


7) Derive an equation for C_T as a function of ANC and pH based on equation 1.11. Use the CMFR model to obtain ANC as a function of time (the results from question 3). Plot this measured CT versus hydraulic residence time (t/θ) on the same graph produced in answering question #6 with the plot labeled (in the legend) as “closed C_T.”


```python

Kw = 10**(-14)
C_T_closed = ((ANC_out -  (Kw/(10**(-pH)))+(10**(-pH))) / (a1+2*a2))

plt.figure('ax',(10,8))
plt.plot(HRT,C_T_closed,label='Closed C_T')
plt.plot(HRT, C_T_out, label='conservative C_T')
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
plt.xlabel('Hydraulic Residence Time (cycle)', fontsize=15)
plt.ylabel('C_T', fontsize=15)
plt.title('Concentration of Carbonate Species vs Hydraulic Residence Time', fontsize=15)
plt.show()
```

8) Plot the equilibrium concentration of C_T (a function of pH) versus hydraulic residence time (t/θ) on the same graph produced in answering question 6 with the plot labeled (in the legend) as “equilibrium C_T”
	VALUES FOR EQUILIBRIUM SUPER LOW, WHAT IS THE DEAL

```python

#C_T_equilibrium = PCO2*KH/a0 rewritten to be direct function of pH
C_T_equilibrium = PCO2*KH*(1+(K1*(10**pH))+(K1*K2*(10**(2*pH))))


plt.figure('ax',(10,8))
plt.plot(HRT,C_T_equilibrium, label='CT_equilibrium')
plt.plot(HRT,C_T_closed,label='C_T closed')
plt.plot(HRT, C_T_out, label= 'C_T out')
plt.legend(loc='upper right')
plt.xlabel('Hydraulic Residence Time (cycle)', fontsize=15)
plt.ylabel('C_T', fontsize=15)
plt.title('C_T vs Hydraulic Residence Time', fontsize=15)
plt.show()

```



9) Compare the two plots and determine whether the lake is best modeled as a volatile or non volatile system. What changes could be made to the lake to bring the lake into equilibrium with atmospheric CO_2?

After comparing the two plots, it is evident that the lake is best modeled as a volatile system. This is due to the fact that in real life, the lake will be open to the atmosphere, which has a big effect on the CO2 concentrations.

In order to bring the lake into equilibrium with the atmospheric CO_2, one can either increase mixing or increase the hydraulic residence time. The hydraulic residence time can be increased by decreasing the flow rate into and out of the lake, as theta= V/Q, and V is constant.

10) Analyze the data from the 2nd experiment and graph the data appropriately. What did you learn from the 2nd experiment?


In the second experiment, we did not use any stirring until 10 minutes in. We saw that without the stirring, the pH actually decreased rapidly, as the sodium bicarbonate stayed undissolved at the bottom of the lake. This showed how important stirring can be, especially in real life situations. When we started stirring again, the pH slowly increased for some time before it started decreasing again.

```python

file2= pd.read_csv('Lab3_Data2.csv')
array2 = np.array(file)

V= 4.027*u.L
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


Questions

1) What do you think would happen if enough NaHCO3 were added to the lake to maintain an ANC greater than 50 µeq/L for 3 residence times with the stirrer turned off? How much NaHCO3 would need to be added?

If the lake is not being stirred, you would need to add a much greater amount of NaHCO3 than if there was stirring. This is because the sodium bicarbonate would not react as fast with the water, so in order to reach an ANC greater than 50 micro eq/L for 3 residence times, there would have to be much more sodium bicarbonate. Looking at the prelab where there's mixing, you would have to add 6.75g of NaHCO3 to keep ANC levels above 50 micro eq/L for 3 hydraulic residence times. This is assuming an influent pH of 3.0, a lake volume of 4 L, and a current ANC of 0 micro eq/L. Without stirring, the amount added would need to be greater than 6.75g.


2) What are some of the complicating factors you might find in attempting to remediate a lake using CaCO3? Below is a list of issues to consider.

• extent of mixing
• solubility of CaCO3 (find the solubility and compare with NaHCO3)
• density of CaCO3 slurry (find the density of CaCO3)

CaCO3 is not very soluble in water, around 47 mg/L, or 0.047 g/L  when being dissolved in pure water. This is compared to NaHCO3 having a solubility of around 96 g/L, a magnitude of 10 greater than CaCO3. Additionally, the CaCO3 slurry would be much denser, as CaCO3 has a density of 2.711 g/cm^3 and NaHCO3 has a density of around 1.2 g/cm^3 (as a powder). This higher density would also contribute to the difficulty of dissolving CaCO3 in water.

Since CaCO3 is only barely soluble in water, it would require an infinitely greater amount of mixing than NaHCO3. This is definitely a complicating factor when looking at real lakes, because it is not as easy as stirring 4 L of water with a small stirring rod.
