 # Data Analyst Job Market Analysis

## Overview

This project analyzes the data analyst job market to identify the most in-demand skills, salary trends, and the skills that offer the best combination of demand and salary.

The analysis uses Python to explore job postings and answer questions about which skills are most valuable for data analysts to develop.

The project was completed as part of my learning journey in Python and data analytics, with a focus on data cleaning, manipulation, analysis, and visualization.

## The Questions

The project focuses on answering the following questions:

1. What skills are most in demand for the top data roles?
2. How are in-demand skills distributed across Data Analyst jobs?
3. How well do Data Analyst jobs and skills pay?
4. What are the most optimal skills for Data Analysts to learn based on both demand and salary?

---

## Tools I Used

- **Python** – Used for data analysis and manipulation
- **Pandas** – Used for cleaning, transforming, grouping, and analyzing the dataset
- **Matplotlib** – Used to create data visualizations
- **Seaborn** – Used to create statistical visualizations
- **Jupyter Notebook** – Used to write and execute the analysis
- **Git & GitHub** – Used for version control and sharing the project

---

## Data Preparation and Cleaning

Before analyzing the data, I prepared the dataset to make it suitable for analysis.

The main steps included:

- Loading the dataset using Pandas
- Filtering the dataset to focus on Data Analyst roles
- Handling missing salary values
- Exploding skill lists so individual skills could be analyzed
- Grouping jobs by skill
- Counting how frequently each skill appeared
- Calculating the percentage of Data Analyst jobs requiring each skill
- Calculating median salary for jobs requiring each skill
- Categorizing technical skills by technology type

 
 # The Analysis
 ## 1. What are the most demanded skills for the top 3 most popular data roles?

To identify the most in-demand skills for the three most popular data roles, I first identified the top three job titles based on their frequency in the dataset. I then analyzed the top five skills associated with each role. This highlights the skills most commonly requested for each position and helps identify which skills to focus on depending on the role being targeted.

View my notebook with detailed steps here: [2_skills_Count.ipynb](3_Project\2_skills_Count.ipynb)

### Visualize Data

``` python
fig, ax = plt.subplots(len(job_titles), 1)
for i, job_title in enumerate(job_titles):
    df_plot = df_skills_count[df_skills_count['job_title_short'] == job_title].head(5)
    df_plot.plot(kind='barh', x='job_skills', y='skill_count', ax=ax[i], title=job_title)
ax[i].invert_yaxis()
ax[i].set_ylabel('')
ax[i].legend().set_visible(False)

fig.suptitle('Counts of Top Skills in job Postings', fontsize=15)
plt.show()
```
### Results
![Visualization of Top Skills for Data Nerds](images/output.png)

### Insights
- Python's Cross-Role Dominance: Python is widely required across all three positions, peaking at 72% for Data Scientists and 65% for Data Engineers.  
 - Core Language Demands: SQL leads demand for both Data Analysts and Data Scientists, appearing in more than 50% of listings for each role, whereas Python takes the top spot for Data Engineers at 68%.   
 - Tool Specialization vs. General Analytics: Data Engineering positions lean heavily toward specialized cloud and big data infrastructure (AWS, Azure, Spark), while Data Analysts and Data Scientists focus more on core analytical and visualization utilities like Excel and Tableau.   


 ## 2. How are in-demand skills trending for Data Analysts?

### Visulaize Data

```python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')
sns.despine()

plt.title('Tredning Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Postings')
plt.xlabel('2023')
plt.legend().remove() 


ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i])
```
### Results

![Trending Top Skills for Data Analysts in the US](images\output_2.png)
*Line Graph visualizing the trending top skills for data analysts in the US in 2023.*

### Insights 
* **SQL Dominates the Market**: SQL is by far the most requested skill, consistently appearing in between 53% and 63% of all US job postings throughout the year.
* **Excel Holds Strong as Second Place**: Excel maintains a steady second position, hovering mostly between 35% and 45% demand, though it experiences a noticeable drop in the fall (September to November) before rebounding in December.
* **Python and Tableau Run Neck-and-Neck**: Python (red line) and Tableau (green line) maintain a close race in the 25% to 35% range. Interestingly, they briefly intersect around June, and Python finishes the year with a slight upward tick in December.
* **Power BI Remains Steady at the Bottom**: Power BI stays the least demanded among these top five skills, maintaining a relatively flat and consistent trend line between 18% and 24% throughout 2023.


 ## 3. How well do jobs and skills pay for Data Analysts?

 ### Salary Analysis for Data Nerds

 #### Visualize Data

