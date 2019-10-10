# MSSQL for Docker and Kubernetes

## Reference
- https://hub.docker.com/_/microsoft-mssql-server


## Docker deployment to the local workstation

~~~
# start the container
docker run --name mssql -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Adm1nAdm1n' -e 'MSSQL_PID=Developer' -p 1433:1433 -d mcr.microsoft.com/mssql/server

# see the status
docker container ls

# destroy the container
docker container stop mssql
docker container rm mssql
~~~


## Kubernetes deployment to the local workstation (macOS only)

## Prep your local workstation (macOS only)
1. Clone this repo and work in it's root directory
1. Install Docker Desktop for Mac (https://www.docker.com/products/docker-desktop)
1. In Docker Desktop > Preferences > Kubernetes, check 'Enable Kubernetes'
1. Click on the Docker item in the Menu Bar. Mouse to the 'Kubernetes' menu item and ensure that 'docker-for-desktop' is selected.

### Deploy (run these commands from the root folder of this repo)
~~~
./local-apply.sh
~~~

Note: The default mssql sa password is Adm1nAdm1nAdm1n

### Delete
~~~
./local-delete.sh
~~~

### Create a backup of the persistent volume
~~~
./local-backup.sh
~~~

### Restore from backup (pass it the backup folder name)
~~~
./local-restore.sh 2019-10-31_20-05-55
~~~

### Restart the deployment
~~~
./local-restart.sh
~~~

### Scale the deployment
~~~
kubectl scale --replicas=4 deployment/mssql
~~~

### Shell into the container
~~~
./local-shell-mssql.sh
~~~

### Get the logs from the container
~~~
./local-logs-mssql.sh
~~~

### Use a port-forward to connect a SQL management tool to the mssql instance
~~~
POD=$(kubectl get pod -l app=mssql -o jsonpath="{.items[0].metadata.name}")
kubectl port-forward $POD 1433:1433
~~~