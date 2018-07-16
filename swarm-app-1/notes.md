## create networks

```bash
docker network create -d overlay frontend
docker network create -d overlay backdend
```

## create services

### create 'db' service

```bash
docker service create --name db --network backend --mount type=volume,source=db-data,target=/var/lib/postgresql/data postgres:9.4
```

### create 'redis' service

```bash
docker service create --name redis --network frontend redis:3.2
```

### create 'worker' service

Depends on: db and redis

```bash
docker service create --name worker --network backend --network frontend dockersamples/examplevotingapp_worker
```

### create 'vote' service

```bash
docker service create --name vote --network frontend --replicas 3 -p 80:80 dockersamples/examplevotingapp_vote:before
```

### create 'result' service

```bash
docker service create --name result --network backend -p 5001:80 dockersamples/examplevotingapp_result:before
```