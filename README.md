# redis-cache

Redis caching layer for the Exodus Rush game. Used for frequently accessed data like auth sessions and terrain maps.

## Overview

**Service Configuration:**
- **Port:** 6379
- **Replicas:** 1 (with persistence via AOF)
- **Access URL:** `redis-cache:6379`

## Use Cases

- JWT token blacklist
- Session storage for auth-service
- Terrain map cache for terrain-service
- Rate limiting counters
- Temporary data storage

## Features

- Append-Only File (AOF) persistence enabled
- LRU eviction policy with 256MB max memory
- Fast in-memory operations
- Simple key-value and pub/sub support

## Deployment

```bash
# Create namespace
kubectl create namespace passover

# Deploy Redis
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

# Verify deployment
kubectl get pods -n passover -l app=redis-cache
kubectl get svc -n passover redis-cache
```

## Usage

Services connect to Redis at:

```
redis-cache:6379
```

Example connection strings:
- Node.js: `redis://redis-cache:6379`
- Python: `redis://redis-cache:6379/0`

## Health Check

```bash
# Test Redis connection
kubectl exec -n passover deployment/redis-cache -- redis-cli ping

# Check memory usage
kubectl exec -n passover deployment/redis-cache -- redis-cli info memory
```

## Configuration

- **Persistence:** AOF (appendonly.aof)
- **Max Memory:** 256MB
- **Eviction Policy:** allkeys-lru (evict least recently used keys)

