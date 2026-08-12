# Overview
Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.  

The data sourced from [Luke Barousse](https://huggingface.co/datasets/lukebarousse/data_jobs) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# Questions:
Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?  
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

# Tools I Used
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

* **Python**: The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:  
    * **Pandas Library**: This was used to analyze the data.
    * **Matplotlib Library**: I visualized the data.  
    * **Seaborn Library**: Helped me create more advanced visuals.    
* **Jupyter Notebooks**: The tool I used to run my Python scripts which let me easily include my notes and analysis.
* **Visual Studio Code**: My go-to for executing my Python scripts.
* **Git & GitHub**: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

## Import & Clean Up Data
I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.
```
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```
# The Analysis
## 1. What are the most demanded skills for the top 3 most popular data roles?
To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting. 

View my notebook with detailed steps here: [2_Skill_Demand](/3_Project/2_Skills_Count.ipynb).

### Visualize Data
```
fig, ax=plt.subplots(len(job_titles), 1)

sns.set_theme(style='ticks')

for i, title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == title].head(5)
    # df_plot.plot(kind='barh', x='job_skills', y='skill_percent', ax=ax[i], title=title)
    sns.barplot(df_plot, y='job_skills', x='skill_percent', ax=ax[i], hue='skill_count', palette='dark:b_r')
  ```
### Results
![Visualizaion for top skills for data Experts](/3_Project/Images/Skill%20Demand%20for%20Roles.png)

### Insights
**Top Skill for Data Analysts**: Excel leads with 35%, closely followed by SQL at 33%.

**Top Skill for Data Scientists**: Python is the most requested skill, appearing in 55% of job postings.

**Top Skill for Software Engineers**: Python dominates this category as well, requested in 44% of postings.

**Python's Cross-Role Importance**: Python ranks high across all three professions (30% for Data Analysts, 55% for Data Scientists, and 44% for Software Engineers).

## 2. How are in-demand skills trending for Data Analysts?
To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023

View my notebook with detailed steps here: [3_Skill_Trend](/3_Project/3_Skill_trend.ipynb)

### Visualize Data
```
from matplotlib.ticker import PercentFormatter

sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')
plt.title('Trending Top Skills for Data Analysts in Kenya')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
sns.despine()
plt.legend().remove()

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))
```
### Results
![Skill Trend](/3_Project/Images/Skill%20Trend.png)

### Insights
**Leading Skill at Year-End**: SQL finished December as the most heavily requested skill for data analysts in Kenya, reaching approximately 48% likelihood in job postings.

**High Volatility for Python**: Python experienced extreme fluctuations throughout the year—dropping to a low of 5% in March before staging a major recovery and maintaining around 45% to 47% demand in the final quarter.

**Steady Demand for Excel**: Excel maintained a consistent and stable presence across 2023, hovering mostly between 26% and 40% without drastic volatility.

**Tableau and R Trends**: Tableau generally stayed at the lower end of demand for most of the year, but saw a sharp spike in November near 45% before dropping back down in December. R fluctuated moderately, peaking alongside Python in October at around 42%.

## 3. How well do jobs and skills pay for Data Analysts?
To identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most.

View my notebook with detailed steps here: [Salary Analysis](/3_Project/4_Salary_Analysis.ipynb)

### Visualize Data
```
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')
sns.despine()

# this is all the same
plt.title('Salary Distributions of Data Jobs in the US')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000) 
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
### Results
![Salary Distribution](/3_Project/Images/Salary%20Distribution.png)

### Insights
There's a significant variation in salary ranges across different job titles. Senior Data Scientist positions tend to have the highest salary potential, with up to $600K, indicating the high value placed on advanced data skills and experience in the industry.

Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewer outliers.

The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.

## Highest Paid & Most Demanded Skills for Data Analysts
 I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

 ### Visualize Data
 ```
 sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')
ax[0].legend().remove()
# original code:
# df_DA_top_pay[::-1].plot(kind='barh', y='median', ax=ax[0], legend=False) 
ax[0].set_title('Highest Paid Skills for Data Analysts in the US')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))
 ```

### Results
![Most Paid](/3_Project/Images/Most%20Paid.png)

### Insights
* The top graph shows specialized technical skills like `dplyr`, `Bitbucket`, and `Gitlab` are associated with higher salaries, some reaching up to `$200K`, suggesting that advanced technical proficiency can increase earning potential.  
* The bottom graph highlights that foundational skills like `Excel`, `PowerPoint`, and `SQL` are the most in-demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analysis roles.
* There's a clear distinction between the skills that are highest paid and those that are most in-demand. Data analysts aiming to maximize their career potential should consider developing a diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills

## 4. What are the most optimal skills to learn for Data Analysts?
To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.
View my notebook with detailed steps here: [Optimal Skills](/3_Project/5_Optimal_Skills.ipynb)

### Visualize Data
```
from matplotlib.ticker import PercentFormatter
# Get current axes, set limits, and format axes
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))
```
### Results
![Skill Scatter](/3_Project/Images/Skill%20Scatter.png)

### Insights
* The skill `Oracle` appears to have the highest median salary of nearly $97K, despite being less common in job postings. This suggests a high value placed on specialized database skills within the data analyst profession.

* More commonly required skills like `Excel` and `SQL` have a large presence in job listings but lower median salaries compared to specialized skills like Python and Tableau, which not only have higher salaries but are also moderately prevalent in job listings.

* Skills such as `Python`, `Tableau`, and `SQL Server` are towards the higher end of the salary spectrum while also being fairly common in job listings, indicating that proficiency in these tools can lead to good opportunities in data analytics.

## Visualizing Different Techonologies
Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

### Visualize Data
```
sns.despine()
sns.set_theme(style='ticks')
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary ($USD)')  # Assuming this is the label you want for y-axis
plt.title('Most Optimal Skills for Data Analysts in the US')

plt.show()

```
### Results
![Optimal skills](/3_Project/Images/Optimal%20Skill.png)

### Insights
* The scatter plot shows that most of the `programming` skills (colored blue) tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefits within the data analytics field.

* The database skills (colored orange), such as Oracle and SQL Server, are associated with some of the highest salaries among data analyst tools. This indicates a significant demand and valuation for data management and manipulation expertise in the industry.

* Analyst tools (colored green), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visualization and data analysis software are crucial for current data roles. This category not only has good salaries but is also versatile across different types of data tasks.


# What I learned
Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

* **Advanced Python Usage**: Utilizing libraries such as `Pandas` for data manipulation, `Seaborn` and `Matplotlib` for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.  
* **Data Cleaning Importance**: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.  
* **Strategic Skill Analysis**: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

# Insights
This project provided several general insights into the data job market for analysts:

* **Skill Demand and Salary Correlation**: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.  
* **Market Trends**: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.  
* **Economic Value of Skills**: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.

# Challenges I Faced
This project was not without its challenges, but it provided good learning opportunities:

* **Data Inconsistencies**: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.  
* **Complex Data Visualization**: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.  
* **Balancing Breadth and Depth**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion
This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.