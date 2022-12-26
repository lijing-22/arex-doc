## 前提条件

在开始使用 AREX 之前，请确保你已经安装以下应用：

Docker 和 Docker Compose。

## 安装 AREX

首先，通过 git 命令克隆 AREX 仓库：

```
git clone https://github.com/arextest/deployments.git cd deployments
```

接着通过 `docker-compose` 启动 AREX。

```
docker-compose up -d
```

安装完成后，将自动安装好包括前端、服务、数据库等在内的所有组件，每个组件只有 1 个实例，具体如下：

| ID   | Instance | Model Name                                                   | Description                                                  |
| ---- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | 1        | [Schedule Service](https://github.com/arextest/arex-replay-schedule) | A set of schedule APIs that provide replay send and retrieve all responses for comparison. |
| 2    | 1        | [Replay Report Service](https://github.com/arextest/arex-report) | A set of report APIs that provide difference summaries and show the difference result details after the responses are compared. |
| 3    | 1        | [Storage Service](https://github.com/arextest/arex-storage)  | A set of remote storage APIs that provide [Agent Hook Service](https://github.com/arextest/arex-agent-java) to save records and get responses as mocks. |
| 4    | 1        | [Front-End](https://github.com/arextest/arex-front-end)      | A visual web site that provide entry to all operations in your **AREX**. |
| 5    | 1        | MongoDB                                                      | 数据存储及配置管理数据库                                     |
| 6    | 1        | Redis                                                        | 高速回放缓存                                                 |

你可以通过在运行 Docker 的宿主机上执行 `docker-compose ps` 的命令查看各服务运行情况及端口。

```
[~ deployments]# docker-compose ps   Name          Command        State         Ports ------------------------------------------------------------------------------------------ arex-front    docker-entrypoint.sh node  ...  Up    0.0.0.0:8088->8080/tcp arex-mongodb   docker-entrypoint.sh --auth    Up    0.0.0.0:27017->27017/tcp arex-redis    docker-entrypoint.sh --app ...  Up    0.0.0.0:6379->6379/tcp arex-report   catalina.sh run          Up    0.0.0.0:8090->8080/tcp arex-schedule  catalina.sh run          Up    0.0.0.0:8092->8080/tcp arex-storage   catalina.sh run          Up    0.0.0.0:8093→8080/tcp
```

检查所有服务日志命令

```
cd deployments docker-compose logs
```

检查 AREX 日志命令

```
docker-compose logs arex
```

检查调度服务日志命令

```
docker-compose logs arex-schedule-service
```

检查报告分析服务日志命令

```
docker-compose logs arex-report-service
```

检查存储服务日志命令

```
docker-compose logs arex-storage-service
```

## 后续操作

### 部署 AREX Agent

**AREX Agent** 是实现服务录制回放的核心组件，你可通过配置 `-javaagent` 使 agent 动态注入到 jvm，以此来运行 AREX Agent。

AREX Agent 的运行依赖 AREX 的存储服务([AREX storage service](https://github.com/arextest/arex-storage))。

首先通过 git 进行 arex-agent-java 项目的拉取以及编译：

```
git clone https://github.com/arextest/arex-agent-java.git cd arex-agent-java mvn clean install
```

编译成功后可在 arex-agent-java 文件夹得到一个名为 arex-agent-jar 的新文件夹，其中包含两个 jar 包。

之后，你可以选择以下任意一种方式部署 Agent：

#### 配置 Java 参数运行模式

通过运行如下命令修改两个依赖服务的主机和端口：

```
java -javaagent:/path/to/arex-agent-<version>.jar    -Darex.service.name=your-service-name    -Darex.storage.service.host=[storage.service.host:port](storage.service.host:port) //（此地址是 Deployments 在 Docker 环境部署后对应的 IP 及存储服务的端口号）    -jar your-application.jar
```

> 注：
> - arex-agent- .jar 是 AREX 提供或者自行编译的 jar 包名称，注意修改路径
> - your-service-name 你的被测试服务的名称，不同的服务需使用不同的名称
> - your-application.jar 你的被测试服务的 jar 包文件

#### 配置文件运行模式

你也可以通过新建一个名为：agent.conf 的配置文件来进行配置，其中内容为：

[arex.service.name](http://arex.service.name)=your-service-name 

arex.storage.service.host=<storage.service.host:port> 

然后运行如下命令完成配置：

```
java -javaagent:/path/to/arex-agent-<version>.jar     -Darex.storage.path=/path/to/arex.agent.conf     -jar your-application.jar
```

#### 修改 JAVA_OPTS 运行模式

比如运行 tomcat，可以直接修改 catalina.sh，修改 JAVA_OPTS 运行，也可以直接在环境变量中配置，以 Linux 运行为例：

```
export JAVA_OPTS=-Djavaagent:/path/to/arex-agent- .jar -Darex.storage.path=/path/to/arex.agent.conf
```

#### 通过 ArexCli 运行的本地模式

运行以下命令行：

```
chmod 550 bin/arex-cli.sh cd ./bin/ ./arex-cli
```

或者以下命令行即可通过 ArexCli 启动本地模式：

```
git clone https://github.com/arextest/arex-agent-java.git cd arex-agent-java mvn clean install java -cp "./arex-cli-parent/arex-cli/target/arex-cli.jar" io.arex.cli.ArexCli
```

运行成功：

![img](https://arextest.github.io/arex-doc/resource/arexcli.png)

支持的命令如下:

- record：录制数据或设置录制速率。

  - `[option: -r/--rate]` set record rate, default value 1, record once every 60 seconds
  - `[option: -c/--close]` shut down record

- replay：回放录制好的数据并对比差异。

  - `[option: -n/--num]` replay numbers, default the latest 10

- watch：查看回放结果及差异。

  - `[option: -r/--replayId]` replay id, multiple are separated by spaces

- debug：对特定用例进行本地调试。

  - `[option: -r/--recordId]` record id, required Option

需要注意的是，在本地模式中，AREX 使用 [H2](https://www.h2database.com/) 作为本地存储保存测试数据，不再依赖配置服务和存储服务。同时，你将无法使用 AREX 的前端界面。

### 版本升级更新

进入 docker-compose.yml 所在目录，更新前需先停止原有服务：（比如 0.2.4 版本中去除了配置服务，在配置服务未停止时更新会导致更新失败，需要用户自己手动停止）

```
cd deployments docker-compose down -v
```

注：如果不想保留原有的 Mongodb 数据或日志，请手动删除当前运行目录的 arex-data、arex-logs 目录（请慎重操作，删除后将无法回退！）

更新 deployments 仓库，重新启动 AREX：

```
git pull docker-compose up -d
```