```python 
sns.boxplot(data=df_US_top6, x ='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')

plt.title('Salary Distributions in the United States')
plt.xlabel('Yearly Salary ($USD)')
ax = plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
plt.xlim(0, 600000)
plt.show
```
#### Results
![Salary Distributions of Data Jobs in the US](images\output3.png)
*Box plot visulaizing the salary distributions for the top 6 data job titles.*

### Insights
* **Senior Roles Command Higher Medians**: Senior titles (Senior Data Scientist, Senior Data Engineer, and Senior Data Analyst) consistently show higher median salary baselines, with their box ranges shifting further to the right compared to their mid-level counterparts.

* **Data Analysts Have the Lowest Baseline**: The standard Data Analyst role has the lowest median and interquartile range, sitting primarily under $100K, whereas specialized and senior engineering/science roles scale significantly higher.

* **Significant Right Skew and Outliers**: All roles feature a long right tail with multiple outlier data points stretching past $300K and up to nearly $600K, indicating that top earners or niche positions can command exceptionally high compensation.

### The Highest Paid & Most In-Demand Skills for Data
#### Visualize Data
```python
fig, ax = plt.subplots(2, 1)

sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, ax=ax[0], hue='median', palette='dark:b_r')
ax[0].legend().remove()

sns.set_theme(style='ticks')

#df_DA_top_pay[::-1].plot(kind='barh', y='median', ax=ax[0], legend=False)

ax[0].set_title('Top 10 Highest Paid Skills for Data Analysts')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_skills.index, ax=ax[1], hue='median', palette='light:b')
ax[1].legend().remove()


#df_DA_skills.plot(kind='barh', y='median', ax=ax[1], legend=False)
ax[1].set_title('Top 10 Most In-Demand Skills for Data Analysts')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary ($USD)')
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

fig.tight_layout()
```
#### Results
in-demand skills for data analysts in the US:

![The Highest Paid & Most In-Demand Skills for Data
Analysts in the US](images\output4.png)

### Insights

* **Top-Paid Skills Command Over $140K:** The highest-paying skills for Data Analysts all start above $140K, with specialized tools like `dplyr` leading the pack approaching nearly $200K.
* **Developer and DevOps Tools Dominate High Pay:** High-compensation skills lean heavily toward engineering and version control ecosystems—such as `bitbucket`, `gitlab`, `solidity`, and `ansible`—rather than traditional office tools.
* **In-Demand Core Skills Span Similar Salary Ranges:** The top 10 most in-demand skills (led by `python`, `tableau`, and `r`) also cluster around high median salaries ranging from roughly $145K up to nearly $200K.

## 4. What is the most optimal skill to learn for Data Analysts?

 #### Visualize Data

 ```python 

 df_DA_skills = df_DA_US_explode.groupby('job_skills')['salary_year_avg'].agg(['count', 'median']).sort_values(by='count', ascending=False)

df_DA_skills = df_DA_skills.rename(columns={
    'count': 'skill_count',
    'median': 'median_salary'
})

DA_job_count = len(df_DA_US)

df_DA_skills['skill_percent'] = df_DA_skills['skill_count'] / DA_job_count * 100

skill_percent = 1

df_DA_skills_high_demand = df_DA_skills[
    df_DA_skills['skill_percent'] > skill_percent
]
```
#### Results

![Most Optimal Skills for Data
Analysts in the US](images\output5.png)
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US.*

#### Insights:
- SQL is the most in-demand skill — it appears in about 5.1% of Data Analyst job postings, making it the strongest combination of demand and salary among the skills shown.

- Python and Tableau offer high salaries with moderate demand — both have a median salary of around $90K, while appearing in roughly 2.8–2.9% of postings.

- Some lower-demand skills have very high salaries — Looker has the highest median salary at about $95.5K, but appears in less than 0.5% of postings. This suggests that salary alone isn't enough to determine the most optimal skill; demand + salary together matter.

## Conclusion

This project gave me practical experience using Python to analyze a real-world job market dataset.

The analysis showed that demand and salary do not always move together. Some skills are highly requested by employers, while others have lower demand but are associated with higher salaries.

The project also helped me understand how data analysis can be used to answer practical career questions and make decisions based on evidence rather than assumptions.

Most importantly, it strengthened my skills in Python, Pandas, data cleaning, data manipulation, and data visualization.