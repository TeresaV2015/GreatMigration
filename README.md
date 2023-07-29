# StatePopulation-HPI-Income
Probability and Statistics 1's Final Project

This project, fondly called "The Great Migration," was a group project completed for my Probability and Statistic 1 class (Winter 2022) at the University of Denver. 

Note that the bulk of this README can be found in the code (along with formulas and graphs). 

## Research Question and Significance of Research
Do more people move based on cost of housing and income during times of economic uncertainty? Times of economic uncertainty in this timespan relate to the Great Recession (2008-2010) and, more recently, the impacts of COVID-19. While there are other conditions that cause economic hardship for different groups of people, everyone was affected by the two events stated earlier. Our research question was: "Do more people move based on cost of housing and income during times of economic uncertainity?" Based on the inter-state migration of people in recent years, we are hypothesizing that the population for each fluctuates based on the Housing Price Index and income. 

Two members of the group live in states where there has been a huge influx of people from areas with higher housing costs recently, thus impacting housing prices in the new areas. By understanding this phenomenon via testing the hypothesis that more people are moving during times of economic uncertainty, like the COVID-19 pandemic, we may be able to better predict how housing costs impact population growth in each state. On a smaller level, this could help prepare people for what to expect in future housing searches. However, on a larger level, this could help people in the Planning, Development, and Survey sectors plan for more efficient types of infrastructure needed to support this influx of people.

## Datasets Used
The final dataset used for analysis consists of mean Housing Price Index, median income and population for each state for the years 2000-2020. The inputs for this final dataset consists of 5 separate datasets described below. Since most states have different costs of living, income, and data, the data was sorted by state to provide similar context for each data point.

