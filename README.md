# Climate-Project

## Introduction
In this project, I will be researching extreme weather events in order to find trends, conduct a hypothesis test, and create a predictive model for future weather events.

I gathered my data from the climate.gov website, where I downloaded csv files of severe storms and extreme events from 1950-2024. Here is the url of the base website: https://www.climate.gov/maps-data/dataset/severe-storms-and-extreme-events-data-table

Here is some information about the dataset:

## Data Cleaning
My first step was to iterate through all the different zip files of csv files and load them into one large pandas dataframe. In order to be able to iterate through the files using a simple f string, I had to generalize them, which I accomplished through shortening the unique identifier of about 8 characters at the end of each file name. Once I cleaned up the file names, I was then able to load them all into a large pandas dataframe through iteration. 

Now that we have the rough dataset loaded into a pandas dataframe, our next step is to drop unnecessary columns: `YEAR`, `MONTH_NAME`, `BEGIN_DATE_TIME`, `END_DATE_TIME`.
We can drop these columns because they are repititve of other time data contained in the dataset. However, these other columns need to be formatted properly into one DateTime column. To do so we use datetime and timedelta objects to combine the `BEGIN_YEARMONTH`, `BEGIN_DAY`, and `BEGIN_TIME` columns as well as the `END_YEARMONTH`, `END_DAY`, and `END_TIME` into `BEGIN_DATETIME` and `END_DATETIME` respectively.

Now, the first few columns and rows of our dataframe will look like this:

## Exploratory Data Analysis
### Univariate Analysis
I first wanted to see how the magintude of extreme weather events changed over time. There was a clear positive trend in the average magnitude from 1980 until the present. However, this could be a result of difference in volume, so it is hard to be completely sure this trend is correct.
<iframe
  src="year_vs_magnitude_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

```
Woah, look at this cool code: System.out.print(hello world)
```
