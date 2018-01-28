# Laboratory 1 PreLab Questions
## Clayton Sasaki

1. Why are contact lenses hazardous in the laboratory?

>Contact lenses are hazardous because they can hold contaminants into the eye and are difficult to remove in the event of a splash.

2. What is the minimum information needed on the label for each chemical? When are Right-To-Know labels required?

>The minimum information needed on the label for each chemical is the full chemical name (not just the chemical formula), concentration, and date prepared. Right-To-Know labels are required when a chemical is designated as a hazardous material (corrosivity, ignitability, toxicity, reactivity, etc.) and if it is made into a solution or repackaged as a solid or liquid in a concentration greater than 1% (0.1% for a carcinogen). The label must duplicate the hazard warnings, precautions, and first aid steps found on the original label.

3. Why is it important to label a bottle even if it only contains distilled water?

>It is important to know what is hazardous as well as what is not because the danger of anything not labeled is unknown.

4. Find an SDS for sodium nitrate.
    1. Who created the SDS?

    >SSQM Northamerica Corporation

    2. What is the solubility of sodium nitrate in water?

    >480 g/L (20 °C )

    3. Is sodium nitrate carcinogenic?

    >Equivocal carcinogenic agent according to
    animal tests.

    4. What is the LD50 oral rat?

    >1267 mg/Kg

    5. How much sodium nitrate would you have to ingest to give a 50% chance of death (estimate from available LD50 data).

    ```python
    #Since LD 50 is 1267 mg/Kg
    #and I am 55kg

    LD50_me = 1267 * 55
    print(LD50_me)

    #ingesting 69685 mg of
    #sodium nitrate would
    #give me a 50% chance of death
    ```
    6. How much of a 1 M solution would you have to ingest to give a 50% chance of death?

    >$$\frac{1\,mol}{L} \times \frac{85.0\,g}{mol} = \frac{85.0\,g}{L}$$
    >$$69685\,mg \times \frac{1\,g}{1000.\,mg} \times \frac{L}{85.0\,g}$$

    ```python
    volume = 69685./1000./(1*85.0)
    print(volume)

    #ingesting 0.820 L of
    #1 M sodium nitrate
    #solution would give me a
    #50% chance of death
    ```
    7. Are there any chronic effects of exposure to sodium nitrate?

    >No adverse effects observed on developmental toxicity. Equivocal carcinogenic agent according to
    animal tests. Mutagenic and reproductive hazard at very high level doses according to animal tests.

5. You are in the laboratory preparing chemical solutions for an experiment and it is lunchtime. You decide to go to CTB to eat. What must you do before leaving the laboratory?

> You must wash your hands and arms throughly before leaving the laboratory.

# Laboratory 2 PreLab Questions
## Clayton Sasaki

1.  You need 100 mL of a 1 µM solution of zinc that you will use as a standard to calibrate an atomic adsorption spectrophotometer. Find a source of zinc ions combined either with chloride or nitrate (you can use the internet or any other source of information). What is the molecular formula of the compound that you found?

>Zinc Nitrate: $Zn(NO_3)_2$
__

Zinc disposal down the sanitary sewer is restricted at Cornell and the solutions you prepare may need to be disposed of as hazardous waste. As an environmental engineering student you strive to minimize waste production. How would you prepare this standard using techniques readily available in the environmental laboratory so that you minimize the production of solutions that you don’t need? Note that we have pipettes that can dispense volumes between 10 $\mu L$ and 1 mL and that we have 100 mL and 1 L volumetric flasks. Include enough information so that you could prepare the standard without doing any additional calculations. Consider your ability to accurately weigh small masses. Explain your procedure for any dilutions. Note that the stock solution concentration should be an easy multiple of your desired solution concentration so you don’t have to attempt to pipette a volume that the digital pipettes can’t be set for such as 13.6 $\mu L$.

> I would first start by creating a 100 mL of 100 µM solution. This would be done by measuring out 0.001894 grams of Zinc Nitrate. Then put the Zinc Nitrate and distilled water in a 100 mL volumetric flask. Work shown below:

>$$100\,mL \times \frac{1\,L}{1000.\,mL} \times \frac{100\,\mu mol}{L} \times\,...$$
>$$...\frac{1 mol}{10^{6} \mu mol} \times\frac{189.4 g}{mol} = 0.001894\,grams$$

>Next, this solution would have to be diluted. 1 mL of this solution (using a pipette) would be mixed with 99mL of distilled water in a 100 mL volumetric flask.
Work is below:

Solve:
>$$(100\,\mu M)(X\,mL)+(0\,\mu M)(Y\,mL) = (1\,\mu M)(100 mL)$$
>$$X\,mL+Y\,mL=100\,mL$$

By substitution:
>$$X=100-Y$$

so:
>$$(100)(100-Y)+(0)(Y) = (1)(100)$$
>$$(10000-100Y) = 100$$
>$$-100Y = -9,900$$
>$$Y = 99\,mL$$

and:
>$$X=100-Y$$
>$$X=100-99$$
>$$X=1\,mL$$

> This procedure would create waste but would minimize the concentration of the waste while keeping reasonable precision in measurements. 

2.  The density of sodium chloride solutions as a function of concentration is approximately 0.6985C + 998.29 (kg/$m^{3}$) (C is kg of salt/$m^{3}$). What is the density of a 1 M solution of sodium chloride?

>$$\frac{1\,mol}{L} \times \frac{58.4\,g}{mol} \times
\frac{1\,kg}{1000\,g} \times \frac{1000\,L}{1 m^{3}}$$
> $$= 58.4\,kg\,of\,salt/m^{3}$$

>$$0.6985C + 998.29 (kg/m^{3})$$
> $$0.6985(58.4) + 998.29$$
> $$=1,039\,kg/m^{3}$$
