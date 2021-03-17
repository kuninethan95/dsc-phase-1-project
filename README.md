#  Movie Analysis
Please fill out:

- Student name: Ethan Kunin
- Student pace: Full Time
- Scheduled project review date/time: March 23rd 4:00pm EST
- Instructor name: James Irving
- Blog post URL: https://github.com/kuninethan95/dsc-phase-1-project

# Business Problem

Microsoft sees all the big companies creating original video content and they want to get in on the fun. They have decided to create a new movie studio, but they don’t know anything about creating movies. You are charged with exploring what types of films are currently doing the best at the box office. You must then translate those findings into actionable insights that the head of Microsoft's new movie studio can use to help decide what type of films to create.

--------------------------

## Questions to address

- Which directors generate the highest revenue per film?
- What month generates the highest revenue for films?
- What film length generates the highest revenue?
- Appendix A: Do franchise films earn more than non-franchise films?
- Appendix B: How have movie revenue trends changed over the past 10 years

--------------------------------------------------------------

## Data
- Originated from IMBD and The Numbers
- Focused on commercial budget releases, classified as those with budgets exceeding $20 million 
- Analyzing movies from 2010 onwards 
- Limiting to movies exceeding 80 minutes
- https://www.marketwatch.com/storynetflix-reportedly-set-to-produce-90-movies-a-year-with-budgets-up-to-200-million-2018-12-16
- https://screenwriting.io/what-is-a-feature-film/

----------

## Analyzing how runtime impacts revenue
We want to address if runtime has a positive correlation with worldwide gross revenue. We will also evaluate how seasonality generally impacts revenue. This may be dependent on a number of factors such as awards seasons, consumer spending habits, quality of releases, and much more


### *Visualization A: Boxplot to illustrate how runtime impacts revenue*


<img src="/Users/ethankunin/Desktop/movie_month.png" width = 80%>
                                                        
                                                       

    


### *Conclusion*
- Long movies have the highest median
- Long movies have the largest distributional spread
- Short and medium length movies have similar distributions
- Overall, the analysis suggests that 'long' movies generate higher revenues than 'short' and 'medium' lenght movies

### *Visualization B: Linear regression plot to illustrate how runtime impacts revenue*


image will go here


### Conclusion 
- There is a positive trend between film duration and worldwide gross
- Long movies have more outlier values

# Analyze which directors generate the highest revenue
- Are the top directors generally profitable?
- How much revenue do top directors' films earn?
- Are all films that top directors produce domestically and globabally profitable
- Choosing to hone in on revenue and cost because those show the absolute impact to Microsoft balance sheet












<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_name</th>
      <th>domestic_gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Ryan Coogler</td>
      <td>700059566.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Colin Trevorrow</td>
      <td>652270625.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Joss Whedon</td>
      <td>541142707.5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Angus MacLane</td>
      <td>486295561.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Joe Russo</td>
      <td>448882263.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>J.A. Bayona</td>
      <td>417719760.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Patty Jenkins</td>
      <td>412563408.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Adam Green</td>
      <td>400738009.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Tim Miller</td>
      <td>363070709.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Artie Mandelberg</td>
      <td>356461711.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Brad Bird</td>
      <td>351009033.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Jared Bush</td>
      <td>341268248.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Jon Favreau</td>
      <td>338217227.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jon Watts</td>
      <td>334201140.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>James Gunn</td>
      <td>333172112.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Andy Muschietti</td>
      <td>327481748.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Taika Waititi</td>
      <td>315058289.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Lee Unkrich</td>
      <td>312365447.5</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Sam Mendes</td>
      <td>304360277.0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>David Slade</td>
      <td>300531751.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
tp_dgsum = movie_direct.groupby('primary_name')['domestic_gross'].sum().sort_values().nlargest(20).reset_index()
```


```python
tp_wgmean = movie_direct.groupby('primary_name')['worldwide_gross'].mean().sort_values().nlargest(20).reset_index()
```


```python
tp_wgsum = movie_direct.groupby('primary_name')['worldwide_gross'].sum().sort_values().nlargest(20).reset_index()
tp_wgsum
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>primary_name</th>
      <th>worldwide_gross</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Joe Russo</td>
      <td>3902605502</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Peter Jackson</td>
      <td>2922948044</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Joss Whedon</td>
      <td>2920949860</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Michael Bay</td>
      <td>2911998250</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Francis Lawrence</td>
      <td>2543191543</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Zack Snyder</td>
      <td>2420920114</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Kyle Balda</td>
      <td>2195063923</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Christopher Nolan</td>
      <td>2001741385</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Lee Unkrich</td>
      <td>1866887623</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Steven Spielberg</td>
      <td>1762841457</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Mike Mitchell</td>
      <td>1760496511</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Tim Burton</td>
      <td>1689848988</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Colin Trevorrow</td>
      <td>1648854864</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Chris Renaud</td>
      <td>1632032904</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Jon Favreau</td>
      <td>1584010936</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Bryan Singer</td>
      <td>1488087924</td>
    </tr>
    <tr>
      <th>16</th>
      <td>David F. Sandberg</td>
      <td>1485961283</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Marc Webb</td>
      <td>1466886603</td>
    </tr>
    <tr>
      <th>18</th>
      <td>David Yates</td>
      <td>1454622939</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Brad Bird</td>
      <td>1449148229</td>
    </tr>
  </tbody>
</table>
</div>



## Create Visualization: Top 20 Directors, 2 Graphs to display WW and Mean/Sum


