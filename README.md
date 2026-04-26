# notify-engine
A high-throughput event-driven notification system built with Node.js, TypeScript, Kafka, Redis, and MySQL.

## Status
Active development — v0.1.0 in progress

## What this is
notify-engine is a production-grade notification infrastructure system designed to handle high-volume async notification workloads reliably across multiple channels — email, SMS, and in-app.

Built to demonstrate real reliability engineering: at-least-once delivery guarantees, idempotent processing, zero message loss across deployments, and observable failure handling.

## Architecture
API Gateway (Express + TypeScript)  
|  
| HTTP POST /notifications  
v  
Kafka Broker  
| Partitioned by user_id  
| Ordered processing per entity  
v  
Consumer Group (Node.js + TypeScript)  
| Idempotent processing  
| Manual offset commit after processing  
v  
Redis (Deduplication layer)  
| Event dedup key: event_id + consumer_group  
| TTL-based expiry  
v  
Notification Dispatchers  
| Email dispatcher  
| SMS dispatcher  
| In-app dispatcher  
v  
MySQL (Notification state + delivery log)  
|  
v  
Dead Letter Queue (Failed events)  
| Replay service with backoff  
| Alert on repeated failures  

## Tech Stack
Layer | Technology  
API Gateway | Node.js, Express, TypeScript  
Message Queue | Apache Kafka  
Deduplication | Redis  
Database | MySQL  
Containerization | Docker, Docker Compose  
Validation | Zod  
Testing | Jest  


## Key Engineering Decisions
See the docs/adr directory for full Architecture Decision Records.

- ADR-001: Kafka over RabbitMQ and BullMQ for message queuing
- ADR-002: MySQL over MongoDB for notification state
- ADR-003: Two-layer Redis idempotency strategy
- ADR-004: Manual offset commit pattern for zero message loss


## Project Structure
notify-engine/  
packages/  
api-gateway/  
notification-consumer/  
email-dispatcher/  
sms-dispatcher/  
infrastructure/  
docker-compose.yml  
kafka/  
mysql/  
redis/  
docs/  
adr/  
architecture.md  


## Getting Started

Prerequisites: Docker, Docker Compose, Node.js 18 plus

git clone https://github.com/nikita-tech/notify-engine  
cd notify-engine  
docker-compose up -d  
npm install  
npm run dev  


## Why I built this

To demonstrate production-grade backend engineering beyond typical CRUD applications. The patterns here — event-driven architecture, idempotent consumers, graceful shutdown, dead letter queues — are the same patterns used in high-traffic production systems at scale.
