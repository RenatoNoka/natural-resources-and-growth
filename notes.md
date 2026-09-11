**Methodology Notes accompanying my Natural Resources and Growth project**

Detailed notes on decisions and construction/approximation of variables and results supporting the summary in README.md.



**Variable 1: Approximating Sachs & Warner's SXP (1995) - SPM**

Since this project's version measures the sum as a share of merchandise exports rather than GNP (as in the original), it's labelled SPM (share of primary products in merchandise exports) rather than SXP, to avoid implying a replication.

Sachs and Warner's original resource-dependence variable, SXP, was defined as:

Share of exports of primary products in GNP in 1970. Primary products (natural resource exports) are exports of "fuels" and "non-fuel primary products"  corresponding to SITC revision 1  categories 0, 1, 2, 4, and 68.

SITC categories as of revision 3:

0	Food and live animals
1	Beverages and tobacco
2	Crude materials, inedible, except fuels
3	Mineral fuels, lubricants and related materials
4	Animal and vegetable oils, fats and waxes
68	Non-ferrous metals

The World Bank's contemporary data uses SITC revision 3, not revision 1, so an exact replication isn't possible. Available World Bank series were used to map SPM as closely as possible:

World Bank series in SPM and which	SITC categories are covered 
Food exports (% of merchandise exports):	0, 1, 4, and 22 
Fuel exports (% of merchandise exports):	3
Agricultural raw materials exports (% of merchandise exports):	2, excluding 27 and 28
Ores and metals exports (% of merchandise exports):	27, 28, 68


Summing up, these four series present in our SPM approximationcovers SITC 0, 1, 2, 3, 4, and 68 - matching the original definition of SXP - but also picks up categories 22 (oil seeds, oil nuts, oil kernels), 27 (crude fertilisers/minerals, excl. coal/petroleum/precious stones), and 28 (metalliferous ores and scrap), which the original definition excluded. These additions are arguably reasonable under a broad definition of "natural resources," but it does create some distance between SXP and SPM which has the scope to alter results. (Ferrous metals were likely excluded from the original SITC list because they are usually already processed and or refined since they refer mostly to alloys including iron, unlike raw ores such as iron ore itself.)

Sample and cleaning
All 217 economies tracked by the World Bank were considered (a broader set than the 193 UN members, including territories and special administrative regions such as Macao). Countries with any missing value across the four 1995 export series (and later further cleaned if missing growth data) were dropped rather than backfilled from another source, to avoid mixing data collected under different methodologies.

In the original paper, Sachs and Warner noted their methodology led to 10 countries (Cape Verde, Gambia, Bahrain, Iran, Kuwait, Oman, Philippines, Vanuatu, Libya, Kiribati) producing over 100% dependency. Note, however, that no capping was necessary in this sample.

**Variable 2: Annualised GDP per capita growth (1995–2014)**

Sachs and Warner's original growth variable was defined as such:

Average annual growth in real GDP divided by the economically active population between 1970 and 1990: 100 × (1/20) × ln(GDPEA90/GDPEA70) 

This project uses total population rather than economically-active population (ages 15–64) as a simplification, to avoid mixing in an additional dataset. Otherwise, the calculation follows the same logic which involves taking into consideration the impact of compound growth effects using logarthimic functions(rather than a perhaps intuitive arithmetic average), applied to 1995–2014 World Bank annual growth-rate data (constant 2015 USD) rather than a two-point start/end (1970 and 1989) level series of absolute GDP levels as was originally used.

an intuitive explanation of the exact code can be found in the markdown, but the method used is as follows: 

100 * rowMeans(log(1 + growth/100))

Which is mathematically equivalent to Sachs and Warner's endpoint-based formula.

At first, i used a simple arithmetic mean of the yearly percentage growth rates. However i later amended the method, since it would overstate true compounded growth, particularly for volatile economies. This is because a year of +100% growth followed by −50% growth nets to zero overall change, which the log-based method correctly returns as 0%, while an arithmetic mean would incorrectly show +25%, therefore the arithmetic mean method would not give a result applicable to this situation (rather an "average rate")

an interesting note is that every country with complete 1995 export data also had complete 1995–2014 growth data, so no additional countries were dropped at the merge stage. This could be because industry specific export data is harder to collect than growth data(or perhaps is less important) and so countries that have the more intricate and difficult to obtain data in exports by industry, will usually have the easier easier and more foundational growth data (but not necessarily the other way around).

**Regression and Results discussion**

