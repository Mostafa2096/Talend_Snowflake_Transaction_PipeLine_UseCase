# Talend ETL Pipeline for Transaction Data to Snowflake DWH

This repository contains a Talend ETL pipeline designed to process transaction data from CSV files and load it into a Snowflake Data Warehouse (DWH). The pipeline consists of five jobs that handle the extraction, transformation, and loading (ETL) of data into the respective tables in Snowflake.

## Table of Contents
1. [Overview](#overview)
2. [Jobs Description](#jobs-description)
   - [1. DimDateJob](#1-dimdatejob)
   - [2. DimBranchJob](#2-dimbranchjob)
   - [3. DimSalesAgentJob](#3-dimsalesagentjob)
   - [4. TransactionJob](#4-transactionjob)
   - [5. MasterJob](#5-masterjob)
3. [Folder Structure](#folder-structure)
4. [How to Run](#how-to-run)
5. [Screenshots](#screenshots)

---

## Overview

The pipeline processes daily CSV files (`transactions`, `salesagents`, and `branches`) from a specified folder. The data is transformed and loaded into the following Snowflake tables:
- `DimDate`
- `DimBranch`
- `DimSalesAgent`
- `DimCustomer`
- `DimProduct`
- `Transactions`

After processing, the files are moved to a `done` folder. If no files are found, a warning message is displayed.

---

## Jobs Description

### 1. DimDateJob
This job generates serial numbers starting from 1 to the number of days between a specified start date and end date. It checks if the records already exist in the `DimDate` table in Snowflake and inserts only new records.

**Key Steps:**
- Generate serial numbers for dates.
- Lookup existing records in `DimDate`.
- Insert new records into `DimDate`.

**Screenshot:**
![DimDateJob](https://github.com/user-attachments/assets/733af5d5-63ce-4e4e-bd4d-aa04ad9ddf69)


---

### 2. DimBranchJob
This job lists files in the folder with the regex `dimbranch*`. If a file is found, it converts the string date to a date data type, performs a lookup on the `DimBranch` table in Snowflake, and inserts only new rows.

**Key Steps:**
- List files with regex `dimbranch*`.
- Convert string date to date data type.
- Lookup existing records in `DimBranch`.
- Insert new records into `DimBranch`.

**Screenshot:**
![DimBranchJob](https://github.com/user-attachments/assets/5cb44207-2a8d-4add-88d7-2ad04ecb59be)


---

### 3. DimSalesAgentJob
This job processes files with the regex `dimsalesagent*`. It maps the foreign key (FK) with `CompleteDate` from the `DimDate` table using a lookup and inserts only new rows into the `DimSalesAgent` table.

**Key Steps:**
- List files with regex `dimsalesagent*`.
- Map FK with `CompleteDate` from `DimDate`.
- Lookup existing records in `DimSalesAgent`.
- Insert new records into `DimSalesAgent`.

**Screenshot:**
![DimSalesAgentJob](https://github.com/user-attachments/assets/3a5850c9-1c73-4135-bfbf-f15019ccf8c0)

---

### 4. TransactionJob
This job processes files with the regex `transaction*` and maps them to three paths:
1. **Product Path**: Extracts unique product rows, checks for new rows using a lookup on the `DimProduct` table, and inserts new records.
2. **Customer Path**: Extracts unique customer rows, checks for new rows using a lookup on the `DimCustomer` table, and inserts new records.
3. **Transaction Path**: Maps the FK with the actual date from the `DimDate` table and filters out existing rows using a lookup on the `Transactions` table.

After processing, the file is moved to the `done` folder.

**Key Steps:**
- List files with regex `transaction*`.
- Process product, customer, and transaction paths.
- Perform lookups on `DimProduct`, `DimCustomer`, and `Transactions`.
- Insert new records into respective tables.
- Move processed files to the `done` folder.

**Screenshot:**
![TransactionJob](https://github.com/user-attachments/assets/781dfdf1-0da3-4bd4-bd7a-638da1a0b1cf)


---

### 5. MasterJob
This is an orchestration job that runs `DimBranchJob`, `DimSalesAgentJob`, and `TransactionJob` sequentially.

**Key Steps:**
- Execute `DimBranchJob`.
- Execute `DimSalesAgentJob`.
- Execute `TransactionJob`.

**Screenshot:**
![MasterJobJob](https://github.com/user-attachments/assets/783e4b82-ab4c-4e1f-b7c6-d748bd86a397)

---

## Folder Structure

project-folder/
├── input/ # Folder for incoming CSV files  ├── done/ # Folder for processed files (inside input folder).
├── screenshots/ # Screenshots of Talend jobs.
├── UseCase Exported Zip File/ # Talend whole use case to be imported 
└── README.md # This file.


---

## How to Run

1. Place the daily CSV files (`transactions`, `salesagents`, and `branches`) in the `input` folder.
2. Open Talend Studio and import the jobs.
3. Run the `MasterJob` to execute the entire pipeline.
4. Check the `done` folder for processed files .

---

## Screenshots

Below are the screenshots of each job for reference:

1. **DimDateJob**  
![DimDateJob](https://github.com/user-attachments/assets/f330838c-d827-42d8-b7c1-ff3b5552dddc)

2. **DimBranchJob**  
![DimBranchJob](https://github.com/user-attachments/assets/9ab686b6-05ad-45fd-b20b-6af496893fbf)

3. **DimSalesAgentJob**  
![DimSalesAgentJob](https://github.com/user-attachments/assets/b5e6cd93-67a1-435c-aedb-8642371026d2)

4. **TransactionJob**  
![TransactionJob](https://github.com/user-attachments/assets/18086fb5-8081-4f6b-9e6c-b77b16fa00e5)

5. **MasterJob**  
![MasterJobJob](https://github.com/user-attachments/assets/b9a799b4-52c3-4364-a6fc-30fd9e953671)

---

For any questions or issues, please open an issue in this repository.
