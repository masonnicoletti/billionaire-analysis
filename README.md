# DS 4320 Project 2: Billionaire Analysis

## Project Details

### Executive Summary

### Project Details

| Spec | Value |
|------|-------|
| Name | Mason Nicoletti |
| NetID | cxx6sw |
| DOI | |
| Press Release | |
| Pipeline | |
| License | |

---

## Problem Definition

### Problem Statement

**General Problem:** Quantifying the wealth of billionaires.

**Specific Problem:** Quantifying the distribution of the wealth among the population of the world's billionaires to a dollar amount and evaluating demographic factors associated with billionaire status.

### Motivation

Wealth inequality is sometimes regarded as one of the most divisive and systemically problematic issues affecting the world. And despite being such a well-recognized problem, the level of wealth inequality is increasing quite significantly without signs of slowing down. This project aims to explore the sheer amount of wealth, held by individuals at the threshold above billionaire, that exists in the world. Many individuals who are billionaires are elevated to celebrity status. Their wealth often puts them in positions where they can exert notable influence over processes that affect common people across the world. Wealth is often tied to power and influence, and it is important to understand how this wealth is diversified across its holders. However, wealth is hard to quantify as there is much uncertainty that exists in accurately estimating the worth of an individual. This project seeks to tackle this issue as well as quantify the value held by the top 0.00003%.

### Rationale

The original problem was framed with regards to considering a comprehensive list of billionaires and their calculated net worths. This general statement lacks specificity regarding how that wealth is quantified, and it has a incomplete definition of the population captured in this analysis. The specific problem was refined to fully leverage the information sourced from the Forbes World's Billionaires List. With this data, there is the potential to explore demographic factors such as age and gender in order to visualize how the ultra-wealthy are distributed among these features. Futhermore, an analysis could look into industries and sources of wealth associated with billionaire status, and determine which areas produce the most significant wealth. Overall, this problem reframing will prove useful by establishing an effective overview of the variables for which wealth is concentrated.

### Press Release


---

## Domain Exposition

### Terminology

| Term | Description |
|------|-------------|
| Bismarckian Welfare | The modern world's first state-led social insurance system |
| Centibillionaire | An individual with a net worth exceeding $100 Billion |
| Egalitarianism | Philosophical branch asserting the fundamental equality of all humans | 
| Financial Asset | Intangible, liquid property with value derived from contractual claim |
| Gini Coefficient | Statistical Measure of economic inequality (0 to 1) within a population |
| Intergenerational Transmission | The transfer of wealth from older to younger generations |
| Lorenz Curve | Graphical representation of income or wealth inequality in a population |
| Macroeconomic | The overall economy or market system, focusing on large-scale phenomena |
| Net Worth | The sum of all assets minus all liabilities |
| Non-Financial Assets | Physical item with value derived from utility |
| Philanthropy | Voluntary donation of personal income to the common good |
| Redistributive Policies | Government actions that transfer wealth to reduce economic inequality |
| Social Mobility | The ability to move between socioeconomic strata in a society | 
| UHNWI | Ultra High Net Worth Individual; an individual with a net worth exceeding $30 Million |
| Wealth Concentration | Uneven distribution of assets across a population |

### Domain Description

This project exists in domains concerned with finance, macroeconomics, and wealth distribution. First, the achievement of billionaire status is highly tied to financial decision-making, particularly investments and capital appreciation. Additionally, the determination of the Forbes World’s Billionaires ranking requires sufficient financial literacy in order to identify and aggregate the value of diverse assets and estimate overall net worth. Further, the economy is highly influenced by billionaires, as their business activity and personal decision-making can shape larger economic developments. From a macroeconomic perspective, this domain relates to the study of wealth and the distribution of resources across populations, providing insight into how extreme wealth compares to national and global economic indicators. Considering the concept of wealth distribution, this project highlights disparities in the concentration of wealth among a small proportion of individuals relative to the overall population. Understanding this domain requires knowledge of financial systems, economicas, and the social implications of wealth concentration.

### Background Reading

