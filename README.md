
# Real-Time Financial Streaming Analytics 

This project demonstrates a real-time streaming analytics pipeline built on Microsoft Azure.  Live financial-like events are generated, ingested, processed in real time, and stored in a data lake for analytics.  The solution uses Azure services to simulate a production-grade streaming architecture.

# Azure Services Used

1) Azure Event Hub => To generate continuous streaming events
2) Azure Logic Apps => High throughput event ingestion layer
3) Azure Stream Analytics => For real time stream processing and aggregation
4) Azure Data Lake Storage Gen2 => Stores processed streaming outputs.


# Architecture

### This project uses the following Architecture.

<img width="763" height="394" alt="image" src="https://github.com/user-attachments/assets/7da79be4-0044-4ad5-b5f7-cf14c4935f27" />



# Process

1) Create the Azure Event Hub Service for Input Streaming Data

<img width="1903" height="814" alt="image" src="https://github.com/user-attachments/assets/deae563b-3342-46e2-a3f9-392036a87fa6" />



Resource Group for this project => streaming-project-grp
Name of the Event Hub Service => marketevents



2) Create a new event Hub for generating the input data.

<img width="959" height="407" alt="image" src="https://github.com/user-attachments/assets/45018046-5c5c-4fff-9741-9ac316087d4f" />



Create another "Event Hub" inside the event hub service called "market-events"

<img width="1919" height="878" alt="image" src="https://github.com/user-attachments/assets/599b027f-b19e-4a49-8e4e-193c7822d195" />



The created "Event Hubs" will be visible in the Entities -> Event Hubs section

<img width="1919" height="818" alt="image" src="https://github.com/user-attachments/assets/46a9a9ca-589c-4558-8087-8a0f4501e83f" />





3) Create an Azure Logic Apps Service to ingest the live data.

<img width="1919" height="817" alt="image" src="https://github.com/user-attachments/assets/4e724113-c121-48c9-9b02-94b5a3c1486a" />



<img width="1919" height="826" alt="image" src="https://github.com/user-attachments/assets/8eb6df13-51dc-48dc-a372-9c710e110d51" />




4) Create a flow to ingest live data for every 5 seconds.

<img width="1919" height="818" alt="image" src="https://github.com/user-attachments/assets/6f6e1e2d-a5e2-44a8-83cf-6776131e0bd4" />



<img width="1909" height="816" alt="image" src="https://github.com/user-attachments/assets/d81a68cc-bcfd-499e-82c8-8d1d0e78958e" />



Paste this sample JSON Code in the 'content' section inside the 'Advanced Parameters'

{
  "symbol": "BTCUSDT",
  "price": 43000,
  "volume": 1.5,
  "eventTime": "@{utcNow()}"
}


Run this Logic Apps Flow


<img width="1919" height="862" alt="image" src="https://github.com/user-attachments/assets/d7b748c7-1507-457b-be46-8fcee36bd349" />


You can see the Run History here

<img width="1919" height="887" alt="image" src="https://github.com/user-attachments/assets/2a93f1ca-8dc9-4995-9953-8f0e866a70d7" />



5) Create a "Stream Analytics" Service to process this live input data.

<img width="1919" height="876" alt="image" src="https://github.com/user-attachments/assets/e40c4999-97ce-4f69-aa97-1e253560e85e" />


<img width="1919" height="866" alt="image" src="https://github.com/user-attachments/assets/22902ddf-fce9-42f7-a0a8-f1506ab4b2f6" />


6) Add an 'Input Stream' inside the created 'Stream Analytics'. 

<img width="1919" height="870" alt="image" src="https://github.com/user-attachments/assets/ad63a24c-867b-4456-87d8-a934c4365f09" />


<img width="1919" height="882" alt="image" src="https://github.com/user-attachments/assets/a521e2db-7864-491b-862d-5bd8af60de23" />


Give all the 'Event Hub' credentials to this input stream

<img width="1919" height="867" alt="image" src="https://github.com/user-attachments/assets/eda8cd6b-4c9a-405f-89e7-aae5629c2237" />


<img width="1919" height="871" alt="image" src="https://github.com/user-attachments/assets/015e2a37-13b2-458f-b4bb-3afc8babfe6b" />


7) Create an ADLS storage for output data

<img width="1913" height="873" alt="image" src="https://github.com/user-attachments/assets/3b3c1531-5de8-4ce3-b7ff-176a490f8f62" />


<img width="1919" height="885" alt="image" src="https://github.com/user-attachments/assets/e2100acf-baa7-4e49-b43e-9fee048a453b" />


8) Add the output file in the created 'Stream Analytics' service

<img width="1917" height="887" alt="image" src="https://github.com/user-attachments/assets/c55b62ea-dbc8-4697-a1c9-164ff31fc423" />


Provide the details of the ADLS Gen2 storage that is already created

<img width="1919" height="875" alt="image" src="https://github.com/user-attachments/assets/fb58243b-1c01-47cc-84f1-f915dcd681de" />



<img width="1919" height="875" alt="image" src="https://github.com/user-attachments/assets/ff675d4a-7e4c-471e-949b-782e1d0ec224" />



9) In the 'Query' tab, connect to the input and output data sources and run the sample query for test.

<img width="1916" height="868" alt="image" src="https://github.com/user-attachments/assets/84ce7acd-34ce-4d1d-9351-c9bed9f25f84" />



Here, I'm going to retrieve the average of price and the number of events per minute using this code

SELECT
    symbol,
    AVG(price) AS avg_price,
    COUNT(*) AS trade_count,
    System.Timestamp() AS window_end
INTO
    outputstreamingdata
FROM
    inputlivedata
TIMESTAMP BY eventTime
GROUP BY
    symbol,
    TumblingWindow(minute, 1)


<img width="1917" height="872" alt="image" src="https://github.com/user-attachments/assets/c83795ae-97af-4ea5-97f2-34b6cb2d8990" />


This is the result upon testing the query


<img width="1919" height="864" alt="image" src="https://github.com/user-attachments/assets/7b85da9c-b3e5-4bbf-bd7f-e5137895adf0" />





Start the job to process this live data to output ADLS

<img width="1919" height="877" alt="image" src="https://github.com/user-attachments/assets/b08f9705-4a35-4996-b7fd-8aeda5711d8f" />



The job is up and running


<img width="1919" height="881" alt="image" src="https://github.com/user-attachments/assets/f7e547de-543d-4782-879f-e7ccdefd77a5" />




10) Monitor the data flow in the output file


<img width="1919" height="856" alt="image" src="https://github.com/user-attachments/assets/06f4d01d-056f-4bee-ae26-98ae76f490e2" />



<img width="1919" height="824" alt="image" src="https://github.com/user-attachments/assets/4bb18ae9-7c2c-4cbc-9440-d303a08aad05" />



<img width="1919" height="868" alt="image" src="https://github.com/user-attachments/assets/ddd32bae-2fbf-4198-a952-4a09cd98d6c5" />


We can find changes in the number of logs in the above 2 screenshots.




# Key Learnings :

1) Designed a real-time streaming pipeline using Azure services (Azure Event Hubs, Stream Analytics)
2) Using Stream Analytics SQL for aggregating the input live data.
3) Processing the results into ADLS Gen2
   



































