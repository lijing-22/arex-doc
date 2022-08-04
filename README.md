# <img src="https://avatars.githubusercontent.com/u/103105168?s=200&v=4" alt="Arex Icon" width="27" height=""/>AREX  
**Real automated API testing with real data.**   

**Relieve the pain points**:  
	1. Reduce the cost of the API testing  
	2. Improve interface test coverage  
	
**The Application Programming Interface testing** is a type of software testing, which includes two types of testing: in a narrow sense, it refers to testing directly against the functions of the application program interface (abbreviated as API testing); Indicators such as functional completion, reliability, security and performance. The activities of interface testing include, test design, test case writing, test script writing, test data construction, test execution, and further, in the process of continuous development and maintenance, Ongoing inspection and maintenance of these four active outputs is to be carried out.  
**AREX it can** minimize the implementation cost of test activities, improve test coverage, improve quality, and release more development engineers and test engineers' time.  

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

## How to getting started

### Prerequisites

- Check if your JDK version supports it, we at least support JDK8 and above.
- List components you use in your project, which components are supported by the [AREX's Agent](https://github.com/arextest/arex-agent-java).
    If not you can extend it.

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


# API Testing  
API Testing Introduction

## 发送请求

	使用、auth、cookies、variables、response


## 接口CASE

	生成、保存、编辑、前置、断言、Test （refer？？？）、执行（with Mock or Not）、批量执行、Collections


## 环境变量/全局变量/临时变量


# Compare Testing  
compare Testing introduce

## Build compare testing

## Case Management

# Replay real data Testing

## 执行回放  
todo  
## 查看回放记录  
todo 

## 查看Report  
todo 

## 固化CASE
todo  

# API管理? 模板管理

## Record配置
## Replay配置

## 比对配置模板 Comparing diff template

## 模版管理, Global, Application, Temp template

# Script  （可参考ApiFox、Postman）

##  介绍

##  参考

## 前置
todo  
##  TEST
todo  

## 断言
todo  


# 贡献指南

# 鸣谢

# Contributing

  Welcome to join us, you can change the source code to build what you want by yourself.
  1. Fork it
  1. Create your feature branch
  1. Commit your code changes and push to your feature branch
  1. Create a new Pull Request
 
# License
- Code: [Apache-2.0](https://github.com/arextest/arex-agent-java/blob/main/LICENSE)
