# Project A/B testing:
Evaluation of a new recommendation system through statistical comparison of metrics in two groups to decide on the rollout of the new system to all users.

## Description

This project focuses on analyzing delivery data, with an emphasis on data preprocessing, reading files, and conducting a preliminary data study. The analysis revolves around selecting specific metrics for testing, including:

- Number of order cancellations
- Average number of orders per user
- Average cost of an order
- Average order size

These metrics aim to provide insights into the operational efficiency and customer behavior related to deliveries.

## Key Sections

1. **Test 1. Number of order cancellations**  
   Examines the statistical significance of differences in order cancellations between control and experimental groups. Using (Chi-square)

2. **Test 2. Average number of orders per user**  
   Analyzes the average number of orders placed by users, aiming to understand user engagement and ordering patterns. Using (Student's t-test)

3. **Test 3. Average cost of an order** 
   Analyzes the average order values. This indicator is primarily important for the business, as an increase in this indicator promises higher profits. Using (Mann-Whitney U test)

4. **Test 4. Average order size**  
   Focuses on the average size of orders, requiring data transformation for analysis. This metric is crucial for understanding customer purchasing behavior and order fulfillment  efficiency. Using (Student's t-test)

## Technical Overview

- **Number of Code Snippets:** 26
- **Number of Visualizations:** 7

The notebook contains a series of code snippets that perform data preprocessing, analysis, and visualization. The visualizations play a key role in understanding the data and communicating findings related to the key metrics of interest.

## Major Libraries Used

- pandas
- pingouin
- statsmodels.api 
- scipy.stats
- seaborn
- matplotlib.pyplot


## Getting Started

To run this project, you will need a Python environment capable of executing Jupyter notebooks. The analysis relies on standard data science libraries such as pandas, numpy, and matplotlib for data manipulation and visualization.

## Conclusion

The new recommendation system significantly influenced users' desire to place orders more frequently, which is a positive sign. The data for the other metrics could have changed, and sometimes even for the worse, but no statistically significant differences were found for such data in the groups. Accordingly, we cannot say that the new recommendation system has negatively affected the selected metrics.