**Background Reading Folder:**
[Billionaire Analysis Domain Exposition](https://myuva-my.sharepoint.com/:f:/g/personal/cxx6sw_virginia_edu/IgD5DZS1v4NNRKqkYqB2-DgrAbpXC2J3SuYi9JWCt3YZCLM?e=TXutdr)

| Title | Description | Link |
|-------|-------------|------|
| Forbes World’s Billionaires List: Facts And Figures | Describes the constituents of Forbes' ranking of billionaires, highlighting significant updates as of 2026 | [Forbes World's Billionaires List](https://myuva-my.sharepoint.com/:u:/g/personal/cxx6sw_virginia_edu/IQBCQjU-ET5JSZjfLcVRC-NQAUbX8USOMoypy8jwHBrwVGk?e=qSRHDZ) |
| World Ultra Wealth Report 2025 | Defines individuals classified as ultra-wealthy, and outlines the profile of this group of individuals | [World Ultra Wealth Report 2025](https://myuva-my.sharepoint.com/:u:/g/personal/cxx6sw_virginia_edu/IQAUZNYuynPyS4tEndfFtPhJAV6g-qX_PQ-nKWzVcezWPKs?e=hsoyE3) |
| Characteristics of Billionaires | Presents an analysis of billionaires based on demographic characteristics, with supporting data visualizations and modeling | [Characteristics of Billionaires](https://myuva-my.sharepoint.com/:u:/g/personal/cxx6sw_virginia_edu/IQC1kYEqBfGnQIIObmA5v0PeASohgBpWe7GsSgC_HvJlzgc?e=zYTQIO)|
| How Wealth Inequality Shapes Our Future | Provides a narrative account of the impact of wealth inequality, then dives into theory and analysis of this issue | [How Wealth Inequality Shapes Our Future](https://myuva-my.sharepoint.com/:u:/g/personal/cxx6sw_virginia_edu/IQCtqAhK748nRo2z_uC7XBAPAUpTgr0IMd3cCNd2Xwp5kl4?e=tmlyUK) |
| Wealth Without Limits: in Defense of Billionaires | Defends the existence of billionaires and justifies their positive impact across society | [Wealth Without Limits in Defense of Billionaires](https://myuva-my.sharepoint.com/:u:/g/personal/cxx6sw_virginia_edu/IQC4TLBPpWh3SbNUjg7yTeuHAaLf2JMV6hSQ8xW7Vo5XO4w?e=smir0r)
| The Wealth of Billionaires | Examines the how the wealth of billionaires ties to their respective companies and investments | [The Wealth of Billionaires](https://myuva-my.sharepoint.com/:u:/g/personal/cxx6sw_virginia_edu/IQDoceq4O3oLSqGkGgmEn7a5Ae1sOsDkDKVQwDolBgXTw-A?e=l3cOxh) |

---

## Data Creation

### Provenance

This data is sourced from the Forbes World's Billionaires List. The Forbes platform provides a publicly-available real-time ranking of individuals in the world with a net worth greater that $1 billion. The particular data used in this analysis was scraped from the platform when it was last updated on August 20, 2025. It does not reflect the current state of the Forbes World's Billionaires List.

The raw data was acquired as a single file from a public GitHub repository called `free-json-datasets`. This repo belongs to GitHub user `sharmadhiraj`. Within the `docs` folder, under the category labeled `economy-finance`, I downloaded the file `forbes_billionaires_list.json`. This file was manipulated locally in a data preparation notebook to produce the MongoDB database and all constituent documents used for this analysis.

### Code

| Code | Description | Link |
|------|-------------|------|
| `data_prep.ipynb` | Loads in raw json file, connects to MongoDB, extracts individual documents, and inserts into database | [Link to Code](https://github.com/masonnicoletti/billionaire-analysis/blob/main/pipeline/data_prep.ipynb) |

### Rationale

The fields included in each documents are all those that were reported from the raw `forbes_billionaires_list.json` file, sourced from the Forbes World's Billionaires List. My data preparation process did not remove or edit the values of any of these fields, ensuring that all the reported data was included in each document. The only fields added to documents was the `source_metadata` object, which includes several features that are standardized across all documents used in this analysis. These fields are not necessarily useful for the analysis results but provide contextual information about the data source.

Uncertainty in the findings from this analysis can be attributed to the absense of up-to-date data. The data uploaded to this database is static, and it is reported from data as of August 20, 2025. As time carries on, the `current_worth`, `age`, and `rank` of the subjects of documents will continue to shift. With this in mind, I decided it was important to explicitly include the field, `updated_at`, in documents, inserted manually as part of a metadata object. Additionally, measure were taken throughout the analysis to continuously specify that findings are from past data and their relevance many vary by today's standards.

### Bias Identification

Bias is a common point of contingency in net worth reporting. The reported net worth values for each subject on the Forbes Billionaires List is prone to misrepresentation due to a variety of factors. Underestimation of net worth is common due to the ownership of hidden assets. The ultrawealthy are incentivized to operate in the "hidden economy" and cover up as much monetary value as they can in order to preserve it. Overestimation can also be a problem, as the wealthy may decide to report inflated values, the media often tends to sensationalize net worth values of wealthy figures, and illiquid assets may be included in net worth calculations.

### Bias Mitigation

Bias mitigation in net worth calculations requires a clear strategy for drawing calculations and collecting data from all the possible sources of information, as well as leveraging technology for asset valuation. Forbes calculates net worth by summing the value of all personal assets, including investments, private companies, property and possessions, cash, etc., and subtracting debts. Estimations are made when transparency is limited. In these cases, biases are mitigated by drawing additional information from SEC filings, interviews, court records, and analyst work. In analysis, these biases can be accounted for by studying the distribution of wealth and understanding that variations exist due to misrepresented values.

---

## Metadata

### Implicit Schema

Each document in the `billionaires` collection within the `forbes` database contains data about one individual or family on the Forbes World's Billionaires List. All documents inserted in this collection should follow a standardized structure, displaying common fields for consistency. Recurring features in each document include `rank`, `name`, `last_name`, `source`, `industries`, `country`, `gender`, `age`, `current_worth`, `previous_worth`, and `image_url`. These fields are present across all documents extracted from the raw json-formatted data of Forbes billionaires. The `rank` field should be unique across all documents, establishing a natural procession of billionaires that can be validated by the `current_worth` field. The `industries` field stores any quantity of entries as an array of strings. The `_id` field is created upon insertion into MongoDB. Additionally, manually included in each document is a `soruce_metadata` object that holds fundamental aspects about the data source, including `title`, `subtitle`, and `updated_at`. The values for these fields are standardized across every current document in the collection, although than can vary upon the insertion of new data from an updated or additional source.

### Data Summary

| Collection | Document Count | Description |
|------------|----------------|-------------|
| `billionaires` | 3109 | Contains documents for all individuals reported in the Forbes World's Billionaires List from August 20, 2025 |

### Data Dictionary

| Name | Data Type | Description | Example |
|------|-----------|-------------|---------|
| _id | ObjectID | Unique identifier for document assinged upon insertion | ObjectID('69e4549bc450016f7d46f1e0') |
| Rank | Integer | Ordered value of billionaire based on current worth | 10 |
| Name | String | Full name of the billionaire | Warren Buffet |
| Last Name | String | Last name of the billionaire | Buffet |
| Source | String | Source from which the billionaire's wealth is derived | Berkshire Hathaway |
| Industries | Array | List of industries the billionaire is associated with | ['Finance & Investments'] |
| Country | String | Country of origin for the billionaire | United States |
| Gender | String | Gender of the billionaire, indicated as 'M' or 'F' | M |
| Age | Integer | Age of the billionaire in years | 95 |
| Current Worth | Integer | Total value of all assets minus all liabilities; Net Worth | 144815736000 |
| Previous Worth | Integer | Net worth one year prior to current worth | 143003432844 |
| Image URL | String | URL to JPG image of billionaire | https://specials-images.forbesimg.com/imageserve/5babb7f1a7ea4342a948b79a/416x416.jpg |
| Source Metadata | Object | Stores `title`, `subtitle`, and `updated_at` | {title: "Forbes Billionaires List", subtitle: "Up-to-date ranking of the world's wealthiest individuals as of August 20, 2025", updated_at: "August 20, 2025 09:42 AM UTC"} |
| Title | String | Title of raw data source | Forbes Billionaires List |
| Subtitle | String | Subtitle of raw data source | Up-to-date ranking of the world's wealthiest individuals as of August 20, 2025 |
| Updated At | String | Date from which raw data was acquired | August 20, 2025 09:42 AM UTC |

### Numerical Data Dictionary

| Name | Mean | SD | Min | Max |Uncertainty | Rationale |
|------|------|----|-----|-----|-------------|-----------|
| Rank | 1554.87 | 897.64 | 1 | 3016 | Rank ± 2 | Subject to uncertainty in net worth calculations |
| Age | 64.32 | 16.32 | 0 | 104 | Age ± 1 | Small correction for possibility of misreporting |
| Current Worth | 5633608919.24 | 15518688176.11 | 0 | 413046544000 | Current Worth ± (0.1 * Current Worth) | Apply a 10% margin of error in net worth calculations |
| Previous Worth | 5645819019.03 | 15696641441.60 | 0 | 416338889179 | Previous Worth ± (0.1 * Previous Worth) | Apply a 10% margin of error in net worth calculations |

---
