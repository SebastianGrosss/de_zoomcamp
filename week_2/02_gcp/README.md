## Source:

https://github.com/discdiver/prefect-zoomcamp/tree/main/flows/02_gcp

https://www.youtube.com/watch?v=W-rMz_2GwqQ&list=PL3MmuxUbc_hJed7dXYoJw8DoCuVHhGEQb&index=20

## Set-Up/Usage:

1. login to GCP
2. create new project (prefect-de-zoomcamp)
3. create Cloud Storage Bucket (prefect-de-zoomcamp-bucket)
4. prefect orion start
5. prefect block register -m prefect_gcp
6. Choose a Block: GCS Bucket -> Add
7. set a bucket name and paste name of Cloud storage Bucket (prefect-de-zoomcamp-bucket)
8. Add GCP Credentials Block (name: zoom-gcp-creds)
9. In GCP create a new service account (name: zoom-de-sercvice-acc-prefect)
10. Add BigQuery-Admin and Storage-Admin Role
11. create a json key for service account
12. in prefect orion ui paste key in "Service Account Info"
13. create, select zoom-gcp-creds and click create again to create the GCS Bucket block
14. python etl_web_to_gcs.py


https://www.youtube.com/watch?v=Cx5jt-V5sgE&list=PL3MmuxUbc_hJed7dXYoJw8DoCuVHhGEQb&index=22

1. in GCP -> BigQuery click Add Data
2. click on Google Cloud Storage
3. select a file from the gcs bucket
4. create a dataset Dataset name: dezoomcamp, table: rides
5. DELETE FROM *.rides WHERE true;
6. delete data/yellow/yellow_tripdata_*
7. python etl_gcs_to_bq.py


