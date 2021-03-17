#  Final Project Submission Test
Please fill out:

- Student name: Ethan Kunin
- Student pace: Full Time
- Scheduled project review date/time: March 23rd 4:00pm EST
- Instructor name: James Irving
- Blog post URL: https://github.com/kuninethan95/dsc-phase-1-project

# Business Problem

Microsoft sees all the big companies creating original video content and they want to get in on the fun. They have decided to create a new movie studio, but they don’t know anything about creating movies. You are charged with exploring what types of films are currently doing the best at the box office. You must then translate those findings into actionable insights that the head of Microsoft's new movie studio can use to help decide what type of films to create.

## Questions and Conclusions

- Which directors generate the highest revenue per film?
- What month generates the highest revenue for films?
- What film length generates the highest revenue?
- Do franchise films earn more than non-franchise films


```python
# Import necessary libraries and packages

import os,glob
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib as mpl
import seaborn as sns
import requests
```


```python
# Display where the data is contained

folder = "/Users/ethankunin/Documents/Flatiron/Phase_1/Movie_Project1/dsc-phase-1-project/zippedData/"
os.listdir(folder)
```




    ['imdb.title.crew.csv.gz',
     'tmdb.movies.csv.gz',
     'imdb.title.akas.csv.gz',
     'imdb.title.ratings.csv.gz',
     'imdb.name.basics.csv.gz',
     'rt.reviews.tsv.gz',
     'imdb.title.basics.csv.gz',
     'rt.movie_info.tsv.gz',
     'tn.movie_budgets.csv.gz',
     'bom.movie_gross.csv.gz',
     'imdb.title.principals.csv.gz']




```python
files = glob.glob(f"{folder}*.csv*")
```


```python
# Load in files and display preview

tables = {}
dashes='---'*25

for file in files:
    ## Save a variable-friendly version of the file name
    table_name = file.replace('.csv.gz','').split('/')[-1].replace('.','_')
    print(dashes)
    
    ## Load and preview dataframe
    print(f"Preview of {table_name}")
    tables[table_name] = pd.read_csv(file)
    display(tables[table_name].head(5))
    print()
```

    ---------------------------------------------------------------------------
    Preview of imdb_title_crew



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
      <th>tconst</th>
      <th>directors</th>
      <th>writers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tt0285252</td>
      <td>nm0899854</td>
      <td>nm0899854</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tt0438973</td>
      <td>NaN</td>
      <td>nm0175726,nm1802864</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tt0462036</td>
      <td>nm1940585</td>
      <td>nm1940585</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tt0835418</td>
      <td>nm0151540</td>
      <td>nm0310087,nm0841532</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tt0878654</td>
      <td>nm0089502,nm2291498,nm2292011</td>
      <td>nm0284943</td>
    </tr>
  </tbody>
</table>
</div>


    
    ---------------------------------------------------------------------------
    Preview of tmdb_movies



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
      <th>Unnamed: 0</th>
      <th>genre_ids</th>
      <th>id</th>
      <th>original_language</th>
      <th>original_title</th>
      <th>popularity</th>
      <th>release_date</th>
      <th>title</th>
      <th>vote_average</th>
      <th>vote_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>[12, 14, 10751]</td>
      <td>12444</td>
      <td>en</td>
      <td>Harry Potter and the Deathly Hallows: Part 1</td>
      <td>33.533</td>
      <td>2010-11-19</td>
      <td>Harry Potter and the Deathly Hallows: Part 1</td>
      <td>7.7</td>
      <td>10788</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>[14, 12, 16, 10751]</td>
      <td>10191</td>
      <td>en</td>
      <td>How to Train Your Dragon</td>
      <td>28.734</td>
      <td>2010-03-26</td>
      <td>How to Train Your Dragon</td>
      <td>7.7</td>
      <td>7610</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>[12, 28, 878]</td>
      <td>10138</td>
      <td>en</td>
      <td>Iron Man 2</td>
      <td>28.515</td>
      <td>2010-05-07</td>
      <td>Iron Man 2</td>
      <td>6.8</td>
      <td>12368</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>[16, 35, 10751]</td>
      <td>862</td>
      <td>en</td>
      <td>Toy Story</td>
      <td>28.005</td>
      <td>1995-11-22</td>
      <td>Toy Story</td>
      <td>7.9</td>
      <td>10174</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>[28, 878, 12]</td>
      <td>27205</td>
      <td>en</td>
      <td>Inception</td>
      <td>27.920</td>
      <td>2010-07-16</td>
      <td>Inception</td>
      <td>8.3</td>
      <td>22186</td>
    </tr>
  </tbody>
</table>
</div>


    
    ---------------------------------------------------------------------------
    Preview of imdb_title_akas



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
      <th>title_id</th>
      <th>ordering</th>
      <th>title</th>
      <th>region</th>
      <th>language</th>
      <th>types</th>
      <th>attributes</th>
      <th>is_original_title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tt0369610</td>
      <td>10</td>
      <td>Джурасик свят</td>
      <td>BG</td>
      <td>bg</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tt0369610</td>
      <td>11</td>
      <td>Jurashikku warudo</td>
      <td>JP</td>
      <td>NaN</td>
      <td>imdbDisplay</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tt0369610</td>
      <td>12</td>
      <td>Jurassic World: O Mundo dos Dinossauros</td>
      <td>BR</td>
      <td>NaN</td>
      <td>imdbDisplay</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tt0369610</td>
      <td>13</td>
      <td>O Mundo dos Dinossauros</td>
      <td>BR</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>short title</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tt0369610</td>
      <td>14</td>
      <td>Jurassic World</td>
      <td>FR</td>
      <td>NaN</td>
      <td>imdbDisplay</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>


    
    ---------------------------------------------------------------------------
    Preview of imdb_title_ratings



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
      <th>tconst</th>
      <th>averagerating</th>
      <th>numvotes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tt10356526</td>
      <td>8.3</td>
      <td>31</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tt10384606</td>
      <td>8.9</td>
      <td>559</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tt1042974</td>
      <td>6.4</td>
      <td>20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tt1043726</td>
      <td>4.2</td>
      <td>50352</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tt1060240</td>
      <td>6.5</td>
      <td>21</td>
    </tr>
  </tbody>
