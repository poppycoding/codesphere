```shell
#创建Volume虚拟磁盘：docker volume create --name <名称>
#删除Volume虚拟磁盘：docker volume rm <名称>

docker ps -a

docker rm mongo
docker rmi mongo

docker inspect --format f4b9375b157
docker inspect -f '{{.NetworkSettings.IPAddress}}' f4b9375b157


docker pull mongo
docker volume create --name mongodata
docker run --name mongo -d -p 27018:27017 -v mongodata:/data/db -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=admin mongo
docker run --name mongo -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=admin mongo
docker exec -it mongo bash
mongo -u admin -p admin

use starbucks;
# db.createUser({user: "starbucks",pwd:"starbucks",roles:[{role:"readWrite",db:"starbucks"}]});


docker pull redis
docker volume create --name redisdata
docker run --name redis -d -p 6379:6379 -v redisdata:/data/db redis
docker exec -it redis bash


docker search mssql
docker pull microsoft/mssql-server-linux
docker volume create --name mssqldata
docker run --name mssql -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=@Db12345" --privileged=true -v mssqldata:/data/db  -p 1433:1433 -d microsoft/mssql-server-linux
docker exec -it mssql bash
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '@Db12345'
# select name from SysDatabases;
go
CREATE DATABASE byor;
go
use byor


docker pull postgres
docker volume create --name postgresdata
docker run --name postgres -p 5432:5432 -v postgresdata:/data/db -e POSTGRES_USER=sonar -e POSTGRES_PASSWORD=sonar -d postgres

docker pull sonarqube
docker volume create --name sonarqubedata
docker run --name sonarqube -v sonarqubedata:/data/db --link postgres -e SONARQUBE_JDBC_URL=jdbc:postgresql://postgres:5432/sonar -e SONARQUBE_JDBC_USERNAME=sonar -e SONARQUBE_JDBC_PASSWORD=sonar -p 9092:9000 -d sonarqube
# http://localhost:9008/ admin/admin -> admin/sonar
docker exec -it sonarqube bash
ls
cat logs/sonar.log


docker exec -it postgres bash
psql -U sonar -h localhost -d sonar
help
\d
# select * from users;


docker pull rabbitmq
docker pull rabbitmq:management
docker volume create --name rabbitdata
docker run --name rabbitmq -d -p 5672:5672 -v rabbitdata:/data/db -p 15672:15672 -e RABBITMQ_DEFAULT_USER=rabbit -e RABBITMQ_DEFAULT_PASS=rabbit rabbitmq:management



docker pull portainer/portainer
docker volume create --name portainerdata
docker run -d -p 8091:8000 -p 9091:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainerdata:/data portainer/portainer-ce
# http://localhost:9091/ admin/portainer


docker pull redislabs/redisinsight:latest
docker volume create --name redisinsightdata
docker run -d --link redis -v redisinsightdata:/data/db -p 8001:8001 --name=redisinsight redislabs/redisinsight:latest
docker run -d -v redisinsightdata:/data/db -e AWS_ACCESS_KEY=redisinsight -e AWS_SECRET_KEY=redisinsight -p 8001:8001 --name=redisinsight redislabs/redisinsight:latest

```