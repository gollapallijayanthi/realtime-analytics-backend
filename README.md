\# Real-Time Analytics Backend API



\## Project Overview

This project is a robust backend API for a simulated real-time analytics service.  

It is designed using production-grade backend engineering patterns to ensure high availability, performance, and fault tolerance.



The application demonstrates:

\- Redis-based caching

\- Redis-backed rate limiting

\- Circuit breaker pattern for external service resilience

\- Dockerized deployment with docker-compose

\- Automated unit and integration testing



This project is suitable for enterprise backend systems that must handle varying loads and external service failures reliably.



---



\## Tech Stack

\- \*\*Backend Framework:\*\* FastAPI (Python)

\- \*\*Caching \& Rate Limiting:\*\* Redis

\- \*\*Containerization:\*\* Docker, Docker Compose

\- \*\*Testing:\*\* Pytest

\- \*\*Language:\*\* Python 3.9+



---



\## Architecture Overview

\- Metrics are ingested via REST APIs.

\- Aggregated summaries are cached in Redis using a read-through strategy.

\- Rate limiting protects the API from abuse.

\- A circuit breaker protects calls to an unreliable external service.

\- All services run in isolated Docker containers.



---



\## Project Structure

```



.

├── src/

│   ├── main.py

│   ├── api/

│   │   └── metrics.py

│   ├── services/

│   │   ├── cache\_service.py

│   │   ├── rate\_limiter.py

│   │   ├── circuit\_breaker.py

│   │   └── external\_service.py

│   └── config/

│       └── settings.py

├── tests/

│   ├── test\_api\_integration.py

│   ├── test\_rate\_limiting.py

│   └── test\_circuit\_breaker.py

├── Dockerfile

├── docker-compose.yml

├── requirements.txt

├── .env.example

└── README.md



````



---



\## Setup \& Running the Application



\### Prerequisites

\- Docker

\- Docker Compose



\### Build and Run

```bash

docker-compose up --build

````



The API will be available at:



```

http://localhost:8000

```



---



\## Health Check



```bash

curl http://localhost:8000/health

```



Response:



```json

{"status":"ok"}

```



---



\## API Endpoints



\### 1. Create Metric



\*\*POST /api/metrics\*\*



```bash

curl -X POST http://localhost:8000/api/metrics \\

-H "Content-Type: application/json" \\

-d '{"timestamp":"2025-01-01T10:00:00","value":75,"type":"cpu"}'

```



Response:



```json

{"message":"Metric stored successfully"}

```



---



\### 2. Get Metric Summary



\*\*GET /api/metrics/summary\*\*



```bash

curl "http://localhost:8000/api/metrics/summary?metric\_type=cpu"

```



Response:



```json

{"type":"cpu","count":1,"average":75.0}

```



This endpoint uses Redis read-through caching with a configurable TTL.



---



\### 3. External Service (Circuit Breaker Protected)



\*\*GET /external\*\*



```bash

curl http://localhost:8000/external

```



Possible responses:



```json

{"external":"ok","value":134}

```



Fallback when circuit is open:



```json

{"fallback":true,"reason":"external service failure"}

```



---



\## Caching Strategy (Redis)



\* Read-through caching for metric summaries

\* Configurable TTL

\* Cache invalidation on metric creation

\* Prevents repeated computation of summaries



---



\## Rate Limiting



\* Redis-backed rate limiter

\* Applied to POST `/api/metrics`

\* Default limit: 5 requests per minute per IP

\* Exceeding limit returns:



```http

429 Too Many Requests

Retry-After: <seconds>

```



---



\## Circuit Breaker Pattern



\* Protects external service calls

\* States:



&nbsp; \* Closed

&nbsp; \* Open

&nbsp; \* Half-Open

\* Automatically recovers after reset timeout

\* Prevents cascading failures



---



\## Running Tests



All unit and integration tests can be run using:



```bash

pytest

```



Expected output:



```

5 passed in Xs

```



---



\## Docker \& Health Checks



\* Application and Redis run in separate containers

\* Redis health check included

\* Application health check verifies `/health` endpoint

\* Services start only when dependencies are healthy



---



\## Conclusion



This project demonstrates a fault-tolerant, scalable backend API using modern distributed system patterns. It is production-ready and suitable for real-world backend engineering roles.



