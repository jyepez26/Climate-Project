# Climate-Project

## Introduction
In this project, I will be researching extreme weather events in order to find trends, conduct a hypothesis test, and create a predictive model for future weather events.

I gathered my data from the climate.gov website, where I downloaded csv files of severe storms and extreme events from 1950-2024. Here is the url of the base website: https://www.climate.gov/maps-data/dataset/severe-storms-and-extreme-events-data-table

Here is some information about the dataset:

## Data Cleaning
My first step was to iterate through all the different zip files of csv files and load them into one large pandas dataframe. In order to be able to iterate through the files using a simple f string, I had to generalize them, which I accomplished through shortening the unique identifier of about 8 characters at the end of each file name. Once I cleaned up the file names, I was then able to load them all into a large pandas dataframe through iteration. My next step was to drop unnecessary columns from the dataframe. 

```
Woah, look at this cool code: System.out.print(hello world)
```
