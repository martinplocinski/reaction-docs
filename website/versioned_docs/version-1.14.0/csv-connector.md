---
id: version-1.14.0-csv-connector
title: CSV Connector
original_id: csv-connector
---
    
Mainly, the CSV connector allows you to import and export data from and into CSV files. These will help you in several ways.

- To build your shop's initial data
- To update your data in bulk, like current inventory and price of your products, for more efficient shop management
- To view your current data in a file which you can feed to other parties (e.g. your vendors and resellers) or systems (e.g. an ERP system) so that they can easily integrate with your shop

There are three ways by which you can import data with a CSV file:

- Manual - you will upload the file through your Reaction dashboard
- AWS S3 - the file will be fetched from an S3 bucket
- SFTP - the file will be fetched from an SFTP server

Similarly, there are three ways by which you can export data into a CSV file:

- Manual - you will download the file from your Reaction dashboard
- AWS S3 - the file will be sent to an AWS S3 bucket
- SFTP - the file will be sent to an SFTP server

 ## Configuration

If you plan to use AWS S3 or an SFTP server as source or destination of your CSV file, you will need to set up your credentials in your Reaction dashboard. Otherwise, you are good to proceed with manual imports or exports.

Go to Shop settings and look for Import/Export settings. 

For **AWS S3**, enter

- Access Key
- Secret Access Key
- Bucket

You can check how to create an access key [here](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_CreateAccessKey). If you intend to use import with AWS S3 for import, make sure that the access key is allowed to have read operation on the bucket. For export, the key needs to have write operation access on the bucket.

Hit Save, Test for Import, and Test for Export to see if you have correctly set up your AWS S3 credentials.

For **SFTP**, enter

- IP Address
- Port
- Username
- Password

Like in AWS S3, ensure that the the SFTP account you have provided have read access for imports and write access for exports.

Hit Save, Test for Import, and Test for Export to see if you have correctly set up your SFTP credentials.

## Creating a mapping template

We expect that imports and exports will be a repetitive process in your daily operations, so we designed this connector in a way that you can reuse previously created configuration to set up new import or export jobs.

To start, you have to create a mapping template which is a mapping of your CSV column headers to technical reaction fields. For the following steps, you will not be creating a job yet.

1. Go to Connectors on your dashboard.
2. Under "What do you want to do?", select "Create a mapping template", then click "Next."
3. Select the data type.
4. Once you have selected the data type, you will see a link for "Download a sample CSV". Click this and check the downloaded file if you need to know the structure of Reaction schema.
5. Upload a CSV file with at least column names in the first row. Note that the column names can be arbitrary.
6. Click "Next."
7. Map the column names in your CSV file to the Reaction fields. For columns that do not want to save, map them to "Ignore."
8. Enter a name for the new mapping template.
9. Hit "Save."

## Imports

1. Under "What do you want to do?", select "Import CSV", then click "Next."
2. If it is your first time to import for a certain data type, select "Create new job".
3. Choose the file source.
    * If you select "Manual", upload your CSV file.
    * If you select "SFTP", enter the file path of the CSV file relative to the root directory of the SFTP server.
    * If you select "S3", enter the file path of the CSV file relative to the S3 bucket.
4. Select the data type.
5. Select the mapping template. Here, if you pick the default template, it means that the column names of your CSV file should match Reaction field names.
6. Enter a name for the import job.
7. Click "Next." If the file source is S3 or SFTP, then your CSV file will be added to the job queue for processing.
8. If file source is "Manual", you can review if the mapping of the column names and Reaction fields are correct. Then you can either save this mapping to overwrite selected mapping template or create a new mapping template. When you are done, click "Next" and your CSV file will be added to the job queue.

### Reusing import jobs

1. Under "What do you want to do?", select "Import CSV", then click "Next."
2. Click "Import from previous job."
3. Select a previous job. Here, Reaction will export the same data type with the same mapping template and with the same file source as the selected previous job. If the job is "Manual", upload your CSV file .
4. Click "Done."

Note that if your CSV file has a column name corresponding to the ID of the data type, Reaction will update existing documents matching the ID in a row. Otherwise, Reaction will create new documents instead.

## Exports
1. Under "What do you want to do?", select "Export CSV", then click "Next."
2. If it is your first time to export for a certain data type, select "Create new job".
3. Choose the file sources, possibly both SFTP and S3. By default, you can always download the export file from your dashboard.
    * If you select "SFTP", enter the file path of the CSV export file relative to the root directory of the SFTP server.
    * If you select "S3", enter the file path of the CSV export file relative to the S3 bucket.
4. Select the data type.
5. Select the mapping template. The export file will only have the columns that are provided in the mapping template. If you pick the default template, then all fields will be exported.
6. Enter a name for the export job.
7. Click "Done." The export job will be added to the job queue.

### Reusing export jobs

1. Under "What do you want to do?", select "Export CSV", then click "Next."
2. Click "Export from previous job." 
3. Select a previous job. Here, Reaction will export the same data type with the same mapping template and with the same file source/s as the selected previous job.
4. Click "Done."

## Job Queue

Reaction queues import and export jobs for more optimal performance. The job queue displays the status of your jobs. If your CSV file has import errors, the errors will be reported through a downloadable CSV file once the job is done.