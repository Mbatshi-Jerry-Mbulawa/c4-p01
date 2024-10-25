# c4-p01
# -*- coding: utf-8 -*-
"""
Created on Tue Oct 22 10:41:34 2024

@author: MBATSHI
"""

#from sklearn.impute import KNNImputer
import pandas


movie_data=pandas.read_csv("movie_dataset.csv")

#data preview, first five rows
movie_data.head()

#cleaning column names
movie_data.columns = movie_data.columns.str.replace(' ','_')
print("Columns after cleaning:", movie_data.columns)

#Check for missing values before handling
print("Missing values before handling:\n", movie_data.isnull().sum())



"""

#Testing Grouped Median Imputation
movie_data['Revenue_(Millions)']= movie_data.groupby(['Runtime_(Minutes)','Rating','Votes'])['Revenue_(Millions)'].transform(lambda x: x.fillna(x.median()))

movie_data['Metascore']= movie_data.groupby(['Runtime_(Minutes)','Rating','Votes'])['Metascore'].transform(lambda x: x.fillna(x.median()))

#Broader grouping
movie_data['Revenue_(Millions)']= movie_data.groupby(['Runtime_(Minutes)','Rating'])['Revenue_(Millions)'].transform(lambda x: x.fillna(x.median()))

movie_data['Metascore']= movie_data.groupby(['Runtime_(Minutes)','Rating'])['Metascore'].transform(lambda x: x.fillna(x.median()))
"""



#Even Broader Grouping
movie_data['Revenue_(Millions)']= movie_data.groupby(['Runtime_(Minutes)'])['Revenue_(Millions)'].transform(lambda x: x.fillna(x.median()))

movie_data['Metascore']= movie_data.groupby(['Runtime_(Minutes)'])['Metascore'].transform(lambda x: x.fillna(x.median()))

movie_data['Revenue_(Millions)']= movie_data.groupby(['Rating'])['Revenue_(Millions)'].transform(lambda x: x.fillna(x.median()))

movie_data['Metascore']= movie_data.groupby(['Rating'])['Metascore'].transform(lambda x: x.fillna(x.median()))

movie_data['Revenue_(Millions)']= movie_data.groupby(['Votes'])['Revenue_(Millions)'].transform(lambda x: x.fillna(x.median()))

movie_data['Metascore']= movie_data.groupby(['Votes'])['Metascore'].transform(lambda x: x.fillna(x.median()))



#Check for missing values after imputation
print("Missing values after handling:\n", movie_data.isnull().sum())



"""
#this line will handle the missing values in the  Revenue and Metascore columns
movie_data['Revenue_(Millions)'].fillna(movie_data['Revenue_(Millions)'].median(), inplace=True)

movie_data['Metascore'].fillna(movie_data['Metascore'].median(), inplace=True)
"""



remaining_gaps = movie_data[movie_data['Revenue_(Millions)'].isnull()]
print("\nRemaining gaps in Revenue_(Millions):")
print(remaining_gaps[[ 'Runtime_(Minutes)','Rating', 'Votes','Revenue_(Millions)','Metascore']])

remaining_gaps = movie_data[movie_data['Metascore'].isnull()]
print("\nRemaining gaps in Metascore:")
print(remaining_gaps[[ 'Runtime_(Minutes)','Rating', 'Votes','Revenue_(Millions)','Metascore']])


"""
#numeric_columns = ['Year', 'Runtime_(Minutes)','Rating', 'Votes','Revenue_(Millions)','Metascore']
#imputer = KNNImputer(n_neighbors=5)
"""

#movie_data[numeric_columns] = imputer.fit_transform(movie_data[numeric_columns])
#print(movie_data.head())
  


#find the row with the highest rating and print tbe corresponding movie title
highest_rated_movie = movie_data.loc[movie_data['Rating'].idxmax(), 'Title']
#.idxmax() find the index with the maximum value in the rating column

print("\nthe highest rated movie is:\n", highest_rated_movie)


#This line calculates the average revenue of all movies
average_revenue = movie_data['Revenue_(Millions)'].mean()

print("\nThe average revenue of all movies is:\n", average_revenue, "Million")

# Filter movies released between 2015 and 2017 and calculate the average revenue
avg_revenue_2015_2017 = movie_data[(movie_data['Year'] >= 2015) & (movie_data['Year'] <= 2017)]['Revenue_(Millions)'].mean()

print("\nThe average revenue of movies from 2015 to 2017 is:\n", avg_revenue_2015_2017, "Million")

# Count the number of movies released in 2016
movies_2016_count = movie_data[movie_data['Year'] == 2016].shape[0]

print("\nNumber of movies released in 2016:\n", movies_2016_count)

#Count the number of movies directed by Christopher Nolan
cnolan_movie_count = movie_data[movie_data['Director'] == 'Christopher Nolan'].shape[0]

print("\nNumber of movies directed by Christopher Nolan:\n", cnolan_movie_count)

#Count the number of movies with a rating of at least 8.0
highest_movie_rating_count = movie_data[movie_data['Rating'] >= 8.0].shape[0]

print("\nNumber of movies with a rating of at least 8.0 :\n", highest_movie_rating_count)

# Calculate the median rating of movies directed by Christopher Nolan
cnolan_median_rating = movie_data[movie_data['Director'] == 'Christopher Nolan']['Rating'].median()

print("\nMedian rating of movies directed by Christopher Nolan:\n", cnolan_median_rating)

#find the year with the highest average rating
year_highest_avg_rating = movie_data.groupby('Year')['Rating'].mean().idxmax()

print("\nThe year with the highest average rating is:\n", year_highest_avg_rating)

# Calculate the percentage increase in the number of movies between 2006 and 2016
movies_2006 = movie_data[movie_data['Year'] == 2006].shape[0]
movies_2016 = movie_data[movie_data['Year'] == 2016].shape[0]
percentage_increase = ((movies_2016 - movies_2006) / movies_2006) * 100
#difference = movies_2016 - movies_2006
#print(difference)
print("\nThe percentage increase in the number of movies between 2006 and 2016 is:\n", percentage_increase, "%")


#Finding the most frequent actor
from collections import Counter

all_actors = movie_data['Actors'].str.split(',').explode()

#Filter for the actors mentioned in the options
specified_actors = ['Bradley Cooper', 'Matthew McConaughey', 'Chris Pratt', 'Mark Wahlberg']

filtered_actors = all_actors[all_actors.isin(specified_actors)]

#find the most commmon actor in the specified list
most_common_actor = Counter(filtered_actors).most_common(1)[0][0]

print("\nThe most common actor is:\n", most_common_actor)

#Finding unique genres in the dataset
unique_genres = set(movie_data['Genre'].str.split().explode().unique())

print("\nNumber of unique genres:\n", len(unique_genres))



