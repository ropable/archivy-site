---
date: 06-08-21
id: 38
path: ''
tags: []
title: Azure Python SDK
type: note
---

# Azure Blob storage API
https://github.com/Azure/azure-storage-python

Generate a blob SAS token: https://docs.microsoft.com/en-us/python/api/azure-storage-blob/azure.storage.blob?view=azure-python#azure_storage_blob_generate_blob_sas

Example:

```python
from datetime import date, timedelta
from azure.storage.blob import BlobServiceClient, BlobSasPermissions, generate_blob_sas


blob_service_client = BlobServiceClient.from_connection_string(blob_account_connection_string)
out_filename_csv = out_filename + '.csv'
blob_sas_token = generate_blob_sas(
    account_name=blob_service_client.account_name,
    container_name="analytics",
    blob_name=out_filename_csv,
    account_key=blob_service_client.credential.account_key,
    permission=BlobSasPermissions(read=True),
    expiry=date.today() + timedelta(days=365 * 3),  # Expiry = three years.
)
blobdir_https = 'https://{}.blob.core.windows.net/{}'.format(blob_account_name, blob_container_name)
outputlink = '{}/{}?{}'.format(blobdir_https, out_filename_csv, blob_sas_token)
```

Synapse - secure access to linked resources:

https://docs.microsoft.com/en-us/azure/synapse-analytics/spark/apache-spark-secure-credentials-with-tokenlibrary?pivots=programming-language-python