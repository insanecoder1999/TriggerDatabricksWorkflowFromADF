# TriggerDatabricksWorkflowFromADF
## Description:

The pipeline which will trigger the databricks workflow from Azure Data Factory

## Architecture:

<img width="1657" height="462" alt="image" src="https://github.com/user-attachments/assets/41b2752b-eef9-4cf7-a2ab-54c276fba836" />

## Process Flow:

1.The web activity(TriggersDatabricksWorkflow) triggers the particular workflow using REST API POST request. 
