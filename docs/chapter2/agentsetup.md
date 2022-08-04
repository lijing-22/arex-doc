### Installation AREX's Agent

   In the Java, there are two ways to install AREX's agent:
   1. Use the JVM parameter `-javaagent`, the example is as follows:
      ```jvm
      java -javaagent:/path/to/agent.jar -jar myapp.jar
      ```
   1. Write an application yourself using VirtualMachine's attach API. For more information, you can find online examples.
   
   Note that there are additional parameters in our AREX's Agent required:`arex.service.name` and `arex.storge.model`.
   
   More information on installation can be found [here](https://github.com/arextest/arex-agent-java#installation).