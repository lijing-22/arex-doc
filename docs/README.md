# <img src="https://avatars.githubusercontent.com/u/103105168?s=200&v=4" alt="Arex Icon" width="27" height=""/>AREX  
**Real automated API testing with real data.**   

## 解决痛点  
1. 降低接口测试成本
2. 提高接口测试覆盖率  

**接口测试**是软件测试的一种，它包括两种测试类型：狭义上指的是直接针对应用程序接口（缩写API测试）的功能进行的测试；广义上指集成测试中，通过调用API测试整体的功能完成度、可靠性、安全性与性能等指标. 接口测试的活动包括,测试设计, 测试用例编写, 测试脚本编写,测试数据构造,测试执行, 更进一步的是,在持续开发维护过程中,要对此四种活动输出物进行持续的检查和维护. 
  
**AREX**可以最大程度的降低测试活动的实施成本,提高测试覆盖率, 提升质量, 释放更多开发工程师和测试工程师的时间.
  
## 版本更新
* v0.2.2版本 2022/09/09  
1. Deployment仓库中Docker-compose中版本设置为latest,不再写指定版本号  
   1.1 本次生成的docker版本为 0.2.2, 已经更新Latest Tag,都已上传Docker HUB  
   1.2 demo.arextest.com 已同步更新最新版本(arex-aws-demo可以执行回放)  
2. 修复0.2.1中的BUG  
   2.1 前端界面优化界面优化  
   2.2 Arex Agent版本已经升级,用户需更新AREX-Agent  
3. 增加功能  
   3.1 前端的Record配置,Replay配置等功能  
   3.2 增加Sidecar配置demo(sidecar目录)  
   3.3 Helm配置(arex-chart目录)  
   需要用户自行修改storageClass  
   安装命令 helm install your-demo-name ./arex-chart  --namespace yourNamespace  
4. 计划9/26日版本(0.2.3)的主要目标  
   a. 修复0.2.2的BUG  
   b. 增加AREX录制回放测试用例抽取(接口已经完成/Report, 前端开发中)  
   c. 优化比对测试的结果展示界面(推迟)  
5. 已知问题  
	1. AREX回放测试中,单用例Rerun失败  
	2. 常规测试用例中自定义cookie设置未生效  

* v0.2.1 2022/08/29
	1. 去MySQL: Config服务,schedule服务去mysql,改为读写mongodb
	2. 生成的docker版本tag统一为 0.2.1
	3. Arex-agent升级,版本升级为0.1.0
	4. 登录时邮箱注册码功能升级(不需要用户感知和配置smtp服务器,arex统一提供服务)
	5. 发布分支: arex项目发布的分支是rel/1.0.1,其他的都是main分支
	6. 主要修复bug
		a. 回放提示录制action为空的问题
		b. tomcat下录制回放失败问题
	7. 在9/2日计划中,延时发布的功能
		a. 完成Helm配置(调试未完成)
		b. 优化比对测试的结果展示界面(推迟)
		c. 增加AREX录制回放测试用例抽取(接口已经完成/Report, 前端开发中)
	8. 原计划9/2日的版本不再发布,计划9/12日(间隔两周)发布新版本0.2.2
	9. 9/12日版本 0.2.2 主要目标
		a. 前端的录制配置,回放配置等功能发布
		b. 修复BUG

* v0.2 2022/08/19
    * UI全新改版
    * 增加常规测试,比对测试
    * 修改各个服务的参数读取方式,支持用户配置接口地址
    * 前端镜像名称从replay-front-end改为arex, 服务名也改为arex
    * 镜像版本升级为0.2
    * 已知问题:版本不兼容(AREX去MYSQL未完成,当后续升级后(无mysql),历史"录制回放记录"会丢失)

* v0.1 2022/03/16 初始化版本