# ETLDataPipeline
ETL data pipeline example on a local computer

# 'ETL Data pipeline Project'
Hey! Welcome to this repository where I practice building a data engineering pipeline. You can find here the basics to run down of the project:

# Problem Statement

Suppose a Business Intelligence Department and other users of the company, for instance, marketing, need real-time information about e-commerce sales and trends in a reliable way. For this need, an end-to-end data pipeline will be created, to stream data in almost real-time, from a source outside our environment, where raw data is available, to the company's database and will be updated automatically, each time more data is available in the source. Thus, the BI Department can visualize it through a visualization tool, Streamlit. Thereby, further analysis can be done and see what products are being demanded the most, sales trends over time, and make clusters, among others. 

This data engineering project uses a real-world e-commerce dataset (invoices) from Kaggle. The primary concern of data engineering efforts in this project is to create a way to help decision-makers, to make the analysis as fast as possible with the foundations of big data: velocity, volume, value, variety, and veracity.

Different tools are programmed as part of this project using python, NoSQL, Kafka, FastAPI, and pySpark to build pipelines that ingest data simulating a client, apply some transformations and manipulations, and then load the cleansed data set into a Docker environment where the containers are.

# Dataset in this project
The dataset of choice for this project is from Kaggle: https://www.kaggle.com/datasets/carrie1/ecommerce-data

# Step-by-step methodology:
1. Transform the data set from CSV to .json format through a python script.
2. Build an API with FastAPI and test it. Also, transform the date of the data to a standard date format. For this, we will import the datetime library.
3. Run Docker containers: Install Kafka, Zookeeper, Spark Streaming, and MongoDB.
4. Setup and config Kafka, test it with the producer and consumer to make sure Kafka is running correctly.  
5. Setup Spark Streaming, through Jupyter notebook, configure the parameters so Spark can communicate and read from Kafka port. In addition, setup the same so Spark can process data and write it to MongoDB.
6. Streamlit: Make a pip installation in python to use Streamlit on your Browser.
7. Create a "test data" of the Pipeline with the API from the client (the source), query it on MongoDB, and visualize it on Streamlit. 

All the code needed has been provided for each step.

# Data Pipeline diagram:


# Prerequisites
Software required to run the project, please install:

 a. Docker
 b. Python3
 c. docker-compose
 d. Linux distribution on WSL 2
 e. Postman
 
 All the best!
