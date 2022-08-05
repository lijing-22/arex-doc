## AREX Agent部署

***配置-javaagent使agent动态注入到jvm,来运行AREX Agent***
  
AREX agent运行依赖AREX配置服务([AREX config service](https://github.com/arextest/arex-config))和AREX存储服务([AREX storage service](https://github.com/arextest/arex-storage)).

### 配置java参数运行模式
调整修改两个依赖服务的主机和端口, 模板如下
* arex-agent-<version>.jar 是AREX提供或者自行编译的jar包名称,注意修改路径
* your-service-name 被测试服务的名称,不同的服务必须不同名称
* your-application.jar 你被测试服务的JAR
```other
java -javaagent:/path/to/arex-agent-<version>.jar
      -Darex.service.name=your-service-name
      -Darex.storage.service.host=[storage.service.host:port](storage.service.host:port) 
      -Darex.config.service.host=[config.service.host:port](config.service.host:port)
      -jar your-application.jar
```

### 配置文件运行模式
你可以通过配置`arex.agent.conf` 配置文件(内容同上),如

```other
arex.service.name=your-service-name  
arex.storage.service.host=<storage.service.host:port> 
arex.config.service.host=<config.service.host:port> 
```

然后运行如下命令:

```other
java -javaagent:/path/to/arex-agent-<version>.jar
      -Darex.config.path=/path/to/arex.agent.conf
      -jar your-application.jar
```

### 通过ArexCli运行的本地模式

```
git clone https://github.com/arextest/arex-agent-java.git
cd arex-agent-java
mvn clean install
```
运行命令行
```
chmod 550 bin/arex-cli.sh
cd ./bin/
./arex-cli
```

或者运行如下命令行
```other
git clone https://github.com/arextest/arex-agent-java.git
cd arex-agent-java
mvn clean install
java -cp "./arex-cli-parent/arex-cli/target/arex-cli.jar" io.arex.cli.ArexCli
```
#### 支持的命令如下:
- **record**- record data or set record rate  
    - `[option: -r/--rate]` set record rate, default value 1, record once every 60 seconds  
    - `[option: -c/--close]` shut down record  
- **replay**- replay recorded data and view differences
    - `[option: -n/--num]` replay numbers, default the latest 10  
- **watch**- view replay result and differences  
    - `[option: -r/--replayId]` replay id, multiple are separated by spaces  
- **debug**- local debugging of specific cases  
    - `[option: -r/--recordId]` record id, required Option  

![](../resource/arexcli.png)
  
在本地模式中,AREX使用[H2](https://www.h2database.com)作为本地存储保存测试数据, 不再依赖配置服务和存储服务,但你就无法在使用AREX的前端界面了.



## Agent Setup (En)

***Enable the instrumentation agent by configuring a `javaagent` flag to the JVM to run arex：***

AREX agent works along with the [AREX config service](https://github.com/arextest/arex-config) and the [AREX storage service](https://github.com/arextest/arex-storage).

You could just configure the host and port of them respectively, like below

```other
java -javaagent:/path/to/arex-agent-<version>.jar
      -Darex.service.name=your-service-name
      -Darex.storage.service.host=[storage.service.host:port](storage.service.host:port) 
      -Darex.config.service.host=[config.service.host:port](config.service.host:port)
      -jar your-application.jar
```


Alternatively, you can put those configuration item in `arex.agent.conf` file, like below

```other
arex.service.name=your-service-name  
arex.storage.service.host=<storage.service.host:port> 
arex.config.service.host=<config.service.host:port> 
```


Then simply run:

```other
java -javaagent:/path/to/arex-agent-<version>.jar
      -Darex.config.path=/path/to/arex.agent.conf
      -jar your-application.jar
```


***Also, You can Run with CLI in local mode:***

Simply click the script in the `arex-agent-java/bin` directory to start the command line tool, or run it by following `java` command:

```other
java -cp "/path/to/arex-cli-parent/arex-cli/target/arex-cli.jar" io.arex.cli.ArexCli
```


The supported commands are as follows:


- **record**- record data or set record rate

	`[option: -r/--rate]` set record rate, default value 1, record once every 60 seconds

	`[option: -c/--close]` shut down record


- **replay**- replay recorded data and view differences

	`[option: -n/--num]` replay numbers, default the latest 10


- **watch**- view replay result and differences

	`[option: -r/--replayId]` replay id, multiple are separated by spaces


- **debug**- local debugging of specific cases

	`[option: -r/--recordId]` record id, required Option

In local mode, AREX uses [H2](https://www.h2database.com) as a local storage to save the recorded data for testing purpose,  Config Service and Storage Service are no longer required, But you can't use the AREX-UI in this mode either.
