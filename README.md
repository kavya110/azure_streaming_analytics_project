
# Real-Time Financial Streaming Analytics 

This project demonstrates a real-time streaming analytics pipeline built on Microsoft Azure.  Live financial-like events are generated, ingested, processed in real time, and stored in a data lake for analytics.  The solution uses Azure services to simulate a production-grade streaming architecture.

# Azure Services Used

1) Azure Event Hub => To generate continuous streaming events
2) Azure Logic Apps => High throughput event ingestion layer
3) Azure Stream Analytics => For real time stream processing and aggregation
4) Azure Data Lake Storage Gen2 => Stores processed streaming outputs.

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










