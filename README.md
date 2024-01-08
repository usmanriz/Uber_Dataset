
<h1 style =  "background-color: black ; color: white; text-align: center; text-shadow: 2.5px 2px; border-radius: 8px; font-family:Times New Roman, Times, serif; padding: 25px" > UBER DATA ANALYSIS </h1><p>Author: Muhammad Usman </p> Dated: 06/Jan/23

<b> Dataset Overview:</b>

The dataset, focusing on Uber drives, was explored and analyzed for better understanding.

<strong>Data Cleaning:</strong>

Imported necessary libraries for data manipulation and visualization.<br>
Established alternative color codes for visualizations.<br>
Loaded the dataset using the Pandas library and created a cleaned version.<br>
Handled special characters, missing values, and duplicates.<br>
Split and formatted date columns for better analysis.<br>

<b>Data Analysis:</b>

Explored the dataset through various methods:<br>
Checked the number of rows and columns.<br>
Obtained a concise summary of the dataset.<br>
Calculated statistics, checked for missing values, and explored unique values.<br>
Investigated combinations of start-stop locations, purposes, and categories.<br>
Identified duplicates and cleaned the dataset accordingly.<br>

<b>Data Visualization:</b>

Visualized key aspects of the data to derive insights:<br>
Explored ride categories, trip purposes, and day-wise trip counts.<br>
Analyzed the number of trips per month and day of the week.<br>
Identified the top start and stop locations.<br>
Investigated round-trip versus single-trip distribution.<br>
Examined the distribution of trip durations and miles per category.<br>
<h2 style = "background-color: grey; color: white; text-shadow:1.5px 1.5px; border-radius: 20px; text-align: center; font-family:Times New Roman, Times, serif; padding: 20px; display: inline-block" > Data Analyzing on Uber Drives</h2>
* <b>Import Libraries</b>
# This Python 3 environment comes with many helpful analytics libraries installed

# Import libraries for data manuplication
import pandas as pd
import numpy as numpy

# import libraries for data visulization
import matplotlib.pyplot as plt
import seaborn as sns

# Warning ignore
import warnings
warnings.filterwarnings("ignore") 
* <b>Selection of color for data~visulization</b>
# Alternative color codes

#1F78B4 (blue)
#33A02C (green)
#FF7F00 (orange)

# Creating a Seaborn color palette with two colors (blue and green)
mck_color2_alternative = sns.color_palette(['#1F78B4', '#33A02C'])

# Creating a blended Seaborn color palette with 9 colors (blue to green gradient)
mck_color_alternative = sns.blend_palette(['#1F78B4', '#33A02C', 'black'], 9)

# Creating a blended Seaborn color palette with 12 colors (blue to green gradient)
mck_color12_alternative = sns.blend_palette(['#1F78B4', '#33A02C', 'black'], 12)

# Creating a blended Seaborn color palette with 25 colors (purple to blue to green gradient)
mck_color25_alternative = sns.blend_palette(['#6A0572', '#1F78B4', '#33A02C', 'black'], 25)

# Setting the Seaborn color palette to 'Blues'
sns.color_palette('Blues')

# Setting the Seaborn color palette to mck_color_alternative
sns.color_palette(mck_color_alternative)

* <b> Load dataset using Pandas Library
original_df = pd.read_csv("Uber_Drives_2016.csv")
original_df
* <b> To keep both the original dataset and the cleaned dataset separately, you can create a new variable to hold the cleaned version while preserving the original dataset.
cleaned_df = original_df.copy()
<h2 style = "background-color: grey; color: white; text-shadow:1.5px 1.5px; border-radius: 20px; text-align: center; font-family:Times New Roman, Times, serif; padding: 20px; display: inline-block" > Quick look inside a dataset</h2>
* <b>To view the first five rows of dataset for exploration or analysis</b>
original_df.head()
* <b>To view the last five rows of dataset for exploration or analysis</b>
original_df.tail()
* <b>To view any few rows of dataset for exploration or analysis</b>
# Display the randomly sampled row
original_df.sample()

# # Alternatively, you can sample 4 rows for a more comprehensive exploration 
original_df.sample(4)
* <b> Checking the number of rows and columns in the original DataFrame using the shape attribute </b>

