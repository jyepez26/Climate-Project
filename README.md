# Climate-Project
<iframe
  src="assets/hurricane_image.jpg"
  width="800"
  height="600"
  frameborder="0"
></iframe>
## Introduction
In this project, I will be researching extreme weather events in order to find trends, conduct a hypothesis test, and create a predictive model for future weather events.

I gathered my data from the climate.gov website, where I downloaded csv files of severe storms and extreme events from 1950-2024. However, due to high volume I am only using data from 1980-2024. Here is the url of the base website: https://www.climate.gov/maps-data/dataset/severe-storms-and-extreme-events-data-table

The data are collected by the National Weather Service. Weather offices detect the events by using instruments and visual observations, as well as receiving information from storm spotters (people who call in to report severe weather events). Tornadoes, high wind speeds, and storm cell data are collected using radar. Here are the descriptions of some of the relevant columns in the dataset to be used in my project:
| Column                    | Description                                                              |
|---------------------------|--------------------------------------------------------------------------|
| `'YEAR'`                  | Year of the event                                                      |
| `'EPISODE_ID'`            | ID assigned by NWS to denote the storm episode; Episodes may contain multiple Events   |
| `'EVENT_ID'`           | ID assigned by NWS for each individual storm event contained within a storm episode                |
| `'STATE'`        | The state name where the event occurred |
| `'STATE_FIPS'`      | A unique number (State Federal Information Processing Standard) assigned to the county by the National Institute for Standards and Technology (NIST) |
| `'EVENT_TYPE'`         | Type of extreme weather event. Includes only types listed in Table 1 of Section 2.1.1 of NWS Directive 10-1605  |
| `'CZ_FIPS'`     | The county FIPS number is a unique number assigned to the county by the National Institute for Standards and Technology (NIST) or NWS Forecast Zone Number                               |
| `'WFO'`     | The National Weather Service Forecast Office’s area of responsibility (County Warning Area) in which the event occurred  |
| `'BEGIN_DATE_TIME'` | Start of event datetime; MM/DD/YYYY hh:mm:ss (24 hour time usually in LST)               |
| `'END_DATE_TIME'` | End of event datetime; MM/DD/YYYY hh:mm:ss (24 hour time usually in LST)                |
| `'INJURIES_DIRECT'`        | The number of injuries directly caused by the weather event                      |
| `'DEATHS_DIRECT'`       | The number of deaths directly caused by the weather event         |
| `'DAMAGE_PROPERTY'`        | The estimated amount of damage to property incurred by the weather event (e.g. 10.00K = $10,000; 10.00M = $10,000,000)               |
| `'MAGNITUDE'`    | The measured extent of the magnitude type ~ only used for wind speeds (in knots) and hail size (in inches to the hundredth)           |
| `'BEGIN_LOCATION'`    | The name of city, town or village from which the range is calculated and the azimuth is determined   |


## Data Cleaning
My first step was to iterate through all the different zip files of csv files and load them into one large pandas dataframe. In order to be able to iterate through the files using a simple f string, I had to generalize them, which I accomplished through shortening the unique identifier of about 8 characters at the end of each file name. Once I cleaned up the file names, I was then able to load them all into a large pandas dataframe through iteration. 

Now that we have the rough dataset loaded into a pandas dataframe, our next step is to drop unnecessary columns: `MONTH_NAME`, `BEGIN_DATE_TIME`, `END_DATE_TIME`.
We can drop these columns because they are repititve of other time data contained in the dataset. However, these other columns need to be formatted properly into one DateTime column. To do so we use datetime and timedelta objects to combine the `BEGIN_YEARMONTH`, `BEGIN_DAY`, and `BEGIN_TIME` columns as well as the `END_YEARMONTH`, `END_DAY`, and `END_TIME` into `BEGIN_DATETIME` and `END_DATETIME` respectively.

Next, I found that the values in the `DAMAGE_PROPERTY` column were not floats, but strings formatted such as '150K' or '100M' instead. Therefore, I used a function and mapping to convert these values into their respective float values.

Now, the first few columns and rows of our dataframe will look like this:
<table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>BEGIN_DATETIME</th>      <th>END_DATETIME</th>      <th>STATE</th>      <th>EVENT_TYPE</th>      <th>EVENT_ID</th>      <th>INJURIES_DIRECT</th>      <th>DAMAGE_PROPERTY</th>      <th>MAGNITUDE</th>    </tr>  </thead>  <tbody>    <tr>      <th>0</th>      <td>1980-04-13 00:30:00</td>      <td>1980-04-13 00:30:00</td>      <td>LOUISIANA</td>      <td>Hail</td>      <td>10046120</td>      <td>0</td>      <td>0.0</td>      <td>1.75</td>    </tr>    <tr>      <th>1</th>      <td>1980-05-29 14:10:00</td>      <td>1980-05-29 14:10:00</td>      <td>NEBRASKA</td>      <td>Hail</td>      <td>10065016</td>      <td>0</td>      <td>0.0</td>      <td>1.75</td>    </tr>    <tr>      <th>2</th>      <td>1980-07-22 00:10:00</td>      <td>1980-07-22 00:10:00</td>      <td>NEBRASKA</td>      <td>Thunderstorm Wind</td>      <td>10065375</td>      <td>0</td>      <td>0.0</td>      <td>0.00</td>    </tr>    <tr>      <th>3</th>      <td>1980-09-03 18:30:00</td>      <td>1980-09-03 18:30:00</td>      <td>MINNESOTA</td>      <td>Thunderstorm Wind</td>      <td>10054907</td>      <td>0</td>      <td>0.0</td>      <td>70.00</td>    </tr>    <tr>      <th>4</th>      <td>1980-09-17 08:30:00</td>      <td>1980-09-17 08:30:00</td>      <td>ALABAMA</td>      <td>Thunderstorm Wind</td>      <td>9975991</td>      <td>0</td>      <td>0.0</td>      <td>0.00</td>    </tr>  </tbody></table>

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
  src="assets/hyp_test_plot.html"
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