</table>
</div>


    
    ---------------------------------------------------------------------------
    Preview of imdb_name_basics



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
      <th>nconst</th>
      <th>primary_name</th>
      <th>birth_year</th>
      <th>death_year</th>
      <th>primary_profession</th>
      <th>known_for_titles</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>nm0061671</td>
      <td>Mary Ellen Bauder</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>miscellaneous,production_manager,producer</td>
      <td>tt0837562,tt2398241,tt0844471,tt0118553</td>
    </tr>
    <tr>
      <th>1</th>
      <td>nm0061865</td>
      <td>Joseph Bauer</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>composer,music_department,sound_department</td>
      <td>tt0896534,tt6791238,tt0287072,tt1682940</td>
    </tr>
    <tr>
      <th>2</th>
      <td>nm0062070</td>
      <td>Bruce Baum</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>miscellaneous,actor,writer</td>
      <td>tt1470654,tt0363631,tt0104030,tt0102898</td>
    </tr>
    <tr>
      <th>3</th>
      <td>nm0062195</td>
      <td>Axel Baumann</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>camera_department,cinematographer,art_department</td>
      <td>tt0114371,tt2004304,tt1618448,tt1224387</td>
    </tr>
    <tr>
      <th>4</th>
      <td>nm0062798</td>
      <td>Pete Baxter</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>production_designer,art_department,set_decorator</td>
      <td>tt0452644,tt0452692,tt3458030,tt2178256</td>
    </tr>
  </tbody>
</table>
</div>


    
    ---------------------------------------------------------------------------
    Preview of imdb_title_basics



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
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tt0063540</td>
      <td>Sunghursh</td>
      <td>Sunghursh</td>
      <td>2013</td>
      <td>175.0</td>
      <td>Action,Crime,Drama</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tt0066787</td>
      <td>One Day Before the Rainy Season</td>
      <td>Ashad Ka Ek Din</td>
      <td>2019</td>
      <td>114.0</td>
      <td>Biography,Drama</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tt0069049</td>
      <td>The Other Side of the Wind</td>
      <td>The Other Side of the Wind</td>
      <td>2018</td>
      <td>122.0</td>
      <td>Drama</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tt0069204</td>
      <td>Sabse Bada Sukh</td>
      <td>Sabse Bada Sukh</td>
      <td>2018</td>
      <td>NaN</td>
      <td>Comedy,Drama</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tt0100275</td>
      <td>The Wandering Soap Opera</td>
      <td>La Telenovela Errante</td>
      <td>2017</td>
      <td>80.0</td>
      <td>Comedy,Drama,Fantasy</td>
    </tr>
  </tbody>
</table>
</div>


    
    ---------------------------------------------------------------------------
    Preview of tn_movie_budgets



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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Dec 18, 2009</td>
      <td>Avatar</td>
      <td>$425,000,000</td>
      <td>$760,507,625</td>
      <td>$2,776,345,279</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>May 20, 2011</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>$410,600,000</td>
      <td>$241,063,875</td>
      <td>$1,045,663,875</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Jun 7, 2019</td>
      <td>Dark Phoenix</td>
      <td>$350,000,000</td>
      <td>$42,762,350</td>
      <td>$149,762,350</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>May 1, 2015</td>
      <td>Avengers: Age of Ultron</td>
      <td>$330,600,000</td>
      <td>$459,005,868</td>
      <td>$1,403,013,963</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Dec 15, 2017</td>
      <td>Star Wars Ep. VIII: The Last Jedi</td>
      <td>$317,000,000</td>
      <td>$620,181,382</td>
      <td>$1,316,721,747</td>
    </tr>
  </tbody>
</table>
</div>


    
    ---------------------------------------------------------------------------
    Preview of bom_movie_gross



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
      <th>title</th>
      <th>studio</th>
      <th>domestic_gross</th>
      <th>foreign_gross</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Toy Story 3</td>
      <td>BV</td>
      <td>415000000.0</td>
      <td>652000000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alice in Wonderland (2010)</td>
      <td>BV</td>
      <td>334200000.0</td>
      <td>691300000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Harry Potter and the Deathly Hallows Part 1</td>
      <td>WB</td>
      <td>296000000.0</td>
      <td>664300000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Inception</td>
      <td>WB</td>
      <td>292600000.0</td>
      <td>535700000</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Shrek Forever After</td>
      <td>P/DW</td>
      <td>238700000.0</td>
      <td>513900000</td>
      <td>2010</td>
    </tr>
  </tbody>
</table>
</div>


    
    ---------------------------------------------------------------------------
    Preview of imdb_title_principals



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
      <th>tconst</th>
      <th>ordering</th>
      <th>nconst</th>
      <th>category</th>
      <th>job</th>
      <th>characters</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tt0111414</td>
      <td>1</td>
      <td>nm0246005</td>
      <td>actor</td>
      <td>NaN</td>
      <td>["The Man"]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tt0111414</td>
      <td>2</td>
      <td>nm0398271</td>
      <td>director</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>tt0111414</td>
      <td>3</td>
      <td>nm3739909</td>
      <td>producer</td>
      <td>producer</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tt0323808</td>
      <td>10</td>
      <td>nm0059247</td>
      <td>editor</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tt0323808</td>
      <td>1</td>
      <td>nm3579312</td>
      <td>actress</td>
      <td>NaN</td>
      <td>["Beth Boothby"]</td>
    </tr>
  </tbody>
</table>
</div>


    


# Description of each table with unique identifiers

- imdb_title_crew: id's to link crew members with titles

- tmdb_movies - titles and stats from IMDB

- imdb_title_akas - link between id and movie title

- imdb_title_ratings - link between title and IMDB ratings