```python
# Opted to use mean because the outliers are a valuable data point
# Outlier movies can produce significant impact on production company balance sheet

fig, (ax1, ax2) = plt.subplots(nrows=1, ncols=2, figsize=(12,6))

x_labels1 = ['$0.0', '$0.2', '$0.4', '$0.6', '$0.8', '$1.0', '$1.2', '$1.4', '$1.6']
x_labels2 = ['$0.0', '$0.5', '$1.0', '$1.5', '$2.0', '$2.5', '$3.0', '$3.5', '$4.0']
ax1 = sns.barplot(data=tp_wgmean, x='worldwide_gross', y='primary_name', ax=ax1, palette='crest')
ax1.set_title('Top Directors by Average Worldwide Gross')
ax1.set_xlabel('Worldwide Gross ($ billions)')
ax1.set_ylabel('Director')
ax1.set_xticklabels(x_labels1)
formatter = FuncFormatter(millions)
ax1.xaxis.set_major_formatter(formatter)
ax1.tick_params(axis='x', labelrotation = 45)


ax2 = sns.barplot(data=tp_wgsum, x='worldwide_gross', y='primary_name', ax=ax2, palette='crest')
ax2.set_title('Top Directors by Total Worldwide Gross')
ax2.set_xlabel('Worldwide Gross')
ax2.set_ylabel('Director')
ax2.set_xticklabels(x_labels2)
formatter = FuncFormatter(millions)
ax2.xaxis.set_major_formatter(formatter)
ax2.tick_params(axis='x', labelrotation = 45)
fig.tight_layout();
```

    <ipython-input-52-43d536eb58ca>:9: UserWarning: FixedFormatter should only be used together with FixedLocator
      ax1.set_xticklabels(x_labels1)
    <ipython-input-52-43d536eb58ca>:19: UserWarning: FixedFormatter should only be used together with FixedLocator
      ax2.set_xticklabels(x_labels2)



    
![png](output_64_1.png)
    


### Conclusion 
- Many of the directors on the left chart also appear on the right
- Valuable list to parse through when considering who will direct the first film


```python
# Create list of top 20 directors by total worldwide gross

td = list(tp_wgsum['primary_name'])
```


```python
# Group table into top 20 directors and determine how profitable they are

dftd = movie_direct[movie_direct['primary_name'].isin(td)]
```


```python
dftd2 = dftd.copy()
```


```python
# Profit column for absolute

dftd2['profit'] = dftd2['worldwide_gross'] - dftd2['production_budget']
```


```python
# Feature engineer column to display if movie is profitable (domestic/worldwide)

def profitable(num):
    if num > 0:
        return True
    elif num < 0:
        return False
```


```python
dftd2['pwtf'] = dftd2['profit'].map(profitable)
```


```python
# Profit margin column

dftd2['pmarg'] = (dftd2['worldwide_gross'] - dftd2['domestic_gross'])/(dftd2['worldwide_gross']) * 100
```


```python
dftd2['profitdom'] = dftd2['domestic_gross'] - dftd2['production_budget']
```


```python
# Insert column with boolean values for film profitability 

dftd2['pdtf'] = dftd2['profitdom'].map(profitable).rename('Domestically Profitable')
dftd2.rename(columns={'pdtf': 'Domestically_Profitable'}, inplace=True)
```

## Create Visualization: Analyze profitablity and relationship between budget and worldwide revenue for Top Directors


```python
with plt.style.context('bmh'):
    fig, ax = plt.subplots(figsize=(8,5))
    sns.scatterplot(x='production_budget', y='worldwide_gross', data=dftd2,
                    hue='Domestically_Profitable',hue_order=(True, False))

    ax.set_title('Top 20 Directors\' Films By Average Worldwide Gross and Production Budget', fontdict={'fontsize':10})
    ax.set_xlabel('Production Budget')
    ax.set_ylabel('Worldwide Gross')
    
    formatter = FuncFormatter(millions)
    ax.yaxis.set_major_formatter(formatter)
    formatter = FuncFormatter(millions)
    ax.xaxis.set_major_formatter(formatter)

```


    
![png](output_76_0.png)
    


### Conclusion 
- Positive relationship between production budget & worldwide gross
- Not all movies are domestically profitable
- All movies are profitable when comparing production budget to worldwide gross

# Analyze when the best time to release a movie is
- Create visualization based on month showing average earnings
- Analyze which season is generates the highest amount of revenue
- Analyze which month generates the highest revenue per film on average

## Clean Data: 
- Feature engineer month column
- Feature engineer season column


```python
budgets_seasons = movie_direct.copy()
```


```python
# Add month column

budgets_seasons['month'] = budgets_seasons['release_date'].map(lambda x: x[0:3])
```


```python
# Map month to numbers

month_map = {'Jan':1, 'Feb':2, 'Mar':3, 'Apr':4, 'May':5, 'Jun': 6, 'Jul': 7, 'Aug': 8, 'Sep': 9, 'Oct': 10, 'Nov':11, 'Dec':12}
budgets_seasons['month_num'] = budgets_seasons['month'].map(month_map)
```


```python
# Check for NaN
# No relevant NaN values

budgets_seasons.isna().sum()
```




    id                     0
    release_date           0
    movie                  0
    production_budget      0
    domestic_gross         0
    worldwide_gross        0
    tconst_x               0
    region                 0
    primary_name           0
    birth_year           151
    death_year           684
    month                  0
    month_num              0
    dtype: int64




```python
# Quickly analyze distribution of film releases by month

budgets_seasons['month_num'].value_counts(normalize=True).sort_values().plot(kind='barh')
```




    <AxesSubplot:>




    
![png](output_84_1.png)
    



```python
# Groupby month and display domestic/worlwide growth

g2p = budgets_seasons.groupby('month_num')[['domestic_gross', 'worldwide_gross']].agg('mean').reset_index()
```

