# TriggerDatabricksWorkflowFromADF
## Description:

The pipeline which will trigger the databricks workflow from Azure Data Factory

## Architecture:

<img width="1657" height="462" alt="image" src="https://github.com/user-attachments/assets/41b2752b-eef9-4cf7-a2ab-54c276fba836" />

## Process Flow:

1.The web activity(TriggersDatabricksWorkflow) triggers the particular workflow using REST API POST request. 
2.The until activity(WaitUntilJobCompletes) will be checking the status of the job every 60secs. After the successful completion or failure of the job it will go ahead with the next activities.

  2.1 The wait activity(SleepTask) will wait for 60 secs before checking the status of the job of that particular run id.
  2.2 The web activity(CheckJobStatus) will trigger a GET request for that particular job using the corresponding run id.
  2.3 The set variable activity(SetRunStatus) will be used to set the value of the status variable to the output of the GET request.

3. The if-else activity(DecisionOnJobStatus) will mark the pipeline as Succeeded or failed based on the status of the job run.

   3.1 If the status is positive the pipeline will wait for 60 secs and will be Succeeded.
   3.2 If the status is negative this activity will execute the fail activity which will fail the pipeline with error message and error code.
