```python
from aide_design.play import*
```

# Lab 4: Acid Neutralizing Capacity
# Clayton Kristen Megan
# due: March 2, 2018

Clayton
Kristen
Megan

Full lab reports (+)
Write the laboratory report as if the experiment were your idea. Imagine that you are working for a consulting firm or in a research laboratory and that you needed to do laboratory research to investigate options for a real world project. You can use the Lab Manual as an example of a well formatted WORD document. The Atom reports should have similar formatting with the required python code. Full laboratory reports should include the following sections:
Objectives
Write a paragraph on the goals of the experiment. Why did you decide to do this experiment? Introduce your approach by explaining what needs to be done to meet your goal for your real world project. Explain what you hoped to learn through this research. How did you expect this experiment to guide your decisions about the real world project that you are working on?
Procedures
Provide an overview of the methods that you used in your investigation. The best procedures give an overview of the method with an explanation of why you used those methods. There is no need to restate the step-by-step procedures as outlined in the lab manual: it is sufficient to cite the lab manual and include information on any deviations from the manual procedures. When method development is part of the laboratory exercise, a detailed description of the methods should be included. Methods and procedures need to be detailed enough so that one of your classmates could duplicate your work.
Results and Discussion
Present results in a clearly labeled format. Data analysis methods, equations, graphs, and tables should be presented in this section. The report text should refer to each equation, figure, and table. Include a table of relevant experimental parameters (e.g., measured flow rates, sample sizes, concentrations of reagents, etc.).
Compare theoretical expectations with your results and discuss reasons for any observed deviations. If the results weren't as expected, suggest reasons why the laboratory results may have differed from theory and suggest improved techniques to obtain more accurate results or modifications to the theory to better describe the experimental conditions.
Make sure that responses to specific questions and data analysis requested in the lab manual are included in this section. But don't answer the questions in a list format. Instead, include your answers as part of the narrative that is designed to meet your objectives.
Conclusions
The conclusions section should not include any new observation. It is the place to summarize the results in a few sentences. Make sure you connect your conclusions to your objectives for doing the research.
Suggestions/comments
I value your thoughts on where changes are needed as well as what is working well. Include ideas for:
improving the data acquisition software
modifying the experimental apparatus
making the experiment easier to understand
modifying the experiment to include some new experimental aspect related to the main topic
alternate ways to analyze the data

















Questions

1) Plot the titration curve of the t=0 sample with 0.05 N HCl (plot pH as a function of titrant volume). Label the equivalent volume of titrant. Label the 2 regions of the graph where pH changes slowly with the dominant reaction that is occurring. (Place labels with the chemical reactions on the graph in the pH regions where each reaction is occurring.) Note that in a third region of slow pH change no significant reactions are occurring (added hydrogen ions contribute directly to change in pH).
Clayton

2) Prepare a Gran plot using the data from the titration curve of the t=0 sample. Use linear regression on the linear region or simply draw a straight line through the linear region of the curve to identify the equivalent volume. Compare your calculation of Ve with that calculated by the pH meter computer program.

Megan

``` python
file = pd.read_csv('NoLabels_Lab4-Acid-round1-sample1.csv')
array = np.array(file)
Titrant_volume = array[:,0]*u.mL
pH = array[:,1]

plt.figure()
plt.scatter(Titrant_volume, pH)
plt.xlabel(‘Titrant volume (mL)’)
plt.ylabel(‘pH’)
plt.title(‘Titrant Volume vs. pH’)
plt.show


```

Ve calculated by the pH meter computer program was 0.446114 mL

3) Plot the measured ANC of the lake on the same graph as was used to plot the
conservative, volatile, and nonvolatile ANC models (see questions 2 to 5 of the Acid Precipitation and Remediation of an Acid Lake lab). Did the measured ANC values agree with the conservative ANC model?

Kristen
