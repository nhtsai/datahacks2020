## Race in Diabetes Healthcare Case Study

Project submitted to UCSD DataHacks 2020 by Nathan Tsai, David Laub, and Joseph Bui.

Winner of 2nd Place Finalist in the Science Track.

### Introduction

Diabetes is one of the most prevalent diseases that affects many people yearly. 
However, according to “Unequal Treatment: Confronting Racial and Ethnic Disparities in Health Care,” there is evidence demonstrating the unequal medical treatment towards racial minorities compared to whites in the U.S. healthcare system. 

Furthermore, Penner et al. report that “on average, White, Hispanic, and Asian physicians all display relatively large implicit racial preferences for Whites over Blacks.
Physicians also hold implicit stereotypes, characterizing Whites as more compliant and ‘better patients’ than Blacks.” 
This led us to want to verify these studies by finding evidence in the CDC Chronic Disease dataset. 
Further studies found that “African Americans, Hispanics, and Native Americans are 50-100% more likely to experience an illness or mortality from diabetes than white Americans” (Chow et al., 2012). 

Due to this fact, we wanted to focus our research towards disparities between African Americans and other races. 
We also chose to limit our data analysis to normalized mortality rates as this metric is more robust to the influence of confounding factors and bypasses established evidence that, according to the Mayo Clinic, African Americans are more likely to contract diabetes, although there is not yet evidence that they are more at risk to die from it.


### Data Cleaning and Processing

```python
clean_df = df.drop(['Response', 'ResponseID', 'StartificationCategory2', 'Stratification2', 
                    'StratificationCategory3', 'Stratification3', 'StratificationCategoryID2', 'StratificationID2', 
                    'StratificationID3', 'StratificationCategoryID3'], axis=1)
clean_df['DataValue'] = pd.to_numeric(cleaned['DataValue'], errors='coerce')
clean_df['DataValueAlt'] = pd.to_numeric(cleaned['DataValueAlt'], errors='coerce')
clean_df = clean_df.dropna(subset=['DataValue', 'DataValueAlt'], how='all')
diabetes = clean_df.loc[clean_df['Topic'] == 'Diabetes'].reset_index(drop=True)
```

#### Assumptions

For our data cleaning process, we dropped columns that contained solely null or blank values which we thought of as meaningless. 
The columns that we dropped are: `Response`, `ResponseID`, `StratificationCategory2`, `Stratification2`, `StratificationCategory3`, `Stratification3`,  `StratificationCategoryID2`, `StratificationID2`, `StratificationCategoryID3`, and `StratificationID3`. 
We dropped most Stratification columns because `Stratification1` contained 0 null values and gave us enough information to help us identify a person’s ethnicity or gender. 
Moreover, we dropped `DataValueFootnoteSymbol` because it contained weird symbols which were incomprehensible. 
We also dropped columns like `TopicID`, `DataValueTypeID`, and `LocationID` because we usually referred to these columns by their original name, e.g. `Topic`, `DataValueType` and `Location`, instead of their IDs. 
We deleted rows from the dataframe that contained null values for both `DataValue` and `DataValueAlt` because we knew that if both of these values did not have data, the row wouldn’t be useful in data analysis. 
We dropped the non-mainland states like `US`, `DC`, `PR`, and `GU` because it cannot be graphed on the map.
We were more concerned about the states within the US. 

One challenge we encountered was when we were trying to delete rows that contained null values for `DataValue` and `DataValueAlt`, we wanted to delete rows that were null for both columns, not either or. 
We resolved this issue by using `.dropna(subset=[‘DataValue, ‘DataValueAlt], how = ‘all’)` to delete NaN values that are in that subset. 


### EDA Visualizations