## Create Visualization: Side by Side bar plot showing how much revenue each month generates on average


```python
with plt.style.context('bmh'): 
    fig, ax = plt.subplots(figsize=(10,5))
    ax = sns.barplot(x='month_num', y="worldwide_gross", data=g2p, label='Worldwide Gross', palette='crest')
    ax = sns.barplot(x='month_num', y="domestic_gross", data=g2p, label= 'Domestic Gross', palette='flare')
    
    ax.set_xlabel('Month', color='black')
    ax.set_ylabel('Worldwide Gross')
    ax.set_title('Average Movie Gross by Month')
    formatter = FuncFormatter(millions)
    ax.yaxis.set_major_formatter(formatter)
    ax.legend()
```


    
![png](output_87_0.png)
    


### Conclusion 
- June movie generate the highest revenue on average followed by May and July
- Worldwide gross is greater than domestic gross in every month


```python
# Feature engineer function that turns month into corresponding season

def seasons(month):
    if (month >= 3) & (month <= 5):
        return 'Spring'
    elif (month >= 6) & (month <=8):
        return 'Summer'
    elif (month >= 9) & (month <=11):
        return 'Fall'
    else:
        return 'Winter'
```


```python
budgets_seasons['season'] = budgets_seasons['month_num'].map(seasons)
```

## Create Visualization: Boxplot of worldwide gross based on season


```python
with plt.style.context('bmh'):
    fig, ax = plt.subplots(figsize=(8, 6))
    ax = sns.boxplot(x='season', y='worldwide_gross', data=budgets_seasons, palette='crest')
    ax.set_title('Movie Gross Based on Season')
    ax.set_ylabel('Worldwide Gross')
    ax.set_xlabel('Season')
    formatter = FuncFormatter(millions)
    ax.yaxis.set_major_formatter(formatter)


```


    
![png](output_92_0.png)
    


### Conclusion 
- Spring and Summer films have the highest medians
- Spring and Summer films have larger distributions
- Corresponds with our month observations

# Display how movies have performed the past 10 years through trends
- Group movies into median gross by year
- Prefer this to line graph because there's too much noise/daily movements to accurately show the trend
- Show how production costs have varied
- Show how Worldwide/Domestic revenue has varied
- Include this in the introduction


```python
budgets_years = budgets_seasons.copy()
```


```python
# Feature engineer year column

budgets_years['year'] = budgets_years['release_date'].map(lambda x: x[-4:])
budgets_years['year'] = budgets_years['year'].astype(int)
```


```python
#Ensure we are working with movies from the correct year

budgets_years['year'].unique()
```




    array([2011, 2019, 2015, 2018, 2017, 2013, 2012, 2010, 2016, 2014])




```python
# Groupby year and display domestic/worlwide growth/production cost

gby = budgets_years.groupby('year')[['production_budget','domestic_gross', 'worldwide_gross']].agg('mean', 'median').reset_index()
```

## Create Visualization: Lineplot of worldwide/domestic gross/cost based on year


```python
with plt.style.context('bmh'):
    fig, ax = plt.subplots(figsize=(10,6))
    ax.plot(gby['year'], gby['worldwide_gross'], label='Worldwide Gross')
    ax.plot(gby['year'], gby['domestic_gross'], label='Domestic Gross')
    ax.plot(gby['year'], gby['production_budget'], label='Production Cost')

    ax.set_xlabel('Year')
    ax.set_ylabel('Gross / Cost')
    ax.set_title('Average Revenue/Cost of Commercial Movies from 2010 - 2019')
    formatter = FuncFormatter(millions)
    ax.yaxis.set_major_formatter(formatter)
    
    ax.legend(facecolor='white')
    ax.set_xticks(np.arange(2010, 2020, 1))
```


    
![png](output_100_0.png)
    


# Analyze how franchises perform compared to non-franchises
- Over time has the trend changed
- Do they have a higher positive correlation between production cost and worldwide gross
- Use this site to gather data: https://www.filmsite.org/

## Gather Data on franchises
- Use read_html to gather the data
- Turn the data into DF's
- Feature engineer a way to properly show the data we are looking for
- Create test to show if movie is part of a franchise


