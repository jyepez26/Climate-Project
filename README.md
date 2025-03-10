# Climate-Project

## Introduction
In this project, I will be researching extreme weather events in order to find trends, conduct a hypothesis test, and create a predictive model for future weather events.

I gathered my data from the climate.gov website, where I downloaded csv files of severe storms and extreme events from 1950-2024. Here is the url of the base website: https://www.climate.gov/maps-data/dataset/severe-storms-and-extreme-events-data-table

Here is some information about the dataset:

## Data Cleaning
My first step was to iterate through all the different zip files of csv files and load them into one large pandas dataframe. In order to be able to iterate through the files using a simple f string, I had to generalize them, which I accomplished through shortening the unique identifier of about 8 characters at the end of each file name. Once I cleaned up the file names, I was then able to load them all into a large pandas dataframe through iteration. 

Now that we have the rough dataset loaded into a pandas dataframe, our next step is to drop unnecessary columns: `MONTH_NAME`, `BEGIN_DATE_TIME`, `END_DATE_TIME`.
We can drop these columns because they are repititve of other time data contained in the dataset. However, these other columns need to be formatted properly into one DateTime column. To do so we use datetime and timedelta objects to combine the `BEGIN_YEARMONTH`, `BEGIN_DAY`, and `BEGIN_TIME` columns as well as the `END_YEARMONTH`, `END_DAY`, and `END_TIME` into `BEGIN_DATETIME` and `END_DATETIME` respectively.

Next, I found that the values in the `DAMAGE_PROPERTY` column were not floats, but strings formatted such as '150K' or '100M' instead. Therefore, I used a function and mapping to convert these values into their respective float values.

Now, the first few columns and rows of our dataframe will look like this:<table border="1" class="dataframe">\n  <thead>\n    <tr style="text-align: right;">\n      <th></th>\n      <th>BEGIN_DATETIME</th>\n      <th>END_DATETIME</th>\n      <th>STATE</th>\n      <th>EVENT_TYPE</th>\n      <th>EVENT_ID</th>\n      <th>INJURIES_DIRECT</th>\n      <th>MAGNITUDE</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>0</th>\n      <td>1980-04-13 00:30:00</td>\n      <td>1980-04-13 00:30:00</td>\n      <td>LOUISIANA</td>\n      <td>Hail</td>\n      <td>10046120</td>\n      <td>0</td>\n      <td>1.75</td>\n    </tr>\n    <tr>\n      <th>1</th>\n      <td>1980-05-29 14:10:00</td>\n      <td>1980-05-29 14:10:00</td>\n      <td>NEBRASKA</td>\n      <td>Hail</td>\n      <td>10065016</td>\n      <td>0</td>\n      <td>1.75</td>\n    </tr>\n    <tr>\n      <th>2</th>\n      <td>1980-07-22 00:10:00</td>\n      <td>1980-07-22 00:10:00</td>\n      <td>NEBRASKA</td>\n      <td>Thunderstorm Wind</td>\n      <td>10065375</td>\n      <td>0</td>\n      <td>0.00</td>\n    </tr>\n    <tr>\n      <th>3</th>\n      <td>1980-09-03 18:30:00</td>\n      <td>1980-09-03 18:30:00</td>\n      <td>MINNESOTA</td>\n      <td>Thunderstorm Wind</td>\n      <td>10054907</td>\n      <td>0</td>\n      <td>70.00</td>\n    </tr>\n    <tr>\n      <th>4</th>\n      <td>1980-09-17 08:30:00</td>\n      <td>1980-09-17 08:30:00</td>\n      <td>ALABAMA</td>\n      <td>Thunderstorm Wind</td>\n      <td>9975991</td>\n      <td>0</td>\n      <td>0.00</td>\n    </tr>\n  </tbody>\n</table>

## Exploratory Data Analysis
### Univariate Analysis
I first wanted to see how the magintude of extreme weather events changed over time. There was a clear positive trend in the average magnitude from 1980 until the present. However, this could be a result of difference in volume, so it is hard to be completely sure this trend is correct.
<iframe
  src="assets/year_vs_magnitude_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Next, I looked at the different types of extreme weather events. My first thought when exploring this was which events were the most common and which were the least common. This is important to look at in order to better understand the distibution of the data. First, I plotted a bar chart representing the count of every single weather event in the dataset.
<iframe
  src="assets/event_type_count_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
However, from this I observed that the top few events dominated the count, and anything outside the top 10-20 most popular events were quite infrequent. Therefore, I made a new plot just looking at the count of the top 10 most common weather events to get a better sense of their count.
<iframe
  src="assets/top10_event_type_count_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Bivariate Analysis
I next moved on to looking into some of the relationships between mutliple columns.
I first wanted to figure out the most destructive events. Therefore, I plotted the average damage of each type of event and took the top 10 most destructive event types to display in the plot, using columns `EVENT_TYPE` and `DAMAGE_PROPERTY`. These findings show that tornadoes, TSTM (low risk thunderstorm) wind, and hail caused the most damage by a long amount.
<iframe
  src="assets/avg_damage_by_event_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I also explored the relationship between columns `MAGNITUDE` and `INJURIES_DIRECT` expecting to see a positive correlation between the two--as the magnitude of the event increased so would the amount of injuries caused directly by the extreme weather event. However, the results surprised me: there was no noticeable correlation! In fact, there seemed to be some events reported with more injuries for the events with lower magnitudes. Not enough for a negative correlation, but still an interesting observation.

## Hypothesis Testing
Hurricanes are a prevalent event in our dataset. Climate change is proven to significantly impact the intensity and
speed of hurricanes! We want to test if our data aligns with this fact and follows these scientifically proven trends. We will measure
the change in speed which is proven to actually decrease due to climate change, which leads to a longer duration of the hurricane
and therefore more time to create damage.
Here are our parameters for our hypothesis test:
#### Null Hypothesis: There is no significant change in the duration of hurricanes over the past 50 years.
#### Alternative Hypothesis: The duration of hurricanes has increased significantly over the past 50 years.
#### Test statistic: Difference in means of hurricane duration (1996-2010) and (2010-2024).
#### Statistically significant p-value: 0.05

For my hypothesis test, I randomly shuffled the `YEAR` column 1000 times, each time calculation an observation in the same way I did originally. This
allows for me to see if our observed value is significant or if any random simulation would produce a similar value. Here is my plotted histogram of the observed values from my simulation compared to my observed value from the original data:
<iframe
  src="assets/avg_damage_by_event_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

After conducting this hypothesis test, I found some shocking results. My data actually proved the opposite of what I was trying to show! With a
p value of 1.0, it turns out that my observed value was statistically significant, but in the wrong way. Which means that my test is essentially
saying that the duration of hurricanes has _decreased_ over time rather than increased which does not align with the fact that climate change
is proven to decrease the speed of hurricanes which _increases_ their duration. Therefore, while I cannot conclude anything for certain from this
data, I am going to speculate that I did not have enough data in my dataset to have proper evidence to conduct this climate analysis.

```
Unfortunately, the dataset I was using in this was not complete enough to use for a trustworthy data analysis, and I had to cut my project short.
This project did give me good practice with data cleaning, as the dataset was very messy when I first received and I was able to make it suitable
for analysis, as well as practice with data exploration. I did get practice with hypothesis testing additionally, but through it I discovered
that my dataset was problematic. Therefore, I am stopping short and not using this data to create a predictive model like I planned, since
I don't believe that it will be a fair and accurate model. On to the next project!
```