<table align="center">
  <thead>
    <tr>
      <th>
        Overall Stratification
      </th>
      <th>
        Black, not Hispanic Stratification
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <img height="432" src="/datahacks2020/images/overallmap2010.png" title="Overall Diabetes Mortality in 2010.">
      </td>
      <td>
        <img height="432" src="/datahacks2020/images/bnhmap2010.png" title="Black, not Hispanic Diabetes Mortality in 2010.">
      </td>
    </tr>
    <tr>
      <td>
        <img height="432" src="/datahacks2020/images/overallmap2011.png" title="Overall Diabetes Mortality in 2011.">
      </td>
      <td>
        <img height="432" src="/datahacks2020/images/bnhmap2011.png" title="Black, not Hispanic Diabetes Mortality in 2011.">
      </td>
    </tr>
    <tr>
      <td>
        <img height="432" src="/datahacks2020/images/overallmap2012.png" title="Overall Diabetes Mortality in 2012.">
      </td>
      <td>
        <img height="432" src="/datahacks2020/images/bnhmap2012.png" title="Black, not Hispanic Diabetes Mortality in 2012.">
      </td>
    </tr>
    <tr>
      <td>
        <img height="432" src="/datahacks2020/images/overallmap2013.png" title="Overall Diabetes Mortality in 2013.">
      </td>
      <td>
        <img height="432" src="/datahacks2020/images/bnhmap2013.png" title="Black, not Hispanic Diabetes Mortality in 2013.">
      </td>
    </tr>
    <tr>
      <td>
        <img height="432" src="/datahacks2020/images/overallmap2014.png" title="Overall Diabetes Mortality in 2014.">
      </td>
      <td>
        <img height="432" src="/datahacks2020/images/bnhmap2014.png" title="Black, not Hispanic Diabetes Mortality in 2014.">
      </td>
    </tr>
  </tbody>
</table>


<p align="center">
  <em>Figure 1. Overall vs. African-American Age-Adjusted Diabetes Mortality Rate (2010 - 2014)</em>
</p>


Looking at the Diabetes question, “Mortality due to diabetes reported as any listed cause of death”, across 2010 to 2014, we saw that there was a significant increase in this statistic in the African-American, or `Black, non-Hispanic`, stratification across the entire United States. We used the Plotly Express library to create choropleths of the age-adjusted diabetes mortality rate, comparing the overall and African-American stratifications. Looking at the choropleths, we can see there is a clear difference between the two stratifications. African-Americans seem to be dying from diabetes at a higher rate across the country.


<table align="center">
  <thead>
    <tr>
      <th>
        Overall vs. Black, not Hispanic Mortality Rates Across Diseases (p < 0.05)
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <img height="432" src="/datahacks2020/images/diabetesmortality.png" title="Diabetes Mortality Comparison.">
      </td>
    </tr>
    <tr>
      <td>
        <img height="432" src="/datahacks2020/images/chronickidneydiseasemortality.png" title="Chronic Kidney Disease Mortality Comparison.">
      </td>
    </tr>
    <tr>
      <td>
        <img height="432" src="/datahacks2020/images/prematuremortality.png" title="Premature Mortality Comparison.">
      </td>
    </tr>
    <tr>
      <td>
        <img height="432" src="/datahacks2020/images/cardiovascularmortality.png" title="Cardiovascular Mortality Comparison.">
      </td>
    </tr>
  </tbody>
</table>

<p align="center">
  <em>Figure 2. Black, non-Hispanic vs. Population Mortality Rates across Diseases, Conditions (p < 0.05)</em>
</p>


While the focus of our analysis was on African Americans within the topic of diabetes, we also noticed that the African-American population had significantly greater mortality rates across many different diseases. Overlaid box & swarm plots clearly show this significant difference. This supports our claim that there are systemic issues in the U.S. that may have a fatal effect on the African-American population.

