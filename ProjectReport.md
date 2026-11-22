# Project Report

## 1. Refined Research Question, Audience, and Potential Use

### Research question

**What spatial patterns characterize crime across Washington, DC census tracts in 2025, and how are tract-level employment, poverty, and household income associated with those patterns?**

### Audience

1. **DC Metropolitan Police Department (MPD):** district commanders and crime analysts
2. **DC government agencies that run workforce and anti-poverty programs:** the Department of Employment Services (DOES) and the Office of Neighborhood Safety and Engagement (ONSE)
3. **Nonprofits that run violence-prevention or youth programs**
4. **Small business owners**

### Relevance: what problem or information need does each audience have?

**DC Metropolitan Police Department:**

They have to locate and assign officers across districts, and by knowing which neighborhoods have more crime along with the dominant crime type, staffing can match the actual pattern.

**Government agencies - Workforce and anti-poverty programs:**

They can decide which neighborhoods to focus on for job programs and violence-prevention outreach. They can also plan their programs better if they know whether the tracts with the most crime are also the ones with low employment, low household income, and high poverty. If the two overlap, a combined approach can be effective and efficient. 

**Nonprofits:**

They can use the tract level information on the kind of crime and economic conditions of various areas to locate programs and which communities to serve. They can also use such patterns to support grant applications and to place services where they are most needed.

**Small business owners:**

They need to decide where to open and how much to spend on security. They need to know which types of incidents are common in a given neighborhood, in order to choose level of security accordingly.

### Potential use: how could our findings inform a decision, comparison, or further investigation?

**DC Metropolitan Police Department**
- **Decision:** Use the spatial and offense-type patterns to organize how many officers are assigned to each area.
- **Comparison:** Compare tracts based on a few summary dimensions like overall number of crimes versus violent versus property crime instead of based on too many separate offense columns.
- **Further investigation:** Tracts whose crime profile is unusual for their economic context can be flagged and reviewed in more detail to understand better.

**Government agencies - running workforce and anti-poverty programs**
- **Decision:** Prioritize which neighborhoods receive job programs or violence-prevention outreach, depending on whether crime overlaps with low employment, low household income, and high poverty.
- **Comparison:** Compare tracts with similar economic conditions but different crime levels, to see where economic hardship alone does not explain the pattern.
- **Further investigation:** Combine crime and economic patterns with data on where existing programs already operate, to check whether programs are reaching the tracts where crime and hardship overlap. Then examine specific economic factors across those tracts, to design programs suited to each type of area.

**Nonprofits running violence-prevention or youth programs**
- **Decision:**  Choose where to locate programs and which communities to serve based on the spatial and economic patterns, or expand programs in tracts with higher crime rates and where poverty is high and employment is low.
- **Comparison:**  Compare neighborhoods to find areas with similar profiles, so previous successful programs can be applied in other comparable areas.
- **Further investigation:** Use the tract-level findings as evidence of need in grant applications, then follow up with community-level data.

**Small business owners**
- **Decision:** Business owners can decide which neighborhoods to consider for a new location, and which kind of protection to budget for, based on the dominant offense type in the chosen tract.
- **Comparison:** Compare different neighborhoods for a new location based on their crime volume, dominant offense types, and economic conditions, instead of relying on only overall crime counts.
- **Further investigation:** Look more closely at the specific blocks near a planned location, since tract-level results are too broad for a single address.

## 2. Data Integration and PCA Summary
Each incident was assigned to a census tract using spatial join where counts were aggregated by tract and offense types. The offenses were grouped into two types; violent crime and property crime. Violent crime, violent_n included :homicide, robbery, assault, and sex abuse. And property crime, property_n included: burglary, motor vehicle theft, theft auto, and other theft. Then the ACS Economic Characteristics dataset was merged with the tract-level crime data on GEOID, after converting GEOID to a string in the ACS data so that the object type matched. The merged data contains 206 tracts. No tracts were removed or had to be imputed since GEOID was confirmed to be unique, and mean household income and poverty level had no missing values. For PCA, violent crime, property crime, mean household income, and poverty level were selected. Because these variables have very different units, they were standardized so that no single variable would subjugate the components on scale.

## 3. Component selection rationale
We retain PC1 and PC2 for three reasons:
1. **Scree plot:** the explained variance drops sharply after PC2.
2. **Cumulative variance:** the first two components together explain about 80% of the variation in the four variables.
3. **Eigenvalues:** only PC1 and PC2 have eigenvalues above 1, meaning each explains more variance than a single standardized variable. PC3 and PC4 fall below 1.

The loadings of PC3 and PC4 also show no clear pattern, which supports leaving them out.

### Component interpretation
The heatmap shows the following:
- **PC1: economic conditions versus violent crime.** Mean income loads strongly positive (0.84), while poverty (-0.77) and violent crime (-0.72) load strongly negative. Tracts with high PC1 scores have higher income, lower poverty, and fewer violent incidents, and tracts with low scores show the opposite. Property crime contributes little to PC1 (-0.25).
- **PC2: property crime.** Property crime has the largest loading (0.88), with a smaller contribution from violent crime (0.52). Income and poverty contribute less. PC2 mainly captures the volume of property crime, which varies largely independently of tract income and poverty. 
- **PC3 and PC4:** no notable pattern, so they are not interpreted.

Biplot of PC1 (46.8%) and PC2 (33.2%). 

- `mean_income` points right and `povertylvl` points left, so income and poverty move in opposite directions across tracts.
- `violent_n` points to the left, opposite `mean_income`, so tracts with higher income tend to have fewer violent incidents. `violent_n` and `povertylvl` point the same way, so violent crimes tend to be higher where poverty is higher but since the arrows are fairly far apart, the link is only moderate.
- `property_n` points mostly upward and is far from `mean_income`, suggesting that property crime is independent of tract income.
- `violent_n` and `property_n` are fairly close, so tracts with more violent incidents also tend to have more property incidents.

## Relation to the research question and audience

The main spatial pattern in crime across DC tracts has two dimensions. One links economic conditions to violent crime (PC1), and the second one is property crime, which is largely independent of income and poverty (PC2). For the agencies that run workforce and violence-prevention programs, this suggests that low-income, high-poverty tracts tend to coincide with more violent crime, so economic and violence-prevention programs could be considered together there. For the Police Department, small businesses, and nonprofits, it suggests that violent and property crime should be looked at separately, since they follow different patterns across tracts. However, one thing to consider is that these are associations between tracts, not causes.

[FinalProject2](FinalProject2.ipynb)
