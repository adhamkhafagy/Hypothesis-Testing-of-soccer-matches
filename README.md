# Notebook Export README



## Markdown Cell 1


![A soccer pitch for an international match.](soccer-pitch.jpg)

You're working as a sports journalist at a major online sports media company, specializing in soccer analysis and reporting. You've been watching both men's and women's international soccer matches for a number of years, and your gut instinct tells you that more goals are scored in women's international football matches than men's. This would make an interesting investigative article that your subscribers are bound to love, but you'll need to perform a valid statistical hypothesis test to be sure!

While scoping this project, you acknowledge that the sport has changed a lot over the years, and performances likely vary a lot depending on the tournament, so you decide to limit the data used in the analysis to only official `FIFA World Cup` matches (not including qualifiers) since `2002-01-01`.

You create two datasets containing the results of every official men's and women's international football match since the 19th century, which you scraped from a reliable online source. This data is stored in two CSV files: `women_results.csv` and `men_results.csv`.

The question you are trying to determine the answer to is:

> Are more goals scored in women's international soccer matches than men's?

You assume a **10% significance level**, and use the following null and alternative hypotheses:

$H_0$ : The mean number of goals scored in women's international soccer matches is the same as men's.

$H_A$ : The mean number of goals scored in women's international soccer matches is greater than men's.


## Code Cell 2


```python
# Start your code here!
import pandas as pd
import matplotlib.pyplot as plt
import pingouin
from scipy.stats import mannwhitneyu
```


## Code Cell 3


```python
men=pd.read_csv(r"D:\Power BI Course\20- Projects\Hypothesis Testing with men's and women's soccer matches\men_results.csv")

print(men.head(5))
```


### Outputs


```
   Unnamed: 0        date home_team away_team  home_score  away_score  \
0           0  1872-11-30  Scotland   England           0           0   
1           1  1873-03-08   England  Scotland           4           2   
2           2  1874-03-07  Scotland   England           2           1   
3           3  1875-03-06   England  Scotland           2           2   
4           4  1876-03-04  Scotland   England           3           0   

  tournament  
0   Friendly  
1   Friendly  
2   Friendly  
3   Friendly  
4   Friendly  

```


## Code Cell 4


```python
women=pd.read_csv(r"D:\Power BI Course\20- Projects\Hypothesis Testing with men's and women's soccer matches\women_results.csv")
print(women.head(5))
```


### Outputs


```
   Unnamed: 0        date home_team  away_team  home_score  away_score  \
0           0  1969-11-01     Italy     France           1           0   
1           1  1969-11-01   Denmark    England           4           3   
2           2  1969-11-02   England     France           2           0   
3           3  1969-11-02     Italy    Denmark           3           1   
4           4  1975-08-25  Thailand  Australia           3           2   

         tournament  
0              Euro  
1              Euro  
2              Euro  
3              Euro  
4  AFC Championship  

```


## Code Cell 5


```python
print(men.info())
print(women.info())
```


### Outputs


```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 44353 entries, 0 to 44352
Data columns (total 7 columns):
 #   Column      Non-Null Count  Dtype 
---  ------      --------------  ----- 
 0   Unnamed: 0  44353 non-null  int64 
 1   date        44353 non-null  object
 2   home_team   44353 non-null  object
 3   away_team   44353 non-null  object
 4   home_score  44353 non-null  int64 
 5   away_score  44353 non-null  int64 
 6   tournament  44353 non-null  object
dtypes: int64(3), object(4)
memory usage: 2.4+ MB
None
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 4884 entries, 0 to 4883
Data columns (total 7 columns):
 #   Column      Non-Null Count  Dtype 
---  ------      --------------  ----- 
 0   Unnamed: 0  4884 non-null   int64 
 1   date        4884 non-null   object
 2   home_team   4884 non-null   object
 3   away_team   4884 non-null   object
 4   home_score  4884 non-null   int64 
 5   away_score  4884 non-null   int64 
 6   tournament  4884 non-null   object
dtypes: int64(3), object(4)
memory usage: 267.2+ KB
None

```


## Code Cell 6


```python
men['tournament'].value_counts()

```


### Outputs


```
tournament
Friendly                                17519
FIFA World Cup qualification             7878
UEFA Euro qualification                  2585
African Cup of Nations qualification     1932
FIFA World Cup                            964
                                        ...  
Real Madrid 75th Anniversary Cup            1
Évence Coppée Trophy                        1
Copa Confraternidad                         1
TIFOCO Tournament                           1
FIFA 75th Anniversary Cup                   1
Name: count, Length: 141, dtype: int64
```


## Code Cell 7


```python
women['tournament'].value_counts()
```


### Outputs


```
tournament
UEFA Euro qualification                 1445
Algarve Cup                              551
FIFA World Cup                           284
AFC Championship                         268
Cyprus Cup                               258
African Championship qualification       226
UEFA Euro                                184
African Championship                     173
FIFA World Cup qualification             172
CONCACAF Gold Cup qualification          143
AFC Asian Cup qualification              141
Copa América                             131
Olympic Games                            130
CONCACAF Gold Cup                        126
Friendly                                 111
AFC Asian Cup                            111
Four Nations Tournament                  106
OFC Championship                          78
African Cup of Nations qualification      58
CONCACAF Championship                     42
SheBelieves Cup                           39
Euro                                      20
African Cup of Nations                    16
OFC Nations Cup                           16
Olympic Games qualification               15
Tournament of Nations                     12
Tournoi de France                         12
CONCACAF Invitational Tournament           6
OFC Nations Cup qualification              6
Basque Country Women's Cup                 4
Name: count, dtype: int64
```


## Code Cell 8


