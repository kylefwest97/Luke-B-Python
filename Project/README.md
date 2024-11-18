## Introduction

### I completed this project as a part of Luke Barouse's Python for Data Analytics Course. This was a guided portfolio project building on everything learned in the course. 

## The Tools I Used

- Python
- Pandas
- Matplotlib
- Seaborn
- Git

## The Analysis

### The first question I looked to answer was what skills were most in demand for the top 3 data jobs. The jobs I looked at were Data Analyst, Data Scientist and Data Engineer.

### You can view my work here: 
[Skills_Count](/Project/2_Skills_Count.ipynb)

### First I exploded the job skills column. This allowed me to count how often each skill was mentioned in a job posting. Next, I used group by to get the skill count by job tille. Finally I had to use a merge so I could have the total count for each job title. This allowed me to display the percentage of jobs that contained each skill.


### Creating the Visualization
```python

for i, job in enumerate(top_3):
    job_skills = df_pct[df_pct['job_title_short'] == job].value_counts().reset_index()
    job_skills = job_skills.sort_values(by='skill_count', ascending=False).head()
    job_skills = job_skills.reset_index()
    sns.barplot(data=job_skills, x='percentage', y='job_skills', ax=ax[i], hue= 'percentage', legend = False)
    ax[i].set_title(f'{job}')
    ax[i].set_xlim(0, 75)
    fig.tight_layout()
    fig.suptitle('Percentages of Postings with Top Skills')
    ax[i].set_xlabel('')
    ax[i].set_ylabel('')
    ax[i].set_xticks([])
    plt.rcParams['font.size'] = 11.
    for n, v in enumerate(job_skills['percentage']):
        v = round(v)
        ax[i].text(v+.3, n+.4, f'{v}%')
    ax[2].set_xticks([0, 10, 20, 30, 40, 50, 60, 70 ])
```

### Results

![Skills_Count_Visualization](/Top_Skills_Chart.png)

### SQL is one of the top skills fot all three jobs. Python is mentioned in all three jobs as well, but it is not very common for Data Analyst Jobs. There are a lot of similarities between Data Scientists and Data Engineer job postings. SQL and Python are the most popular skills. Besides that Data Scientists skills lean more towards statistical analysis while data engineer jobs have cloud tools mentioned. 


### How are in demand skills trending for data analysts?

### The dataset I am working with has job postings for all of 2023. I wanted to see how different skills were trending across 2023. To accopmplish this I looked at the the top 5 skills for data analysts and the percentage of postings containing each by month. 

```python

df_DA_US_pivot.loc['Total'] = df_DA_US_pivot.sum()
df_DA_US_pivot = df_DA_US_pivot[df_DA_US_pivot.loc['Total'].sort_values(ascending=False).index]

# Get monthly totals
DA_totals = df_DA_US.groupby('job_posted_month_no').size()
DA_totals
# divide first 12 rows of df_DA_pivot by DA_totals
df_DA_US_percent = df_DA_US_pivot.iloc[:12].div(DA_totals/100, axis=0)
# changes month number to month name
df_DA_US_percent = df_DA_US_percent.reset_index()
df_DA_US_percent['job_posted_month'] = df_DA_US_percent['job_posted_month_no'].apply(lambda x: pd.to_datetime(x, format='%m').strftime('%b'))
df_DA_US_percent = df_DA_US_percent.set_index('job_posted_month')
df_DA_US_percent = df_DA_US_percent.drop(columns='job_posted_month_no')

df_DA_US_percent.iloc[:12,:5].plot()
plt.legend(loc = 'center right')
plt.title('Percentage of Postings with Top Skills over 2023')
plt.ylabel('Percent')
plt.xlabel('2023')
```

### You can view my work here: 
[Skills_Trends](/Project/3_Skills_Trend.ipynb)

![Skills_Count_Visualization](/Skills_Trend.png)

### SQL is by far the most in demand skill for the entire year. It also showed the most significant decline through 2023. Excel also showed a high demand compared to the other skills and had a sharp decline in the fall. Tableau, Python and Power BI were not in as high demand but were steady throughout the year. 

### How do Median Salaries differ for the top Data Jobs?

### Next I wanted to take a look at how the salaries top 6 data jobs in the United States compared. I began by filtering by the country, and then finding the top six most common jobs in the United States. The top six job titles were data analyst, senior data analyst, data scientist, senior data scientist, date engineer and senior data engineer. 

