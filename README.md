# Goal of the Project
The goal of this project is to explore the concept of bias in data. I used Wikipedia articles  about cities in different US states, and a machine learning service called ORES to predict the quality of the articles about the cities. I combined them with the state population data to analyze how the coverage of US cities on Wikipedia and how the quality of the articles about cities varies among states.

# License and Sources
- This repository is under the [MIT License](https://tlo.mit.edu/learn-about-intellectual-property/software-and-open-source-licensing).
- A list of Wikipedia article pages about US cities from each state, check the link [here](https://en.wikipedia.org/wiki/Category:Lists_of_cities_in_the_United_States_by_state).
- Population estimates for every US state from [US Census Bureau](https://www.census.gov/data/tables/time-series/demo/popest/2020s-state-total.html).
- Reference Code
	- The reference Wikimedia REST API code is licensed [CC-BY](https://creativecommons.org/licenses/by/4.0/).
	- Sample code for [making page request](https://drive.google.com/file/d/15UoE16s-IccCTOXREjU3xDIz07tlpyrl/view?usp=sharing) to obtain revision id for making ORES request.
	- Sample code for [making ORES request](https://drive.google.com/file/d/17C9xsmR9U3lJeD52UTbAedlHDetwYsxs/view?usp=sharing) to obtain article quality grade.

# API Documentation
- Wikipedia page info: https://www.mediawiki.org/wiki/API:Info
- ORES: https://www.mediawiki.org/wiki/ORES


# Data Files

### NST-EST2022-ALLDATA.csv
- Find link of source [here](https://www.census.gov/data/tables/time-series/demo/popest/2020s-state-total.html).
- This data file contains many fields but only 5 fields are used:
	- *REGION*: a census region code
	- *DIVISION*: a census divison code
	- *STATE*: a state FIPS code
	- *NAME*: the name of state
	- *POPESTIMATE2022*: the residen total population estimate by 7/1/2022

### US States by Region - US Census Bureau.xlsx
- This data file contains a hierarchical list of region, divison, and state of the US
	- *REGION*: the region of the US, divided into 4 which are Northeast, Midwest, South, and West
	- *DIVISION*: the division of the US, there are 2-3 divisions from each region
	- *STATE*: the state of the US

### us_cities_by_state_SEPT.2023.csv
- Find link [here](https://drive.google.com/file/d/1khouDmMaZyKo0y5WkFj4lu7g8o35x_98/view?usp=sharing).
- This data file contains 3 fields:
	- *state*: state of the US
	- *page_title*: title of the Wikipedia page of a city
	- *url*: the url of the web page

### us_cities_by_state_SEPT.2023_revid.csv
- Output data file with revision id. Compared to the data file above, 1 more field is added to it:
	- *article_revid*: the revision id of the article

### us_cities_by_state_SEPT.2023_ores.csv
- Output data file with ORES score. Compared to the data file above, 1 more field is added to it:
	- *ores_score*: the ORES estimate score of the article

### wp_scored_city_articles_by_state.csv
- Output data file with the following fields:
	- *state*: state of the US
	- *article_title*: title of the city Wikipedia article
	- *revision_id*:  revision id of the article
	- *article_quality*: article quality predicted by ORES
	- *regional_division*: division of the state of the article city
	- *population*: population of the state of the article city

# Special Considerations
Since this project is only considering state-level comparison, `District of Columbia` is excluded.

The list of cities scraped from Wikipedia missed the following 2 states: `Connecticut`, `Nebraska`, so these 2 states are also excluded from the analysis.

# Research Implications
In the process of analyzing the Wikipedia article dataset, I learned the importance of data comprehensiveness and accuracy in drawing significant insights to certain research question. The primary limitations of the dataset would be the imcomplete of the low-population cities and inaccuracies in article quality scores. By looking at the top 10 and bottom 10 states total coverage ratio and high-quality coverage ratio, I noticed that certain states appeared to have less coverage, particularly those with higher population. This might suggest a potential bias towards less populous or well-known cities in the dataset.

It's interesting to find out that the intersection of top 10 states from all articles and high quality articles has 6 states, while the intersection of bottom 10 states from all articles and high quality articles has 9 states. It might be too arbitrary to conclude the relationship between the quality of articles, but it is possible to further explore in the future.

# Reflection Questions
### 1. What biases did you expect to find in the data (before you started working with it), and why?
- **Quality Estimate**:  The article quality is obtained by estimation of ORES. Since the scores are made by the machine learning model instead of scraping the real grades, there might be discrepancies between the ORES estimates and the real gades. For example, the city Abbeville, Alabama has an ORES estimate grade of `C`, but its real grade shown on Wikipedia is `Start`.
- **Historical / Touristic Significance**: Larger cities, historically/culturally significant cities, or cities with major attractions to the tourists are more likely to have a higher grade as there are more to introduce.

### 2. What (potential) sources of bias did you discover in the course of your data processing and analysis?
- **Incomplete List of Cities**: Before performing analysis on the coverage for each state, I took a cursory review on the population and count of articles for each state. I noticed that North Carolina, ranked 8th most populous state in the US as of 2022, has only 50 articles of its cities which is an unusually low number. So I took a close look at the source website that the list is obtained, found out that on the page [List of municipalities in North Carolina](https://en.wikipedia.org/wiki/List_of_municipalities_in_North_Carolina), only the 50 largest municipalities are listed. There are a lot more other cities in NC that have a Wikipedia page but not listed here.
- **Population Density**: There exists states with only a few cities but a relatively large population, and in this case, the per capita coverage could be biased a lot. For example, Arizona ranked 13th most populous state but only has 91 cities, which is even fewer than Wyoming as the least populous state. The coverage calculation in this case may be biased towards those states with lower population but more cities.


### 3. Can you think of a realistic data science research situation where using these data (to train a model, perform a hypothesis-driven research, or make business decisions) might still be appropriate and useful, despite its inherent limitations and biases?
- **Scenario**: The research team wants to analyze the popular topics of US cities.
- **Method**: Extract topics from the articles of cities based on the quality scores (e.g. only include FA, GA, and B class), and rank the most frequently occurring topics across cities.
- **Outcomes not affected by limitations**: 
	- The missing low population cities won't affect the extract of topics since the main purpose would be gather a broad audience. Larger or more famous cities would be align better with the target demographic.
	- The incorrect article quality scores participates in a small part of the project. The research is not using the scores to analyze the quality but simply as a factor to extract topics, so even if the score is off by a margin, the general theme of the article still remains the same.