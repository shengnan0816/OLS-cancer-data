
# OLS regression on cancer data

A ordinary least squares regression model applied on cancer data of US

## Data

 - [mortality data](https://data.world/nrippner/cancer-analysis-hackathon-challenge/workspace/file?filename=death+.csv)
 - [cancer incidence data](https://data.world/nrippner/cancer-analysis-hackathon-challenge/workspace/file?filename=incd.csv)
 - [poverty data](https://data.world/uscensusbureau/acs-2015-5-e-poverty)
 - [income data](https://data.world/uscensusbureau/acs-2015-5-e-income)
 - [health insurance data](https://data.world/uscensusbureau/acs-2015-5-e-healthinsurance)
 - [population data](https://data.world/nrippner/us-population-estimates-2015/workspace/file?filename=CO-EST2015-alldata.csv)

## Data preprocessing and preparation for regression model

- preprocess each data file and export for later use
    - drop empty FIP records because I need FIP to join with other data files 
    - drop columns irrelevant 
    - code is saved in a seperate file with name 'data preprocessing' to make the main code file 'regression' clean
    - format 'StateFIPS' and 'CountyFIPS'
    ```py
    # incidence data example
    incddf = pd.read_csv('incd.csv')
    incddf.rename(columns={' FIPS':'FIPS'}, inplace=True)
    mortdf['FIPS'] = mortdf.FIPS.apply(lambda x: str(int(x))) .astype(np.object_).str.pad(5, 'left', '0')
    incddf['FIPS'] = incddf.FIPS.apply(lambda x: str(int(x))).astype(np.object_).str.pad(5, 'left', '0')
    ```
- merge dataframes on column FIPS
```py
dfs = [povdf, incomedf, hinsdf, incddf, mortdf]
for i, j in enumerate(dfs):
    if i == 0:
        fulldf = j.copy()
    else:
        fulldf = fulldf.merge(j, how='inner', on='FIPS')
```
- convert columns with string data to numeric (last 3 variables have same information so I decide to keep one only)
    - Mortality_Rate    
    - Med_Income
    - Incidence_Rate
    - Avg_Ann_Incidence
    - Avg_Ann_Deaths
- convert categorical data to dummy variables
    - RecentTrend
- visual exploratory analysis 
    - observe correlation between each variables and drop those redundant
    ```py
    X.drop(['M_Poverty_PC', 'F_Poverty_PC'], axis=1, inplace=True)
    X.drop(['M_With_PC', 'F_With_PC'], axis=1, inplace=True)
    X.drop(['M_Without_PC', 'F_Without_PC'], axis=1, inplace=True)
    ```

## linear regression model and result

- build linear regression model
    - initial attemp with 11 variables returned 0.738 R^2
- check multicollinearity (VIF)
    - drop variables have VIF>100 then 7 variables left
- residuals distribution  
    - residuals distribution is left skewed and has larger kurtosis
![image](https://user-images.githubusercontent.com/34575198/212569182-74d6b26c-f755-4d6e-b2b7-2362dded335a.png)
    - QQ plot confirms that residuals distribution more similar to T distribution istead of normal distribution, which means more data from upper/lower extreme values   
![image](https://user-images.githubusercontent.com/34575198/212569664-300c3f90-8706-406a-adf3-7379eb7cae50.png)

- heteroscedasticity
    - This chart suggests that the model tends to somewhat overshoot lower values, and undershoot higher ones.   
![image](https://user-images.githubusercontent.com/34575198/212569770-57ef3646-cb7f-41eb-92b7-5a7583121511.png)
    - The plot of residuals versus fitted values shows that the residuals are fairly symmetric relative to the fitted values.
![image](https://user-images.githubusercontent.com/34575198/212569794-a2999061-2748-4cb1-b8b1-8fbe4a3ff889.png)
