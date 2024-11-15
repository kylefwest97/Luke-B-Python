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


    




