# PY_Ultra_Marathon_Project

## Table of contents

- [Problem Statement](problem-statement)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning](#data-cleaning)
- [Questions and PY codes](questions-and-py-codes)
- [Results/Findings](#resultsfindings)
- [Recommendations](#recommendations)
- [Conclusion](#conclusion)

### Problem Statement

The main objective of this data analytics project was to clean up a large dataset consisting of ultra marathon races from different decades. The focus was in the year 2020 and 50km and 50-mile races in the USA, specifically the West Seattle Beach Run - Winter Edition. Thereafter, some critical analytical questions were answered, and graphical representations were performed to gain more insightful information about this race.
### Data Sources

The primary data used for this analysis were the large CSV datasets, consisting of two hundred centuries of race records. These datasets were cleaned and sorted to focus only on the desired portion of the data.

### Tools

- Visual Studio Code: The tool is a code editor optimized for building and debugging modern web and cloud applications. It integrates different code editors, such as Python and Jupyter Notebooks, which were activated/enabled to conduct this analysis within Visual Studio Code.  [Find here](https://code.visualstudio.com/)

- The analysis was conducted using the functionalities of Python's pandas, seaborn, numpy and Matplotlib.
  
### Data Cleaning

The project in Visual Studio Code began by importing and sorting the data for the relevant races. Thereafter, the data cleaning processes were performed. Tasks during this cleaning section included:

- Removing unwanted values and characters
- Dropping unwanted columns
- Cleaning up null values
- Cleaning up duplicates
- Fixing data types
- Renaming columns
- Reording columns

Final step: exporting the clean data for analytics questions. 

### Questions and PY codes

1. What is the distribution of race distances in the dataset?
```py
ax= sns.histplot(df4['race_distance'])
ax.set_title("Distribution of Race Distances")
```

<img width="1440" alt="distrib_distance" src="https://github.com/JeanBonheur01/PY_Ultra_Marathon_Project/assets/131664311/db646302-11c2-415f-aaf6-3db370affd93">

2. What is the distribution of race distances by gender?
```py

ax = sns.histplot(df4, x = 'race_distance', hue = 'gender')
ax.set_title("Distribution of Race Distance by Gender")
```

<img width="1440" alt="distr_gender" src="https://github.com/JeanBonheur01/PY_Ultra_Marathon_Project/assets/131664311/b1829051-fbf5-4a96-aec3-9b46fc1954a7">

3. What is the distribution of average speed for 50-mile races?
```py

ax = sns.displot(df4[df4['race_distance'] == '50mi']['average_speed'])
plt.title("Distribution of Average Speed for 50-Mile Races")
```

<img width="1440" alt="avg_speed_50mi" src="https://github.com/JeanBonheur01/PY_Ultra_Marathon_Project/assets/131664311/d06f8c49-6f0b-4720-9df3-4661558da058">

4. What is the distribution of average speed for 50km races?
```py

ax = sns.displot(df4[df4['race_distance'] == '50km']['average_speed'])
plt.title("Distribution of Average Speed for 50km Races")
```

<img width="1440" alt="avg_speed_50km" src="https://github.com/JeanBonheur01/PY_Ultra_Marathon_Project/assets/131664311/f2cd3d0e-f1e3-4c77-ab67-73ee8779de71">

5. What is the distribution of average speed by race distance and gender?
```py

sns.violinplot(data= df4, x = 'race_distance', y= 'average_speed', hue = 'gender')
plt.title("Distribution of Average Speed by Race Distance and Gender")
```

<img width="1440" alt="violinplot_avg" src="https://github.com/JeanBonheur01/PY_Ultra_Marathon_Project/assets/131664311/0fd3b415-ac2d-4c02-aa4b-d778a417aa5b">

```py

sns.violinplot(data= df4, x = 'race_distance', y= 'average_speed', hue = 'gender', split = True, inner = 'quart', linewidth=1)
plt.title("Distribution of Average Speed by Race Distance and Gender (Split Violin)")
```

<img width="1440" alt="Split_violin_avg" src="https://github.com/JeanBonheur01/PY_Ultra_Marathon_Project/assets/131664311/0c0f5c48-b8dc-4487-a940-8abb79fb43b6">

6. How does the average speed vary with age for different genders?
```py

sns.lmplot(data=df4, x= 'age', y= 'average_speed', hue= 'gender')
plt.title("Relationship Between Age and Average Speed, by Gender")
```

<img width="1440" alt="age_avg" src="https://github.com/JeanBonheur01/PY_Ultra_Marathon_Project/assets/131664311/104c3616-8b9f-4cb2-aeb4-e8db54e33a34">

7. What was the difference in speed between males and females for the 50Km and 50mi races?
```py

df4.groupby(['race_distance', 'gender'])['average_speed'].mean()
```

8. Which age group runs faster and which runs slower in the 50-mile races?
- Faster runners (Descending raking)
```py

df4.query('race_distance == "50mi"').groupby('age') ['average_speed'].agg (['mean', 'count']).sort_values('mean', ascending = False).query('count>19')
```

- Slower runners (Ascending ranking)
```py

df4.query('race_distance == "50mi"').groupby('age') ['average_speed'].agg (['mean', 'count']).sort_values('mean', ascending = True).query('count>19')
```
9. In which seasons do the athletes have higher performance?
```py

df4['race_month']= df4['race_date'].str.split('.').str.get(1).astype(int)

df4['race_season'] = df4['race_month'].apply(lambda x: 'Winter' if x > 11 else 'Fall' if x > 8 else 'Summer' if x > 5 else 'Sprint' if x >2 else 'Winter')

df4.groupby('race_season')['average_speed'].agg(['mean', 'count']).sort_values('mean', ascending = False)
```

|Race_season|Mean|Count|
|-----------|----|-----|
|Sprint|7.672551|3357|
|Winter|7.519917|11659|
|Fall|7.403707|8407|
|Summer|6.872740|2667|

10. What is the performance rate in the 50-mile races?
```py
df4.query('race_distance == "50mi"').groupby('race_season')['average_speed'].agg(['mean', 'count']).sort_values('mean', ascending = False)
```

|Race_season|Mean|Count|
|-----------|----|-----|
|Fall|7.496351|2019|
|Sprint|7.073452|847|
|Winter|7.049402|1982|
|Summer|6.515813|820|


### Results/Findings

1. Finding shows that more athlets run the 50Km race than the 50mi

2. For the 50Km distance, the participation is almost equal between males and females. However, the 50mi distance is dominated by men.
   
3. It is very common for the average speed to fall between 6 and 8 in both race types
   
4. The resulting violin plot represents the distribution of average speeds for participants in the 50KM and 50mi races, while also comparing the distributions between males and females. This may suggest that the distribution of average speeds for participants in races with a distance of 50 kilometers is more concentrated around certain values compared to races with a distance of 50 miles, where the distribution is more spread out and covers a broader range of speeds. We can also note that the average speed for male participants in the races tends to be higher and more concentrated around certain values, while the average speed for female participants is lower and spread over a wider range. These observations provide indications of potential differences in performance or distribution of speeds between different types of races and between genders. It is important to analyze these patterns more carefully and possibly conduct statistical analyses to draw more reliable conclusions.
   
5. Overall, men tend to run faster than women, and the average speed decreases as you get older for the both gender.

7. Men have a higher average than women in both races, but that differance is not hinger in the 50mi races.
   
8. Athletes who are 29 years old are the fastest, with the highest mean in the dataset, followed by those who are 23, 28, 30, and 25 years old. Overall, athletes between 25 and 30 years old are the fastest in the race. Athletes between of 50 and above have the lowest average speed.

9. In general athletes perform better and faster in the spring, followed by winter and fall. However, their performance is lower in the summer when it is warmer. But for 50-mile races, summer still remains the season where athletes perform the lowest, but the ranking has changed for other seasons. Fall has the highest performance rate, followed by spring and winter.
    
### Recommendations
### Conclusion

💻🖱️💻🤖🖱️🤖💻
