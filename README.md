# DOMO Asana Connector

## Problem
The out of the box Domo Asana connector pulls ALL historical data everyday which can take more than a day to complete the sync. 

## Solution
1. Monday to Saturday it pulls only yesterday's modified tasks. It filters the tasks where `modified_at` field is yesterday
2. On Sunday it pulls tasks that have been created in the last 365 days. It filters the tasks where `created_at` field is more than 365 days.
This is required in cases where a task itself is not updated but task's project or task's custom fields have been updated. In these cases task's `modified_at` field is not updated and the task is not synced with Domo.
3. Everyday the connector pulls a list of deleted tasks which later get removed from Domo by Magic ETL

## API Access 
The connector is using two different API access tokens:
1. Personal Access Token (PAT) is used to access Tasks API to get tasks that have been modified or created. The PAT belongs to `domouser@michaeljfox.org` Asana user. The PAT can access all public projects and all the private projects that are assigned to the user. PAT allows to control which Private projects should be pulled into Domo. 
2. Asana Service Account is used to access a Audit Log Events API to get a list of deleted tasks. PAT cannot access this API endpoint. 

## Domo ETL
The data lands in a Domo dataset which is overwritten everyday with the new data. The Magic ETL runs right after the new data arrives and performs merge upsert operations with the historical data.

## Deployment to Domo
1. Login to Domo as `domouser@michaeljfox.org` Domo user.
2. Go to connector builder dev environment (https://api.domo.com/builder/index.html). Use `michaeljfox-org` as domain when logging in.
2. Copy the code from GitHub repo to test and submit the connector for deployment. Once submitted Domo Connector team will review the code changes and deploy the connector.

