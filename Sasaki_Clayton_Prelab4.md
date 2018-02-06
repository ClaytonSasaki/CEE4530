1)	Compare the ability of Cayuga lake and Wolf pond
(an Adirondack lake) to withstand an acid rain runoff event
(from snow melt) that results in 20% of the original lake water
being replaced by acid rain. The acid rain has a pH of 3.5 and
is in equilibrium with the atmosphere. The ANC of Cayuga lake
is 1.6 meq/L and the ANC of Wolf Pond is 70 µeq/L. Assume that
carbonate species are the primary component of ANC in both lakes,
and that they are in equilibrium with the atmosphere. What is
the pH of both bodies of water after the acid rain input?
Remember that ANC is the conservative parameter (not pH!).
Hint: You can use the scipy optimize root finding function
called brentq. Scipy can’t handle units so the units must be
removed using .magnitude.

```python
from aide_design.play import*

pHrain = 3.5
ANC_cayuga_pre = 1.6*(10**-3)
ANC_wolf_pre = 70*(10**-6)
restime = 0.2

#[H+] = 10^-pH
H = 10**(-pHrain)

#For pH below about 4.3: ANC = -[H+]
ANC_rain = -H

ANC_cayuga_post = ANC_rain*(1-np.exp(-restime))+ANC_cayuga_pre*np.exp(-restime)
ANC_wolf_post = ANC_rain*(1-np.exp(-restime))+ANC_wolf_pre*np.exp(-restime)
print(ANC_cayuga_post)
print(ANC_wolf_post)


#from carbonates file
pH_cayuga_post = 8.453
ph_wolf_post = 5.683
```

2)	What is the ANC of a water sample containing only carbonates
and a strong acid that is at pH 3.2? This requires that you
inspect all of the species in the ANC equation and determine
which species are important.

```python
pH = 3.2

#[H+] = 10^-pH
H = 10**(-pH)

#For pH below about 4.3: ANC = -[H+]
ANC= -H

print(ANC)

#prints
-0.0006309573445
```

3)	Why is [H+] not a conserved species?

[H+] is not conserved because it reacts with carbonates to form Bicarbonate and Carbonic Acid. It is only conserved at very low pHs where ANC is 0.