```python
# Begin loading in datasets and transform into readable Dataframes
#Page 1
#https://stackoverflow.com/questions/39710903/pd-read-html-imports-a-list-rather-than-a-dataframe

url = 'https://www.filmsite.org/series-boxoffice.html'
dfs = pd.read_html(url)
df1 = pd.concat(dfs)
df1.drop(columns=[0,1,5], inplace=True)
df1 = df1.rename(columns={2: 'series_list', 3:'number_of_films_in_franchise', 4:'top_movie'})
df1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>series_list</th>
      <th>number_of_films_in_franchise</th>
      <th>top_movie</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Greatest Film Series Franchises  (part 1 of 4,...</td>
      <td>Greatest Film Series Franchises  (part 1 of 4,...</td>
      <td>Greatest Film Series Franchises  (part 1 of 4,...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Individual Films in Franchise  (if clickable, ...</td>
      <td># of Films in Franchise</td>
      <td># 1 Film in Franchise</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Marvel's Cinematic  Universe  Iron Man (2008) ...</td>
      <td>23</td>
      <td>Avengers: Endgame (2019)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Pixar-Disney Animations *  Toy Story (1995)  A...</td>
      <td>23</td>
      <td>Incredibles 2 (2018)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Star Wars  (Ranked) Star Wars: (Original Trilo...</td>
      <td>12</td>
      <td>Star Wars: Episode VII - The Force Awakens (2015)</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Disney (Live-Action Animations Reimagined)  Th...</td>
      <td>15</td>
      <td>The Lion King (2019)</td>
    </tr>
    <tr>
      <th>6</th>
      <td>J.K. Rowling Wizardry Harry Potter  Harry Pott...</td>
      <td>10  (or 8)</td>
      <td>Harry Potter and the Deathly Hallows, Part 2 (...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Avengers  (see Marvel's Cinematic Universe)  M...</td>
      <td>4</td>
      <td>Avengers: Endgame (2019)</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Spider-Man Spider-Man (Sam Raimi) Spider-Man (...</td>
      <td>9</td>
      <td>Spider-Man (2002)</td>
    </tr>
    <tr>
      <th>9</th>
      <td>X-Men  X-Men (Original Series) X-Men (2000)  X...</td>
      <td>12</td>
      <td>Deadpool (2016)</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Batman  Batman (Early Films) Batman: The Movie...</td>
      <td>11</td>
      <td>The Dark Knight (2008)</td>
    </tr>
    <tr>
      <th>11</th>
      <td>DC Extended  Universe  Man of Steel  (2013)  B...</td>
      <td>8</td>
      <td>Wonder Woman (2017)</td>
    </tr>
    <tr>
      <th>12</th>
      <td>James Bond (ranked) Dr. No (1962)  From Russia...</td>
      <td>25</td>
      <td>Skyfall (2012)</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jurassic Park  Jurassic Park Jurassic Park (19...</td>
      <td>5</td>
      <td>Jurassic World (2015)</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Middle Earth  The Lord of the Rings The Lord o...</td>
      <td>6</td>
      <td>The Lord of the Rings: The Return of the King ...</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Fast and the Furious  The Fast and the Furious...</td>
      <td>9</td>
      <td>Fast &amp; Furious 7 (2015)</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Transformers  Transformers: The Movie (1986)  ...</td>
      <td>7</td>
      <td>Transformers: Revenge of the Fallen (2009)</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Pirates of the Caribbean  Pirates of the Carib...</td>
      <td>5</td>
      <td>Pirates of the Caribbean: Dead Man's Chest (2006)</td>
    </tr>
    <tr>
      <th>18</th>
      <td>The Hunger Games  The Hunger Games (2012)  The...</td>
      <td>4</td>
      <td>The Hunger Games: Catching Fire (2013)</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Shrek (Original and Sequels) Shrek (2001)  Shr...</td>
      <td>5</td>
      <td>Shrek 2 (2004)</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Star Trek Star Trek (Original) Star Trek: The ...</td>
      <td>13</td>
      <td>Star Trek (2009)</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Twilight Saga  Twilight (2008)  The Twilight S...</td>
      <td>5</td>
      <td>The Twilight Saga: Eclipse (2010)</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Toy Story  (see Pixar-Disney) Toy Story (1995)...</td>
      <td>4</td>
      <td>Toy Story 4 (2019)</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Despicable Me  Despicable Me  Despicable Me (2...</td>
      <td>4</td>
      <td>Despicable Me 2 (2013)</td>
    </tr>
    <tr>
      <th>24</th>
      <td>The Dark Knight Trilogy (see Batman)  Batman B...</td>
      <td>3</td>
      <td>The Dark Knight (2008)</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Mission: Impossible  Mission: Impossible (1996...</td>
      <td>6</td>
      <td>Mission: Impossible - Fallout (2018)</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Superman  Superman (Original) Superman (1978) ...</td>
      <td>7</td>
      <td>Batman v Superman: Dawn of Justice (2016)</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Iron Man (see Marvel's Cinematic Universe) Iro...</td>
      <td>3</td>
      <td>Iron Man 3 (2013)</td>
    </tr>
    <tr>
      <th>28</th>
      <td>The Lord of the Rings Trilogy (see Middle Eart...</td>
      <td>3</td>
      <td>The Lord of the Rings: The Return of the King ...</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Indiana Jones  Raiders of the Lost Ark (1981) ...</td>
      <td>4</td>
      <td>Indiana Jones and the Kingdom of the Crystal S...</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Captain America  (see Marvel's  Cinematic Univ...</td>
      <td>3</td>
      <td>Captain America: Civil War (2016)</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Jumanji  Jumanji (1995)  Jumanji: Welcome to t...</td>
      <td>3</td>
      <td>Jumanji: Welcome to the Jungle (2017)</td>
    </tr>
    <tr>
      <th>32</th>
      <td>The Hobbit (see The Lord of the Rings) The  Ho...</td>
      <td>3</td>
      <td>The Hobbit: An Unexpected Journey (2012)</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Bourne  The Bourne Identity (2002)  The Bourne...</td>
      <td>5</td>
      <td>The Bourne Ultimatum (2007)</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Planet of the Apes  Planet of the Apes (Origin...</td>
      <td>9</td>
      <td>Dawn of the Planet of the Apes (2014)</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Ice Age  Ice Age (2002)  Ice Age: The Meltdown...</td>
      <td>5</td>
      <td>Ice Age 3: Dawn of the Dinosaurs (2009)</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Rocky  Rocky (1976)  Rocky II (1979)  Rocky II...</td>
      <td>8</td>
      <td>Rocky IV (1985)</td>
    </tr>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Page 2

url2 = 'https://www.filmsite.org/series-boxoffice2.html'
dfs2 = pd.read_html(url2)
df2 = pd.concat(dfs2)
df2.drop(columns=[0,1,5], inplace=True)
df2 = df2.rename(columns={2: 'series_list', 3:'number_of_films_in_franchise', 4:'top_movie'})
```


