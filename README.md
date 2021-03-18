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














### *Visualization A: Top 20 Directors, 2 Graphs to display WW and Mean/Sum*


image will go here
    


### *Conclusion*
- Many of the directors on the left chart also appear on the right
- Valuable list to parse through when considering who will direct the first film
- Outliers are a valuable data point because they can make a significant impact on balance sheet




### *Visualization B: Analyze profitablity and relationship between budget and worldwide revenue for Top Directors*


image will go here
    

### *Conclusion* 
- Positive relationship between production budget & worldwide gross
- Not all movies are domestically profitable
- All movies are profitable when comparing production budget to worldwide gross

# Analyze when the best time to release a movie is
- Create visualization based on month showing average earnings
- Analyze which season is generates the highest amount of revenue
- Analyze which month generates the highest revenue per film on average

   
image
    

### *Visualization A: Stacked (domestic + worldwide) bar plot showing how much revenue films generate per month on average*




image goes here
    
![png](output_87_0.png)
    


### *Conclusion*
- June movie generate the highest revenue on average followed by May and July
- Worldwide gross is greater than domestic gross in every month



### *Visualization B: Boxplot of worldwide gross based on season*


image goes here
    
![png](output_92_0.png)
    


### Conclusion 
- Spring and Summer films have the highest medians
- Spring and Summer films have larger distributions
- Corresponds with our month observations

### Apendix A: Analyze how movies have performed the past 10 years through trends
- Analyze how production costs have changed over the last 10 years
- Check if domestic versus worldwise gross move in the same directions
- Check if there are any years when costs exceeded either domestic or worldwide revenue



### *Visualization A: Lineplot of worldwide/domestic gross/cost based on year*


image goes here


    
![png](output_100_0.png)


 ### *Conclusion*
 - In 2017 worldwide revenue began to decline
 - In 2018 domestic revenue began to decline
 - Production costs have steadily risen but remain relatively flat
 - Worldwide revenue has exceeded production costs every year
 - Between 2018 and 2019 production costs exceeded domestic revenue


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
