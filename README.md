# demoX-deploy_prod
deploy entire system for production

## Architecture

```
  Upcoming Requests

          │
          │
          │
┌─────────▼─────────┐
│                   │
│                   │
│      Ingress      │ ...
│                   │
│                   │
└─────────┬─────────┘
          │
          │
          │
          │
          │
 ┌────────▼────────┐
 │                 │
 │                 │
 │       API       │ ...
 │                 │
 │                 │
 └────────┬────────┘
          │
          │
          │
          │
          │
          │
┌─────────▼─────────┐                 ┌────────────────┐
│                   │                 │                │
│                   │                 │                │
│    Redis/Kafka    ├─────────────────►     Worker     │ ...
│                   │                 │                │
│                   │                 │                │
└───────────────────┘                 └────────────────┘
    (Clusterable)

```

## Component
- `Ingress`: Load balancer + reverse proxy
- `API`: API service for business logic
- `Redis/Kafka`: Message broker/queue
- `Worker`: Worker service for processing business logic

# Usage

## Requirements

- Install Kubernetes
- Install Ingress-nginx extensions
- Need 2 docker images ready for deployment
  - rk_api:latest image: https://github.com/emilebui/demoX-rk_api
  - rk_worker:latest image: https://github.com/emilebui/demoX-rk_worker

## Deployment
### Do the following steps:
  - `Setup config_map`: `kubectl apply -f config_map.yaml`
  - `Deploy redis`: `kubectl apply -f config_map.yaml`
  - `Deploy API`: `kubectl apply -f rk_api.yaml`
  - `Deploy Worker`: `kubectl apply -f rk_api.yaml`
  - `Deploy Ingress`: `kubectl apply -f ingress.yaml`
### Note
- After deploying ingress, you should wait a bit so for ingress to return an IP address
- Then, you can access the system