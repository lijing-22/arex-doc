# Getting Started

## Introduction
### What is AREX?

  AREX, short for Automatic Replay X, is a regression testing tool for software development 
  that allows you to validate APIs and quickly find out what causes differences in responses.
  
  Its core goal is to improve the quality of software development and reduce the cost of testing.
  
  It works by record from entry to all dependencies (such as remote calls and local cache calls) 
  and then restoring all dependencies on replay. Everything is done automatically.
  
  Therefore, it also absolutely supports read and write operations, 
  including synchronous, asynchronous and transactional.
  
### How is it implemented in Java?

  We use [ByteBuddy](https://bytebuddy.net) to modify the bytecode of the target class and
  inject the logic for recording and replaying to 
  build a dynamic instrumentation agent for the Java application
  which should be bootstrap or attached.

  You can find more information about our AREX's agent implementation [here](https://github.com/arextest/arex-agent-java).

### What modules does AREX consist of?

  There is a basic chart that looks like this:
  ```mermaid
  graph TD;
    A[AREX]-->B[Configuration Service];
    A-->C[Schedule Service];
    A-->D[Agent Hook Service];
    A-->E[Replay Report Service];    
    A-->H[Storage Service];
    A-->I[Front-End]
    
  ```
* [Configuration Service](https://github.com/arextest/arex-config) is a sets of configuration APIs for the
  recording and replaying functions.
  
  For example, 
  what is the frequency and period of recording, and which operations need to be recorded,
  and which use cases can be replayed, etc.
* [Schedule Service](https://github.com/arextest/arex-replay-schedule) is a set of schedule APIs 
  that provide replay send and  retrieve all responses for comparison.
* [Agent Hook Service](https://github.com/arextest/arex-agent-java) is a basic component that
  provides replaying and recording. 
  By default, we implement common technology stack usage like Http Servlet.
* [Replay Report Service](https://github.com/arextest/arex-report) is a set of report APIs that 
  provide difference summaries and show the difference result details after the responses are compared.
* [Storage Service](https://github.com/arextest/arex-storage) is a set of remote storage APIs that
  provide [Agent Hook Service](https://github.com/arextest/arex-agent-java) to save records and get responses as mocks.
  It also provide that [Schedule Service](https://github.com/arextest/arex-replay-schedule) to load user cases for replay.
* [Front-End](https://github.com/arextest/arex-front-end) is a visual web site that provide entry to all operations in your **AREX**. 
  Such as response comparison configuration, create schedules to run replay,etc.
  
  There is a sample image for you to preview as follows:
  
  ![Report Details](https://github.com/arextest/arex-front-end/raw/main/documents/screenshots/report-detail.png)
  
### What are the features?

  There are some features as follows:
  - It can be integrated and used without modifying the source code of your project.
  - Supports read and write operations whether synchronous, asynchronous, or transactional.
  - No dirty data is generated during testing and no manual data cleaning is required.
  - Regression testing no longer needs to generate test cases manually, and it can be reused.
  - Support one-click Debug to quickly reproduce online problems.
