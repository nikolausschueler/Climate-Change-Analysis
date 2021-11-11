<div>
  <img src="/Images/img_1.jpg" ></img>
</div>

# Welcome to Climate Change Analysis Project
<div>
<img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" alt="github"/>
<img src="https://img.shields.io/badge/Made%20with-Jupyter-orange?style=for-the-badge&logo=Jupyter" alt="jupyter" />
<img src="http://ForTheBadge.com/images/badges/built-with-science.svg" />
</div>
<em><p>Climate change is the long-term alteration of temperature and typical weather patterns in a place. Climate change could refer to a particular location or the planet as a whole. Climate change may cause weather patterns to be less predictable.</p></em>

## Project Info
<p>In this project, We will be developing Climate Prediction Model. We will be using the Temperature Variation Dataset of Earth to analyse, observe and develop a model to predict the climate change across the globe.
</p>

## Tools Used

<div>
  <img src="https://img.shields.io/badge/Numpy-777BB4?style=for-the-badge&logo=numpy&logoColor=white" /> 
  <img src="https://img.shields.io/badge/Pandas-2C2D72?style=for-the-badge&logo=pandas&logoColor=white" /> 
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="python"/>
  <img src="https://img.shields.io/badge/conda-342B029.svg?&style=for-the-badge&logo=anaconda&logoColor=white"  />
  <img src="https://img.shields.io/badge/Made%20with-Jupyter-orange?style=for-the-badge&logo=Jupyter" alt="jupyter" />
  <img src="https://img.shields.io/badge/Plotly-239120?style=for-the-badge&logo=plotly&logoColor=white" />

</div>

## Dataset Information
<p> The Dataset Chosen for this Project is from <a href="http://berkeleyearth.org/archive/about/"><b>Berkeley Earth</b></a>.
<br>Berkeley Earth supplies comprehensive open-source world air pollution data and highly user-accessible global temperature data that is timely, impartial, and verified.</p>
<p>Given this complexity, there are a range of organizations that collate climate trends data. The three most cited land and ocean temperature data sets are NOAA’s MLOST, NASA’s GISTEMP and the UK’s HadCrut.

We have repackaged the data from a newer compilation put together by the Berkeley Earth, which is affiliated with Lawrence Berkeley National Laboratory. The Berkeley Earth Surface Temperature Study combines 1.6 billion temperature reports from 16 pre-existing archives. It is nicely packaged and allows for slicing into interesting subsets (for example by country). They publish the source data and the code for the transformations they applied. They also use methods that allow weather observations from shorter time series to be included, meaning fewer observations need to be thrown away.
  
The Dataset contain several attributes/fields,such as:
  - Date: starts in 1750 for average land temperature and 1850 for max and min land temperatures and global ocean and land temperatures
  - LandAverageTemperature: global average land temperature in celsius
  - LandAverageTemperatureUncertainty: the 95% confidence interval around the average
  - LandMaxTemperature: global average maximum land temperature in celsius
  - LandMaxTemperatureUncertainty: the 95% confidence interval around the maximum land temperature
  - LandMinTemperature: global average minimum land temperature in celsius
  - LandMinTemperatureUncertainty: the 95% confidence interval around the minimum land temperature
  - LandAndOceanAverageTemperature: global average land and ocean temperature in celsius
  - LandAndOceanAverageTemperatureUncertainty: the 95% confidence interval around the global average land and ocean temperature
  
  
The raw data comes from the Berkeley Earth data page.</p>
<p> Dataset Link: <a href="https://www.kaggle.com/berkeleyearth/climate-change-earth-surface-temperature-data"> Berkeley Climate Change Dataset</a></p> 


The Dataset consist of multiple type of Data:

* Global Land and Ocean-and-Land Temperatures (GlobalTemperatures.csv)
* Global Average Land Temperature by Country (GlobalLandTemperaturesByCountry.csv)
* Global Average Land Temperature by State (GlobalLandTemperaturesByState.csv)
* Global Land Temperatures By Major City (GlobalLandTemperaturesByMajorCity.csv)
* Global Land Temperatures By City (GlobalLandTemperaturesByCity.csv)
  
## Installation of Tools and Libraries 
### Make Sure you have installed Python 3.x + version for this project

### Using PIP 

You would be needing multiple libraries such as Math, Numpy, Pandas and Seabon for basic operations and many pre-processing and ML Model libraries.

```sh
pip install [Library_Name]
```

### Using Conda

If you are using Anaconda, then I would recommend you to create an seperate Enviornment for this project. (Not mandatory! Just a Good Practice)
You can use the command mentioned below to install libraries in your enviornment. (Make sure that the appropriate Environment is activated to avoid Code Errors and redundancy!)

```sh
conda install [Library_Name]
```

## Getting Started with the Project

### Importing the required Libraries 

```python
import pandas as pd
import numpy as np
import os
import matplotlib.pyplot as plt
import seaborn as sns
import sys
```

### Reading the CSV File from the local machine

The cwd and files command is to see the current directory details. 

```python
# Reading the dataset 
# Get the current working directory (cwd)
cwd = os.getcwd()   
# Get all the files in that directory
files = os.listdir(cwd) 
print("Files in %r: %s" % (cwd, files))

#Reading the GLOBAL TEMPERATURE BY CITY CSV 
data = pd.read_csv(f"Datasets\GlobalLandTemperaturesByCity.csv",delimiter=",")
data
```

### Basic Exploration on the Dataset 

This section of code helps us to comprehend the basic understanding of the dataset. We retrieved the dataset in a form of Pandas Data Frame.

```python
# Gives the Basic Structure of the Data Frame such as Data type and Col names.
data.info()

# Show the basic Statistical Values of the data such as Mean, Count, StD, Min and Quartiles
data.describe()

# Shape of the dataset
data.shape

# Names of the Column 
data.columns

# First 5 Records in the dataset
data.head()

# Last 5 Records in the dataset
data.tail()

# Here we can see the unique values exist in each column
data.nunique()

# Shows the number of Null values in each column 
data.isnull().sum()
```

<u>Note: There exist few Null values.</u>
<p>
Many real world datasets contain missing values, often encoded as blanks, NaNs or other placeholders.
  
  * A basic strategy to use incomplete datasets is to discard entire rows and/or columns
    containing missing values or NaN.
  * Another Strategy is to use Imputer Class, it replaces the empty/null values with the       mean, median or most frequent
  
  
Since, the dataset is quite large, so we will be dropping the null records
</p>

```python
# Dropping all null values
data = data.dropna(how='any' ,axis=0)
data.shape
```

### Manipulation in the Data Frame

Since, we are going to perform Time Series Analysis on the Dataset. So, now we will be changing the structure according to our ease and analysis techniques.

Transforming the Dt column to Date column ( Object datatype to Date Type)

```python
# Converting Dt into Date
data['Date'] = pd.to_datetime(data['Date'])
# Setting the Index 
data.set_index('Date',inplace = True)
data.index
```

Now, we will make a new Column namely "Year" 

```python
# Now we use year as index
data['Year']= data.index.year
data.head()
```


