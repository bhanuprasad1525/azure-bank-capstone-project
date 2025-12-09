# Azure-Bank-Capstone-Project

# DAY-01

The goal of Day 1 was to set up all the foundational Azure services required for the pipeline and verify that the cloud environment is ready for development and integration.
No transformation or fraud logic is executed today – only environment setup.

Tasks Completed on Day 1 :

* Created Azure Storage Account
   - Purpose: Store raw transaction CSV files uploaded by banking systems
   - Containers created
      * Bank-Data
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
     - CosmosDB database BankDB created with 3 containers:
          - UPIEvents
          - ATMTransactions
          - FraudAlerts
     - Service Bus created:
          - Queue: fraud-alerts
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
            * SERVICEBUS_QUEUE
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


# DAY-03

Data Warehouse + Spark ETL
     - Extend the pipeline from CosmosDB (operational store) to the analytical layer using Azure SQL + Spark ETL.
     
Tasks Completed on Day 3 :

* Azure SQL Data Warehouse Setup
     - Created SQL database for analytics & reporting
     - Configured access for Spark (JDBC + firewall rules)
     - Designed & created the following schema objects :
       * DimCustomer Table
         - Stores customer master records
         - Fields include :
           - CustomerID,FirstName,LastName,DOB,Gender,Phone,Email,Address,city,State,Pincode,KYCTier,CreatedDate,UpdatedDate
       * DimAccount Table
         - Holds account metadata linked to customers.
         - Fields include :
             -  AccountNumber,CustomerID,AccountType,BranchCode,OpenedDate,Status,CurrentBalance,CReatedDate,UpdatedDate
       * FactTransactions Table
         - Unified fact table for ATM + UPI transactions.
         - Fields include :
            - TransactionID, CustomerID, AccountNumber, TransactionAmount, TransactionType, TransactionStatus, TransactionTimestamp, LocationCity, LocationState, IsHighValue, TransactionHour, DayOfWeek,                        CustomerTier, CurrentBalance, BranchCode, CreatedDate

* Databricks Workspace Setup
  - Created Spark workspace
  - Configured access to Storage Account
  - Linked to Cosmos and SQL credentials

* ETL Notebooks Created
     - Bronze_Load :
         * Reads raw ATM & UPI data from CosmosDB
         * Stores CSV to Bronze Layer
     - Silver_Transform :
         * Cleans & standardizes data
         * Fixes schema mismatches, timestamps, null values
         * Removes duplicates
         * Writes cleaned data to Silver Layer
      - Gold_FactTable_Creation :
         * Merges ATM + UPI
         * Creates analytics-ready Fact Transactions
      - Load_To_SQLDW :
         * Loads DimCustomer, DimAccount, FactTransactions
         * Performed validation queries

* ETL Flow Implemented
  - Bronze Layer – Raw data from Cosmos
     * No transformations
     * Stored as raw CSV
  - Silver Layer – Cleaned data
     * Duplicate removal
     * Type conversion
  - Gold Layer – Analytics-ready data
     * Fact table creation
     * Prepared for BI consumption

* SQL Validation Queries Run
   - Customer transaction counts
   - Branch-wise revenue

DAY 04 Activities :

    * Implement real-time fraud detection rules in Azure Function
    * Insert detected fraud events into Cosmos DB (FraudAlerts container)
    * Publish fraud alerts to Azure Service Bus Queue (fraud-alerts)
    * Enhance Function App logging for fraud detection workflow
    * Enable Application Insights: traces, metrics & failure logs
    * Perform end-to-end real-time testing of fraud detection pipeline


# DAY - 04

Real-Time Fraud Detection,Monitoring
  - Enhance pipeline with real-time decisioning, security, and monitoring.

Tasks Completed on Day 4 :

* Fraud Detection Logic Added
   - Implemented multiple rule-based fraud checks :
       * High-Value Transaction Rule
          - Triggers if amount > ₹50,000
       * Rapid ATM Withdrawal Rule
          - 3+ ATM transactions from same customer within 10 minutes
* Fraud Alerts Workflow
    - Suspicious events pushed to Cosmos DB → FraudAlerts
    - In parallel, messages published to Service Bus Queue: fraud-alerts
    - Alert Message Structure :
       * CustomerID
       * TransactionID
       * Amount
       * Timestamp
* Application Insights Monitoring
   - Enabled to track operational metrics :
      * Logging & Monitoring Added
         - Function execution traces
         - Blob trigger logs
         - Cosmos insert status
         - Fraud detection logs
         - Exception + failure tracking

* Security Improvements
  - Restricted public access for Storage Account
  - CosmosDB protected via Private Endpoint
  - Limited SQL access to Spark workspace only

* End-to-End Validation Performed
  Tested full real-time path:
    * Upload transaction file
    * Event Grid → Function triggered
    * ETL processing
    * Fraud rules evaluated
    * Alerts written to Cosmos DB
    * Alert message sent to Service Bus


DAY 05 Activities :

               
      * Connect Power BI to Azure SQL Data Warehouse
      * Create Customer 360 dashboard (Customer → Accounts → Transactions)
      * Build ATM vs UPI transaction analysis dashboard
      * Build Fraud Analytics dashboard using FraudAlerts
      * Validate FactTransactions data in SQL DW
      * Refresh and verify Power BI visuals
      * Perform complete end-to-end demo (Upload → Function → Cosmos → ETL → SQL → Power BI)
      * Finalize project documentation and screenshots
      * Prepare architecture diagram and data flow diagram
      * Review monitoring logs and finalize health checks









