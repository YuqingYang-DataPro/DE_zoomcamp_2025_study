# DE_zoomcamp_2025_study_week1_homework

Question 1:

Command lines:
docker run -it --entrypoint bash python:3.12.8
pip --version
pip 24.3.1 

Question 2:

db:5432

Question 3:

I have uploaded datasets by using this script in the same folder: Upload_green_taxi

SELECT 
    SUM(CASE WHEN trip_distance <= 1 THEN 1 ELSE 0 END) AS "Up to 1 mile",
    SUM(CASE WHEN trip_distance > 1 AND trip_distance <= 3 THEN 1 ELSE 0 END) AS "1-3 miles",
    SUM(CASE WHEN trip_distance > 3 AND trip_distance <= 7 THEN 1 ELSE 0 END) AS "3-7 miles",
    SUM(CASE WHEN trip_distance > 7 AND trip_distance <= 10 THEN 1 ELSE 0 END) AS "7-10 miles",
    SUM(CASE WHEN trip_distance > 10 THEN 1 ELSE 0 END) AS "Over 10 miles"
FROM green_tripdata
WHERE lpep_pickup_datetime >= '2019-10-01' 
AND lpep_pickup_datetime < '2019-11-01';

78992	150921	90059	24082	32306

Question 4:

SELECT 
    DATE(lpep_pickup_datetime) AS pickup_date,
    MAX(trip_distance) AS longest_trip_distance
FROM green_tripdata
GROUP BY pickup_date
ORDER BY longest_trip_distance DESC
LIMIT 4;

"pickup_date"	"longest_trip_distance"
"2019-10-31"	515.89

Question 5:

WITH filtered_trips AS (
    SELECT 
        "PULocationID", 
        SUM(total_amount) AS total_revenue
    FROM public.green_tripdata
    WHERE DATE(lpep_pickup_datetime) = '2019-10-18'
    GROUP BY "PULocationID"
    HAVING SUM(total_amount) > 13000
)
SELECT 
    tzl."Zone" AS pickup_zone, 
    ft.total_revenue
FROM filtered_trips ft
JOIN taxi_zone_lookup tzl 
    ON ft."PULocationID" = tzl."LocationID"
ORDER BY ft.total_revenue DESC;

"pickup_zone"	"total_revenue"
"East Harlem North"	18686.680000000084
"East Harlem South"	16797.260000000075
"Morningside Heights"	13029.790000000034

Question 6:

with East_haarlem_north_tip as (
    SELECT 
        gt."DOLocationID", 
        gt."tip_amount"
    FROM green_tripdata gt
    JOIN taxi_zone_lookup tzl 
        ON gt."PULocationID" = tzl."LocationID"
    WHERE tzl."Zone" = 'East Harlem North'
    AND DATE(gt."lpep_pickup_datetime") >='2019-10-01'
	AND DATE(gt."lpep_pickup_datetime") <='2019-10-31'
	)

	SELECT 
	 tz."Zone",
     eh."tip_amount"
	FROM East_haarlem_north_tip eh
	JOIN public.taxi_zone_lookup tz
	ON eh."DOLocationID" = tz."LocationID"
	ORDER BY eh."tip_amount" DESC 

    "Zone"	"tip_amount"
    "JFK Airport"	87.3

    Question 7:

    terraform init, terraform apply -auto-approve, terraform destroy