```python
men['date'] = pd.to_datetime(men['date'])
men_subset = men[(men["date"] > "2002-01-01") & (men["tournament"].isin(["FIFA World Cup"]))]
men_subset.head(5)

```


### Outputs


```
       Unnamed: 0       date            home_team     away_team  home_score  \
25164       25164 2002-05-31               France       Senegal           0   
25165       25165 2002-06-01              Germany  Saudi Arabia           8   
25166       25166 2002-06-01  Republic of Ireland      Cameroon           1   
25167       25167 2002-06-01              Uruguay       Denmark           1   
25168       25168 2002-06-02            Argentina       Nigeria           1   

       away_score      tournament  
25164           1  FIFA World Cup  
25165           0  FIFA World Cup  
25166           1  FIFA World Cup  
25167           2  FIFA World Cup  
25168           0  FIFA World Cup  
```


## Code Cell 9


```python
women['date'] = pd.to_datetime(women['date'])
women_subset = women[(women['date']>'2002-01-01')& (women['tournament'].isin(['FIFA World Cup']))]
women_subset
```


### Outputs


```
      Unnamed: 0       date      home_team      away_team  home_score  \
1600        1600 2003-09-20        Nigeria    North Korea           0   
1601        1601 2003-09-20         Norway         France           2   
1602        1602 2003-09-20        Germany         Canada           4   
1603        1603 2003-09-20          Japan      Argentina           6   
1604        1604 2003-09-21  United States         Sweden           3   
...          ...        ...            ...            ...         ...   
4465        4465 2019-06-29        Germany         Sweden           1   
4466        4466 2019-07-02        England  United States           1   
4467        4467 2019-07-03    Netherlands         Sweden           1   
4468        4468 2019-07-06        England         Sweden           1   
4469        4469 2019-07-07  United States    Netherlands           2   

      away_score      tournament  
1600           3  FIFA World Cup  
1601           0  FIFA World Cup  
1602           1  FIFA World Cup  
1603           0  FIFA World Cup  
1604           1  FIFA World Cup  
...          ...             ...  
4465           2  FIFA World Cup  
4466           2  FIFA World Cup  
4467           0  FIFA World Cup  
4468           2  FIFA World Cup  
4469           0  FIFA World Cup  

[200 rows x 7 columns]
```


## Code Cell 10


```python
men_subset["group"]="men"
women_subset["group"]="women"
men_subset["goals_scored"]=men_subset["home_score"]+men_subset["away_score"]
women_subset["goals_scored"]=women_subset["home_score"]+women_subset["away_score"]
```


## Code Cell 11


```python
men_subset["goals_scored"].hist()
plt.show()

```


### Outputs


```
<Figure size 640x480 with 1 Axes>
```


## Code Cell 12


```python
women_subset["goals_scored"].hist()
plt.show()
#goals_scored is not normally distributed so we use a non parametic t-test which is Wilcoxon-Mann-Whitney
```


### Outputs


```
<Figure size 640x480 with 1 Axes>
```


## Code Cell 13


```python
both = pd.concat([women_subset, men_subset], axis=0, ignore_index=True)
both
```


### Outputs


```
     Unnamed: 0       date      home_team    away_team  home_score  \
0          1600 2003-09-20        Nigeria  North Korea           0   
1          1601 2003-09-20         Norway       France           2   
2          1602 2003-09-20        Germany       Canada           4   
3          1603 2003-09-20          Japan    Argentina           6   
4          1604 2003-09-21  United States       Sweden           3   
..          ...        ...            ...          ...         ...   
579       44343 2022-12-10        England       France           1   
580       44345 2022-12-13      Argentina      Croatia           3   
581       44346 2022-12-14         France      Morocco           2   
582       44350 2022-12-17        Croatia      Morocco           2   
583       44352 2022-12-18      Argentina       France           3   

     away_score      tournament  group  goals_scored  
0             3  FIFA World Cup  women             3  
1             0  FIFA World Cup  women             2  
2             1  FIFA World Cup  women             5  
3             0  FIFA World Cup  women             6  
4             1  FIFA World Cup  women             4  
..          ...             ...    ...           ...  
579           2  FIFA World Cup    men             3  
580           0  FIFA World Cup    men             3  
581           0  FIFA World Cup    men             2  
582           1  FIFA World Cup    men             3  
583           3  FIFA World Cup    men             6  

[584 rows x 9 columns]
```


## Code Cell 14


```python
both_subset=both[["goals_scored","group"]]
both_subset_wide=both_subset.pivot(columns="group",values="goals_scored")
both_subset_wide
```


### Outputs


```
group  men  women
0      NaN    3.0
1      NaN    2.0
2      NaN    5.0
3      NaN    6.0
4      NaN    4.0
..     ...    ...
579    3.0    NaN
580    3.0    NaN
581    2.0    NaN
582    3.0    NaN
583    6.0    NaN

[584 rows x 2 columns]
```


## Code Cell 15


```python
result_pg = pingouin.mwu(x=both_subset_wide["women"],y=both_subset_wide["men"],alternative="greater")
result_pg
```


### Outputs


```
       U_val alternative     p_val       RBC      CLES
MWU  43273.0     greater  0.005107  0.126901  0.563451
```


## Code Cell 16


```python
p_val = result_pg['p_val'].values[0]
```


## Code Cell 17


```python
if p_val <= 0.01:
    result = "reject"
else:
    result="fail to reject"
result_dict = {"p_val": p_val, "result": result} 

```


## Code Cell 18


```python
result_dict
```


### Outputs


```
{'p_val': 0.005106609825443641, 'result': 'reject'}
```


## Code Cell 19


```python

```
