#  Movie Analysis
Please fill out:

- Student name: Ethan Kunin
- Student pace: Full Time
- Scheduled project review date/time: March 23rd 4:00pm EST
- Instructor name: James Irving
- Blog post URL: https://github.com/kuninethan95/dsc-phase-1-project

# Business Problem

Microsoft sees all the big companies creating original video content and they want to get in on the fun. They have decided to create a new movie studio, but they don’t know anything about creating movies. You are charged with exploring what types of films are currently doing the best at the box office. You must then translate those findings into actionable insights that the head of Microsoft's new movie studio can use to help decide what type of films to create.

<center><img src="./Images/output_39_0.png" width=75%></center>



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


# Apendix B: Analyze how franchises perform compared to non-franchises
- Check if franchise films generate higher worldwide revenue on aveerage than non-franchise films
- Explore how this trend has varied over the past 10 years
- See which type of film has a greater positive correlation between production buget and revenue
- Use this site to gather data: https://www.filmsite.org/


### *Visualization A: Time trend for Franchise vs. Non-Franchise movies*


image goes here


### *Conclusion:*
- Franchise films have outperformed non-franchise films every year
- They follow a similar trend, both declined towards 2018
- Franchise films have greater standard deviation

### *Visualization B: Linear Regression Scatter Plot displaying relationship between Worldwide Gross and Production Cost for Franchise vs. Non-Franchise Films*

image goes here

### *Conclusion:*
- Franchise films have a stronger positive relationship between revenue and cost than non-franchise films