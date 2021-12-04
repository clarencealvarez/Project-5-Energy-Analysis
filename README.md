# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 5: Panel Ordinary Least Squares Regression

![](https://www.ipsp.org/wp-content/uploads/2016/10/worldbank-banner.png)

## Problem Statement

In this project, the project team, comprised of Annelise Holverstott, Nnenna Isigwe, and Clarence Alvarez, assume the roles of data scientists for The World Bank. As part of Advisory Services and Analytics, the World Bank provides analytical reports, policy notes, hands-on advice, knowledge-sharing workshops, and training programs to support external clients' development goals. Our client is the World Health Organization, which requested a study be done on the effects of energy consumption on life expectancy.

We aim to explore the relationship between these two factors, with the goal of adding urgency to conversations around global warming and sustainable development. We focused our analysis on two subsets of our data: least developed countries and oil exporting countries.

## Technical Requirements

In order to run the notebooks in this repo, module [linearmodels](https://pypi.org/project/linearmodels/) must be installed. Documentation for the models used may be found in the [linearmodels github](https://bashtage.github.io/linearmodels/).

## Data

Data for this project was retrieved from the [The World Bank](https://data.worldbank.org/indicator/SP.DYN.LE00.IN) and [U.S. Energy Information Administration](https://www.eia.gov/international/data/world) websites. Initially, we retrieved data for 231 countries and territories spanning 39 years, totaling 9,240 observations. Additionally, we retreived data on GDP, population, production/consumption of various resources, imports/exports of said resources, and emissions. The total number of 30 features were narrowed down to just those relating to emissions and energy intensity for modeling.

Below are the units used in modeling:

|Feature|Units of Measurement|
|---|---|
|Emissions|Expressed in millions of tons of CO^{2}|
|Energy Intensity|Consumption per capita; Expressed in millions of British thermal units per person|

Emissions data include four sources of emissions: CO^{2}, coal and coke, consumed natural gas, petroleum and other liquids.
British thermal unit is defined as the amount of heat required to raise the temperature of one pound of water by one degree Farenheit.

After cleaning the data, the resulting data was comprised of 173 countries and territories, spanning 27 years, totaling 4,698 observations.

## Model Selection

Considering the project goal, we anticipated that a variation of Ordinary Least Squares regression would be the appropriate model. However, it took some exploring before the team arrived at the right model.

We attempted to model the data using a VAR model (vector autoregression model), which is a time-series model similar to ARIMA. Like ARIMA, it usually has the parameters d, p, and q. However, it was not possible to model our data with VAR. First, our data was not completely stationary after taking the difference three times. Moreover, the covariance matrix for calculating AIC, p, and q included some negative values, which made those calculations impossible.

After determining that the structure was in panel form, we explored regression models that would take multi-indexed dataframes, such as ours. Ultimately, we learned about and deployed Panel OLS model. The team followed the steps outlined in Bernard Brugger's [towards data science article](https://towardsdatascience.com/a-guide-to-panel-data-regression-theoretics-and-implementation-with-python-4c84c5055cf8) during this process.

## Analysis

|   |World *(Fixed Effects)*|Oil Exporters *(Fixed Effects)*|Least Developed Countries *(Random Effects)*|
|---|---|---|---|
|CO^{2}|0.0228\*\*\*|0.0152\*\*\*|-0.0157|
|Coal/Coke|-0.0246\*\*\*|-0.0158\*\*\*|0.0556|
|Petrol & Other Liquids|-0.0127\*\*\*|-0.0112\*\*\*|1.0592\*\*\*|
|Energy Consumption per Capita|0.0120\*\*\*|0.0050|0.1368\*\*\*|
|R-Squared (Overall)|8%|5%|13%|
\*\*\* denotes significance at 1% level of significance

## Conclusion

- Yes, energy consumption does affect life expectancy. However there are nuances, with regards to pollutant, and country's level of development.
- We explored the relationship between energy consumption and life expectancy for 172 countries from 1992-2018, using fixed effects and random effects models. 
- Emissions from petroleum, coal and coke were found to have a significant, negative effect on life expectancy, while C02 emissions showed a significant positive relationship with life expectancy. However, this did not hold in the specific case of Least Developed Countries (LDCs).
- CO^{2} emissions showed a significant, negative relationship with life expectancy in LDCs, whereas emissions from coal,coke and petroleum showed a significant, positive relationship. 
- Energy consumption per capita showed a significant positive effect on life expectancy, worldwide, as well as in the two subsets analyzed.

## Recommendation

- We recommend that there is need to study this relationship on more micro levels, for instance, using more granular data, including more features which explain life expectancy, longer time span to better understand impacts of energy consumption/efficiency on life expectancy.
- There is need for more research into use of coal/coke and petroleum in LDCs. How can we get these countries to move towards cleaner forms of energy? This is especially important given that the goal of sustainable development is a collective one.