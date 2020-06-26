# Witness Finder 

> ***Before a police incident made its way through the court system, it's very helpful to find potential witnesses of the case for future reference***
***
## Motivation

It is important for city attorneys to find the witness or evidence of the case as soon as possible. Traditionally, they heavily rely on people to do the investigation. For instance, they survey the parties involved in the case, search for potential witnesses at the place where the case happened, etc. The time and money spent in this process is significant. However, in the era of social media, an increasing number of reported crimes were captured by witnesses' smart phones. If we can trace these social media posts, associate them with relevant individual cases, and store the results into a database, we can provide a web app to city attorneys to find potential wintesses and evidence for their cases.

---
## Data Source

  - [Police Incident Blotter (30 Day)](https://data.wprdc.org/dataset/police-incident-blotter)
  - [Police Incident Blotter (Archive)](https://data.wprdc.org/dataset/uniform-crime-reporting-data)
  - [Twitter streaming data API](https://developer.twitter.com/en/docs/tutorials/consuming-streaming-data)
---
## System

![system_png](./system.png)

***Batch Job***: Historical twitter data and Police Incident Blotter (Archive) data are ingested from S3 bucket into Spark. Spark does the data linkage based on geospatial and timestamp information from the police incident and the tweet data. Then save the results into the mongoDB. 

***Streaming Job***: Real-time tweet data are streamed by Kafka into Spark Streaming. Spark streaming computes whether the incident and the tweet is associated with each other or not. Then save the result into the mongoDB.

> Since the Police Incident Blotter (30 days) is updated daily at midnight. This process is automated with the help of the Airflow scheduler.
>

***Web App***: Provide the API to query the mongoDB given the unique ID of the case. It will show you relevant tweets with text, image, videos. 

---
## Clusters

- (4 nodes) Spark Cluster - Batch
- (3 nodes) Spark Cluster - Stream
- (3 nodes) Kafka Cluster
- MongoDB node
- Flask Node

12 m4.large AWS EC2 instances are needed. 
