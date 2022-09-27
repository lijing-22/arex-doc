# quick install

## quick install
Use the docker-compose.yml file provided by AREX to directly install AREX including services, databases, front-end and other components
There is only one instance of each component, and multi-instance load balancing and scaling cannot be performed.

### AREX component instance
| ID | Instance | Model Name | Description |
| :----:| :----:| :----- | :----- |
| 1 | 1 | [Configuration Service](https://github.com/arextest/arex-config) | A sets of configuration APIs for the recording and replaying functions. |
| 2 | 1 | [Schedule Service](https://github.com/arextest/arex-replay-schedule) | A set of schedule APIs that provide replay send and retrieve all responses for comparison. |
| 3 | 1 | [Replay Report Service](https://github.com/arextest/arex-report) | A set of report APIs that provide difference summaries and show the difference result details after the responses are compared. |
| 4 | 1 | [Storage Service](https://github.com/arextest/arex-storage) | A set of remote storage APIs that provide [Agent Hook Service](https://github.com/arextest/arex -agent-java) to save records and get responses as mocks. |
| 5 | 1 | [Front-End](https://github.com/arextest/arex-front-end) | A visual web site that provide entry to all operations in your **AREX**. |
| 6 | 1 | MongoDB | Data storage and configuration management database |
| 7 | 1 | Redis | Replay Cache |

### Quick install

````
git clone https://github.com/arextest/deployments.git
cd deployments
docker-compose up -d
````

### Check service operation and port
````
[~ deployments]# docker-compose ps
    Name Command State Ports
-------------------------------------------------- ----------------------------------------
arex-config catalina.sh run Up 0.0.0.0:8091->8080/tcp
arex-front docker-entrypoint.sh node ... Up 0.0.0.0:8088->8080/tcp
arex-mongodb docker-entrypoint.sh --auth Up 0.0.0.0:27017->27017/tcp
arex-redis docker-entrypoint.sh --app ... Up 0.0.0.0:6379->6379/tcp
arex-report catalina.sh run Up 0.0.0.0:8090->8080/tcp
arex-schedule catalina.sh run Up 0.0.0.0:8092->8080/tcp
arex-storage catalina.sh run Up 0.0.0.0:8093->8080/tcp
````
* Check all service log commands
````
cd deployments
docker-compose logs
````
* Check AREX log command docker-compose logs arex
* Check the configure service log command docker-compose logs arex-config-service
* Check the schedule service log command docker-compose logs arex-schedule-service
* Check the report service log command docker-compose logs arex-report-service
* Check storage service log command docker-compose logs arex-storage-service

## Docker-Compose file description

### AREX Frontend
````
  arex:
    image: arexadmin01/arex:0.2.1
    container_name: arex-front
    restart: always
    ports:
      - '8088:8080'
    volumes:
      - ./arex-logs/arex-front:/usr/src/app/logs
    environment:
      - SERVICE_REPORT_URL=http://arex-report-service:8080
      - SERVICE_CONFIG_URL= http://arex-config-service:8080
      - SERVICE_SCHEDULE_URL=http://arex-schedule-service:8080
    depends_on:
      -arex-report-service
      -arex-config-service
````
### Storage Service
```
  arex-storage-service:
    image: arexadmin01/arex-storage-serive:0.2.1
    container_name: arex-storage
    restart: always
    ports:
      - '8093:8080'
    volumes:
      - ./arex-logs/arex-storage:/usr/local/tomcat/logs
    environment:
      - "JAVA_OPTS=-Darex.storage.mongo.host=mongodb://arex:iLoveArex@mongodb:27017/arex_storage_db"
    depends_on:
      - mongodb
      - redis
```

### Schedule Serive
```
  arex-schedule-service:
    image: arexadmin01/arex-replay-schedule:0.2.1
    container_name: arex-schedule
    restart: always
    ports:
      - '8092:8080'
    volumes:
      - ./arex-logs/arex-schedule:/usr/local/tomcat/logs
    environment:
      - JAVA_OPTS=-Dmongo.uri=mongodb://arex:iLoveArex@mongodb:27017/arex_storage_db -Darex.storage.service.api=http://arex-storage-service:8080 -Darex.report.service.api=http://arex-report-service:8080 -Darex.config.service.api=http://arex-config-service:8080
    depends_on:
      - mongodb
      - redis
```

### Report service
```
  arex-report-service:
    image: arexadmin01/arex-report:0.2.1
    container_name: arex-report
    restart: always
    ports:
      - '8090:8080'
    volumes:
      - ./arex-logs/arex-report:/usr/local/tomcat/logs
    environment:
      - JAVA_OPTS=-Dmongo.uri=mongodb://arex:iLoveArex@mongodb:27017/arex_storage_db -Darex.config.service.url=http://arex-config-service:8080 -Darex.storage.service.url=http://arex-storage-service:8080 -Darex.ui.url=http://arex:8088      
    depends_on:
      - mongodb  
```

### Config Service
```
  arex-config-service:
    image: arexadmin01/arex-config:0.2.1
    container_name: arex-config
    restart: always
    ports:
      - '8091:8080'
    volumes:
      - ./arex-logs/arex-config:/usr/local/tomcat/logs
    environment:
      - JAVA_OPTS=-Dmongo.uri=mongodb://arex:iLoveArex@mongodb:27017/arex_storage_db
    depends_on:
      - mongodb
```

### Redis
```
  redis:
    image: redis:6.2.6
    container_name: arex-redis
    restart: always
    command: --appendonly yes
    ports:
      - 6379:6379
    volumes:
      - ./arex-data/redis:/data
      - ./arex-logs/redis:/var/log/redis
```

### Mongodb
* Currently verified versions 4.4.4, 5.0, both supported
* mongo-allone-init.js file contains account initialization function
* Data files are placed in ./arex-data/mongodb in the current directory

```
 mongodb:
    image: mongo:4.4.4
    container_name: arex-mongodb
    restart: always
    ports:
      - 27017:27017
    volumes:
      - ./arex-data/mongodb:/data/db
      - ./mongo-allone-init.js:/docker-entrypoint-initdb.d/mongo-init.js:ro
      - ./arex-logs/mongodb:/var/log/mongodb
    command: --auth
    environment:
      - MONGO_INITDB_ROOT_USERNAME=citizix
      - MONGO_INITDB_ROOT_PASSWORD=S3cret
      - MONGO_INITDB_DATABASE=rootdb
      - MONGO_USERNAME=arex
      - MONGO_PASSWORD=iLoveArex
```