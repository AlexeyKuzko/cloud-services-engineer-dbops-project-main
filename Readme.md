# dbops-project
Source code repository for the "DBOps" project.

## Requirements
* Docker version 27.4.0, build bde2b89 or newer
* Docker Compose version v2.31.0-desktop.2 or newer
* psql (PostgreSQL) 16.8 or newer

## Installation and Setup
To run the project on your local machine, clone the repository and execute the following commands:
```bash
git clone https://github.com/AlexeyKuzko/cloud-services-engineer-dbops-project-main.git
cd cloud-services-engineer-dbops-project-main
sudo docker-compose up -d
```

## Connect to the Database
In order to verify connection to the database, use the following command:
```bash
psql "host=localhost port=5432 dbname=store_default user=user" # it's predefined in docker-compose file
```

## Insert test data
Use insert-data.sh to create a test database contents.
```bash
chmod +x insert-data.sh
./insert-data.sh
```

## Check tables structure
To verify the connection to the database and inspect the structure of all tables, use the following commands:
```bash
psql "host=localhost port=5432 dbname=store_default user=user"
\dt
\d
```

## Create Another Postgres Database
```sql
CREATE DATABASE store OWNER store_user;
```

## Create New Postgres User
```sql
CREATE USER store_user WITH PASSWORD 'store_password';
```

## Connect to the new Database
```bash
psql -h localhost -p 5432 -U store_user -d store
```

## Daily Shipped Orders for the Last 7 Days
Query to show the number of sausages sold each day in the past week.
```sql
-SELECT o.date_created, SUM(op.quantity) AS total_sausages
FROM orders AS o
JOIN order_product AS op ON o.id = op.order_id
WHERE o.status = 'shipped' AND o.date_created > now() - INTERVAL '7 DAY'
GROUP BY o.date_created;
```