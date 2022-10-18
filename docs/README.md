# <img src="https://avatars.githubusercontent.com/u/103105168?s=200&v=4" alt="Arex Icon" width="27" height=""/>AREX

**Real automated API testing with real data.**

## 解决痛点

1. 降低接口测试成本
2. 提高接口测试覆盖率

**接口测试**是软件测试的一种，它包括两种测试类型：狭义上指的是直接针对应用程序接口（缩写 API 测试）的功能进行的测试；广义上指集成测试中，通过调用 API 测试整体的功能完成度、可靠性、安全性与性能等指标. 接口测试的活动包括,测试设计, 测试用例编写, 测试脚本编写,测试数据构造,测试执行, 更进一步的是,在持续开发维护过程中,要对此四种活动输出物进行持续的检查和维护.

**AREX**可以最大程度的降低测试活动的实施成本,提高测试覆盖率, 提升质量, 释放更多开发工程师和测试工程师的时间.

## 版本更新

- v0.2.4 版本 2022/10/18

1. 修复 BUG
   - 修复 GET 请求问号后面的参数和 Params 中的参数在请求的时候会重复拼接
   - 卡片详情展示中针对 invalidCase 做特殊展示
   - Schedule 中所有 dynamicClass 的类都不进行对比
2. 新增功能
   - MOCK 数据,增加增删改查接口
   - Arex-Config 项目合并到 Arex-Report 模块中, Agent 和 Storage 模块都已经修改配置
   - AREX 用例固化功能
3. 计划 10/31 日版本(0.2.5)的主要目标
   - 测试用例的导入导出功能(接口已经完成,前端下个版本提供)
   - AREX 固化用例自动添加 URL
4. 已知问题
   - 接口测试时,数据量过大页面无法加载
   - Replay 中录制回放的配置参数未生效
   - Record 中录制回放的配置参数未生效
   - Assert 脚本功能存在 Bug

- v0.2.3 版本 2022/09/28

1. 修复 BUG
   - Chrome 插件支持用户自定义 cookie,请下载最新插件([链接](https://github.com/arextest/arex-chrome-extension/releases))
   - 修复回放 Rerun 不生效的错误
   - 页面刷新后会根据路由重新定位到当前页，不再显示初始化的空白页面
   - 修复了点击刷新页面偶尔会报错的错误。
   - 修复了设置页面中点击最右侧快速菜单栏而导致整个 tab 消失的错误。
   - 修复了 Get 请求参数拼接两次的问题
2. 新增功能
   - 增加插件版本升级提醒(e.g. Chrome Extension version is incorrect, please install1.0.4)
   - 支持请求参数中可以使用 Environment 变量功能
   - 支持 Environment 克隆功能
   - 增加了回放 Case 列表页的分页功能。
   - 优化对比详情卡片的文字描述。
3. 计划 10/17 日版本(0.2.4)的主要目标  
     \* 修复 0.2.3 的 BUG
   - 增加 AREX 录制回放测试用例抽取(接口已经完成/Report, 前端开发中)
   - 优化比对测试的结果展示界面(推迟)
   - config 模块和 report 模块合并到 arex-api 模块中
   - 前端增加单个比对用例错误展示页
4. 已知问题
   - 接口测试时,数据量过大页面无法加载
   - 脚本 ASSERT 功能存在 BUG
   - Replay 中录制回放的配置参数未生效
   - 回放配置项的"Case Range"缺省为 1,修改此配置,功能未生效.

- v0.2.2 版本 2022/09/09

1. Deployment 仓库中 Docker-compose 中版本设置为 latest,不再写指定版本号  
   1.1 本次生成的 docker 版本为 0.2.2, 已经更新 Latest Tag,都已上传 Docker HUB  
   1.2 demo.arextest.com 已同步更新最新版本(arex-aws-demo 可以执行回放)
2. 修复 0.2.1 中的 BUG  
   2.1 前端界面优化  
   2.2 Arex Agent 版本已经升级,用户需更新 AREX-Agent
3. 增加功能  
   3.1 前端的 Record 配置,Replay 配置等功能  
   3.2 增加 Sidecar 配置 demo(sidecar 目录)  
   3.3 Helm 配置(arex-chart 目录)  
   需要用户自行修改 storageClass  
   安装命令 helm install your-demo-name ./arex-chart --namespace yourNamespace
4. 计划 9/26 日版本(0.2.3)的主要目标  
     a. 修复 0.2.2 的 BUG  
    b. 增加 AREX 录制回放测试用例抽取(接口已经完成/Report, 前端开发中)  
    c. 优化比对测试的结果展示界面(推迟)  
    d. 模板管理基本功能
5. 已知问题
   1. AREX 回放测试中,单用例 Rerun 失败
   2. 常规测试用例中自定义 cookie 设置未生效(浏览器插件)

- v0.2.1 2022/08/29

  1.  去 MySQL: Config 服务,schedule 服务去 mysql,改为读写 mongodb
  2.  生成的 docker 版本 tag 统一为 0.2.1
  3.  Arex-agent 升级,版本升级为 0.1.0
  4.  登录时邮箱注册码功能升级(不需要用户感知和配置 smtp 服务器,arex 统一提供服务)
  5.  发布分支: arex 项目发布的分支是 rel/1.0.1,其他的都是 main 分支
  6.  主要修复 bug
      a. 回放提示录制 action 为空的问题
      b. tomcat 下录制回放失败问题
  7.  在 9/2 日计划中,延时发布的功能
      a. 完成 Helm 配置(调试未完成)
      b. 优化比对测试的结果展示界面(推迟)
      c. 增加 AREX 录制回放测试用例抽取(接口已经完成/Report, 前端开发中)
  8.  原计划 9/2 日的版本不再发布,计划 9/12 日(间隔两周)发布新版本 0.2.2
  9.  9/12 日版本 0.2.2 主要目标
      a. 前端的录制配置,回放配置等功能发布
      b. 修复 BUG

- v0.2 2022/08/19

  - UI 全新改版
  - 增加常规测试,比对测试
  - 修改各个服务的参数读取方式,支持用户配置接口地址
  - 前端镜像名称从 replay-front-end 改为 arex, 服务名也改为 arex
  - 镜像版本升级为 0.2
  - 已知问题:版本不兼容(AREX 去 MYSQL 未完成,当后续升级后(无 mysql),历史"录制回放记录"会丢失)

- v0.1 2022/03/16 初始化版本