```python
# Page 3

url3 = 'https://www.filmsite.org/series-boxoffice3.html'
dfs3 = pd.read_html(url3)
df3 = pd.concat(dfs3)
df3.drop(columns=[0,1,5], inplace=True)
df3 = df3.rename(columns={2: 'series_list', 3:'number_of_films_in_franchise', 4:'top_movie'})
```


```python
# Page 4

url4 = 'https://www.filmsite.org/series-boxoffice4.html'
dfs4 = pd.read_html(url4)
df4 = pd.concat(dfs4)
df4.drop(columns=[0,1,5], inplace=True)
df4 = df4.rename(columns={2: 'series_list', 3:'number_of_films_in_franchise', 4:'top_movie'})
```


```python
df1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>series_list</th>
      <th>number_of_films_in_franchise</th>
      <th>top_movie</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Greatest Film Series Franchises  (part 1 of 4,...</td>
      <td>Greatest Film Series Franchises  (part 1 of 4,...</td>
      <td>Greatest Film Series Franchises  (part 1 of 4,...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Individual Films in Franchise  (if clickable, ...</td>
      <td># of Films in Franchise</td>
      <td># 1 Film in Franchise</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Marvel's Cinematic  Universe  Iron Man (2008) ...</td>
      <td>23</td>
      <td>Avengers: Endgame (2019)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Pixar-Disney Animations *  Toy Story (1995)  A...</td>
      <td>23</td>
      <td>Incredibles 2 (2018)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Star Wars  (Ranked) Star Wars: (Original Trilo...</td>
      <td>12</td>
      <td>Star Wars: Episode VII - The Force Awakens (2015)</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Disney (Live-Action Animations Reimagined)  Th...</td>
      <td>15</td>
      <td>The Lion King (2019)</td>
    </tr>
    <tr>
      <th>6</th>
      <td>J.K. Rowling Wizardry Harry Potter  Harry Pott...</td>
      <td>10  (or 8)</td>
      <td>Harry Potter and the Deathly Hallows, Part 2 (...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Avengers  (see Marvel's Cinematic Universe)  M...</td>
      <td>4</td>
      <td>Avengers: Endgame (2019)</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Spider-Man Spider-Man (Sam Raimi) Spider-Man (...</td>
      <td>9</td>
      <td>Spider-Man (2002)</td>
    </tr>
    <tr>
      <th>9</th>
      <td>X-Men  X-Men (Original Series) X-Men (2000)  X...</td>
      <td>12</td>
      <td>Deadpool (2016)</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Batman  Batman (Early Films) Batman: The Movie...</td>
      <td>11</td>
      <td>The Dark Knight (2008)</td>
    </tr>
    <tr>
      <th>11</th>
      <td>DC Extended  Universe  Man of Steel  (2013)  B...</td>
      <td>8</td>
      <td>Wonder Woman (2017)</td>
    </tr>
    <tr>
      <th>12</th>
      <td>James Bond (ranked) Dr. No (1962)  From Russia...</td>
      <td>25</td>
      <td>Skyfall (2012)</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Jurassic Park  Jurassic Park Jurassic Park (19...</td>
      <td>5</td>
      <td>Jurassic World (2015)</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Middle Earth  The Lord of the Rings The Lord o...</td>
      <td>6</td>
      <td>The Lord of the Rings: The Return of the King ...</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Fast and the Furious  The Fast and the Furious...</td>
      <td>9</td>
      <td>Fast &amp; Furious 7 (2015)</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Transformers  Transformers: The Movie (1986)  ...</td>
      <td>7</td>
      <td>Transformers: Revenge of the Fallen (2009)</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Pirates of the Caribbean  Pirates of the Carib...</td>
      <td>5</td>
      <td>Pirates of the Caribbean: Dead Man's Chest (2006)</td>
    </tr>
    <tr>
      <th>18</th>
      <td>The Hunger Games  The Hunger Games (2012)  The...</td>
      <td>4</td>
      <td>The Hunger Games: Catching Fire (2013)</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Shrek (Original and Sequels) Shrek (2001)  Shr...</td>
      <td>5</td>
      <td>Shrek 2 (2004)</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Star Trek Star Trek (Original) Star Trek: The ...</td>
      <td>13</td>
      <td>Star Trek (2009)</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Twilight Saga  Twilight (2008)  The Twilight S...</td>
      <td>5</td>
      <td>The Twilight Saga: Eclipse (2010)</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Toy Story  (see Pixar-Disney) Toy Story (1995)...</td>
      <td>4</td>
      <td>Toy Story 4 (2019)</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Despicable Me  Despicable Me  Despicable Me (2...</td>
      <td>4</td>
      <td>Despicable Me 2 (2013)</td>
    </tr>
    <tr>
      <th>24</th>
      <td>The Dark Knight Trilogy (see Batman)  Batman B...</td>
      <td>3</td>
      <td>The Dark Knight (2008)</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Mission: Impossible  Mission: Impossible (1996...</td>
      <td>6</td>
      <td>Mission: Impossible - Fallout (2018)</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Superman  Superman (Original) Superman (1978) ...</td>
      <td>7</td>
      <td>Batman v Superman: Dawn of Justice (2016)</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Iron Man (see Marvel's Cinematic Universe) Iro...</td>
      <td>3</td>
      <td>Iron Man 3 (2013)</td>
    </tr>
    <tr>
      <th>28</th>
      <td>The Lord of the Rings Trilogy (see Middle Eart...</td>
      <td>3</td>
      <td>The Lord of the Rings: The Return of the King ...</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Indiana Jones  Raiders of the Lost Ark (1981) ...</td>
      <td>4</td>
      <td>Indiana Jones and the Kingdom of the Crystal S...</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Captain America  (see Marvel's  Cinematic Univ...</td>
      <td>3</td>
      <td>Captain America: Civil War (2016)</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Jumanji  Jumanji (1995)  Jumanji: Welcome to t...</td>
      <td>3</td>
      <td>Jumanji: Welcome to the Jungle (2017)</td>
    </tr>
    <tr>
      <th>32</th>
      <td>The Hobbit (see The Lord of the Rings) The  Ho...</td>
      <td>3</td>
      <td>The Hobbit: An Unexpected Journey (2012)</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Bourne  The Bourne Identity (2002)  The Bourne...</td>
      <td>5</td>
      <td>The Bourne Ultimatum (2007)</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Planet of the Apes  Planet of the Apes (Origin...</td>
      <td>9</td>
      <td>Dawn of the Planet of the Apes (2014)</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Ice Age  Ice Age (2002)  Ice Age: The Meltdown...</td>
      <td>5</td>
      <td>Ice Age 3: Dawn of the Dinosaurs (2009)</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Rocky  Rocky (1976)  Rocky II (1979)  Rocky II...</td>
      <td>8</td>
      <td>Rocky IV (1985)</td>
    </tr>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Feature engineer function that will split the movies in the series_list into 
#separate movies and then add them into one list


def franchise_splitter(df, col):
    
    output = []
    for i in range(2, len(df)-3):
        some_list = df[col][i].split(')') 
        for item in some_list:
            item2=item[2:]
            item3=item2[:-6]
            output.append(item3)
    
    return output
```


