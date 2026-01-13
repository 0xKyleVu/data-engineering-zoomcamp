# Pipeline — Module 1

This folder contains a small ingestion pipeline that downloads NYC Yellow Taxi CSVs and writes them into a Postgres database.

## Quick start (reproduce the module)

1. Start services:
   - `docker compose -f docker-compose.yaml up -d`
2. Build the ingest image (only required if you changed code):
   - `docker build -t taxi_ingest:v001 -f Dockerfile .`
3. Run ingestion for year/month (example):
   - `docker run --rm --network=pg-network taxi_ingest:v001 --pg-user=root --pg-pass=root --pg-host=pgdatabase --pg-db=ny_taxi --year=2021 --month=2 --chunksize=100000`
4. Verify in Postgres:
   - `docker exec -it pipeline-pgdatabase-1 psql -U root -d ny_taxi -c "SELECT table_name FROM information_schema.tables WHERE table_schema='public';"`
   - `docker exec -it pipeline-pgdatabase-1 psql -U root -d ny_taxi -c "SELECT count(*) FROM yellow_taxi_trips_2021_2;"`

## Notes

- The compose file is configured to use an external volume `ny_taxi_postgres_data` so the ingested data persists and can be shared with other containers (e.g., pgAdmin).
- `ingest_data.py` is the main ETL worker (uses Click for CLI parsing). `pipeline.py` is a small demo and not used for ingestion.

## Useful commands

- Stop and remove compose services: `docker compose -f docker-compose.yaml down`
- Inspect logs: `docker compose -f docker-compose.yaml logs -f pgdatabase`

---

*End of module 1 pipeline README.*