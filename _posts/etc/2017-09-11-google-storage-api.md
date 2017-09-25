---
layout: post
title:  "Google Storage API usage"
categories: api google
---

### Get an authorization access token from the [OAuth 2.0 Playground](https://developers.google.com/oauthplayground/)
Choose `Cloud Speech JSON API v1` and make token. Use `Access Token` to GET/POST your cloud storage.

> Your Acess Token will expire in 3600 second. Have to refresh before use

### GET google storage object
Call GET request using by `https://www.googleapis.com/storage/v1/b/bucket-name/o/object-name`    

`https://www.googleapis.com/storage/v1/b/speech-suwon/o/test-mono.flac`

```bash
curl -X GET \
    -H "Authorization: Bearer [OAUTH2_TOKEN]" \
    -o "[SAVE_TO_LOCATION]" \
    "https://www.googleapis.com/storage/v1/b/[BUCKET_NAME]/o/[OBJECT_NAME]?alt=media"
```

> more detail, read [get objects document](https://cloud.google.com/storage/docs/json_api/v1/objects/get)

### POST google storage object
Call POST request using by `POST https://www.googleapis.com/upload/storage/v1/b/bucket/o`.    

Have to use `uploadType=media`.    

```bash
curl -X POST --data-binary @[OBJECT] \
    -H "Authorization: Bearer [OAUTH2_TOKEN]" \
    -H "Content-Type: [OBJECT_CONTENT_TYPE]" \
    "https://www.googleapis.com/upload/storage/v1/b/[BUCKET_NAME]/o?uploadType=media&name=[OBJECT_NAME]"
```

### Upload/Download by python

**Upload to storage**
```python
import argparse
import datetime
import pprint

from google.cloud import storage


def upload_blob(source_file_name, destination_blob_name):
    """Uploads a file to the bucket."""
    storage_client = storage.Client('speech-api')
    bucket = storage_client.get_bucket('speech-suwon')
    blob = bucket.blob(destination_blob_name)

    blob.upload_from_filename(source_file_name)

    print('File {} uploaded to {}.'.format(
        source_file_name,
        destination_blob_name))

upload_blob('source/0911-mono.flac', '0911-mono.flac')
```

**Download from storage**
```python
import argparse
import datetime
import pprint

from google.cloud import storage


def download_blob(source_blob_name, destination_file_name):
    """Downloads a blob from the bucket."""
    storage_client = storage.Client('speech-api')
    bucket = storage_client.get_bucket('speech-suwon')
    blob = bucket.blob(source_blob_name)

    blob.download_to_filename(destination_file_name)

    print('Blob {} downloaded to {}.'.format(
        source_blob_name,
        destination_file_name))


download_blob('test-mono.flac', 'source/test-mono.flac')
```
