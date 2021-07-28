---
title: "Test"
date: 2021-07-26T09:47:57+08:00
summary: "summary"
draft: false
toc: true
tags: ['go', 'traefik','proxy']
author: "bitmyth"
---

# Deploy to Swarm

Add label role=db to node where mysql will runs
```bash
docker node update --label-add role=db NODE
```

### export env variables
```bash
export $(cat .env.prod)
```
### deploy
```bash
docker stack deploy -c stack.yml accounts
```

#### migrateion
```bash
kubectl create configmap migration --from-file=src/database/migrations
kubectl apply -f k8s/migration.yaml
```

#### cert-manager
```bash
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.3.1/cert-manager.yaml
```