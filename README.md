# Preprocessor for Aperio Slide Viewer
Program that makes image pyramids from Aperio SVS files stored in Amazon S3. Also extracts metadata, label image, and thumbnail.

## Workflow
1. Scanner dumps files onto ScanScope Workstation with filename {barcode}_{imageid}.svs
2. aws s3 sync from ScanScope Workstation to images S3 bucket
    - delete local files on successful sync
3. image preprocessor runs:
    - extract metadata and put into DynamoDB slide table
    - extract label and thumbnail and put into viewer S3 bucket
    - generate Deep Zoom and IIIF views into viewer S3 bucket
4.	scan tech reviews images for scanning errors
    - searches for new (unsent) slides
    - deletes and rescans failed slide scans
    - fixes missing barcodes in image filename
    - marks slides to send (metadata) to CDR
5.	pathologist review

## Usage
```
$ alias aperio-proc='docker run --rm -ti -v ~/.aws:/home/svcuser/.aws vanandelinstitute/aperio-proc'
$ aperio-proc --file "barcode_imageid.svs" --source "source-bucket" --dest "dest-bucket" --table "dynamodb-table-name"
```
