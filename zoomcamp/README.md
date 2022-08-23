# Running Postgres with docker and ingest data in Linux
NY Trips dataset : https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page

# Create a Network
```sudo docker network create pg-network```

# Run Postgres
```
sudo docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:13
```  
# Run pgAdmin
```
sudo docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  --network=pg-network \
  --name pgadmin \
  dpage/pgadmin4
```  
# Data ingestion
```
python ingest_data.py \
  --user=root \
  --password=root \
  --host=localhost \
  --port=5432 \
  --db=ny_taxi \
  --table_name=yellow_taxi_trips \
  --url="https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-01.csv.gz" \
``` 
# Build the image

```sudo docker build -t taxi_ingest:v001 .```

# Run script with docker
```
sudo docker run -it \
  --network=pg-network \
  taxi_ingest:v001 \
    --user=root \
    --password=root \
    --host=pg-database1 \
    --port=5432 \
    --db=ny_taxi \
    --table_name=yellow_taxi_trips \
    --url="https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-01.csv.gz" \
```
For run multiple containers use docker-compose.yaml in Docker folder.
```
sudo docker-compose up
sudo docker-compose down
```
# Terraform with GCP
Use archive in Terraform folder.

Google Cloud Storage and BigQuery
```
 terraform init
 terraform plan
 terraform apply
 terraform destroy 
 ```
