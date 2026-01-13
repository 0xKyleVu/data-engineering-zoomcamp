# data-engineering-zoomcamp

## Module 1 — Journal (DataTalksClub 2026)

**Overview** ✅

- Small end-to-end ETL: download NYC Yellow Taxi CSVs, transform with pandas, and load them into PostgreSQL.

**What I did in this module** 🔧

1. Implemented `ingest_data.py` — a Click-based CLI that downloads and ingests month CSVs into Postgres in memory-efficient chunks.
2. Dockerized the worker (`pipeline/Dockerfile`) so ingestion runs reproducibly inside a container.
3. Added `docker-compose.yaml` to run `pgdatabase` (Postgres) and `pgadmin` (pgAdmin); configured to reuse the existing `ny_taxi_postgres_data` volume so data persists across runs.
4. Built and tested the pipeline by ingesting `yellow_taxi_trips_2021_2` (example run produced ~1,371,708 rows).
5. Verified tables and counts using `psql` and pgAdmin.
6. Fixed a CLI confusion: `pipeline.py` is a small demo that reads `sys.argv[1]`; the real worker is `ingest_data.py` which properly parses flags.

**How to reproduce** ▶️

- Start DB & pgAdmin: `docker compose -f pipeline/docker-compose.yaml up -d`
- Build image (if you changed the code): `docker build -t taxi_ingest:v001 -f pipeline/Dockerfile pipeline`
- Run ingestion (example):

  `docker run --rm --network=pg-network taxi_ingest:v001 --pg-user=root --pg-pass=root --pg-host=pgdatabase --pg-db=ny_taxi --year=2021 --month=2 --chunksize=100000`

- Verify the table exists and row count:

  `docker exec -it pipeline-pgdatabase-1 psql -U root -d ny_taxi -c "SELECT count(*) FROM yellow_taxi_trips_2021_2;"`

**Notes & next steps** 💡

- For production: add tests, retries, logging, schema migrations, and orchestration (Airflow, Prefect, etc.).
- Future modules will add transformations, analytics, or scheduling; we'll keep a short journal entry per module here.

---

*(This file serves as the module journal. Future modules will be appended as new sections.)*
