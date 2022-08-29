# AREX服务安装
![](../resource/arch.png)

## Agent通用配置
```
JAVA_OPTS="$JAVA_OPTS -Darex.config.path=/usr/your-path-to-arexconf/arex.agent.conf"
```

## 自定义安装
### 多实例Docker-Compose安装
```
git clone https://github.com/arextest/deployments.git
cd deployments
docker-compose -f docker-compose-distribute.yml up -d

## 停止命令
docker-compose -f docker-compose-distribute.yml down -v
## PS命令
docker-compose -f docker-compose-distribute.yml ps
```

#### 缺省安装实例
* 当前的配置schedule和storage是2个实例,可以参考配置文件,配置多实例
  
| ID | Instance | Model Name | Description |  
| :----:| :----:| :----- | :----- |
| 1 | 1 | [Configuration Service](https://github.com/arextest/arex-config) | A sets of configuration APIs for the   recording and replaying functions. |  
| 2 | 2 | [Schedule Service](https://github.com/arextest/arex-replay-schedule) | A set of schedule APIs that provide replay send and  retrieve all responses for comparison. |  
| 3 | 1 | [Replay Report Service](https://github.com/arextest/arex-report)  | A set of report APIs that provide difference summaries and show the difference result details after the responses are compared. |  
| 4 | 2 | [Storage Service](https://github.com/arextest/arex-storage) | A set of remote storage APIs that  provide [Agent Hook Service](https://github.com/arextest/arex-agent-java) to save records and get responses as mocks. |  
| 5 | 1 | [Front-End](https://github.com/arextest/arex-front-end)  | A visual web site that provide entry to all operations in your **AREX**.  |  
| 6 | 1 | MongoDB | 数据存储及配置管理数据库  |  
| 7 | 1 | Redis | 高速回放缓存  |  
| 8 | 1 | Nginx | Schedule负载均衡服务  |  
| 9 | 1 | Nginx | Storage负载均衡服务  |  

### HELM安装(调试中)
```
git clone https://github.com/arextest/deployments.git
cd deployments/helm
helm install . -n namespace
```

### 服务单独部署及非容器化安装
服务单独部署及非容器化安装,你可以单独安装服务,但服务间调用的配置需要你自己配置完成  
常见的场景  
* 使用用户以后的Mongodb服务
* 使用用户自己的分布式环境,对Schedule,Storage等高CPU服务进行扩缩容  
调用参数配置详见以下文档

## 各个服务的可配置参数

### 在操作系统中配置环境变量范例

#### Windows
```
set   JAVA_OPTS=-Darex.config.service.url=http://10.3.1.3:8080 -Darex.storage.service.url=http://10.3.1.4:8080 -Darex.storage.mongo.host=mongodb://username:password@my-mongodb:27017/arex_storage_db -Darex.report.email.host=smtp.msn.com
set JAVA_OPTS 查看配置
```
#### Linux
```
export JAVA_OPTS=-Darex.config.service.url=http://10.3.1.3:8080 -Darex.storage.service.url=http://10.3.1.4:8080 -Darex.storage.mongo.host=mongodb://username:password@my-mongodb:27017/arex_storage_db -Darex.report.email.host=smtp.msn.com
echo $SERVICE_REPORT_URL 查看配置
```

### AREX UI的配置
包含3个环境变量,分别对应Report的服务地址, 配置服务地址和调度服务地址

#### docker compose配置举例  
如下是缺省配置  
注意端口使用的是docker-compose网络内部的8080端口
```
environment:
  - SERVICE_REPORT_URL=http://arex-report-service:8080
  - SERVICE_CONFIG_URL= http://arex-config-service:8080
  - SERVICE_SCHEDULE_URL=http://arex-schedule-service:8080
```
#### 单独部署的配置举例
```
##windows
set SERVICE_REPORT_URL=http://10.192.1.1:8080  设置
##linux
export SERVICE_REPORT_URL=http://10.192.1.1:8080
```

### AREX Config 服务配置
#### 源码中配置
```
mongo.uri=mongodb://arex:iLoveArex@ip:27017/arex_storage_db
spring.datasource.username=arex_admin
spring.datasource.password=arex_admin_password
```
#### 缺省配置
```
    environment:
      - JAVA_OPTS=-Dmongo.uri=mongodb://arex:iLoveArex@mongodb:27017/arex_storage_db
```
#### 单独部署的配置举例
```
      - JAVA_OPTS=-Dmongo.uri=mongodb://arex-m-name:arex-m-passwd@your-mongodb-address:27017/arex_storage_db


```

### AREX Schedule 服务配置
#### 源码配置
```
arex.storage.service.api=http://arex-storage-service:8080
arex.report.service.api=http://arex-report-service:8080
arex.config.service.api=http://arex-config-service:8080

```
#### 单独部署的配置举例
```
## eg. 10.3.1.1 Storage Services 10.3.1.2 Report Services
environment:
- "JAVA_OPTS=-Darex.storage.service.api=http://10.3.1.1:8080 -Darex.report.service.api=http://10.3.1.2:8080 "
```

### AREX Storage服务配置
#### 源码配置
```
arex.storage.cache.expired.seconds=7200
arex.storage.cache.provider.host=
mongo.host=mongodb://arex:iLoveArex@mongodb:27017/arex_storage_db
```
#### 单独部署的配置举例
```
environment:
  - "JAVA_OPTS=-Darex.storage.service.api=http://arex-storage-service:8080 -Darex.report.service.api=http://arex-report-service:8080 -Darex.config.service.api=http://arex-config-service:8080"
```

### AREX Report 服务配置
#### 源码配置
```
arex.report.mongo.uri=mongodb://arex:iLoveArex@ip:27017/arex_storage_db
arex.config.service.url=http://arex-config-service:8080
arex.storage.service.url=http://arex-storage-service:8080
arex.ui.url=http://your_arex_ip_address:port


```
#### 单独部署的配置举例
```
environment:
- "JAVA_OPTS=-Darex.config.service.url=http://10.3.1.3:8080 -Darex.storage.service.url=http://10.3.1.4:8080 -Darex.storage.mongo.host=mongodb://username:password@my-mongodb:27017/arex_storage_db -Darex.report.email.host=smtp.msn.com -Darex.ui.url=http://your_arex_ip_address:port"
```
## Mongodb配置
* 在docker-compose yaml中删除Mongodb的相关配置,
* MongoDB没有初始化的配置,依赖mongodb的服务,其参数中按照你自己的实际服务来配置