```python
page1 = franchise_splitter(df1, 'series_list')
```


```python
page2 = franchise_splitter(df2, 'series_list')
```


```python
page3 = franchise_splitter(df3, 'series_list')
```


```python
page4 = franchise_splitter(df4, 'series_list')
```


```python
all_pages = page1 + page2 + page3 + page4
```


```python
# Feature engineer column that displays if movie is part of a franchise

def franchise_tf(movie):
    if movie in all_pages:
        return True
    else:
        return False
```


```python
#Test
display(franchise_tf('Titanic'))
franchise_tf('Thor')
```


    False





    True



## Clean tn_movie_budgets and include boolean column for if the film is part of a franchise


```python
tn_movie_budgets2 = tn_movie_budgets.copy()
```


```python
tn_movie_budgets2['is_franchise'] = tn_movie_budgets2['movie'].map(franchise_tf)
```


```python
# Make release_date into DateTime and extract year

tn_movie_budgets2['release_date'] = pd.to_datetime(tn_movie_budgets2['release_date'])
```


```python
tn_movie_budgets2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>release_date</th>
      <th>movie</th>
      <th>production_budget</th>
      <th>domestic_gross</th>
      <th>worldwide_gross</th>
      <th>is_franchise</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2009-12-18</td>
      <td>Avatar</td>
      <td>$425,000,000</td>
      <td>$760,507,625</td>
      <td>$2,776,345,279</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2011-05-20</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>$410,600,000</td>
      <td>$241,063,875</td>
      <td>$1,045,663,875</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2019-06-07</td>
      <td>Dark Phoenix</td>
      <td>$350,000,000</td>
      <td>$42,762,350</td>
      <td>$149,762,350</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>2015-05-01</td>
      <td>Avengers: Age of Ultron</td>
      <td>$330,600,000</td>
      <td>$459,005,868</td>
      <td>$1,403,013,963</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2017-12-15</td>
      <td>Star Wars Ep. VIII: The Last Jedi</td>
      <td>$317,000,000</td>
      <td>$620,181,382</td>
      <td>$1,316,721,747</td>
      <td>False</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5777</th>
      <td>78</td>
      <td>2018-12-31</td>
      <td>Red 11</td>
      <td>$7,000</td>
      <td>$0</td>
      <td>$0</td>
      <td>False</td>
    </tr>
    <tr>
      <th>5778</th>
      <td>79</td>
      <td>1999-04-02</td>
      <td>Following</td>
      <td>$6,000</td>
      <td>$48,482</td>
      <td>$240,495</td>
      <td>False</td>
    </tr>
    <tr>
      <th>5779</th>
      <td>80</td>
      <td>2005-07-13</td>
      <td>Return to the Land of Wonders</td>
      <td>$5,000</td>
      <td>$1,338</td>
      <td>$1,338</td>
      <td>False</td>
    </tr>
    <tr>
      <th>5780</th>
      <td>81</td>
      <td>2015-09-29</td>
      <td>A Plague So Pleasant</td>
      <td>$1,400</td>
      <td>$0</td>
      <td>$0</td>
      <td>False</td>
    </tr>
    <tr>
      <th>5781</th>
      <td>82</td>
      <td>2005-08-05</td>
      <td>My Date With Drew</td>
      <td>$1,100</td>
      <td>$181,041</td>
      <td>$181,041</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>5782 rows × 7 columns</p>
</div>




```python
tn_movie_budgets2['year'] = pd.DatetimeIndex(tn_movie_budgets2['release_date']).year
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>release_date</th>
      <th>movie</th>
      <th>production_budget</th>
      <th>domestic_gross</th>
      <th>worldwide_gross</th>
      <th>is_franchise</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2009-12-18</td>
      <td>Avatar</td>
      <td>$425,000,000</td>
      <td>$760,507,625</td>
      <td>$2,776,345,279</td>
      <td>False</td>
      <td>2009</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2011-05-20</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>$410,600,000</td>
      <td>$241,063,875</td>
      <td>$1,045,663,875</td>
      <td>False</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2019-06-07</td>
      <td>Dark Phoenix</td>
      <td>$350,000,000</td>
      <td>$42,762,350</td>
      <td>$149,762,350</td>
      <td>False</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>2015-05-01</td>
      <td>Avengers: Age of Ultron</td>
      <td>$330,600,000</td>
      <td>$459,005,868</td>
      <td>$1,403,013,963</td>
      <td>True</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2017-12-15</td>
      <td>Star Wars Ep. VIII: The Last Jedi</td>
      <td>$317,000,000</td>
      <td>$620,181,382</td>
      <td>$1,316,721,747</td>
      <td>False</td>
      <td>2017</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Turn production_budget/worldwide_gross/domestic_gross into integers

