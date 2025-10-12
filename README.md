# User Activity Tracking

## 📌 Description  
smothing


## 🐳 Running via docker-compose
The main docker-compose.yml resides at the root of this repository. This *.yml file runs all the necessary services (Grafana, Prometheus, databases, API and website).
### 1️⃣ Build the image and run all needed services
```sh
docker-compose up --build -d
```

## 📋 Daily job description
1. **11.10.2025**:
   - Requirements analysis and development of a follow-up work plan;
   - Development of the first API for creating user activity (POST /api/event) records and saving them to a database (PostgreSQL);
   - Development of an API for obtaining user activity based on the following criteria: user ID, start and end of the search by time (the time the record was created, not when it was active);
   - Implementation of a cron job and its jobs based on counting the number of activities for each user over a period (in our case, every 4 hours);
   - Implementation: Dockerfile and docker-compose.yml;
   - Search for information on working with Grafana and Go;
2. **12.10.2025**:
   - Finding information on developing a minimal website in React;
