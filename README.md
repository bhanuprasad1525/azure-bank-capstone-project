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
  * Write clean data into Cosmos DB
  * Write fraud data into FraudAlerts
  * Test end-to-end pipeline
