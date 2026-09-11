**Natural-Resources-and-Growth**

A short, independent economics project completed alongside the first year of my undergraduate studies in Economics, testing whether the "resource curse", the idea that resource-rich countries tend to grow more slowly, still holds up using two more contemporary decades worth of data than was used in the original 1995 Sachs and Warner study "The Curse of Natural Resources".

**Background**

The starting point for this project is Sachs and Warner's The Curse of Natural Resources (1995 working paper; published in the European Economic Review in 2001). The paper found that economies heavily reliant on primary product exports tended to grow more slowly between 1970 and 1990, and argued this was consistent with mechanisms such as Dutch disease and the crowding out of manufacturing.

**Research question**

Does the resource curse still show up in the data 20 years on from the original period studied?

**Approach**
Natural Resources dependency in 1995. Sachs and Warner's original measure (SXP) was the share of primary product exports in GNP, using 1970s-era SITC export categories. This project approximates it with contemporary World Bank export data - food, fuel, agricultural raw materials, and ores & metals exports as a share of merchandise exports in 1995 - referred to here as SPM (share of primary products in merchandise exports). Full category mapping and caveats are in notes.
Growth in GDP per capita annualised 1995–2014. Annual GDP per capita growth rates (constant 2015 USD) are converted into a single annualised, compound growth measure using a log-based method, mirroring the logic of Sachs and Warner's original growth formula bar taking the economically active population into account.
Regression. A simple OLS regression of annualised GDP per capita growth on 1995 Natural Resource share of  Merchandise exports (SPM), across 110 countries with complete data for both variables.
Visualisation. A labelled scatterplot (Figure 1) with the fitted regression line, highlighting a handful of outliers and well-fitting cases.


**Headline results**
estimated:

Intercept	2.521% 
nr_exports coefficient	−0.0126 (p = 0.018)
R²	0.051
N	110 countries

A rise in resource export share from 20% to 80% of merchandise exports is associated with roughly 0.756 percentage points slower annualised growth - a modest and statistically significant effect (5% level).

**Interpretation**

The result points the same direction as the original paper: more resource-dependent economies seemingly grew more slowly. But the relationship here is much weaker; R² is low(see limitations), and the effect size is modest  suggesting that perhaps the resource curse now has less grounding than in the 1970-89 period, though is not entirely absent in the 1995–2014 period.

**Repository contents**
NRsmarkdown.Rmd - fully commented R Markdown showing data cleaning, variable construction, regression and scatter plot
NRs95.xlsx, grcap.xlsx - World Bank export and growth data
figure1.png - scatterplot visualising the relationship between dependency on natural resources(spm) and growth
notes - full methodology, variable construction and approximation decisions, and important simplification and data caveats
README.md - this file


**Limitations**
Single-regressor model with no controls, which is a deliberate simplification for a first-year undergraduate project. Adding controls (e.g. institutional quality, initial income, trade openness) and especially controlling for previous growth as was done in the original paper(1960s growth), would be the logical next step to stress test the results given by this paper, and would also increase the R^2 value which was quite low in this piece. 

World Bank export categories use SITC revision 3, whilst Sachs and Warner used revision 1. the mapping used to approximate the original definition, and its imperfections, are detailed in NOTES.md.

the growth variable doesn't include economically active population rather the general population which may alter growth rate results in countries that have particularly strange relative economically active population ratios, E.g. many Sub-Saharan African nations such as Angola and developed nations such as Japan and Germany.

the SMP variable is as a percentage of merchandise exports rather than GDP which can over/understate reliance on natural resource exports or simply exports as a whole in an economy which can change the relation between the two variables. 

Countries with missing data across the four export categories, or even a single observation across the growth window, are excluded,  which happens to drop some of the most resource-dependent economies in the world (UAE, Equatorial Guinea) from the regression sample and leaves us with a sample size of only 110 out of the included 217 countries.



 