<table align="center">
  <thead>
    <tr>
      <th width="35%">
        Topic
      </th>
      <th>
        Most Common Words
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        Alcohol
      </td>
      <td>
        'alcohol', 'binge', 'drinking', 'excise', 'tax', 'beverage', 'drink', 'heavy', 'women', 'use'
      </td>
    </tr>
    <tr>
      <td>
        Nutrition, Physical Activity, and Weight Status
      </td>
      <td>
        'school', 'students', 'high', 'physical', 'activity', 'care', 'obesity', 'daily', 'consumption', 'education'
      </td>
    </tr>
    <tr>
      <td>
        Cardiovascular Disease
      </td>
      <td>
        'heart', 'disease', 'mortality', 'high', 'coronary', 'stroke', 'blood', 'pressure', 'vaccination', 'noninstitutionalized'
      </td>
    </tr>
    <tr>
      <td>
        Arthritis
      </td>
      <td>
        'arthritis', 'doctor', 'diagnosed', 'limitation', 'diabetes', 'physical', 'inactivity', 'obese', 'fair', 'poor'
      </td>
    </tr>
    <tr>
      <td>
        Diabetes
      </td>
      <td>
        'diabetes', 'diagnosed', 'vaccination', 'noninstitutionalized', 'listed', 'examination', 'mortality', 'reported', 'cause', 'death'
      </td>
    </tr>
    <tr>
      <td>
        Tobacco
      </td>
      <td>
        'tobacco', 'smoking', 'cigarette', 'smoke', 'smokeless', 'use', 'states', 'youth', 'pneumococcal', 'vaccination'
      </td>
    </tr>
    <tr>
      <td>
        Overarching Conditions
      </td>
      <td>
        'health', 'women', 'poverty', 'self', 'rated', 'status', 'insurance', 'high', 'school', 'completion'
      </td>
    </tr>
    <tr>
      <td>
        Chronic Obstructive Pulmonary Disease
      </td>
      <td>
        'chronic', 'obstructive', 'pulmonary', 'disease', 'diagnosis', 'diagnosed', 'hospitalization', 'first', 'listed', 'vaccination'
      </td>
    </tr>
    <tr>
      <td>
        Cancer
      </td>
      <td>
        'cancer', 'incidence', 'mortality', 'invasive', 'use', 'women', 'female', 'papanicolaou', 'smear', 'cervix'
      </td>
    </tr>
    <tr>
      <td>
        Chronic Kidney Disease
      </td>
      <td>
        'disease', 'end', 'stage', 'renal', 'incidence', 'treated', 'mortality', 'chronic', 'kidney', 'attributed'
      </td>
    </tr>
    <tr>
      <td>
        Asthma
      </td>
      <td>
        'asthma', 'vaccination', 'noninstitutionalized', 'rate', 'influenza', 'pneumococcal', 'hospitalizations', 'emergency', 'department', 'visit'
      </td>
    </tr>
    <tr>
      <td>
        Disability
      </td>
      <td>
        ‘disability’
      </td>
    </tr>
    <tr>
      <td>
        Oral Health
      </td>
      <td>
        'dental', 'visits', 'teeth', 'lost', 'health', 'preventive', 'children', 'adolescents', 'water', 'six'
      </td>
    </tr>
    <tr>
      <td>
        Older Adults
      </td>
      <td>
        'proportion', 'older', 'date', 'core', 'clinical', 'preventive', 'services', 'medicare', 'persons', 'hospitalization'
      </td>
    </tr>
    <tr>
      <td>
        Mental Health
      </td>
      <td>
        'mentally', 'unhealthy', 'days', 'least', 'women', 'postpartum', 'depressive', 'symptoms'
      </td>
    </tr>
    <tr>
      <td>
        Immunization
      </td>
      <td>
        'influenza', 'vaccination', 'noninstitutionalized'
      </td>
    </tr>
    <tr>
      <td>
        Reproductive Health
      </td>
      <td>
        'checkup', 'timeliness', 'routine', 'health', 'care', 'women', 'postpartum', 'folic', 'acid', 'supplementation'
      </td>
    </tr>
  </tbody>
</table>

<p align="center">
  <em>Table 1. NLP Text Frequency Analysis</em>
</p>


Using Regex and NLTK, we analyzed the questions in each topic to find the top ten most common words in each topic. 
The main complication we ran into was performing analysis on limited text data. For topics like `Immunization` and `Disability`, there was only one unique question, so the text data for those topics were just one sentence. 
This made it difficult to find frequent keywords among all 17 topics because the usable text data was so limited. 
We then used Matplotlib to plot the top 3 most frequent words in the four topics we explored above, shown below.


<table align="center">
  <thead>
    <tr>
      <th>
        Top 3 Most Frequent Words
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <img height="432" src="/datahacks2020/images/top3diabetes.png" title="Top 3 Words in Diabetes Surveys.">
      </td>
    </tr>
    <tr>
      <td>
        <img height="432" src="/datahacks2020/images/top3chronickidney.png" title="Top 3 Words in Chronic Kidney Disease Surveys.">
      </td>
    </tr>
    <tr>
      <td>
        <img height="432" src="/datahacks2020/images/top3overarching.png" title="Top 3 Words in Overarching Conditions Surveys.">
      </td>
    </tr>
    <tr>
      <td>
        <img height="432" src="/datahacks2020/images/top3cardiovascular.png" title="Top 3 Words in Cardiovascular Disease Surveys.">
      </td>
    </tr>
  </tbody>
