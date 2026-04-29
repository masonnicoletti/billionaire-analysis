# Data Analysis


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

import os
import pymongo
import logging
from dotenv import load_dotenv

from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split, GridSearchCV
from sklearn.linear_model import ElasticNet
from sklearn.metrics import r2_score, mean_squared_error
```


```python
# Setup logger

logging.basicConfig(
    filename='../logs/data_analysis.log',
    level=logging.INFO, 
    format='%(asctime)s - %(levelname)s - %(message)s',
    filemode='w'
)
logger = logging.getLogger(__name__)

logger.info("Data Analysis Started")
```


```python
# Connect to MongoDB

try:

    # Load environment variables for MongoDB credentials
    load_dotenv()
    username = os.getenv("MONGO_USERNAME")
    password = os.getenv('MONGO_PASSWORD')
    
    # Establish MongoDB connection
    uri = f'mongodb+srv://{username}:{password}@cluster0.sssb7.mongodb.net/'
    client = pymongo.MongoClient(uri)
    logger.info("Connected to MongoDB")

    # Connect to database
    db = client["forbes"]
    logger.info("Created MongoDB database")

    # Connect to billionaires collection
    collection = db["billionaries"]
    logger.info("Created MongoDB collection")

except Exception as e:
    print(f"Error: {e}")
    logger.error(f"An error occurred: {e}")

```


```python
# Confirm connection to billionaires

collection.count_documents({})
```




    3109




```python
# Convert documents to pandas df

try:

    # Extract documents
    docs = list(collection.find({}))
    logger.info("Extracted documents from MongoDB")
    
    # Convert documents to a pandas df
    df = pd.DataFrame(docs)
    logger.info("Saved documents to dataframe")


except Exception as e:
    print(f"Error: {e}")
    logger.error(f"An error occurred: {e}")