original_df.shape
* <b>To get a concise summary of the dataset, including data types and missing values.</b>
original_df.info()
* <b>To generate summary statistics like mean, min, max, and quartiles for numerical columns.</b>
original_df.describe()
* <b>To view all columns</b>
original_df.columns
* <b>To identify the number of missing values in each column.</b>
print(f"Sum of missing values:\n\n{original_df.isna().sum()}")
print()
print(f"Sum of all missing(NaN) values:\n{original_df.isna().sum().sum()}")
* <b>Calculating the % of nan values.. If it is above 75% above we usually assume that column is useless.. but it is all about reqirnment</b>
print(f"Required percentage of dataset:\n\n{original_df.isna().sum()*100/len(original_df)}")
* <b>Count the occurrences of unique pairs of 'PURPOSE*' and 'CATEGORY*' in the original DataFrame</b>
* <b>This provides insights into the frequency of combinations of trip purposes and categories.</b>
original_df[["PURPOSE*","CATEGORY*"]].value_counts()
* <b> Similarly for "START*" and "STOP*"</b>
original_df[["START*","STOP*"]].value_counts()
* <b>Count unique values in each column of the DataFrame</b>
original_df.nunique()
* <b>To view duplicate rows.</b>
original_df[original_df.duplicated()]
<h2 style = "background-color: grey; color: white; text-shadow:1.5px 1.5px; border-radius: 20px; text-align: center; font-family:Times New Roman, Times, serif; padding: 20px; display: inline-block" > Data Cleaning</h2>
* <b>Repalce special character from columns</b>
cleaned_df.columns=cleaned_df.columns.str.replace("*","")
* <b> Capatilize each word of first letter in column</b>
cleaned_df.columns=cleaned_df.columns.str.title()
cleaned_df
* <b> We noticed that there are some special character in our dataset
cleaned_df[cleaned_df.Start.str.contains('\?') == True]
# cleaned_df[cleaned_df.Stop.str.contains('\?') == True]
* <b> Removing special characters from our dataset
cleaned_df["Start"] = cleaned_df["Start"].replace({"\?":"a"}, regex=True)
cleaned_df["Stop"] = cleaned_df["Stop"].replace({"\?":"a"}, regex=True)

* <b> Now we noticed that there are NaN values in purpose column so we replace it by "Not Available"
cleaned_df["Purpose"]=cleaned_df["Purpose"].fillna("Not Available")
cleaned_df
* <b> Dropping duplicate row</b>
cleaned_df.drop_duplicates()
* <b>Split the Start_Date column
# Convert 'Start_Date' column to datetime format
cleaned_df['Start_Date'] = pd.to_datetime(cleaned_df['Start_Date'], format='%m/%d/%Y %H:%M')

# Extract and create a new column for the year
cleaned_df["Year"] = cleaned_df["Start_Date"].dt.year

# Extract and create a new column for the month (abbreviated)
cleaned_df["Month"] = cleaned_df["Start_Date"].dt.strftime("%b")

# Extract and create a new column for the day (abbreviated)
cleaned_df["Departure Day"] = cleaned_df["Start_Date"].dt.strftime("%a")

# Extract and create a new column for the departure time in HH:MM format
cleaned_df["Departure Time"] = cleaned_df["Start_Date"].dt.strftime("%H:%M")
* <b>Also split End_Date column
# Convert 'End_Date' column to datetime format
cleaned_df["End_Date"] = pd.to_datetime(cleaned_df["End_Date"], format='%m/%d/%Y %H:%M')

# Extract and create a new column for the day of the week when the event ended
cleaned_df["Arrival Day"] = cleaned_df["End_Date"].dt.strftime("%a")

#Extract and create a new column for the arrival time in HH:MM format
cleaned_df["Arrival Time"] = cleaned_df["End_Date"].dt.strftime("%H:%M")
* <b>Calculating Duration on trip
# Calculate the duration of the trip and create a new column 'Duration' in minutes
cleaned_df['Duration'] = (cleaned_df['End_Date'] - cleaned_df['Start_Date']).dt.total_seconds() / 60
cleaned_df.sample(4)

<h2 style = "background-color: grey; color: white; text-shadow:1.5px 1.5px; border-radius: 20px; text-align: center; font-family:Times New Roman, Times, serif; padding: 20px; display: inline-block" > Analyzing Cleaned Dataset</h2><br>
* <b>After cleaning and editing the whole dataset we again analyze our dataset for better understanding</b>
* <b> Most frequent start and stop locations</b>
most_frequent_start = cleaned_df['Start'].mode()[0]
most_frequent_stop = cleaned_df['Stop'].mode()[0]

print(f'Most frequent start location: {most_frequent_start}')
print(f'Most frequent stop location: {most_frequent_stop}')

* <b>Analyzing the 'START*' column in the original_df DataFrame</b>
# Printing the number of unique starting points and the top 10 most frequent starting points

print(f"Unique starting points: {len(cleaned_df['Start'].unique())}")
cleaned_df['Start'].value_counts(ascending=False)[:10]
* <b>Total rides/month </b>
cleaned_df['Month'].value_counts()
* <b>Filter the DataFrame to include only rows where the starting location ('START*') of a trip is different from the ending location ('STOP*').</b>
original_df[original_df['START*']!=original_df['STOP*']]
* <b> Counting occurrences of "Unknown Location" in the 'Start' column of the cleaned_df DataFrame
cleaned_df[cleaned_df["Start"]=="Unknown Location"]["Start"].value_counts()
* <b> Counting occurrences of "Unknown Location" in the 'Stop' column of the cleaned_df DataFrame</b>
cleaned_df[cleaned_df["Stop"]=="Unknown Location"]["Stop"].value_counts()
* <b>Analyzing Highest distance covered by "Start" & "Stop" Locations</b> 
cleaned_df.groupby(['Start','Stop'])['Miles'].sum().sort_values(ascending=False)[1:11]
Morrisville-Cary & Cary-Durham ,vice versa are the farthest distance ride.
* <b>Function to determine if a trip is a round trip based on the 'Start' and 'Stop' locations <br>
* <b>If 'Start' and 'Stop' locations are the same, the trip is considered a round trip (returning 'YES'), otherwise 'NO'
def is_roundtrip(cleaned_df):
    if cleaned_df['Start'] == cleaned_df['Stop']:
        return 'YES'
    else:
        return 'NO'
    
