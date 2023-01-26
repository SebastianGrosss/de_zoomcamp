## Useful Notes: ##

https://github.com/ziritrion/dataeng-zoomcamp/blob/main/notes/1_intro.md

## Usage: ## 

Running Postgres and pgAdmin with docker-compose:

    docker-compose up -d

Build ingestion image:

    docker build -t taxi_ingest:v001 .

Find out network name of docker-compose-network:

    docker network ls

run ingestion image:

    docker run -it \
        --network=2_docker_sql_default \
        taxi_ingest:v001 \
        --user=root \
        --password=root \
        --host=pgdatabase \
        --port=5432 \
        --db=ny_taxi \
        --table_name=green_taxi_trips \
        --url="https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2019-01.csv.gz"

modified `ingest_data.py`, build ingestion image again and run:

    docker run -it \
        --network=2_docker_sql_default \
        taxi_ingest:v001 \
        --user=root \
        --password=root \
        --host=pgdatabase \
        --port=5432 \
        --db=ny_taxi \
        --table_name=taxi_zones \
        --url="https://s3.amazonaws.com/nyc-tlc/misc/taxi+_zone_lookup.csv"



browse to `localhost:8080` log in and connect to postgres database as described in "Useful Notes"

shut down container:

    docker-compose down

clean up container:

    docker container prune

delete network:

    docker network rm <network-name>



SQL Querys:

How many taxi trips were totally made on January 15? (Started and finnished on 2019-01-15)

    SELECT 
	    CAST(lpep_pickup_datetime AS DATE),
	    CAST(lpep_dropoff_datetime AS DATE),
	    COUNT(1)
    FROM
	    green_taxi_trips
    WHERE
        CAST(lpep_pickup_datetime AS DATE) = '2019-01-15' AND
        CAST(lpep_dropoff_datetime AS DATE) = '2019-01-15'
    GROUP BY
        1, 2


Which was the day with the largest trip distance Use the pick up time for your calculations.

    SELECT 
        CAST(lpep_pickup_datetime AS DATE),
        trip_distance
    FROM
        green_taxi_trips
    ORDER BY
        trip_distance DESC


In 2019-01-01 how many trips had 2 and 3 passengers? (edit: started on 2019-01-01)

    SELECT 
        CAST(lpep_pickup_datetime AS DATE),
        passenger_count,
        COUNT(1)
    FROM
        green_taxi_trips
    WHERE
        CAST(lpep_pickup_datetime AS DATE) = '2019-01-01'
    GROUP BY
        1, 2


For the passengers picked up in the Astoria Zone which was the drop off zone that had the largest tip? We want the name of the zone, not the id.

    SELECT
        zpu."Zone" AS "pickup_zone",
        zdo."Zone" AS "dropoff_zone",
        t."tip_amount"
    FROM
        green_taxi_trips t,
        taxi_zones zpu,
        taxi_zones zdo
    WHERE
        t."PULocationID" = zpu."LocationID" AND
        t."DOLocationID" = zdo."LocationID" AND
        zpu."Zone" = 'Astoria'
    ORDER BY
        tip_amount DESC