
# OLS regression on cancer data

A ordinary least squares regression model applied on cancer data of US


## Data

 - [mortality data](https://data.world/nrippner/cancer-analysis-hackathon-challenge/workspace/file?filename=death+.csv)
 - [cancer incidence data](https://data.world/nrippner/cancer-analysis-hackathon-challenge/workspace/file?filename=incd.csv)
 - [poverty data](https://data.world/uscensusbureau/acs-2015-5-e-poverty)
 - [income data](https://data.world/uscensusbureau/acs-2015-5-e-income)
 - [health insurance data](https://data.world/uscensusbureau/acs-2015-5-e-healthinsurance)
 - [population data](https://data.world/nrippner/us-population-estimates-2015/workspace/file?filename=CO-EST2015-alldata.csv)


## Roadmap

- import data from each data source
- drop columns not relevant + format 'StateFIPS' and 'CountyFIPS'
```py
incddf = pd.read_csv('incd.csv')
incddf.rename(columns={' FIPS':'FIPS'}, inplace=True)
mortdf['FIPS'] = mortdf.FIPS.apply(lambda x: str(int(x)))\
                            .astype(np.object_)\
                            .str.pad(5, 'left', '0')
incddf['FIPS'] = incddf.FIPS.apply(lambda x: str(int(x)))\
                            .astype(np.object_)\
                            .str.pad(5, 'left', '0')
```
- merge dataframes
- dxploratory data analysis: convert columns with string data to numeric + convert categorical data to dummy variables
- visual exploratory analysis - observe correlation betwen each variables and drop those redundant
- build linear regression model
- check multicollinearity, distribution and heteroscedasticity