### Here is the code to generate my visualization: [Salary Analysis](/Project/4_Salary_Analysis.ipynb)

``` python
df_US = df[df['job_country']=='United States']
top_6_jobs = df_US['job_title_short'].value_counts().index[:6].to_list()
df_US_top6 = df_US[df_US['job_title_short'].isin(top_6_jobs)]
job_order = df_US_top6.groupby('job_title_short')['salary_year_avg'].median().sort_values(ascending=False).index
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)
plt.xlim(0, 300000)
plt.title('Annual Salary Comparison for the Top Data Jobs')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))
```

![Salary Analysis](/Salary_Analysis.png)

### The highest paying job is Senior Data Scientist and the lowest paying job is Data Analyst. Data Engineers and Data Scientists make more on average than Senior Data Analysts. Thins is likely because those positions are more technical. 

### Next I looked at the salary distribution for the top skils. I found both the top ten highest paying skills and the top ten most in demand skills for data analysts. Below is the code I used.

``` python 
df_DA_US = df[(df['job_title_short']=='Data Analyst') & (df['job_country']=='United States')].copy()
df_DA_US = df_DA_US.dropna(subset=['salary_year_avg'])
df_DA_US = df_DA_US.explode('job_skills')
df_DA_top_pay = df_DA_US.groupby('job_skills')['salary_year_avg'].agg(['count', 'median']).sort_values(by='median', ascending=False)
df_DA_top_pay = df_DA_top_pay.head(10)
df_DA_top_demand = df_DA_US.groupby('job_skills')['salary_year_avg'].agg(['count', 'median']).sort_values(by='count', ascending=False)
df_DA_top_demand = df_DA_top_demand.head(10).sort_values(by='median', ascending=False)
df_DA_top_demand
df_DA_top_pay
```

### Next I ploted the most in demand skills and the top ten skills on the same x axis to see how they compared. Here is the code I used to make my plot: 

[Top 10](/Project/5_Data_Analyst_Skills.ipynb)

![Top 10 Visualization](/top_10_salary.png)

### All of the top paying skills are niche skills that are in very low demand. Even though the salaries are much lower for the in demand skills it is much more beneficial to focus effots there than on the higher paying skills. 


### What are the most optimal skills for data analysts?

### The last question I wanted to ansswere was which skills are the most optimal for data analysts to learn. With so many different skills out there, it can be hard to determine which ones are worth spending the time to learn. Most optimal skills are the skills that are in high demand and mentioned in jobs with high salaries.

### Here is a link to my work so you can see how I set up the data to answer this question: [Optimal Skills](/Project/6_Most_Optimal_Skills.ipynb)

### I chose a scatter plot for this analysis because it does the best job comparint 2 atributes. Here is the code I used to create my plot.

``` python
df_DA_US_gb.head(10).plot(kind = 'scatter', x='percentage', y= 'median salary')
top_10 = list(df_DA_US_gb.head(10).index)
texts = []
from adjustText import adjust_text
for i, skill in enumerate(top_10):
    df2 = df_DA_US_gb.iloc[i]
    texts.append(plt.text(df2['percentage']+1, df2['median salary'], skill))
adjust_text (texts, arrowprops = dict(arrowstyle = '->', color = 'grey'))
plt.title('Most Optimal Skills for Data Analysts')
plt.xlabel('Percentage of Postings with Each Skill')
plt.ylabel('Median Salary (USD)')
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'{int(x)}%'))
plt.grid(linestyle = '--')
```
### The visualization:
![Optimal Skills](/Optimal_Skills.png)

### The most in demand skills are Python, SQL, Excel and Tableau. Python is by far the highest paying skill. Based off this chart it is mosst beneficial for aspiring data analysts to focus their efforts on learning Python, Tableau and SQL.

### Python is the clear winner for a coding language to learn. It is both more in demand and higher paying than r.

### The same is true for data visualization tools. Tableau is both higher paying and more in demand than Power BI. 

## Conclusion

### After analyzing thousands of data jobs I am glad I took the time to learn Python. It is consistantly in demand and high paying. After using Python to conduct a variety of different analyses I can say for sure that it is the most powerful and flexable data tool I have ised so far. I am excited to complete mote projects using Python!




