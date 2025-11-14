# Trigger Databricks Workflow From Azure Data Factory

## **Description**
This document explains the Azure Data Factory (ADF) pipeline used to trigger and monitor a Databricks Workflow execution through REST API calls.

---

## **Architecture**

<img width="1657" height="462" alt="Architecture Diagram" src="https://github.com/user-attachments/assets/41b2752b-eef9-4cf7-a2ab-54c276fba836" />

---

## **Process Flow**

### **1. Trigger Databricks Workflow**
- The **Web Activity (`TriggersDatabricksWorkflow`)** initiates a Databricks Workflow execution by sending a **POST** request using the Databricks REST API.

---

### **2. Monitor Job Run Status**
The **Until Activity (`WaitUntilJobCompletes`)** continuously checks the job run status every 60 seconds and proceeds only when the job reaches a terminal state (success or failure).

#### **2.1 Sleep Before Status Check**
- **Wait Activity (`SleepTask`)** pauses execution for 60 seconds before the next API call.

#### **2.2 Fetch Job Run Status**
- **Web Activity (`CheckJobStatus`)** sends a **GET** request to retrieve the latest run status using the job’s `run_id`.

#### **2.3 Store Status Value**
- **Set Variable Activity (`SetRunStatus`)** updates the pipeline variable with the latest job run status from the GET response.

---

### **3. Decision Based on Run Result**
The **If Condition Activity (`DecisionOnJobStatus`)** evaluates the captured job status.

#### **3.1 If Job Succeeded**
- The pipeline waits for 60 seconds and then marks itself as **Succeeded**.

#### **3.2 If Job Failed**
- The pipeline executes a **Fail Activity** which terminates execution with an error code and error message.

---