col_to_int(tn_movie_budgets2, 'production_budget')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>release_date</th>
      <th>movie</th>
      <th>production_budget</th>
      <th>domestic_gross</th>
      <th>worldwide_gross</th>
      <th>is_franchise</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2009-12-18</td>
      <td>Avatar</td>
      <td>425000000</td>
      <td>$760,507,625</td>
      <td>$2,776,345,279</td>
      <td>False</td>
      <td>2009</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2011-05-20</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>410600000</td>
      <td>$241,063,875</td>
      <td>$1,045,663,875</td>
      <td>False</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2019-06-07</td>
      <td>Dark Phoenix</td>
      <td>350000000</td>
      <td>$42,762,350</td>
      <td>$149,762,350</td>
      <td>False</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>2015-05-01</td>
      <td>Avengers: Age of Ultron</td>
      <td>330600000</td>
      <td>$459,005,868</td>
      <td>$1,403,013,963</td>
      <td>True</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2017-12-15</td>
      <td>Star Wars Ep. VIII: The Last Jedi</td>
      <td>317000000</td>
      <td>$620,181,382</td>
      <td>$1,316,721,747</td>
      <td>False</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5777</th>
      <td>78</td>
      <td>2018-12-31</td>
      <td>Red 11</td>
      <td>7000</td>
      <td>$0</td>
      <td>$0</td>
      <td>False</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>5778</th>
      <td>79</td>
      <td>1999-04-02</td>
      <td>Following</td>
      <td>6000</td>
      <td>$48,482</td>
      <td>$240,495</td>
      <td>False</td>
      <td>1999</td>
    </tr>
    <tr>
      <th>5779</th>
      <td>80</td>
      <td>2005-07-13</td>
      <td>Return to the Land of Wonders</td>
      <td>5000</td>
      <td>$1,338</td>
      <td>$1,338</td>
      <td>False</td>
      <td>2005</td>
    </tr>
    <tr>
      <th>5780</th>
      <td>81</td>
      <td>2015-09-29</td>
      <td>A Plague So Pleasant</td>
      <td>1400</td>
      <td>$0</td>
      <td>$0</td>
      <td>False</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>5781</th>
      <td>82</td>
      <td>2005-08-05</td>
      <td>My Date With Drew</td>
      <td>1100</td>
      <td>$181,041</td>
      <td>$181,041</td>
      <td>False</td>
      <td>2005</td>
    </tr>
  </tbody>
</table>
<p>5782 rows × 8 columns</p>
</div>




```python
col_to_int(tn_movie_budgets2, 'domestic_gross')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>release_date</th>
      <th>movie</th>
      <th>production_budget</th>
      <th>domestic_gross</th>
      <th>worldwide_gross</th>
      <th>is_franchise</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2009-12-18</td>
      <td>Avatar</td>
      <td>425000000</td>
      <td>760507625</td>
      <td>$2,776,345,279</td>
      <td>False</td>
      <td>2009</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2011-05-20</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>410600000</td>
      <td>241063875</td>
      <td>$1,045,663,875</td>
      <td>False</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2019-06-07</td>
      <td>Dark Phoenix</td>
      <td>350000000</td>
      <td>42762350</td>
      <td>$149,762,350</td>
      <td>False</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>2015-05-01</td>
      <td>Avengers: Age of Ultron</td>
      <td>330600000</td>
      <td>459005868</td>
      <td>$1,403,013,963</td>
      <td>True</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2017-12-15</td>
      <td>Star Wars Ep. VIII: The Last Jedi</td>
      <td>317000000</td>
      <td>620181382</td>
      <td>$1,316,721,747</td>
      <td>False</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5777</th>
      <td>78</td>
      <td>2018-12-31</td>
      <td>Red 11</td>
      <td>7000</td>
      <td>0</td>
      <td>$0</td>
      <td>False</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>5778</th>
      <td>79</td>
      <td>1999-04-02</td>
      <td>Following</td>
      <td>6000</td>
      <td>48482</td>
      <td>$240,495</td>
      <td>False</td>
      <td>1999</td>
    </tr>
    <tr>
      <th>5779</th>
      <td>80</td>
      <td>2005-07-13</td>
      <td>Return to the Land of Wonders</td>
      <td>5000</td>
      <td>1338</td>
      <td>$1,338</td>
      <td>False</td>
      <td>2005</td>
    </tr>
    <tr>
      <th>5780</th>
      <td>81</td>
      <td>2015-09-29</td>
      <td>A Plague So Pleasant</td>
      <td>1400</td>
      <td>0</td>
      <td>$0</td>
      <td>False</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>5781</th>
      <td>82</td>
      <td>2005-08-05</td>
      <td>My Date With Drew</td>
      <td>1100</td>
      <td>181041</td>
      <td>$181,041</td>
      <td>False</td>
      <td>2005</td>
    </tr>
  </tbody>
</table>
<p>5782 rows × 8 columns</p>
</div>




```python
col_to_int(tn_movie_budgets2, 'worldwide_gross')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>release_date</th>
      <th>movie</th>
      <th>production_budget</th>
      <th>domestic_gross</th>
      <th>worldwide_gross</th>
      <th>is_franchise</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2009-12-18</td>
      <td>Avatar</td>
      <td>425000000</td>
      <td>760507625</td>
      <td>2776345279</td>
      <td>False</td>
      <td>2009</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2011-05-20</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>410600000</td>
      <td>241063875</td>
      <td>1045663875</td>
      <td>False</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2019-06-07</td>
      <td>Dark Phoenix</td>
      <td>350000000</td>
      <td>42762350</td>
      <td>149762350</td>
      <td>False</td>
      <td>2019</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>2015-05-01</td>
      <td>Avengers: Age of Ultron</td>
      <td>330600000</td>
      <td>459005868</td>
      <td>1403013963</td>
      <td>True</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2017-12-15</td>
      <td>Star Wars Ep. VIII: The Last Jedi</td>
      <td>317000000</td>
      <td>620181382</td>
      <td>1316721747</td>
      <td>False</td>
      <td>2017</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5777</th>
      <td>78</td>
      <td>2018-12-31</td>
      <td>Red 11</td>
      <td>7000</td>
      <td>0</td>
      <td>0</td>
      <td>False</td>
      <td>2018</td>
    </tr>
    <tr>
      <th>5778</th>
      <td>79</td>
      <td>1999-04-02</td>
      <td>Following</td>
      <td>6000</td>
      <td>48482</td>
      <td>240495</td>
      <td>False</td>
      <td>1999</td>
    </tr>
    <tr>
      <th>5779</th>
      <td>80</td>
      <td>2005-07-13</td>
      <td>Return to the Land of Wonders</td>
      <td>5000</td>
      <td>1338</td>
      <td>1338</td>
      <td>False</td>
      <td>2005</td>
    </tr>
    <tr>
      <th>5780</th>
      <td>81</td>
      <td>2015-09-29</td>
      <td>A Plague So Pleasant</td>
      <td>1400</td>
      <td>0</td>
      <td>0</td>
      <td>False</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>5781</th>
      <td>82</td>
      <td>2005-08-05</td>
      <td>My Date With Drew</td>
      <td>1100</td>
      <td>181041</td>
      <td>181041</td>
      <td>False</td>
      <td>2005</td>
    </tr>
  </tbody>
