## 常规测试

### 界面范例
![](../resource/normaltest1.png)

### 常规测试
接口测试基本功能,就是配置接口地址及其参数,界面参考Postman
* 配置HTTP的请求类型,GET/POST/PUT/DELETE/PATCH
* 配置params
* 配置Body
* 配置Header
* 配置参数

### 快速请求
![](../resource/c3.5.png)
* 在Collection上选择Add Request来新建新的快速请求
* Duplicate复制快速请求及其case(如果有的话)

#### 测试用例
CASE跟快速请求(Request)的关系
* CASE类似快速请求,可配置内容是一样的
* CASE包含在Request中
* CASE是继承Request的配置参数,为了方便用户测试同一个接口,只重新修改小部分参数,这样简化用户使用

#### 配置Request
![](../resource/c3.5.png)
* CASE用例名可以直接修改当前用例名称
* SAVE按钮保存当前用例
* 应答报文的结果,Stauts,Time和Size等
* 展示: JSON报文和Pretty优化JSON

#### 配置ASSERT
![](../resource/c3.6.png)
* 配置ASSERT
* Test Resuts中显示验证结果

