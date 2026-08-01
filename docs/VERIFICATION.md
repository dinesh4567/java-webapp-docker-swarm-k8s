# Deployment Verification

Deployed to a local `kind` cluster (Kubernetes v1.36.1) from images built by
the multi-stage Dockerfile in this repository.

## Workloads

```
NAME                             READY   STATUS    RESTARTS   AGE
pod/devopsapp-688b68b74c-dtsk4   1/1     Running   0          6m18s
pod/devopsapp-688b68b74c-gq8xs   1/1     Running   0          6m18s
pod/devopsdb-0                   1/1     Running   0          6m18s

NAME               TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/appsvc     LoadBalancer   10.96.137.57   <pending>     8080:31312/TCP   6m18s
service/devopsdb   ClusterIP      None           <none>        3306/TCP         6m18s
```

## StatefulSet and persistent storage

```
NAME                        READY   AGE
statefulset.apps/devopsdb   1/1     6m18s

NAME                                      STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
persistentvolumeclaim/dbdata-devopsdb-0   Bound    pvc-e72027e8-bd97-4af6-8736-c4db35a39088   2Gi        RWO            standard       <unset>                 6m18s
```

## Credentials are externalised

The deployed WAR contains placeholders, not values:

```
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://${DB_HOST:devopsdb}:3306/${MYSQL_DATABASE:accounts}?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull
jdbc.username=${DB_USER:root}
jdbc.password=${MYSQL_ROOT_PASSWORD:}
```

The old hardcoded password appears zero times in the deployed artifact, and
the values injected from the Secret authenticate against MySQL:

```
AUTH OK using the app pod env vars
user_rows: 7
```

## Schema seeded from db_backup.sql

```
Tables_in_accounts
role
user
user_role
```