</table>
<p>5782 rows × 8 columns</p>
</div>




```python
# Filter out movies based on our given criteria

tn_movie_budgets2 = tn_movie_budgets2.loc[(tn_movie_budgets2['production_budget'] > 20000000) & ((tn_movie_budgets2['year'] >= 2010)) & (tn_movie_budgets2['year'] <= 2019)]
```


```python
# Check % of movies which are franchises

tn_movie_budgets2['is_franchise'].value_counts(normalize=True)
```




    False    0.866667
    True     0.133333
    Name: is_franchise, dtype: float64



## Create Visualization A: Time trend for Franchise vs. Non-Franchise movies


```python
with plt.style.context('bmh'):
    fig, ax = plt.subplots(figsize=(10,6))
    sns.lineplot(data=tn_movie_budgets2, x='year', y='worldwide_gross', hue='is_franchise', hue_order=(True, False))
    
    ax.set_xlabel('Year')
    ax.set_ylabel('Worldwide Gross')
    ax.set_title('Comparing Worldwide Gross for Franchise vs Non-Franchise Films')
    
    formatter = FuncFormatter(millions)
    ax.yaxis.set_major_formatter(formatter)
    
    ax.legend(facecolor='white', loc=2)
    ax.set_xticks(np.arange(2010, 2020, 1));
```


    
![png](output_128_0.png)
    


### Conclusion:
- Franchise films have outperformed non-franchise films every year
- They follow a similar trend, both declined towards 2018
- Franchise films have greater standard deviation

## Create Visualization B: Linear Regression Scatter Plot displaying relationship between Worldwide Gross and Production Cost for Franchise vs. Non-Franchise Films


```python
with plt.style.context('bmh'):
    
    #g=sns.FacetGrid(data=tn_movie_budgets2, aspect=(1 * 3))
    g = (sns.lmplot(data=tn_movie_budgets2, x='production_budget', y='worldwide_gross', hue='is_franchise', height=5, aspect=(1.5)))
    
    g.axes[0,0].set_xlabel('Production Cost')
    g.axes[0,0].set_ylabel('Worldwide Gross')
    g.axes[0,0].set_title('Worldwide Gross vs. Production Budget for Franchise vs Non-Franchise Films')
    #factorplot(height=2, aspect=1)
    
    #formatter = FuncFormatter(millions)
    #g = sns.FacetGrid(data = tn_movie_budgets2, col_wrap = 4, margin_titles=True, height=3) #4/3 ratio
    #g.yaxis.set_major_formatter(formatter)
    
    t_labels = ['$50.0M', '$100.0M', '$150.0M', '$200.0M', '$250.0M', '$300.0M', '$350.0M', '$400.0M', '$450.0M', '$500.0M']
    g.set_xticklabels(t_labels)
    y_labs5 = ['$500.0M', '$1000.0M', '$1500.0M','$2000.0M', '$2500.0M', '$3000.0M', '$3500.0M']
    g.set_xticklabels(rotation = 45, ha='right')
    g.set_yticklabels(y_labs5)
#   g.set_xlim(20000000, 500000000)
    g.ax.set_xlim(10000000, 400000000)
    g.ax.set_ylim(100000, 3000000000)
    g.ax.legend(title='Franchise', facecolor='white')
```


    
![png](output_131_0.png)
    



```python

```