- imdb_name_basics - name of cast member and id

- imdb_title_basics - movie title, id, and runtime

- tn_movie_budgets - movie title, release date, and earnings/costs

- bom_movie_gross - movie title, studio, and earnings

- imdb_title_principals - link between movie title and cast cast id


```python
# Link each file to a Pandas dataframe

filepath0 = files[0]
imdb_title_crew = pd.read_csv(filepath0)
```


```python
filepath1 = files[1]
tmdb_movies = pd.read_csv(filepath1)
```


```python
filepath2 = files[2]
imdb_title_akas = pd.read_csv(filepath2)
```


```python
filepath3 = files[3]
imdb_title_ratings = pd.read_csv(filepath3)
```


```python
filepath4 = files[4]
imdb_name_basics = pd.read_csv(filepath4)
```


```python
filepath5 = files[5]
imdb_title_basics = pd.read_csv(filepath5)
```


```python
filepath6 = files[6]
tn_movie_budgets = pd.read_csv(filepath6)
```


```python
filepath7 = files[7]
bom_movie_gross = pd.read_csv(filepath7)
```


```python
filepath8 = files[8]
imdb_title_principals = pd.read_csv(filepath8)
```

# Analyze how runtime impacts revenue
- Do longer movies earn more revenue than shorter movies?
- Is there correlation between film length and run time?


```python
# Merge movie budgets/earnings with titles to display runtime

movie_rt = tn_movie_budgets.merge(imdb_title_basics, left_on='movie', right_on='original_title', how='inner')
```


```python
movie_rt.head()
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
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>May 20, 2011</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>$410,600,000</td>
      <td>$241,063,875</td>
      <td>$1,045,663,875</td>
      <td>tt1298650</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>2011</td>
      <td>136.0</td>
      <td>Action,Adventure,Fantasy</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>Jun 7, 2019</td>
      <td>Dark Phoenix</td>
      <td>$350,000,000</td>
      <td>$42,762,350</td>
      <td>$149,762,350</td>
      <td>tt6565702</td>
      <td>Dark Phoenix</td>
      <td>Dark Phoenix</td>
      <td>2019</td>
      <td>113.0</td>
      <td>Action,Adventure,Sci-Fi</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>May 1, 2015</td>
      <td>Avengers: Age of Ultron</td>
      <td>$330,600,000</td>
      <td>$459,005,868</td>
      <td>$1,403,013,963</td>
      <td>tt2395427</td>
      <td>Avengers: Age of Ultron</td>
      <td>Avengers: Age of Ultron</td>
      <td>2015</td>
      <td>141.0</td>
      <td>Action,Adventure,Sci-Fi</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>Apr 27, 2018</td>
      <td>Avengers: Infinity War</td>
      <td>$300,000,000</td>
      <td>$678,815,482</td>
      <td>$2,048,134,200</td>
      <td>tt4154756</td>
      <td>Avengers: Infinity War</td>
      <td>Avengers: Infinity War</td>
      <td>2018</td>
      <td>149.0</td>
      <td>Action,Adventure,Sci-Fi</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9</td>
      <td>Nov 17, 2017</td>
      <td>Justice League</td>
      <td>$300,000,000</td>
      <td>$229,024,295</td>
      <td>$655,945,209</td>
      <td>tt0974015</td>
      <td>Justice League</td>
      <td>Justice League</td>
      <td>2017</td>
      <td>120.0</td>
      <td>Action,Adventure,Fantasy</td>
    </tr>
  </tbody>
</table>
</div>




```python
movie_rt.info()

