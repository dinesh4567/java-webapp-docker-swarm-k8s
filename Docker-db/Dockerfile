FROM mysql:8.0

# No credentials baked into the image. MYSQL_ROOT_PASSWORD and MYSQL_DATABASE
# are supplied at runtime - from the environment under Docker Swarm, or from a
# Kubernetes Secret (see manifests/db-deployment.yml).
ADD db_backup.sql docker-entrypoint-initdb.d/db_backup.sql

EXPOSE 3306
