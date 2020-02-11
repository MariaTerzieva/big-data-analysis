# Setup
### Programming Language
I used Python as the programming language for this assignment (version 3.6). The reason I used this Python version is not because I prefer it to newer versions. I tried to install newer versions on my machine but there were some problems (mainly because of my Linux distribution that is not the newest) so because of that I decided to go with this Python version. Plus, this was the Python version I used on my last job and I was most familiar with it.
### Additional Tools
#### Jupyter Notebook (version 6.0.3)
I used Jupyter Notebook because it provides me a nice way to structure my solution by giving me the opportunity to have the code, description and results all in one place, thus making it easy to have visibility on the data analysis process every single step of the way.
#### PySpark (version 2.4.5)
Since the solution needs to be flexible and scalable, I decided to go with PySpark because it is good at processing big data.
#### PySparkAudit (version 1.0.0)
I used PySparkAudit for easy analysis on the data and some visualization on that analysis. [Here](https://runawayhorse001.github.io/PySparkAudit/) is a link to its user guide.
#### Pandas (version 1.0.1)
For prettier visualization of datasets.
# Data
I used the [NYC Bike Share Data](https://www.citibikenyc.com/system-data) and took the advice to use only a portion of the data so I went for February 2016 since the [weather dataset](https://www.kaggle.com/mathijs/weather-data-in-new-york-city-2016) I found was for 2016. I used my laptop for processing purposes.
# Data Load
Both datasets were available in CSV format so I used PySpark's **read.csv** functionality and provided a schema for each dataset to make sure that all data types are read as I want them to. Plus, providing a schema explicitely saves us processing time opposed to using PySpark's functionality to infer the schema. I used **nullable=True** to deal with null values. I also provided **timestampFormat** and **dateFormat** to make sure dates and timestamps are read correctly. And lastly, I used the **ignoreLeadingWhiteSpace** and **ignoreTrailingWhiteSpace** to clean unnecessary white spaces just in case, although the data looked pretty clean at first glance. I converted a small portion of each dataset (5 records) to Pandas and visualized it to make sure that everything is imported correctly. I used Pandas for visualization since it looks prettier than PySpark's **show**.
# Data Checks and Validation
I checked if all data is from the right period of time and validated that columns that accept only specific values do not contain any other values.
# Data Prep
For the purposes of joining the bike share dataset with the weather dataset, I split the data in the *Start Time* and *Stop Time* columns into separate date and time columns in order to join on the date column. I also filtered the weather dataset to include data for February 2016 only and converted *T* values to 0.01 in order to have all the data in numeric format (since the loss of accuracy in this case was negligible). If we wanted to maintain all accuracy and still have the data from the *precipitaion*, *snow fall* and *snow depth* columns in numeric format, after joining the two datasets we had to exclude all records with *T* values into another dataset and deal with them separately. I decided that for the pursposes of this task that would be an overkill. After I joined the bike share and the weather datasets together, I dropped the redundant *date* column since it contained the same data as the *Start Day* column.
# Analysis and Visualization
First, I counted all the notnull and distinct values in each column of the full dataset. This way I found out that there are missing values in the *Birth Year* column which could be of use for further analysis purposes.

Counting the distinct values per column, gave me the information that there are 35 different start stations, 39 different end stations and 254 different bikes that were used throughout the month of February 2016.

Further analysis showed that the min temperature for February 2016 was -1F and that the maximum temperature was 61F, the oldest customer/subsriber that we have information for and that used the service in February 2016 was born in 1947 and the youngest - in 1999, etc.

Most popular start and end station was *Grove St PATH* with 998 trips that started and 1440 trips that ended there. Throughout February the service was used more by suscribers than by customers and more by men than by women. Most trips were made when there was no snow coverage or precipitation but there were some trips when the snow depth was 1 or 2 inches, etc.

Although the relationship between temperature and trip duration is weak, there is a negative correlation between the two which means that if minimum temperature decreses or maximum temperature increases that trip duration decreases. The same is true also for the relationship between trip duration and precipitation. The correlations are very weak though and no general conclusions can be drawn. Correlation map was saved in the *Audited* folder (unfortunately with some of the variable names slightly cut off but there wasn't any option to try to correct this).

Lastly, some analysis was made on which the most popular hour of the day, day of the week and day of the month is to take a (long) trip and the results can be seen in the Jupyter notebook.