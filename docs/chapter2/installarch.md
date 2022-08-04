
### Install AREX's Services
```
git clone https://github.com/arextest/deployments.git
cd deployments
docker-compose up -d
```

### Installation AREX's Agent

   In the Java, there are two ways to install AREX's agent:
   1. Use the JVM parameter `-javaagent`, the example is as follows:
      ```jvm
      java -javaagent:/path/to/agent.jar -jar myapp.jar
      ```
   1. Write an application yourself using VirtualMachine's attach API. For more information, you can find online examples.
   
   Note that there are additional parameters in our AREX's Agent required:`arex.service.name` and `arex.storge.model`.
   
   More information on installation can be found [here](https://github.com/arextest/arex-agent-java#installation).

### Extension by yourself

  In the Java, we use [Service Provider Interface](https://docs.oracle.com/javase/tutorial/ext/basics/spi.html) mechanism to load services dynamically. 
  
  You can find service contracts in our components listed above.
  
### A live demo

There is a [live demo](https://github.com/arextest/arex) created from [Spring-Petclinic](https://github.com/spring-projects/spring-petclinic).
