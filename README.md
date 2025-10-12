# User Activity Tracking

## 📌 Description  
smothing
## 📡 API requests example

### ➕ Create activity event
Request:
POST /api/event
```sh
{
  "user_id": 6,
  "event_action_timestamp": "2025-10-10T18:30:00Z",
  "action": "deleted",
  "metadata": {
    "ip": "192.168.1.10",
    "device": "iPhone 14",
    "location": "Kyiv, Ukraine"
  }
}
```
Response:
```sh
Status 201
```

### 📋 Get users activity event by filter
Request:

GET /api/events?user_id=6&from=2025-10-10T15:43:10.380Z&to=2025-10-13T15:43:10.380Z

Response:
```sh
[
  {
    "user_id": 6,
    "event_action_timestamp": "2025-10-12T21:27:08.034Z",
    "action": "created",
    "metadata": {
      "ip": "192.168.1.10",
      "device": "iPhone 14",
      "location": "Kyiv, Ukraine"
    }
  },
  {
    "user_id": 6,
    "event_action_timestamp": "2025-10-12T21:27:08.034Z",
    "action": "deleted",
    "metadata": {
      "ip": "192.168.23.10",
      "device": "iPhone 13",
      "location": "Lviv, Ukraine"
    }
  }
]
```


## 🐳 Running via docker-compose
The main docker-compose.yml resides at the root of this repository. This *.yml file runs all the necessary services (Grafana, Prometheus, databases, API and website).
### 1️⃣ Build the image and run all needed services
```sh
docker-compose up --build -d
```

## 📅 Daily job description
1. **11.10.2025**:
   - Requirements analysis and development of a follow-up work plan;
   - Development of the first API for creating user activity (POST /api/event) records and saving them to a database (PostgreSQL);
   - Development of an API for obtaining user activity based on the following criteria: user ID, start and end of the search by time (the time the record was created, not when it was active);
   - Implementation of a cron job and its jobs based on counting the number of activities for each user over a period (in our case, every 4 hours);
   - Implementation: Dockerfile and docker-compose.yml;
   - Search for information on working with Grafana and Go;
2. **12.10.2025**:
   - Finding information on developing a minimal website in React;
   - Connecting Grafana to the developed API;
   - Configuring datasource and dashboards (Grafana);
   - Website development (React);
   - Implementing Dockerfiles and docker-compose for a React application;
   - Implementing a common docker-compose for more convenient deployment of all services and applications
3. **13.10.2025**:
   - Described README file;

## 📋 Notes on any optional parts
1. The database stores two dates: the date the user's activity was recorded in the system and the date the activity was performed by the user (the user selects the date);

2. User activity is searched by the date the record was added to the system, not the date the activity was performed by the user;

3. Two metrics have been added to Grafana: user_activity_tracking_api_requests_total (shows the total number of requests) and user_activity_tracking_api_requests_errors_total (shows the number of requests that responded with some kind of error);
   
4. The application was split into two repositories, as it's more appropriate to keep different system levels in separate repositories. But for your convenience, I created a single shared repository where I added the submodules.

5. The cron settings were moved to the configs, as the interval may change in the future, and to make testing easier.