# Going to have to turn production_budget/domestic_gross/worldwide_gross into integers
# Only column with significant null values is runtime_minutues, may account for this by imputing the median
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 3537 entries, 0 to 3536
    Data columns (total 12 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   id                 3537 non-null   int64  
     1   release_date       3537 non-null   object 
     2   movie              3537 non-null   object 
     3   production_budget  3537 non-null   object 
     4   domestic_gross     3537 non-null   object 
     5   worldwide_gross    3537 non-null   object 
     6   tconst             3537 non-null   object 
     7   primary_title      3537 non-null   object 
     8   original_title     3537 non-null   object 
     9   start_year         3537 non-null   int64  
     10  runtime_minutes    3070 non-null   float64
     11  genres             3473 non-null   object 
    dtypes: float64(1), int64(2), object(9)
    memory usage: 359.2+ KB


## Clean Data
- Sort for commercial release by only including films with production_budgets > $20,000000 (classified as commercial budget)
- Only use movies from 2010 onwards (streaming more prominence)
- Convert production_budget/domestic_gross/worldwide_gross into integers
- Impute runtime_minutes with either mean/median
- Check for outliers
- Feature engineer short/medium/long
- Drop unnecessary columns
- https://www.marketwatch.com/story/netflix-reportedly-set-to-produce-90-movies-a-year-with-budgets-up-to-200-million-2018-12-16


```python
# Feature engineer function to turn budget/gross into integers

def col_to_int(df, colm):
    df[colm] = df[colm].map(lambda x: x.replace('$', '')).map(lambda x: x.replace(',', '')).astype('int')
    return df
```


```python
col_to_int(movie_rt, 'production_budget')
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
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>May 20, 2011</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>410600000</td>
      <td>$241,063,875</td>
      <td>$1,045,663,875</td>
      <td>tt1298650</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>2011</td>
      <td>136.0</td>
      <td>Action,Adventure,Fantasy</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>Jun 7, 2019</td>
      <td>Dark Phoenix</td>
      <td>350000000</td>
      <td>$42,762,350</td>
      <td>$149,762,350</td>
      <td>tt6565702</td>
      <td>Dark Phoenix</td>
      <td>Dark Phoenix</td>
      <td>2019</td>
      <td>113.0</td>
      <td>Action,Adventure,Sci-Fi</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>May 1, 2015</td>
      <td>Avengers: Age of Ultron</td>
      <td>330600000</td>
      <td>$459,005,868</td>
      <td>$1,403,013,963</td>
      <td>tt2395427</td>
      <td>Avengers: Age of Ultron</td>
      <td>Avengers: Age of Ultron</td>
      <td>2015</td>
      <td>141.0</td>
      <td>Action,Adventure,Sci-Fi</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>Apr 27, 2018</td>
      <td>Avengers: Infinity War</td>
      <td>300000000</td>
      <td>$678,815,482</td>
      <td>$2,048,134,200</td>
      <td>tt4154756</td>
      <td>Avengers: Infinity War</td>
      <td>Avengers: Infinity War</td>
      <td>2018</td>
      <td>149.0</td>
      <td>Action,Adventure,Sci-Fi</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9</td>
      <td>Nov 17, 2017</td>
      <td>Justice League</td>
      <td>300000000</td>
      <td>$229,024,295</td>
      <td>$655,945,209</td>
      <td>tt0974015</td>
      <td>Justice League</td>
      <td>Justice League</td>
      <td>2017</td>
      <td>120.0</td>
      <td>Action,Adventure,Fantasy</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3532</th>
      <td>68</td>
      <td>Jul 6, 2001</td>
      <td>Cure</td>
      <td>10000</td>
      <td>$94,596</td>
      <td>$94,596</td>
      <td>tt5936960</td>
      <td>Cure</td>
      <td>Cure</td>
      <td>2014</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3533</th>
      <td>70</td>
      <td>Apr 1, 1996</td>
      <td>Bang</td>
      <td>10000</td>
      <td>$527</td>
      <td>$527</td>
      <td>tt6616538</td>
      <td>Bang</td>
      <td>Bang</td>
      <td>2015</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3534</th>
      <td>73</td>
      <td>Jan 13, 2012</td>
      <td>Newlyweds</td>
      <td>9000</td>
      <td>$4,584</td>
      <td>$4,584</td>
      <td>tt1880418</td>
      <td>Newlyweds</td>
      <td>Newlyweds</td>
      <td>2011</td>
      <td>95.0</td>
      <td>Comedy,Drama</td>
    </tr>
    <tr>
      <th>3535</th>
      <td>78</td>
      <td>Dec 31, 2018</td>
      <td>Red 11</td>
      <td>7000</td>
      <td>$0</td>
      <td>$0</td>
      <td>tt7837402</td>
      <td>Red 11</td>
      <td>Red 11</td>
      <td>2019</td>
      <td>77.0</td>
      <td>Horror,Sci-Fi,Thriller</td>
    </tr>
    <tr>
      <th>3536</th>
      <td>81</td>
      <td>Sep 29, 2015</td>
      <td>A Plague So Pleasant</td>
      <td>1400</td>
      <td>$0</td>
      <td>$0</td>
      <td>tt2107644</td>
      <td>A Plague So Pleasant</td>
      <td>A Plague So Pleasant</td>
      <td>2013</td>
      <td>76.0</td>
      <td>Drama,Horror,Thriller</td>
    </tr>
  </tbody>
</table>
<p>3537 rows × 12 columns</p>
</div>




```python
col_to_int(movie_rt, 'domestic_gross')
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
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>May 20, 2011</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>410600000</td>
      <td>241063875</td>
      <td>$1,045,663,875</td>
      <td>tt1298650</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>2011</td>
      <td>136.0</td>
      <td>Action,Adventure,Fantasy</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>Jun 7, 2019</td>
      <td>Dark Phoenix</td>
      <td>350000000</td>
      <td>42762350</td>
      <td>$149,762,350</td>
      <td>tt6565702</td>
      <td>Dark Phoenix</td>
      <td>Dark Phoenix</td>
      <td>2019</td>
      <td>113.0</td>
      <td>Action,Adventure,Sci-Fi</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>May 1, 2015</td>
      <td>Avengers: Age of Ultron</td>
      <td>330600000</td>
      <td>459005868</td>
      <td>$1,403,013,963</td>
      <td>tt2395427</td>
      <td>Avengers: Age of Ultron</td>
      <td>Avengers: Age of Ultron</td>
      <td>2015</td>
      <td>141.0</td>
      <td>Action,Adventure,Sci-Fi</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>Apr 27, 2018</td>
      <td>Avengers: Infinity War</td>
      <td>300000000</td>
      <td>678815482</td>
      <td>$2,048,134,200</td>
      <td>tt4154756</td>
      <td>Avengers: Infinity War</td>
      <td>Avengers: Infinity War</td>
      <td>2018</td>
      <td>149.0</td>
      <td>Action,Adventure,Sci-Fi</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9</td>
      <td>Nov 17, 2017</td>
      <td>Justice League</td>
      <td>300000000</td>
      <td>229024295</td>
      <td>$655,945,209</td>
      <td>tt0974015</td>
      <td>Justice League</td>
      <td>Justice League</td>
      <td>2017</td>
      <td>120.0</td>
      <td>Action,Adventure,Fantasy</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3532</th>
      <td>68</td>
      <td>Jul 6, 2001</td>
      <td>Cure</td>
      <td>10000</td>
      <td>94596</td>
      <td>$94,596</td>
      <td>tt5936960</td>
      <td>Cure</td>
      <td>Cure</td>
      <td>2014</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3533</th>
      <td>70</td>
      <td>Apr 1, 1996</td>
      <td>Bang</td>
      <td>10000</td>
      <td>527</td>
      <td>$527</td>
      <td>tt6616538</td>
      <td>Bang</td>
      <td>Bang</td>
      <td>2015</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3534</th>
      <td>73</td>
      <td>Jan 13, 2012</td>
      <td>Newlyweds</td>
      <td>9000</td>
      <td>4584</td>
      <td>$4,584</td>
      <td>tt1880418</td>
      <td>Newlyweds</td>
      <td>Newlyweds</td>
      <td>2011</td>
      <td>95.0</td>
      <td>Comedy,Drama</td>
    </tr>
    <tr>
      <th>3535</th>
      <td>78</td>
      <td>Dec 31, 2018</td>
      <td>Red 11</td>
      <td>7000</td>
      <td>0</td>
      <td>$0</td>
      <td>tt7837402</td>
      <td>Red 11</td>
      <td>Red 11</td>
      <td>2019</td>
      <td>77.0</td>
      <td>Horror,Sci-Fi,Thriller</td>
    </tr>
    <tr>
      <th>3536</th>
      <td>81</td>
      <td>Sep 29, 2015</td>
      <td>A Plague So Pleasant</td>
      <td>1400</td>
      <td>0</td>
      <td>$0</td>
      <td>tt2107644</td>
      <td>A Plague So Pleasant</td>
      <td>A Plague So Pleasant</td>
      <td>2013</td>
      <td>76.0</td>
      <td>Drama,Horror,Thriller</td>
    </tr>
  </tbody>
</table>
<p>3537 rows × 12 columns</p>
</div>




```python
col_to_int(movie_rt, 'worldwide_gross')
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
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>May 20, 2011</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>410600000</td>
      <td>241063875</td>
      <td>1045663875</td>
      <td>tt1298650</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>Pirates of the Caribbean: On Stranger Tides</td>
      <td>2011</td>
      <td>136.0</td>
      <td>Action,Adventure,Fantasy</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>Jun 7, 2019</td>
      <td>Dark Phoenix</td>
      <td>350000000</td>
      <td>42762350</td>
      <td>149762350</td>
      <td>tt6565702</td>
      <td>Dark Phoenix</td>
      <td>Dark Phoenix</td>
      <td>2019</td>
      <td>113.0</td>
      <td>Action,Adventure,Sci-Fi</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>May 1, 2015</td>
      <td>Avengers: Age of Ultron</td>
      <td>330600000</td>
      <td>459005868</td>
      <td>1403013963</td>
      <td>tt2395427</td>
      <td>Avengers: Age of Ultron</td>
      <td>Avengers: Age of Ultron</td>
      <td>2015</td>
      <td>141.0</td>
      <td>Action,Adventure,Sci-Fi</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>Apr 27, 2018</td>
      <td>Avengers: Infinity War</td>
      <td>300000000</td>
      <td>678815482</td>
      <td>2048134200</td>
      <td>tt4154756</td>
      <td>Avengers: Infinity War</td>
      <td>Avengers: Infinity War</td>
      <td>2018</td>
      <td>149.0</td>
      <td>Action,Adventure,Sci-Fi</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9</td>
      <td>Nov 17, 2017</td>
      <td>Justice League</td>
      <td>300000000</td>
      <td>229024295</td>
      <td>655945209</td>
      <td>tt0974015</td>
      <td>Justice League</td>
      <td>Justice League</td>
      <td>2017</td>
      <td>120.0</td>
      <td>Action,Adventure,Fantasy</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3532</th>
      <td>68</td>
      <td>Jul 6, 2001</td>
      <td>Cure</td>
      <td>10000</td>
      <td>94596</td>
      <td>94596</td>
      <td>tt5936960</td>
      <td>Cure</td>
      <td>Cure</td>
      <td>2014</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3533</th>
      <td>70</td>
      <td>Apr 1, 1996</td>
      <td>Bang</td>
      <td>10000</td>
      <td>527</td>
      <td>527</td>
      <td>tt6616538</td>
      <td>Bang</td>
      <td>Bang</td>
      <td>2015</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3534</th>
      <td>73</td>
      <td>Jan 13, 2012</td>
      <td>Newlyweds</td>
      <td>9000</td>
      <td>4584</td>
      <td>4584</td>
      <td>tt1880418</td>
      <td>Newlyweds</td>
      <td>Newlyweds</td>
      <td>2011</td>
      <td>95.0</td>
      <td>Comedy,Drama</td>
    </tr>
    <tr>
      <th>3535</th>
      <td>78</td>
      <td>Dec 31, 2018</td>
      <td>Red 11</td>
      <td>7000</td>
      <td>0</td>
      <td>0</td>
      <td>tt7837402</td>
      <td>Red 11</td>
      <td>Red 11</td>
      <td>2019</td>
      <td>77.0</td>
      <td>Horror,Sci-Fi,Thriller</td>
    </tr>
    <tr>
      <th>3536</th>
      <td>81</td>
      <td>Sep 29, 2015</td>
      <td>A Plague So Pleasant</td>
      <td>1400</td>
      <td>0</td>
      <td>0</td>
      <td>tt2107644</td>
      <td>A Plague So Pleasant</td>
      <td>A Plague So Pleasant</td>
      <td>2013</td>
      <td>76.0</td>
      <td>Drama,Horror,Thriller</td>
    </tr>
  </tbody>
</table>
<p>3537 rows × 12 columns</p>
</div>




```python
# Filter out movies with production budgets under $20,000,000

movie_rt = movie_rt.loc[movie_rt['production_budget'] > 20000000]
```


```python
# Feature Engineer year column. Not going to use DateTime yet because will want to engineer a seasonal column
# Convert year into an int

movie_rt['year'] = movie_rt['release_date'].map(lambda x: x[-4:])
movie_rt['year'] = movie_rt['year'].astype('int')
```

    <ipython-input-22-b8c95b37f53e>:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      movie_rt['year'] = movie_rt['release_date'].map(lambda x: x[-4:])
    <ipython-input-22-b8c95b37f53e>:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      movie_rt['year'] = movie_rt['year'].astype('int')



```python
# Slice out movies from before 2010

movie_rt = movie_rt[movie_rt['year'] >= 2010]
```


```python
# We have 8.2% of movies with null runtime_minutes

(movie_rt['runtime_minutes'].isna().sum())/len(movie_rt)* 100
```




    8.018018018018019




```python
# Check for duplicates
# 298 duplicates, upon inspection doesn't look like there's a reason other than inner join
#Drop if they contain the same movie title and release date

movie_rt[movie_rt.duplicated(subset=['movie', 'release_date'])]
movie_rt.duplicated(subset=['movie', 'release_date']).sum()
movie_rt.drop
```




    <bound method DataFrame.drop of       id  release_date                                        movie  \
    0      2  May 20, 2011  Pirates of the Caribbean: On Stranger Tides   
    1      3   Jun 7, 2019                                 Dark Phoenix   
    2      4   May 1, 2015                      Avengers: Age of Ultron   
    3      7  Apr 27, 2018                       Avengers: Infinity War   
    4      9  Nov 17, 2017                               Justice League   
    ...   ..           ...                                          ...   
    1578  69   Aug 4, 2017                                       Kidnap   
    1579  72   Dec 9, 2011                    Tinker Tailor Soldier Spy   
    1581  79   May 6, 2011                                   The Beaver   
    1582  80  Feb 24, 2017                               Bitter Harvest   
    1586  82   Feb 1, 2019                               Velvet Buzzsaw   
    
          production_budget  domestic_gross  worldwide_gross     tconst  \
    0             410600000       241063875       1045663875  tt1298650   
    1             350000000        42762350        149762350  tt6565702   
    2             330600000       459005868       1403013963  tt2395427   
    3             300000000       678815482       2048134200  tt4154756   
    4             300000000       229024295        655945209  tt0974015   
    ...                 ...             ...              ...        ...   
    1578           21000000        30718107         34836080  tt9603698   
    1579           21000000        24149393         81452811  tt1340800   
    1581           21000000          970816          5046038  tt1321860   
    1582           21000000          557241           606162  tt3182620   
    1586           21000000               0                0  tt7043012   
    
                                        primary_title  \
    0     Pirates of the Caribbean: On Stranger Tides   
    1                                    Dark Phoenix   
    2                         Avengers: Age of Ultron   
    3                          Avengers: Infinity War   
    4                                  Justice League   
    ...                                           ...   
    1578                                       Kidnap   
    1579                    Tinker Tailor Soldier Spy   
    1581                                   The Beaver   
    1582                               Bitter Harvest   
    1586                               Velvet Buzzsaw   
    
                                       original_title  start_year  \
    0     Pirates of the Caribbean: On Stranger Tides        2011   
    1                                    Dark Phoenix        2019   
    2                         Avengers: Age of Ultron        2015   
    3                          Avengers: Infinity War        2018   
    4                                  Justice League        2017   
    ...                                           ...         ...   
    1578                                       Kidnap        2012   
    1579                    Tinker Tailor Soldier Spy        2011   
    1581                                   The Beaver        2011   
    1582                               Bitter Harvest        2017   
    1586                               Velvet Buzzsaw        2019   
    
          runtime_minutes                    genres  year  
    0               136.0  Action,Adventure,Fantasy  2011  
    1               113.0   Action,Adventure,Sci-Fi  2019  
    2               141.0   Action,Adventure,Sci-Fi  2015  
    3               149.0   Action,Adventure,Sci-Fi  2018  
    4               120.0  Action,Adventure,Fantasy  2017  
    ...               ...                       ...   ...  
    1578             50.0           Horror,Thriller  2017  
    1579            122.0    Drama,Mystery,Thriller  2011  
    1581             91.0                     Drama  2011  
    1582            103.0         Drama,Romance,War  2017  
    1586            113.0   Horror,Mystery,Thriller  2019  
    
    [1110 rows x 13 columns]>




```python
# Clear out movies with runtimes under 80 minutes
#https://screenwriting.io/what-is-a-feature-film/#:~:text=A%20modern%20feature%20is%20typically,than%2040%20minutes%20a%20feature.

movie_rt = movie_rt[(movie_rt['runtime_minutes'] > 80) & (movie_rt['runtime_minutes'] < 180)]
```


```python
movie_rt.isna().sum()
```




    id                   0
    release_date         0
    movie                0
    production_budget    0
    domestic_gross       0
    worldwide_gross      0
    tconst               0
    primary_title        0
    original_title       0
    start_year           0
    runtime_minutes      0
    genres               0
    year                 0
    dtype: int64




```python
# Find the approxomate cutoff for short/medium/long movies based on quintile
# For analysis, want an equal amount of observations in each row

display(np.quantile(movie_rt['runtime_minutes'], 0.33))
np.quantile(movie_rt['runtime_minutes'], 0.66)
```


    102.0





    117.0




```python
#Now we check again for null values

(movie_rt['runtime_minutes'].isna().sum())/len(movie_rt)* 100
```




    0.0




```python
# Feature engineer function to categorize movie lenght by duration
# Use .33 and 0.66 quintile to distinguish length

def length(mins):
    if mins < 100:
        return 'short'
    elif mins < 120:
        return 'medium'
    else:
        return 'long'
```


```python
movie_rt['duration'] = movie_rt['runtime_minutes'].map(length)
```

    <ipython-input-31-612a18e56b11>:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      movie_rt['duration'] = movie_rt['runtime_minutes'].map(length)



```python
# Most movies fall into the short category, then medium, then long

movie_rt.groupby('duration').count()
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
      <th>tconst</th>
      <th>primary_title</th>
      <th>original_title</th>
      <th>start_year</th>
      <th>runtime_minutes</th>
      <th>genres</th>
      <th>year</th>
    </tr>
    <tr>
      <th>duration</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>long</th>
      <td>273</td>
      <td>273</td>
      <td>273</td>
      <td>273</td>
      <td>273</td>
      <td>273</td>
      <td>273</td>
      <td>273</td>
      <td>273</td>
      <td>273</td>
      <td>273</td>
      <td>273</td>
      <td>273</td>
    </tr>
    <tr>
      <th>medium</th>
      <td>398</td>
      <td>398</td>
      <td>398</td>
      <td>398</td>
      <td>398</td>
      <td>398</td>
      <td>398</td>
      <td>398</td>
      <td>398</td>
      <td>398</td>
      <td>398</td>
      <td>398</td>
      <td>398</td>
    </tr>
    <tr>
      <th>short</th>
      <td>250</td>
      <td>250</td>
      <td>250</td>
      <td>250</td>
      <td>250</td>
      <td>250</td>
      <td>250</td>
      <td>250</td>
      <td>250</td>
      <td>250</td>
      <td>250</td>
      <td>250</td>
      <td>250</td>
    </tr>
  </tbody>
</table>
</div>



## Visualization A: Create boxplot to illustrate how runtime impacts revenue


```python
from matplotlib.ticker import FuncFormatter

def millions(x, pos):
    return '%1.1fM' % (x * 1e-6)

#https://stackoverflow.com/questions/61330427/set-y-axis-in-millions

with plt.style.context('bmh'): 
    fig, ax = plt.subplots(figsize=(8, 6))
    ax = sns.boxplot(x='duration', y='worldwide_gross', data=movie_rt, palette='crest', order=['short', 'medium', 'long'])
    
    ax.set_ylim(5000000, 1200000000)
    ax.set_title('Movie Gross Based on Duration')
    ax.set_ylabel('Worldwide Gross')
    ax.set_xlabel('Duration')
    formatter = FuncFormatter(millions)
    ax.yaxis.set_major_formatter(formatter)
    x_labs = ['80-100 mins', '100-120 mins', '120-180 mins']
    ax.set_xticklabels(x_labs);

```


    
![png](output_39_0.png)
    


### Conclusion 
- Long movies have the highest median
- Long movies have the largest distributional spread
- Short and medium length movies have similar distributions
- Overall, I recommend making long movies based on this figure

## Visualization B: Create linear regression plot to illustrate how runtime impacts revenue


```python
with plt.style.context('bmh'):
    fig, ax = plt.subplots(figsize=(8, 6))
    ax = sns.regplot(x='runtime_minutes', y='worldwide_gross', data=movie_rt, marker ="x")

    ax.set_title('Movie Gross Based on Duration')
    ax.set_ylabel('Worldwide Gross')
    ax.set_xlabel('Minutes')
    ax.axvline(x=100, color='cornflowerblue', linestyle='solid', label='short', alpha=0.5)
    ax.axvline(x=120, color='cornflowerblue', linestyle='solid', label='medium', alpha=0.5)
    formatter = FuncFormatter(millions)
    ax.yaxis.set_major_formatter(formatter)
    ax.legend();
```


    
![png](output_42_0.png)
    


### Conclusion 
- There is a positive trend between film duration and worldwide gross
- Long movies have more outlier values

# Analyze which directors generate the highest revenue
- Are the top directors generally profitable?
- How much revenue do top directors' films earn?
- Are all films that top directors produce domestically and globabally profitable


```python
# Merge to create table that shows primary name with nconst/tconst id

name_titles = imdb_name_basics.merge(imdb_title_principals, how='left', on='nconst')
name_titles.head()
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
      <th>nconst</th>
      <th>primary_name</th>
      <th>birth_year</th>
      <th>death_year</th>
      <th>primary_profession</th>
      <th>known_for_titles</th>
      <th>tconst</th>
      <th>ordering</th>
      <th>category</th>
      <th>job</th>
      <th>characters</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>nm0061671</td>
      <td>Mary Ellen Bauder</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>miscellaneous,production_manager,producer</td>
      <td>tt0837562,tt2398241,tt0844471,tt0118553</td>
      <td>tt2398241</td>
      <td>9.0</td>
      <td>producer</td>
      <td>producer</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>nm0061865</td>
      <td>Joseph Bauer</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>composer,music_department,sound_department</td>
      <td>tt0896534,tt6791238,tt0287072,tt1682940</td>
      <td>tt0433397</td>
      <td>7.0</td>
      <td>composer</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>nm0061865</td>
      <td>Joseph Bauer</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>composer,music_department,sound_department</td>
      <td>tt0896534,tt6791238,tt0287072,tt1682940</td>
      <td>tt1681372</td>
      <td>8.0</td>
      <td>composer</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>nm0061865</td>
      <td>Joseph Bauer</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>composer,music_department,sound_department</td>
      <td>tt0896534,tt6791238,tt0287072,tt1682940</td>
      <td>tt2387710</td>
      <td>8.0</td>
      <td>composer</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>nm0061865</td>
      <td>Joseph Bauer</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>composer,music_department,sound_department</td>
      <td>tt0896534,tt6791238,tt0287072,tt1682940</td>
      <td>tt2281215</td>
      <td>7.0</td>
      <td>composer</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Merge to create table that shows primary name and job function

names_movies = imdb_title_akas.merge(name_titles, how='inner', left_on='title_id', right_on='tconst')
```

## Clean Data
- Sort for only directors and US verion of movies
- Handle duplicates
- Handle null values


```python
# Filter out non-directors

names_movies = names_movies[names_movies['category'] == 'director']
```


```python
# Sort such that we are only showing the American version use 'US'

names_movies = names_movies[names_movies['region'] == 'US']
```


```python
# Filter out directors who are no longer alive

names_movies = names_movies[names_movies['death_year'].isna()]
names_movies.head()
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
      <th>title_id</th>
      <th>ordering_x</th>
      <th>title</th>
      <th>region</th>
      <th>language</th>
      <th>types</th>
      <th>attributes</th>
      <th>is_original_title</th>
      <th>nconst</th>
      <th>primary_name</th>
      <th>birth_year</th>
      <th>death_year</th>
      <th>primary_profession</th>
      <th>known_for_titles</th>
      <th>tconst</th>
      <th>ordering_y</th>
      <th>category</th>
      <th>job</th>
      <th>characters</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>125</th>
      <td>tt0369610</td>
      <td>21</td>
      <td>Jurassic World 3D</td>
      <td>US</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3-D version</td>
      <td>0.0</td>
      <td>nm1119880</td>
      <td>Colin Trevorrow</td>
      <td>1976.0</td>
      <td>NaN</td>
      <td>writer,producer,director</td>
      <td>tt0369610,tt4881806,tt4572792,tt1862079</td>
      <td>tt0369610</td>
      <td>5.0</td>
      <td>director</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>205</th>
      <td>tt0369610</td>
      <td>29</td>
      <td>Jurassic World</td>
      <td>US</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>nm1119880</td>
      <td>Colin Trevorrow</td>
      <td>1976.0</td>
      <td>NaN</td>
      <td>writer,producer,director</td>
      <td>tt0369610,tt4881806,tt4572792,tt1862079</td>
      <td>tt0369610</td>
      <td>5.0</td>
      <td>director</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>215</th>
      <td>tt0369610</td>
      <td>2</td>
      <td>Ebb Tide</td>
      <td>US</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>fake working title</td>
      <td>0.0</td>
      <td>nm1119880</td>
      <td>Colin Trevorrow</td>
      <td>1976.0</td>
      <td>NaN</td>
      <td>writer,producer,director</td>
      <td>tt0369610,tt4881806,tt4572792,tt1862079</td>
      <td>tt0369610</td>
      <td>5.0</td>
      <td>director</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>285</th>
      <td>tt0369610</td>
      <td>36</td>
      <td>Jurassic Park IV</td>
      <td>US</td>
      <td>NaN</td>
      <td>working</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>nm1119880</td>
      <td>Colin Trevorrow</td>
      <td>1976.0</td>
      <td>NaN</td>
      <td>writer,producer,director</td>
      <td>tt0369610,tt4881806,tt4572792,tt1862079</td>
      <td>tt0369610</td>
      <td>5.0</td>
      <td>director</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>375</th>
      <td>tt0369610</td>
      <td>44</td>
      <td>Jurassic Park 4</td>
      <td>US</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>informal alternative title</td>
      <td>0.0</td>
      <td>nm1119880</td>
      <td>Colin Trevorrow</td>
      <td>1976.0</td>
      <td>NaN</td>
      <td>writer,producer,director</td>
      <td>tt0369610,tt4881806,tt4572792,tt1862079</td>
      <td>tt0369610</td>
      <td>5.0</td>
      <td>director</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Merge to create table that shows primary name and movie

movie_direct = movie_rt.merge(names_movies, how='left', left_on='movie', right_on='title')
```


```python
# Clean up excess columns from merge

movie_direct.columns
cols_to_remove = ['original_title', 'runtime_minutes', 'genres', 'year', 'duration', 'title_id', 
                  'title', 'types','nconst','primary_profession', 'known_for_titles',
                  'tconst_y', 'category', 'job', 'primary_title', 'start_year', 'ordering_x', 'attributes',
                   'language', 'is_original_title', 'ordering_y', 'characters']
```


```python
movie_direct = movie_direct.drop(columns=(cols_to_remove))
```


```python
# Check for duplicates from merge

(movie_direct.duplicated(subset=['release_date', 'movie'])).sum()
```




    518




```python
movie_direct = movie_direct.drop_duplicates(subset=['release_date', 'movie'])
```


```python
# Remove null values

movie_direct[movie_direct['primary_name'].isna()]
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
      <th>tconst_x</th>
      <th>region</th>
      <th>primary_name</th>
      <th>birth_year</th>
      <th>death_year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8</th>
      <td>10</td>
      <td>Nov 6, 2015</td>
      <td>Spectre</td>
      <td>300000000</td>
      <td>200074175</td>
      <td>879620923</td>
      <td>tt2379713</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>11</td>
      <td>Jul 20, 2012</td>
      <td>The Dark Knight Rises</td>
      <td>275000000</td>
      <td>448139099</td>
      <td>1084439099</td>
      <td>tt1345836</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>89</th>
      <td>88</td>
      <td>Jul 1, 2016</td>
      <td>The Legend of Tarzan</td>
      <td>180000000</td>
      <td>126643061</td>
      <td>348902025</td>
      <td>tt0918940</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>144</th>
      <td>36</td>
      <td>Dec 21, 2018</td>
      <td>Aquaman</td>
      <td>160000000</td>
      <td>335061807</td>
      <td>1146894640</td>
      <td>tt1477834</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>150</th>
      <td>46</td>
      <td>Jun 10, 2016</td>
      <td>Warcraft</td>
      <td>160000000</td>
      <td>47225655</td>
      <td>425522281</td>
      <td>tt0803096</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
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
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1306</th>
      <td>49</td>
      <td>Oct 31, 2014</td>
      <td>Before I Go to Sleep</td>
      <td>22000000</td>
      <td>3242457</td>
      <td>19563579</td>
      <td>tt1726592</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1313</th>
      <td>52</td>
      <td>Dec 31, 2013</td>
      <td>Metegol</td>
      <td>22000000</td>
      <td>0</td>
      <td>34061097</td>
      <td>tt1634003</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1314</th>
      <td>58</td>
      <td>Jun 22, 2012</td>
      <td>To Rome with Love</td>
      <td>21500000</td>
      <td>16684352</td>
      <td>74326015</td>
      <td>tt1859650</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1325</th>
      <td>79</td>
      <td>May 6, 2011</td>
      <td>The Beaver</td>
      <td>21000000</td>
      <td>970816</td>
      <td>5046038</td>
      <td>tt1321860</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1326</th>
      <td>80</td>
      <td>Feb 24, 2017</td>
      <td>Bitter Harvest</td>
      <td>21000000</td>
      <td>557241</td>
      <td>606162</td>
      <td>tt3182620</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>127 rows × 11 columns</p>
</div>




```python
movie_direct = movie_direct.dropna(axis=0, subset=['primary_name'])
```

## Categorize/List Top Directors
- Groupby sum/mean of domestic/worlwide gross
- Feature engineer T/F of profitable/unprofitable for scatter plot


```python
# Prepare plots by grouping by top 20 director name, type of gross, mean and sum

tp_dgmean = movie_direct.groupby('primary_name')['domestic_gross'].mean().sort_values().nlargest(20).reset_index()
tp_dgmean
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