The data for the Housing Price Index (HPI) was from the Master HPI database on the Federal Housing Finance Agency’s website (https://www.fhfa.gov/DataTools/Downloads/Pages/House-Price-Index-Datasets.aspx#mpo). You can find it on the repository as "HousingPriceIndex.csv". Note that the HPI is a measure of the average cost of homes on a monthly basis in thousands of dollars.

The data for the median house income by state was taken from the U.S. Census Burea, specifically the table H-8 Median Household income by State from https://www.census.gov/data/tables/time-series/demo/income-poverty/historical-income-households.html. The file is saved as h08.xlsx on the repository. 

The population data for 2000-2020 was broken in three tables in the U.S. Census Bureau website. The data for 2000-2010 is from: https://www.census.gov/data/tables/time-series/demo/popest/intercensal-2000-2010-state.html. The data for 2010-2019 is from: https://www.census.gov/data/tables/time-series/demo/popest/2010s-state-total.html. Finally, the data for 2020 is from: https://www.census.gov/data/tables/time-series/demo/popest/2020s-state-total.html. The files are saved as "st-est00int-01.xlsx", "nst-est2019-01.xlsx", and "NST-EST2021-POP.xlsx", respectively. 

## Data Preparation
A lot of the datasets used in this project needed to be cleaned and organized in order to get their values to line up with each other correctly. Within the HPI data, the column names needed to be matched with the new data frame and sorted by year. For the income data, we had to pull specific columns to exclude the standard error columns for each year and exclude the row containing the United States. We included only the years 2000-2020. There were also two “tables” in that excel file - one in “current dollars” and one in “2020 dollars”. We went with the “current dollars” table since the HPI would be in the current dollars as well.

The majority of the location exclusions came in the census data, because the census includes territories such as Puerto Rico in the data while the HPI only included states and Washington, D.C. Cleaning and compiling the population data was a bit more tedious than the income data despite being from the same source. Not only was the population data split between three files, but there were also inconsistencies between the population state labels and the labels in the previously established dataset. There were some extra characters in the “State” column in each file. In order to get the population values to match the income and HPI data for each state, the data had to be cleaned in order to remove these excess characters and establish consistency between the "State" columns.

We dropped Washington, D.C. since it was a big outlier and has heavily skewed the mean HPI data for places with lower populations. 

## Data Visualization
We visualized the data by generating QQ plots for mean HPI, median income, and state population. We could see that the data is not normally distributed across each variable.

## Methodology behind the Model
A linear model and a time series graph was used to get a clear picture of growth trends within the mean HPI, median income, and total population for each state from 2000 to 2020. The linear model is a model for a continuous outcome Y (i.e. population) based on the covariates X (i.e. mean HPI and median income), and can be used to fit linear models to data frames. 

The function “summary()” of the linear model in R is used to obtain and print a summary and analysis of the variance table of the results. R-squared values, p-values for co-variances X, and the overall p-value are included in the summary. The null hypothesis for the linear model is that the individual states’ mean HPI and median income have no impact on their respective states’ populations.

The time series graph can be used to accurately show past data trends based on units of time, as well as forecast future statistics based on previous results. When it comes to forecasting and determining its error, there are multiple methods that can be used. While some are more accurate than others based on the application, in our case it was best to use Holt’s method. Holt’s method is found to be more accurate because it generates forecasts by providing more weight to recent trends as opposed to past observations. Given that the time period of our data spans two decades, it was important to find a forecast that takes all of the data into consideration. The method used in determining the error of the forecast is Mean Absolute Percentage Error (MAPE). MAPE measures the performance of regression models in a percentage.

The lower the MAPE score, the more accurate the forecast is. Fortunately, running the “summary()” function on the Holt model within R also generates a MAPE score in addition to other error metrics for comparison, eliminating the need to calculate it by hand.

## Data Analysis
The linear model for state population with the co-variances X of mean HPI and median income has the p-value of 0.1885, a very low Multiple R-squared (0.00312), and a very low adjusted R-squared value (0.001253), which means we cannot reject the null hypothesis that the individual states’ mean HPI and median income has no impact on the respective state population. Based on this non-rejection of the null hypothesis, we decided to explore the impact of the states’ mean HPI by their respective median incomes and populations. The summary of the new linear model (output 9) with these adjusted parameters has a much higher Multiple R-squared value (0.4739) and Adjusted R-squared value (0.4729) in addition to the super low p-value of 2.2e-16. Since the p-value is below the 0.05 threshold, we can reject the null hypothesis that the states’ median incomes and populations have no impact on the respective states’ mean HPI. Furthermore, there were three stars (aka p-value of 0) for median income but no stars (aka p-value above 0.05) for the population, which indicate that the median income is a significant factor on the HPI whereas the population isn’t.

Based on the time series graphs, you can see that the growth of both HPI and income decreased and slowed at the start of the recession. However, when it came to the growth of population by state, it’s evident that growth remained the same without any of the slowing or decreasing we saw with HPI and income.

Since our initial hypothesis that state populations fluctuate based on HPI and income was incorrect, we decided to forecast the HPI and income for the state of Alabama for the next five years using Holt’s method. In doing so, this paints a picture of what people can expect to happen with their housing costs and if their income is projected to keep up. You can see that both HPI and income are expected to keep going up. However, the HPI is increasing approximately 5.5% annually, while the median income is increasing much slower, approximately 1.5% annually.

## Model Evaluation
Linear model is a good choice to evaluate the impact of states’ population by mean HPI and median income (and after initial analysis, the impact of states’ mean HPI by median income and state population), because it allows us to see which covariance X has a significant contribution/impact (if any) on the Y.

For time series forecasting, using the Holt model is a good choice to evaluate the future values of mean HPI and median income. The MAPE score associated with the forecasting was 1.61% for the HPI and 4.25% for the income which tells us that there’s a small percentage of error occurring in the outputs. In other words, the forecast is fairly accurate. If we had used the Naive method, the associated MAPE scores would be larger. 

## Conclusion
Considering the initial question refers to people moving, it was expected that the population for each state would fluctuate more during the years affected by economic uncertainty due to loss of jobs, housing affordability, and other issues related to lower income. However, as you can see by the linear models in the appendix (outputs 8 and 9), change in population is not significantly related to HPI or income. We came up with a second hypothesis: the mean HPI is affected by the median income and state population. The linear model’s summary indicated that the median income is a significant factor whereas the population isn’t. This paints a picture that HPI and income are more correlated with each other than population growth.

Even though we found that the cost of housing is not tied to population fluctuation, the time series’ forecast is still a relevant tool that can be useful in helping predict trends. In this instance, twenty years of the mean HPI and median income were used to forecast the next five years of HPI and income via Holt’s Smoothing Method for the state of Alabama. In doing so, it provided additional insight into the disproportional growth of housing costs compared to income within Alabama.
