# Azure-Bank-Capstone-Project

# DAY-01

The goal of Day 1 was to set up all the foundational Azure services required for the pipeline and verify that the cloud environment is ready for development and integration.
No transformation or fraud logic is executed today – only environment setup.

Tasks Completed on Day 1 :

* Created Azure Storage Account
   - Purpose: Store raw transaction CSV files uploaded by banking systems
   - Containers created
* Created Azure Event Grid Subscription
   - Purpose: Automatically detect new file uploads in Blob Storage
   - Configured Event Grid to Function App
* Created Azure Function App (Python)
   - Runtime: Python 3.10
   - Trigger: Event Grid Trigger
   - Purpose:
        - Parse CSV from Blob Storage
        - Perform cleaning & duplicate removal
        - Detect fraud transactions
        - Store processed data into Cosmos DB
   - Function is tested and linked to Application Insights successfully.
*  Enabled Application Insights
   - Purpose: Centralized logging & monitoring
   - Logs show:
        - Connecting to Application Insights...
*  Created Azure Cosmos DB
   - Database Name: BankDB
   - Containers:
        - UPIEvents
        - ATMTransactions
        - FraudAlerts 
    - Database ready for ingestion testing on Day 2
 *  Created Azure Service Bus Namespace
    - Purpose:
       - Alert notifications

   Day-02 Activities :

      *  Connect Event Grid with Function App
      *  Implement Python code for :
          - Reading CSV from Blob
          - Cleaning & validating records
          - Removing duplicate transaction_id
          - Normalizing timestamps
          - Applying fraud detection rule
      *  Write clean data into Cosmos DB
      *  Write fraud data into FraudAlerts
      *  Test end-to-end pipeline

   
# DAY-02 

Azure ETL Pipeline Progress Report

* Today’s work focused on wiring the full event-driven ETL pipeline on Azure :
    - Azure Function App deployment (EventGrid Trigger)
    - Storage Account → Event Grid → Function App workflow
    - CosmosDB containers for storing processed datasets
    - Full ETL logic to clean, validate,classify, and insert data
    - Application Insights for monitoring & debugging
      
 Tasks Completed on Day 2 :

 * Azure Infrastructure Setup
     - Function App created (Python stack, v4)
     Storage Account created
     - Raw data container configured
     - Event Grid system topic configured
     - Event Subscription connected to Function App
     - CosmosDB database BhanuDB created with 3 containers:
          - UPIEvents
          - ATMTransactions
          - FraudAlerts
     - Service Bus created:
          - Topic: fraud-alerts
          - Shared access policy created
      
   * Function App Deployment
       - Python project deployed using VS Code
       - Remote build successfully completed
       - Deployment verified inside Function App → Code → Files

   * Environment Variable Configuration
       - Configured inside Azure Function App → Configuration:
            * COSMOS_URL
            * COSMOS_KEY
            * COSMOS_DB_NAME
            * COSMOS_ATM_CONTAINER
            * COSMOS_UPI_CONTAINER
            * COSMOS_FRAUD_ALERTS_CONTAINER
            * SERVICEBUS_CONNECTION
            * SERVICEBUS_TOPIC
            * AZUREWEBJOBSTORAGE
            * APPINSIGHTS_INSTRUMENTATIONKEY
            * APPLICATIONINSIGHTS_CONNECTION_STRING
            * FUNCTIONS_WORKER_RUNTIME : Python
    * Live Event Processing Test
        - Uploaded CSV file into Blob Storage
        - Event Grid triggered successfully
        - Function logs show:
          * Event received
          * Blob URL parsed
          * CSV processed
          * CosmosDB insertions

DAY 03 Activities :

      * Create Azure SQL Data Warehouse
      * Create DimCustomer Table
      * Create DimAccount Table
      * Create FactTransactions Table
      * Create Spark ETL Notebook (Bronze–Silver–Gold)
      * Bronze Layer – Load Raw Data from Cosmos
      * Silver Layer – Clean & Transform Data
      * Gold Layer – Prepare Analytics-Ready Fact Table
      * Create Notebook to Load SQL DW (ETL to DW)
      * Insert into DimCustomer
      * Insert into DimAccount
      * Insert into FactTransactions
      * Run SQL Queries to Validate Data