Simple OLS: log_growth ~ nr_exports, with no additional controls — a deliberate simplification appropriate for a first-year undergraduate project. Adding controls (e.g. initial income, institutional quality, trade openness and especially previous growth) to check robustness is left for after completing further econometrics modules, but would greatly improve this project.

below are the summarised results from the R code: 

Residuals:
    Min      1Q  Median      3Q     Max 
-3.1133 -0.9983 -0.0708  0.7235  6.0631 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept)  2.521248   0.193794  13.010   <2e-16 ***
nr_exports  -0.012643   0.005275  -2.397   0.0183 *  

Residual standard error: 1.499 on 108 degrees of freedom
Multiple R-squared:  0.0505,  Adjusted R-squared:  0.04171 
F-statistic: 5.744 on 1 and 108 DF,  p-value: 0.01826
Results and discussion

The regression suggests a moderate (depending on standpoint of the reader) -and statistically significant at the 5% level- negative relationship between 1995 resource export share and subsequent (1995–2014) annualised GDP per capita growth (β = −0.0126, p = 0.018, R² = 0.05, n = 110). Putting this into context, A move from 20% to 80% resource dependence is associated with roughly 0.756 percentage points slower annual growth.

In figure 1 most data points are shown to be concentrated at between 0 and 4% growth and under 30% reliance which is common for many diversified economies. Relative to Sachs and Warners data, perhaps a few more outliers are seen, which may be as a result of our choices in collating these variables. However the main pattern follows that countries with higher NR dependency do in fact grow slower. 


Some notable observations in the data are seen below:
China - the fastest grower in the plotted sample, with resource dependence of only around 15%. 
Central African Republic - the slowest-growing plotted country, with 54% resource dependence.
The UAE - not plotted due to incomplete export data, but known to have extremely high oil dependency and suffered from the third greatest decline in average gdp per capita of -1.671%, and when World Bank records began in 1999, dependence was about 96%.
Equatorial Guinea -  again not plotted but (IMF,1997) estimated over 90% NR dependency in 1996. Interestingly, however, they actually had the highest average growth rate in the uncleaned data which could be considered an outlier to Sachs and Warner's Resource Curse hypothesis that primary product dependent countries usually dont experience rapid, "sustained growth". However, in 2026 the World Bank estimates their gdp per capita to be one third of its 2008 levels, and so this may be said to be short and not sustained. 

examples of notable slow-growing, high-dependence cases in the sample which may be suffering from the resource curse include: Zimbabwe, Saudi Arabia, Jamaica, Madagascar, and the Gambia.

**Takeaway**
The resource curse still has some empirical backing in this contemporary window, but the relationship is considerably weaker than the original 1970–90 result, both in effect size and explanatory power (an R² value of 0.05 is low compared to the 79% seen in sachs and warner 2001). Whether this reflects erosion of the curse over time or is as a result of the liberties and approximations taken in this project, is an open question; one a more robust regression and variables could help answer.

**Limitations and possible extensions**

No additional regressors (see Regression, above).
SITC revision mismatch between the original (rev. 1) and World Bank contemporary data (rev. 3) means SPM approximates SXP rather than replicating it exactly (see Variable 1, above).
Complete-case filtering excludes some of the most resource-dependent countries in the world (UAE, Equatorial Guinea, ) from the regression, because their data doesn't span the full window used here potentially therefore not being representative of the wider population outside the sample.
Growth is measured relative to total population rather than the economically active population used in the original paper, as a simplification.

**References:**

International Monetary Fund (1996) Guinea: recent economic developments. IMF Staff Country Report No. 96/148. Accessible from: https://www.elibrary.imf.org/view/journals/002/1996/148/article-A001-en.xml [Accessed 3rd September 2026].

Sachs, J.D. and Warner, A.M. (1997) Natural resource abundance and economic growth. [Revised version of NBER Working Paper No. 5398, 1995]. Accessible from: https://www.earth.columbia.edu/sitefiles/file/about/director/documents/NaturalResourceAbundanceandEconomicDevelopmentwithWarner-1997.pdf [Accessed 1st August 2026].

Sachs, J.D. and Warner, A.M. (2001) 'The curse of natural resources', European Economic Review, 45(4–6), pp. 827–838. Accessible from: https://www.sciencedirect.com/science/article/pii/S0014292101001258 [Accessed 9th August 2026].

World Bank (2026) World Development Indicators. Accessible from: https://databank.worldbank.org/source/world-development-indicators [Accessed 24th August 2026].



