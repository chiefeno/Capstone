# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Capstone Project

---


## Table of Content

---

1. Problem Statement
2. Datasets used
3. Cleaning
4. Exploratory Data Analysis
5. Preprocessing
6. Building the recommender

---

## Problem Statement

As an executive in a record company, my task is to improve the recently launched streaming service and ameliorate customers experience by building a tool that will recommend similar songs based on previous songs played or researched songs. By improving customers experience, The company will be able to retainn more customers after the trial period.

---

## Datasets used

The datasets used in this project are from data.world and are accessible at this adress: https://data.world/kcmillersean/billboard-hot-100-1958-2017 

This data has been gathered using the Spotify Web API and a complete Data dictionary can be found here:
https://developer.spotify.com/documentation/web-api/reference/#endpoint-get-audio-features

| Column | Data type | Description |
| --- | --- | --- |
| performer | object | Name of the artist|
| song | object | title of the song|
| spotify_genre | object | List of song genres |
| spotify_track_album | object | Name of the album |
| spotify_track_explicit | object | Contains information on whether or not the song has. explicit lyrics |
| spotify_track_duration_ms | float64 | Duration of the song |
| spotify_track_popularity | float64 | Popularity score of a song |
| danceability | float64 | Danceability score of the song |
| energy | float64 | Energy score of the song |
| key | float64 | Key of the song |
| loudness | float64 | Loudness of the song |
| speechiness | float64 | Speechiness score of the song |
| acousticness | float64 | Acousticness score of the song |
| instrumentalness | float64 | instrumentalness score of the song |
| liveness | float64 | liveness score of the song |
| tempo | float64 | Tempo of the song |
| time_signature | float64 | Time signature of the song |

---

**All the imports and libraries:**

`code(import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from re import search
from sklearn.preprocessing import StandardScaler
from scipy import sparse
from sklearn.metrics.pairwise import pairwise_distances, cosine_distances, cosine_similarity)`


## Cleaning

The main steps I took during the cleaning proces were the removal of null values 
which represented 15% of my data with 4812 values. Then I droped some unused columns like
song_id or track_id. There were also duplicates values that I removed. Some datatypes and format needed 
change like the track_duration_in_ms that I converted in minutes or the spotify_genre column that was initially
a string and got converted to a list of strings. I had to clean up the genres column by creating a preset list 
of genres and a function to extract substrings that matches those genres from the initial genre columns. 


## Exploratory Data Analysis

During the EDA process I looked at various distributions and statistics. I first looked at the song
duration distribution and found that the average song in the dataframe was around 3.6 minutes. I identified some
outliers with songs longer than 20 minutes.
The average danceability score is approximatively 0.6 and the least danceable song has a score of 0 which is surprising. The most danceable song has a score of 0.98.
The average energy score is around 0.62 and the the songs with the least energy have a score of 0.000581 and the most energetic ones have a score of 0.99.
I also looked at the key of the songs in the distribution and found that C, G and D are the most popular keys and D# is the least popular key in this dataframe.

## Preprocessing

The preprocessing steps were the creation of dummies/one-hot encoding of certain columns, mainly the genres column.
I also combined the song title and artist into a single column to use as an index. I also standard scaled all the numerical values except dummies to normmalize the values accross the dataframe. I then joined the dummies columns with the scaled features and the song title + artist column into a recommender dataframe.

## Building the Recommender 

First I had to resize my recommmender dataframe into a sparse dataframe and then computed pairwise distance and cosine similarity 
which is the metric used for the recommender. Once I computed the cosine similarities, I had a matrix of (22112, 22112) and 
set the song + artist as index. 