</table>

<p align="center">
  <em>Figure 3. Top 3 Most Frequent Words.</em>
</p>


### Hypothesis Testing

#### Research Question

We wanted to see if there was a difference in Age-adjusted Rate between African Americans and other races which could have contributed to mortality due to diabetes reported as any listed cause of death.

Therefore, we asked the question: **Are African Americans more likely to have a higher rate of mortality due to diabetes than other races?** More formally, we statistically tested whether the mean age-adjusted rates of mortality (deaths per 100,000) between Black, non-Hispanic Americans and the remaining population were different.

#### Null Hypothesis

African Americans have the same mean Age-adjusted Rate of diabetes as the population.

#### Alternative Hypothesis

African Americans have a higher mean Age-adjusted Rate of diabetes than the population.

#### Analysis

<p align="center">
  <img height="432" src="/datahacks2020/images/diabetesmortalityrate.png" title="Black, non-Hispanic vs Overall Diabetes Mortality Rates.">
  <br>
  <em>Figure 4. Black, non-Hispanic vs Overall Diabetes Mortality Rates.</em>
</p>

For our implementation, we made our observed mean to be the mean of the data values that were Age-adjusted Rates for people who had `Black, non-Hispanic` as their `Stratification1`. We made 1000 trials where we randomly sampled from data values of Age-adjusted Rates from the whole population with replacement. We would find the mean values of these rates and append it to our averages list. Then, we performed an unpaired t-test on the two groups, African-American and Overall stratifications. 

Our **p-value was 0.0**, which is less than the significance level of 0.05. Therefore, we **rejected the null hypothesis**. Our t-statistics were also consistently positive, so we conclude that **African Americans have a significantly higher Age-adjusted Rate of mortality due to diabetes compared to the rest of the population**. 


### Conclusions and Insights

There is established evidence of implicit biases in the American healthcare system that disfavor African Americans. 
Analysis of the CDC Chronic Disease dataset shows that African Americans consistently face significantly higher mortality rates from diabetes across both geography and time.
Outside diabetes, African Americans are also at greater risk for death from chronic kidney disease, cardiovascular disease, and are more likely to face premature death than the population. 

From our analysis, we conclude that African Americans are significantly more likely to die from chronic illness and disease. This strengthens the body of evidence that points to systemic social justice issues in U.S. healthcare.
From these conclusions, we suggest healthcare providers specifically address racial discrepancies in healthcare of chronic diseases and implement healthcare practices that acknowledge and avoid racial bias.

### Research Proposal

During our data analysis, we quickly realized that the CDC dataset was too limited to allow us to rigorously determine possible causes for why there is a racial bias against African Americans in the US medical system. We suggest that more data collection and research into the medical care of African Americans is necessary to understand the mechanisms that drive the mortality trends we discovered. Specifically, data related to patient care in the clinic and how physicians treat different racial and ethnic groups would be very informative for understanding this issue and how to resolve it.

### Further Study

If we were to further improve our research, we would want to use NLP to help predict some classification problems such as what the topic is based on the most common words. We would also want to test if Hispanics or Native Americans were more likely to receive higher mortality rates than white Americans to confirm that there is racial bias against minorities. Then, we could find any discrepancies in their nutrition, physical activity, and weight status that could contribute to mortality rates for diabetes among minorities.

We also recognize limitations of our study, as there are variables in the dataset that are known risk factors for diabetes that can be controlled for in a future study. However, this analysis was beyond the scope of our work.

### References
1. Edward A. Chow, MD, Henry Foster, MD, Victor Gonzalez, MD and LaShawn McIver, MD, MPH. The Disparate Impact of Diabetes on Racial/Ethnic Minority Populations.
Clinical Diabetes 2012 Jul; 30(3): 130-133. https://doi.org/10.2337/diaclin.30.3.130
1. Penner, L. A., Blair, I. V., Albrecht, T. L., & Dovidio, J. F. (2014). Reducing Racial Health Care Disparities: A Social Psychological Analysis. Policy Insights from the Behavioral and Brain Sciences, 1(1), 204–212. https://doi.org/10.1177/2372732214548430
