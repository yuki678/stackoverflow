## Table of Contents
1. Installation
2. Project Description
3. File Description
4. Results
5. Licensing, Authors, Acknowledgements

## 1. Installation
There should be no necessary libraries to run the code here beyond the Anaconda distribution of Python. The code should run with no issues using Python versions 3.*.
Download data from https://insights.stackoverflow.com/survey and unzip to your data/[year] directory (e.g. data/2019/survey_results_public.csv, survey_results_schema.csv).

## 2. Project Description
I followed CRISP-DM methodology to analyze the data.
### 2.1 Business Understanding
The survey is conducted every year to examine developer experiences from career satisfaction and job search to education and opinions on open source software. The latest survey was in 2019 with nearly 90,000 responses from over 170 counties and territories.

In this project, I focus on developers' salary. The survey page itself has nice charts and graphs to visualise the overall result. I wonder, however, the difference between countries should be taken into consideration.

So, my objective here is
 - How much the salary differs between countries for the same type of job?
 - Is there any different salary trend between countries in terms of role or technologies used?
 - Are these trends changed over the last few years?

### 2.2 Data Understanding
Data is from the Stack Overflow Survey page mentioned in 1. Installation.
 * survey_results_public.csv - Result of survey
 * survey_results_schema.csv - Questions asked

There are 84 questions in 2019 data and the following fields are used.
#### Country
Use this as Grouping Key. No missing data but 'Other Country (Not Listed Above)' is there in addition to country names.
#### Age
Numbers but have 0 in some data. There are missing values.
#### YearsCodePro
Numbers except for 'Less than 1 year' and 'More than 50 years'. There are missing values.
#### ConvertedComp
Compensation was asked in local currency for either weekly, monthly or yearly and the value is available in CompTotal.
It was then converted to annual USD salaries using the exchange rate on 2019-02-01, assuming 12 working months and 50 working weeks.
This seems to have a very high number and capped, as well as missing data.
#### DevType
Multi-selection, separated by ";". There are records with missing values.
#### LanguageWorkedWith, DatabaseWorkedWith, PlatformWorkedWith, WebFrameWorkedWith, MiscTechWorkedWith
Multi-selection, separated by ";". There are records with missing values.

### 2.3 Data Preparation
To clean up the data for the analysis, performed the followings.
#### Country
Not sure what locations are included in 'Other Country (Not Listed Above)'. As this is not significant and affect to this anslysis, simply drop those data.
#### Age
Missing values - When analyzing salary against this value, it does not make much sense to assume any value. Simply drop those missing ones.
Also, age less than 10 years old were filtered out as not the group of people to analyse this time.
#### YearsCodePro
'Less than 1 year' - It must be between 0 and 1 year. This is used as average of years of experience in each country group, so convert all to 0.5.
'More than 50 years' - Again, this is also not necessarily accurate as just used to have average and only a small number of records having this. Considering most of then must be close to 50 rather than 100, convert all to 60.
Missing values - When analyzing salary against this value, it does not make much sense to assume any value. Simply drop those missing ones.
#### ConvertedComp
This is the target to analyze, so missing data are dropped.
Also, there are very low or high salary which would not be relevant to this analysis.
Filter out annual salary less than $1000 and more than $250k (For US, more than $500k)
#### DevType
When one respondent selected multiple choices, it would be too much to consider the combination of the selections.
In this analysis, the data are extended to each value when taking group by DevType, resulting in more number of records than it has been.
One drawback here is, the salary might attribute more to a particular types than to the other types.
Drop missing values when grouping by DevType.
#### LanguageWorkedWith, DatabaseWorkedWith, PlatformWorkedWith, WebFrameWorkedWith, MiscTechWorkedWith
The same approach was taken as DevType.

### 2.4 Modelling
This time, machine learning algorithms are not used but rather simple group operation like averaging are used.
Data are first grouped by Country.
Then, some countries are grouped into 4 Country Groups based on the average salary and standard deviation.
Next it was further grouped by DevType or one of the technologies and compared.

### 2.5 Evaluation
The outcome of the analysis are summarised as follows.
#### How much the salary differs between countries for the same type of job?
US is far too well paid than any other countries. Even the lowest developer type with less than 10 years of experience is paid the same level of salary as the highest senior management with more than 15 years experience in the next group of nine countries in the world. The same goes to the following groups. 
The average salary goes like:
 - US: $116.3k
 - UK: $70.1k
 - Japan: $58.1k
 - Italy: $43.6k
 - India: $20.0k
It is definitely better to consider data within the same country when thinking of a particular country to live.

#### Is there any different salary trend between countries in terms of role or technologies used?
In fact, despite the difference in the salary level, the trend within each group are very similar. Well paid roles and technologies are consistent.
##### Role
Apart from managers, data related roles and site reliability are top paid roles. On the other hand, non technical or administrator roles are in lower side.
##### Language
Language wise, Scala is always top and web technologies like html/css/js are the bottom. In US, Go (Google) and Objective-C (Apple) are also among the top, whereas not in other groups where F# is the second.
##### Database
Cassandra is always the top with fewer people using it. MongoDB and MySQL are the lower side.
##### Platform
Kubernetes/Docker is very strong. AWS is the next with large population of people using it. GCP is good in US.
##### Others
Big Data technologies like Spark and Hadoop are the top, followed by IaC techs and ML/AI techs. This is consistent with Database where Cassandra, Elasticsearch, Redis are among the top.

#### Are these trends changed over the last few years?
TBD

### 2.6 Deployment
The main findings of the code can be found at the post available here.
https://medium.com/@yukitakahashi_46678/which-technologies-and-roles-could-earn-more-ca13d14677cb

## File Description
* src/Comp_EDA.ipynb - Include all source code to analyse stackoverflow developer suvery data.

## Licensing, Authors, Acknowledgements
Must give credit to Stack Overflow for the data. Otherwise, feel free to use the code here as you would like!
