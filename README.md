# User Activity Tracking

## 📌 Description  
User Activity Tracking is a user activity tracking system. The system allows you to add four types of user activity ('created', 'updated', 'deleted', 'viewed'), specify the time and date of this activity, and provide metadata that displays more detailed information about the activity (in JSON format). Every 4 hours (0 */4 * * * - defined in the config, can be changed), the system calculates the amount of activity for each user using a cron job. Monitoring is provided using Grafana (monitoring description below). The Prometheus config for Grafana is located in `.\user-activity-tracking-api\prometheus\prometheus.yml`, when raising the entire system, please consider the comment.
I recommend running it via Docker.

### 🏗 Tech Stack
- **Back-end:**: Golang (gin, cors, logrus, gorm, prometheus/client_golang, gocron)
- **Front-end:** React
- **Database:** Postgres:16
- **Monitoring:** Grafana, Prometheus
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

## 🐳 Running via Docker
The main docker-compose.yml resides at the root of this repository. This *.yml file runs all the necessary services (Grafana, Prometheus, databases, API and website).
### 1️⃣ Build the image and run all needed services
```sh
docker-compose up --build -d
```
## 🛠 .env examples
.env for user-activity-tracking-api (better run via docker-compose):
```sh
DB_URL=postgres://postgres:password@localhost:5432/UserActivityTracking?sslmode=disable
CRON_TAB_COUNT_USERS_EVENT_TASK=0 */4 * * *
HTTP_PORT=8080
HTTP_CORS_ALLOWED_ORIGINS=*
HTTP_CORS_ALLOWED_METHODS=GET,POST
HTTP_CORS_ALLOWED_HEADERS=Content-Type
HTTP_CORS_MAX_AGE_HOURS_CACHE=12
```
.env for user-activity-tracking-app (better run via docker-compose):
```sh
REACT_APP_API_BASE_URL=http://localhost:8080/api
```
REACT_APP_API_BASE_URL - `user-activity-tracking-api` link

## 📈 Monitoring setup
When raising all services via a common docker-compose, you can go to `http://localhost:3000` (our Grafana website). Log in (admin/admin) and then, if you want, you can change your password. A ready-made dashboard for monitoring is imported when services are launched (path: `.\user-activity-tracking-api\monitoring\grafana\provisioning\dashboards`). Logs are configured for services that are present in docker-compose. By going to http://localhost:3000/explore you can use the Loki query example `{service="api"}`. By going to `http://localhost:3000/a/grafana-lokiexplore-app/explore` in the filter field you can select the service for which you want to receive logs.

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
   - Fixing cors conflict Website and developed api;
   - Implementing Dockerfiles and docker-compose for a React application;
   - Implementing a common docker-compose for more convenient deployment of all services and applications
3. **13.10.2025**:
   - Described README file;
   - Studying API log output information in Grafana;
   - Implementation of API logs in Grafana (not yet completed);
   - Fixing API, monitoring, and Docker file bugs;
   - Fix README file;

## 📋 Notes on any optional parts
1. The database stores two dates: the date the user's activity was recorded in the system and the date the activity was performed by the user (the user selects the date);

2. User activity is searched by the date the record was added to the system, not the date the activity was performed by the user;

3. Two metrics have been added to Grafana: user_activity_tracking_api_requests_total (shows the total number of requests) and user_activity_tracking_api_requests_errors_total (shows the number of requests that responded with some kind of error);
   
4. The application was split into two repositories, as it's more appropriate to keep different system levels in separate repositories. But for your convenience, I created a single shared repository where I added the submodules;

5. The cron settings were moved to the configs, as the interval may change in the future, and to make testing easier;

6. For Grafana, visit `.\user-activity-tracking-api\monitoring` and you will be able to see the entire Grafana-configuration.