```


```python
# Preview dataframe
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>_id</th>
      <th>rank</th>
      <th>name</th>
      <th>last_name</th>
      <th>source</th>
      <th>industries</th>
      <th>country</th>
      <th>gender</th>
      <th>age</th>
      <th>current_worth</th>
      <th>previous_worth</th>
      <th>image_url</th>
      <th>source_metadata</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>69eada0076de092fde9681ca</td>
      <td>1</td>
      <td>Elon Musk</td>
      <td>Musk</td>
      <td>Tesla, SpaceX</td>
      <td>[Automotive]</td>
      <td>United States</td>
      <td>M</td>
      <td>54</td>
      <td>413046544000</td>
      <td>416338889179</td>
      <td>https://specials-images.forbesimg.com/imageser...</td>
      <td>{'title': 'Forbes Billionaires List', 'subtitl...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>69eada0076de092fde9681cb</td>
      <td>2</td>
      <td>Larry Ellison</td>
      <td>Ellison</td>
      <td>Oracle</td>
      <td>[Technology]</td>
      <td>United States</td>
      <td>M</td>
      <td>81</td>
      <td>279881344000</td>
      <td>295673532565</td>
      <td>https://specials-images.forbesimg.com/imageser...</td>
      <td>{'title': 'Forbes Billionaires List', 'subtitl...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>69eada0076de092fde9681cc</td>
      <td>3</td>
      <td>Mark Zuckerberg</td>
      <td>Zuckerberg</td>
      <td>Facebook</td>
      <td>[Technology]</td>
      <td>United States</td>
      <td>M</td>
      <td>41</td>
      <td>259176673000</td>
      <td>264605418320</td>
      <td>https://specials-images.forbesimg.com/imageser...</td>
      <td>{'title': 'Forbes Billionaires List', 'subtitl...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>69eada0076de092fde9681cd</td>
      <td>4</td>
      <td>Jeff Bezos</td>
      <td>Bezos</td>
      <td>Amazon</td>
      <td>[Technology]</td>
      <td>United States</td>
      <td>M</td>
      <td>61</td>
      <td>239990255000</td>
      <td>243065809282</td>
      <td>https://specials-images.forbesimg.com/imageser...</td>
      <td>{'title': 'Forbes Billionaires List', 'subtitl...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>69eada0076de092fde9681ce</td>
      <td>5</td>
      <td>Larry Page</td>
      <td>Page</td>
      <td>Google</td>
      <td>[Technology]</td>
      <td>United States</td>
      <td>M</td>
      <td>52</td>
      <td>169543401000</td>
      <td>171000557476</td>
      <td>https://specials-images.forbesimg.com/imageser...</td>
      <td>{'title': 'Forbes Billionaires List', 'subtitl...</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Review length and NA entries

n_rows = len(df)
print(f"Number of rows: {n_rows}")

df.isna().sum()
```

    Number of rows: 3109





    _id                  0
    rank                 0
    name                 0
    last_name            0
    source               4
    industries           0
    country              1
    gender               2
    age                  0
    current_worth        0
    previous_worth       0
    image_url          926
    source_metadata      0
    dtype: int64



## Visualization


```python
# Plot billionaire age distribution

df_age = df[df["age"] != 0]
age_range = df_age['age'].max() - df_age['age'].min()
med_age = round(df_age['age'].median()) 

plt.figure()
plt.hist(df_age['age'], bins=age_range, color="Indigo")
plt.axvline(df_age['age'].mean(), linestyle="--", color="DarkOrange", label=f"Median Age: {med_age}")
plt.title("Age Distribution of Billionaires")
plt.xlabel("Age")
plt.ylabel("Number of Billionaires")
plt.legend()
plt.savefig("../figures/age_distribution.png")
plt.show()

logger.info("Visualize billionaire age distribution")

```


    
![png](data_analysis_files/data_analysis_9_0.png)
    



```python
# Plot the billionaire gender distribution

genders = df['gender'].value_counts()
male_pct = round(((genders['M']/len(df)) * 100), 1)
female_pct = round(((genders['F']/len(df)) * 100), 1)
#gender_labels = [f"Male\n({male_pct}%)", f"Female\n({female_pct}%)"]
gender_labels = ["Male", "Female"]

plt.figure()
plt.pie(genders, labels=gender_labels, colors=["RoyalBlue", "LightCoral"], autopct='%0.1f%%', explode=[0.05, 0])
plt.title("Gender Distribution of Billionaires")
plt.savefig("../figures/gender_distribution.png")
plt.legend()
plt.show()

logger.info("Visualize billionaire age distribution")
```


    
![png](data_analysis_files/data_analysis_10_0.png)
    



```python
# Plot the billionaire country distribution

countries = df['country'].explode()
country_counts = countries.value_counts()
other_countries = country_counts[country_counts < 50].index
countries_condensed = countries.where(~countries.isin(other_countries), "Other")
countries_counts_condensed = countries_condensed.value_counts()

plt.figure()
plt.pie(countries_counts_condensed, labels=countries_counts_condensed.index, autopct='%0.1f%%')
plt.title("Country Distribution of Billionaires")
plt.savefig("../figures/country_distribution.png")
plt.legend(bbox_to_anchor=(1.1, 1.1), loc='upper left')
plt.show()

logger.info("Visualize billionaire country distribution")
```


    
![png](data_analysis_files/data_analysis_11_0.png)
    



```python
# Visualize distribution of wealth

df_worth = df[df['current_worth'] != 0]

plt.hist(df_worth['current_worth'], bins=1000)
plt.title("Distribution of Wealth")
plt.xlabel("Net Worth")
plt.savefig("../figures/wealth_distribution.png")
plt.show()

plt.hist(df_worth['current_worth'], bins=1000)
plt.xscale('log')
plt.title("Distribution of Wealth (Log Scale)")
plt.xlabel("Net Worth")
plt.savefig("../figures/wealth_distribution_log.png")
plt.show()

logger.info("Visualize distribution of wealth")
```


    
![png](data_analysis_files/data_analysis_12_0.png)
    



    
![png](data_analysis_files/data_analysis_12_1.png)
    



```python
# Plot billionaire distribution across industries

industries = df['industries'].explode()
industry_counts = industries.value_counts()
other_industries = industry_counts[industry_counts < 60].index
industries_condensed = industries.where(~industries.isin(other_industries), "Other")
industry_counts_condensed = industries_condensed.value_counts()

plt.figure()
plt.barh(industry_counts_condensed.index, industry_counts_condensed.values, color="DarkGreen")
plt.gca().invert_yaxis()
plt.title("Billionaires by Industry")
plt.xlabel("Industry")
plt.ylabel("Number of Billionaires")
plt.savefig("../figures/industry_bar.png")
plt.show()

logger.info("Visualize distribution across industries")
```


    
![png](data_analysis_files/data_analysis_13_0.png)
    


## Quantificantion


```python
# Quantify Billionaire Wealth

total_bill_worth = df['current_worth'].sum()
print(f"Total Billionaire Wealth: ${total_bill_worth}")
```

    Total Billionaire Wealth: $17514890129932



```python
# Calculate confidence interval given 10% uncertainty per billionaire

net_worths = df['current_worth'].values

# Assume a 10% standard deviation
std_dev = 0.10 * net_worths

simulations = []

for _ in range(1000):
    simulated = np.random.normal(net_worths, std_dev)
    simulations.append(simulated.sum())

lower = np.percentile(simulations, 2.5)
upper = np.percentile(simulations, 97.5)

print(f"95% Confidence Interval: (${lower}, ${upper})")

logger.info("Estimated a 95% Confidence Interval of total billionaire worth")
```

    95% Confidence Interval: ($17338721449133.107, $17698890953295.047)



```python
# Bootstrapping to estimate 95% Confidence Interval

net_worth = df['current_worth'].values

n_boot = 1000
totals = []

for _ in range(n_boot):
    sample = np.random.choice(net_worth, size=len(net_worth), replace=True)
    totals.append(sample.sum())

lower = np.percentile(totals, 2.5)
upper = np.percentile(totals, 97.5)

print("Bootstrapping from the Billionaire Population")
print(f"95% Bootstrap Interval: (${lower}, ${upper})")

logger.info("Created bootstrap interval from the billionaire population")
```

    Bootstrapping from the Billionaire Population
    95% Bootstrap Interval: ($15826699609312.025, $19107100705111.65)



```python
# Calculate error bar

error_bar = [total_bill_worth - lower, upper - total_bill_worth]
```


```python
# Hard code other economic values

# Federal Reserve Economic Data
# https://fred.stlouisfed.org/release/tables?eid=813668&rid=453
# Total wealth held by the bottom 50 percent (Q3 2025)
bottom_50_worth = 10246860 * 1000000

# International Institute for Sustainable Development
# https://www.iisd.org/articles/iisd-news/ending-world-hunger-2030-would-cost-330bn-study-finds
# End world hunger by 2030
world_hunger = 330 * 1000000000

# Worldometer
# https://www.worldometers.info/gdp/gdp-by-country/?source=imf&region=europe&year=2025&metric=nominal
# Europe GDP (2025)
eu_gdp = 28.2 * 1000000000000

# Golden Eagle
# https://www.goldeneaglecoin.com/Guide/value-of-all-the-gold-in-the-world 
# Value of all the gold in the world
gold_value = 24397107332381.90

# Parramatta History and Heritage
# https://historyandheritage.cityofparramatta.nsw.gov.au/research-topics/world-war-two/world-war-two-financial-cost
# World War II financial cost (inflation-adjusted)
ww2_cost = 4.1 * 1000000000000

# Mercatus Center
# https://www.mercatus.org/research/working-papers/costs-national-single-payer-healthcare-system
# Universal Health Care (1 year)
health_care = (32.6 * 1000000000000) / 10
```


```python
# Assemble values to dictionary

worth_dict = {
    "Total European GDP": eu_gdp,
    "Total Billionaire Worth": total_bill_worth,
    "Bottom 50% Wealth (US)": bottom_50_worth,
    "WWII Total Cost": ww2_cost,
    "Universal Healthcare (1 Year)": health_care,
    "End World Hunger": world_hunger
}
```


```python
# Visualize total billionaire worth as a bar

total_worth_t = total_bill_worth / 1e12
err_low = error_bar[0] / 1e12
err_high = error_bar[1] / 1e12

label = f"${total_worth_t:.2f} Trillion"

plt.figure(figsize=(6, 8))
plt.bar(
    ["Total Billionaire Worth"],
    [total_worth_t],
    width=1, 
    yerr=[[err_low], [err_high]],
    capsize=8,
    error_kw=dict(ecolor='red', linewidth=2),
    label=label
)
plt.text(0, total_worth_t, f"{total_worth_t:.2f}", ha='center', va='bottom', fontsize=11)
plt.title("Total Billionaire Worth")
plt.ylabel("Trillions of Dollars")
plt.tight_layout()
plt.legend(title="Estimated Worth")
plt.savefig("../figures/total_billionaire_worth.png")
plt.show()

logger.info("Visualize total billionaire worth")
```


    
![png](data_analysis_files/data_analysis_21_0.png)
    



```python
# Visualize total billionaire worth in relation to other values

plt.style.use("seaborn-v0_8-darkgrid")

worth_trillions = {k: v / 1e12 for k, v in worth_dict.items()}
labels = list(worth_trillions.keys())
values = list(worth_trillions.values())
x = np.arange(len(labels))
colors = ["cornflowerblue" if i != 1 else "olivedrab" for i in range(len(values))]

plt.figure(figsize=(10, 6))
bars = plt.bar(x, values, width=0.6, edgecolor="black", linewidth=0.8, color=colors)

plt.title("Comparison of Economic Values", fontsize=16, weight="bold", pad=15)
plt.ylabel("Value (Trillions USD)", fontsize=12)
plt.xticks(x, labels, rotation=30, ha='right')
ax = plt.gca()
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

ax.grid(axis='y', linestyle='--', linewidth=0.6, alpha=0.6)
ax.set_axisbelow(True)

for bar in bars:
    height = bar.get_height()
    plt.text(
        bar.get_x() + bar.get_width() / 2,
        height,
        f"{height:.2f}",
        ha='center',
        va='bottom',
        fontsize=10
    )

plt.tight_layout()
plt.savefig("../figures/economic_comparison.png", dpi=300)
plt.show()

logger.info("Visualize economic comparison")
```


    
![png](data_analysis_files/data_analysis_22_0.png)
    


### Visualization Rationale

My primary motivation when creating data visualizations was to display the distribution of the characteristics of billionaires. The data had a limited number of features, so I was able to generate a visual for all of the relevant characteristics. The age distribution plot, I intended to capture how the world's wealthiest individuals compared in age, assuming that the majority of this population was older since it takes a long time to accumulate that level of wealth. The gender distribution and country distrubution plots were represented as pie charts to see how outcomes appear out of the whole. The countries and industries variables required the encoding of minority values into an "other" category to keep the output in a reasonable range without appearing cluttered. Overall, these distribution plots provided intriguing insights regarding the characteristics of the Forbes billionaires.

My secondary motivation was to design a visual that reveals how great the wealth of billionaires is. I got the idea to research and hard code the values of large economic phenomena. With these values, I would plot them side by side, along with the total worth of the 3.1 thousand individuals represented on the Forbes list. The comparison of Economic Values visualization was meant to serve as a fun but relatable visual that reminds people just how much wealth a small subset of people holds.

## Modeling


```python
# Process data for modeling

# Extract and one hot encode industries
df['industry'] = df['industries'].apply(lambda x: x[0] if isinstance(x, list) and len(x) > 0 else None)
industry_dummies = pd.get_dummies(df['industry'], prefix='industry')
df = pd.concat([df, industry_dummies], axis=1)

# One hot encode common countries
top_countries = df['country'].value_counts().nlargest(10).index
df['country_grouped'] = df['country'].apply(lambda x: x if x in top_countries else 'Other')
country_dummies = pd.get_dummies(df['country_grouped'], prefix='country')
df = pd.concat([df, country_dummies], axis=1)

# One hot encode gender
df['gender'] = df['gender'].map({'M': 1, 'F': 0})

# Drop unnecessary columns
df = df.drop(columns=['industries', 'industry', 'country', 'country_grouped', 'gender'])

logger.info("Completed data preprocessing for modeling")
```


```python
# Prep data

# Create a variable for change in wealth
df['worth_change'] = df['current_worth'] - df['previous_worth']
logger.info("Created target variable")

drop_features = ["_id", "rank", "name", "last_name", "source", "current_worth", "previous_worth", "image_url", "source_metadata", "worth_change"]

# Separate features and target
X = df.drop(columns=drop_features)
y = df['worth_change']
```


```python
# Perform train/test split

X_train, X_test, y_train, y_test = train_test_split(X, y, train_size=0.8, random_state=45)

logger.info("Completed train/test split")
```


```python
# Standardize features

scaler = StandardScaler()

X_train["age"] = scaler.fit_transform(X_train[["age"]])
X_test["age"] = scaler.transform(X_test[["age"]])
```


```python
# Modeling

try: 
    # Create Elastic Net model
    model = ElasticNet()

    # Define parameters to vary
    param_grid = {
        "alpha": [0.001, 0.1, 1, 10],
        "l1_ratio": [0, 0.1, 0.5, 0.9, 1]
    }

    # Set up Grid Search
    grid = GridSearchCV(model, param_grid, cv=4, n_jobs=1, verbose=0)
    warnings.filterwarnings("ignore", category=ConvergenceWarning)
    warnings.filterwarnings("ignore", category=UserWarning)

    # Fit grid search
    grid.fit(X_train, y_train)

    logger.info("Fit model on data")

except Exception as e:
    print(f"Error: {e}")
    logger.error(f"An error occurred: {e}")
```


```python
# Explore the best model

print("Best parameters:", grid.best_params_)
print("Best score:", grid.best_score_)

best_model = grid.best_estimator_
```

    Best parameters: {'alpha': 0.1, 'l1_ratio': 0}
    Best score: 0.024906873235873372



```python
# Predict on the test set and evaluate performance

pred = best_model.predict(X_test)

r2 = r2_score(y_test, pred)
mse = mean_squared_error(y_test, pred)

logger.info("Evaluate model performance on the test set")

print("MSE:", mse)
print("R2 Score:", r2)
```

    MSE: 3.544715944902727e+16
    R2 Score: 0.04895680590633955



```python
# Plot model predictions and actual values

plt.scatter(y_test, pred, alpha=0.6, color="blue")
plt.xlabel("Actual Values")
plt.ylabel("Predicted Values")
plt.title("Prediction Results")
plt.show()
```


    
![png](data_analysis_files/data_analysis_32_0.png)
    



```python
# Extract feature importances

feature_importance = pd.DataFrame({
    "Feature": X.columns,
    "Importance": best_model.coef_}
)

feature_importance = feature_importance.sort_values(by="Importance", ascending=False)

print(feature_importance.head(10))

logger.info("Determine feature importance")
```

                               Feature    Importance
    5        industry_Fashion & Retail  2.934377e+07
    21                   country_China  2.296130e+07
    24                   country_India  2.100167e+07
    7         industry_Food & Beverage  1.638618e+07
    3             industry_Diversified  1.363686e+07
    12  industry_Media & Entertainment  9.776876e+06
    23               country_Hong Kong  7.623938e+06
    14            industry_Real Estate  5.653303e+06
    1              industry_Automotive  5.489563e+06
    16                 industry_Sports  5.328185e+06



```python
# Visualize feature importance

top_features = feature_importance.head(7)

plt.figure(figsize=(10,7))
plt.barh(top_features['Feature'], top_features['Importance'], color="darkorange")
plt.gca().invert_yaxis()
plt.xlabel("Coefficient")
plt.title("Feature Importance - Random Forest")
plt.tight_layout()
plt.savefig("../figures/model_coefficients.png")
plt.show()

logger.info("Visualize model feature importance")
```


    
![png](data_analysis_files/data_analysis_34_0.png)
    


### Analysis Rationale

The goal of this analysis was to create a new variable, `worth_change`, and test the ability to predict it. This variable represents the change in worth of the billionaire, and it was calculated by finding the difference between `current_worth` and `previous_worth`. I found predicting the worth of the billionaire to be unproductive given how little data was available, and I assumed the predictive power of only several feature - age, gender, country, and industry - to be small. However, I was intrigued if it would be possible to find insights when predicting how the wealth of an individual changed within a short period of time. I was hoping this analysis would point to factors, such as particular industries or countries, that were affected by a sharp short-term increase or decrease in net worth for its represented billionaires. 

I decided to use a regression model to fit this data. Because many industry and country related features were added as predictors as a result of one hot encoding, I thought it would be useful to apply shrinkage in order to take care of this post-processed dataset with higher dimensionality and more noise. I employed an Elastic Net regressor in order to test the outcomes of both lasso (L1) and ridge (L2) regularization, as well as several weighted combinations in between. So, this also required a Grid Search to fit the model across a range of several parameters. The best performing model had a `l1_ratio` of 0, meaning that it only used Ridge Regression. Despite being the best performing model, it performed poorly, achieving a minute R2 score of 0.05, and a massive MSE. Overall, this was a failed experiment to predict the net worth change of a billionaire, as the model did not capture any significant relationships from the available data.
