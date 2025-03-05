# Talend_Transaction_UseCase
A Talend pipeline to process daily CSV files (transactions, salesagents, branches) and load data into Snowflake tables (DimDate, DimBranch, DimSalesAgent, DimCustomer, DimProduct, Transactions). Ensures only new records are inserted and moves processed files to a done folder.
