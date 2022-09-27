# AREX service installation
![](../resource/arch.png)

## Agent general configuration
````
JAVA_OPTS="$JAVA_OPTS -Darex.config.path=/usr/your-path-to-arexconf/arex.agent.conf"
````

## custom installation
### Multi-instance Docker-Compose installation
````
git clone https://github.com/arextest/deployments.git
cd deployments
docker-compose -f docker-compose-distribute.yml up -d

## stop command
docker-compose -f docker-compose-distribute.yml down -v
##PS command
docker-compose -f docker-compose-distribute.yml ps
````

#### Default installation instance
* The current configuration schedule and storage are 2 instances, you can refer to the configuration file to configure multiple instances
  
| ID | Instance | Model Name | Description |
| :----:| :----:| :----- | :----- |
| 1 | 1 | [Configuration Service](https://github.com/arextest/arex-config) | A sets of configuration APIs for the recording and replaying functions. |
| 2 | 2 | [Schedule Service](https://github.com/arextest/arex-replay-schedule) | A set of schedule APIs that provide replay send and retrieve all responses for comparison. |
| 3 | 1 | [Replay Report Service](https://github.com/arextest/arex-report) | A set of report APIs that provide difference summaries and show the difference result details after the responses are compared. |
| 4 | 2 | [Storage Service](https://github.com/arextest/arex-storage) | A set of remote storage APIs that provide [Agent Hook Service](https://github.com/arextest/arex -agent-java) to save records and get responses as mocks. |
| 5 | 1 | [Front-End](https://github.com/arextest/arex-front-end) | A visual web site that provide entry to all operations in your **AREX**. |
| 6 | 1 | MongoDB | Data storage and configuration management database |
| 7 | 1 | Redis | Replay Cache |
| 8 | 1 | Nginx | ​​Schedule Load Balancing Service |
| 9 | 1 | Nginx | ​​Storage Load Balancing Service |

### HELM installation (debugging)
````
git clone https://github.com/arextest/deployments.git
cd deployments/helm
helm install . -n namespace
````

### Service deployment and non-containerized installation
For separate deployment and non-containerized installation of services, you can install services separately, but the configuration of calls between services needs to be configured by yourself
common scenarios
* Use the user's future MongoDB service
* Use the user's own distributed environment to expand and shrink the capacity of Schedule, Storage and other high CPU services
For details on calling parameter configuration, please refer to the following documents

## Configurable parameters for each service

### Example of configuring environment variables in the operating system

#### Windows
````
set JAVA_OPTS=-Darex.config.service.url=http://10.3.1.3:8080 -Darex.storage.service.url=http://10.3.1.4:8080 -Darex.storage.mongo.host=mongodb: //username:password@my-mongodb:27017/arex_storage_db -Darex.report.email.host=smtp.msn.com
set JAVA_OPTS View configuration
````
#### Linux
````
export JAVA_OPTS=-Darex.config.service.url=http://10.3.1.3:8080 -Darex.storage.service.url=http://10.3.1.4:8080 -Darex.storage.mongo.host=mongodb: //username:password@my-mongodb:27017/arex_storage_db -Darex.report.email.host=smtp.msn.com
echo $SERVICE_REPORT_URL View configuration
````

### Configuration of AREX UI
Contains 3 environment variables, corresponding to the service address of the Report, the configuration service address and the scheduling service address

#### docker compose configuration example
The following is the default configuration
Note that the port is using port 8080 inside the docker-compose network
````
environment:
  - SERVICE_REPORT_URL=http://arex-report-service:8080
  - SERVICE_CONFIG_URL= http://arex-config-service:8080
  - SERVICE_SCHEDULE_URL=http://arex-schedule-service:8080
````
#### Configuration example for separate deployment
````
##windows
set SERVICE_REPORT_URL=http://10.192.1.1:8080 set
##linux
export SERVICE_REPORT_URL=http://10.192.1.1:8080
````

### AREX Config service configuration
#### Configuration in source code
````
mongo.uri=mongodb://arex:iLoveArex@ip:27017/arex_storage_db
spring.datasource.username=arex_admin
spring.datasource.password=arex_admin_password
````
#### Default configuration
````
    environment:
      - JAVA_OPTS=-Dmongo.uri=mongodb://arex:iLoveArex@mongodb:27017/arex_storage_db
````
#### Configuration example for separate deployment
````
      - JAVA_OPTS=-Dmongo.uri=mongodb://arex-m-name:arex-m-passwd@your-mongodb-address:27017/arex_storage_db


````

### AREX Schedule service configuration
#### Source configuration
````
arex.storage.service.api=http://arex-storage-service:8080
arex.report.service.api=http://arex-report-service:8080
arex.config.service.api=http://arex-config-service:8080

````
#### Configuration example for separate deployment
````
## eg. 10.3.1.1 Storage Services 10.3.1.2 Report Services
environment:
- "JAVA_OPTS=-Darex.storage.service.api=http://10.3.1.1:8080 -Darex.report.service.api=http://10.3.1.2:8080 "
````

### AREX Storage Service Configuration
#### Source configuration
````
arex.storage.cache.expired.seconds=7200
arex.storage.cache.provider.host=
mongo.host=mongodb://arex:iLoveArex@mongodb:27017/arex_storage_db
````
#### Configuration example for separate deployment
````
environment:
  - "JAVA_OPTS=-Darex.storage.service.api=http://arex-storage-service:8080 -Darex.report.service.api=http://arex-report-service:8080 -Darex.config.service .api=http://arex-config-service:8080"
````

### AREX Report Service Configuration
#### Source configuration
````
arex.report.mongo.uri=mongodb://arex:iLoveArex@ip:27017/arex_storage_db
arex.config.service.url=http://arex-config-service:8080
arex.storage.service.url=http://arex-storage-service:8080
arex.ui.url=http://your_arex_ip_address:port


````
#### Configuration example for separate deployment
````
environment:
- "JAVA_OPTS=-Darex.config.service.url=http://10.3.1.3:8080 -Darex.storage.service.url=http://10.3.1.4:8080 -Darex.storage.mongo.host=mongodb ://username:password@my-mongodb:27017/arex_storage_db -Darex.report.email.host=smtp.msn.com -Darex.ui.url=http://your_arex_ip_address:port"
````
## MongoDB configuration
* Delete the related configuration of Mongodb in docker-compose yaml,
* MongoDB has no initial configuration, it depends on the service of mongodb, and its parameters are configured according to your own actual service