cleaned_df['Round_Trip'] = cleaned_df.apply(is_roundtrip, axis=1)
cleaned_df["Round_Trip"].value_counts()
cleaned_df.head(2)
<h2 style = "background-color: grey; color: white; text-shadow:1.5px 1.5px; border-radius: 20px; text-align: center; font-family:Times New Roman, Times, serif; padding: 20px; display: inline-block" > Data Visualization</h2>
x = cleaned_df["Category"].value_counts()
sns.barplot(x=x.index, y=x.values,palette=mck_color_alternative,width=0.4)

plt.ylabel("Counts")
plt.xlabel("Category")
plt.xticks(rotation = 45)
There are 2 ride-categories... Business: For work related & Personal: For personal travel
* <b>Creating a count plot of trip purposes using Seaborn</b>
sns.countplot(y=cleaned_df["Purpose"], palette=mck_color_alternative )
plt.title("Trip of purpose")
plt.figure(figsize=(8,4))
cleaned_df.head(3)
* <b> Analyzing how much trips done in a week
sns.countplot(x='Departure Day', data=cleaned_df,palette=mck_color_alternative,width=0.4, order=cleaned_df['Departure Day'].value_counts().index)

plt.title('Number of Trips per Day of the Week')
plt.xlabel('Day of the Week')
plt.ylabel('Number of Trips')
plt.figure(figsize=(8, 4))
plt.show()
sns.countplot(x='Departure Day',data=cleaned_df,palette=mck_color2_alternative ,order=pd.value_counts(cleaned_df['Departure Day']).index,hue='Category')
FRIDAY was the day at which uber rides were mostly used for Business
* <b> You can analyze the number of trips per Month
sns.countplot(x='Month', data=cleaned_df,palette=mck_color_alternative,width=0.4, order=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])

plt.title('Number of Trips per Month')
plt.xlabel('Month')
plt.ylabel('Number of Trips')
plt.figure(figsize=(8, 4))
plt.show()
sns.countplot(x='Month',data=cleaned_df,palette=mck_color2_alternative,order=pd.value_counts(cleaned_df['Month']).index,hue='Category')
 Most number of rides were in month of December (all of them were Business trips)<br>
 Top 5 months having most trips were: December,August,November,February & March.<br>
 Uber Ride was used at Feb,Mar,Jul,Jun & Apr for personal trips.
* <b> Create a DataFrame for start and stop locations
locations_df = pd.concat([cleaned_df['Start'], cleaned_df['Stop']])

# Count the occurrences of each location
locations_counts = locations_df.value_counts()

# Select the top 10 most frequent locations for better visualization
top_locations = locations_counts.head(10)

# Plotting a horizontal bar chart
plt.figure(figsize=(8, 4))
sns.barplot(x=top_locations.values, y=top_locations.index, palette="viridis")

# Adding labels and title
plt.xlabel('Number of Trips')
plt.ylabel('Location')
plt.title('Top 10 Start and Stop Locations')

plt.show()
* <b> The starting points of trips. Where Do People Start Boarding Their Trip From Most?
start_trip=cleaned_df['Start'].value_counts().head(15)
start_trip.plot(kind='barh',figsize=(8,4),color=mck_color_alternative)
* <b>Similarly for "Stop"</b>
start_trip=cleaned_df['Stop'].value_counts().head(15)
start_trip.plot(kind='barh',figsize=(8,4),color=mck_color_alternative)
<b>Cary</b> is the most popular Stop place for this user.<br>
Maybe his home is in Cary (as mostly start & stop are from here)
* <b>If 'Start' and 'Stop' locations are the same, the trip is considered a round trip (returning 'YES'), otherwise 'NO'....(through visulization)
sns.countplot(x='Round_Trip',data=cleaned_df,width=0.4, palette=mck_color2_alternative, order=cleaned_df['Round_Trip'].value_counts().index)
plt.xticks(rotation = 45)
User mostly take single-trip Uber rides.<br>
Around 75% trip is single-trip and 25% are ROund-Trip
# Distribution of trip durations
cleaned_df['Duration'].hist()

plt.title('Distribution of Trip Durations')
plt.figure(figsize=(8, 4))
plt.show()

 * <b>Boxplot to visualize the distribution of trip distances ('Miles') across different ride categories ('Category')
sns.boxplot(x='Category', y='Miles', data=cleaned_df)

plt.title('Miles per Category')
plt.xlabel('Category')
plt.ylabel('Miles')
plt.figure(figsize=(8, 4))
plt.show()
# Conclusion
* The dataset provides valuable insights into Uber ride patterns and user behavior.<br>
* Users predominantly opt for single-trip rides, with most rides for business purposes.<br>
* Specific locations, days, and months were highlighted for increased ride frequency.<br>
* The dataset has been cleaned and prepared for further in-depth analysis.
