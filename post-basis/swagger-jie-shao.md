# Swagger 介绍



### swagger 是什么

​	Swagger 是一个规范且完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务。

​	Swagger 的目标是对 REST API 定义一个标准且和语言无关的接口，可以让人和计算机拥有无须访问源码、文档或网络流量监测就可以发现和理解服务的能力。当通过 Swagger 进行正确定义，用户可以理解远程服务并使用最少实现逻辑与远程服务进行交互。与为底层编程所实现的接口类似，Swagger 消除了调用服务时可能会有的猜测。

​	直观来讲，后端可依次来维护接口文档，相较于word维护省时省力。并方便实施测试。

#### Swagger 的优势

- 支持 API 自动生成同步的在线文档：使用 Swagger 后可以直接通过代码生成文档，不再需要自己手动编写接口文档了，对程序员来说非常方便，可以节约写文档的时间去学习新技术。
- 提供 Web 页面在线测试 API：光有文档还不够，Swagger 生成的文档还支持在线测试。参数和格式都定好了，直接在界面上输入参数对应的值即可在线测试接口。

### 安装

在此之前仍要确保将GOROOT、GOPATH加入到环境变量。并默认使用的是Go的最新版

官方网址： [swag](https://github.com/swaggo/swag)

```go
 go install github.com/swaggo/swag/cmd/swag@latest
```



### 命令使用

`swag init` 生成swag文档，不加参数的话自动生成到 `./docs` 中，其中的内容全部由swag生成。

`swag fmt` 使用`fmt`格式化 SWAG 注释。(请先升级到最新版本)



主要使用以上两个就可以了，更具体使用也可参见官方中文[README](https://github.com/swaggo/swag/blob/master/README_zh-CN.md)



### 具体使用

由于在`initialize/router`中导入swagger 并注册了一下路由后，运行gin-demo 即可在`127.0.0.1:8081/swagger` 访问到swagger文档了。

```go
import （ginSwagger "github.com/swaggo/gin-swagger"
	_ "github.com/Super-BUAA-2021/Gin-demo/docs"） // 重要
r.GET("/swagger/*any", ginSwagger.WrapHandler(swaggerFiles.Handler))
```

当然在需要添加swagger的api接口上方添加指定格式的注释。再使用`swag init` 生成文档重新运行即可。

下面给出一个API接口代码及其上方的Swagger注释

```go
// Login
// @Summary      用户登录
// @Description  根据用户邮箱和密码等生成token，并将token返回给用户
// @Tags         登录模块
// @Accept       json
// @Produce      json
// @Param        data  body     model.LoginQ  true  "用户名，密码"
// @Success      200       {string}  string  "{"success": true, "message": "登录成功", "data": "model.User的所有信息"}"
// @Router       /api/v1/login [post]
func Login(c *gin.Context) {
	// var user model.User
	// var notFound bool
	var data model.LoginQ
	if err := c.ShouldBindJSON(&data); err != nil {
		panic(err)
	}

	c.JSON(http.StatusOK, gin.H{
		"success": true,
		"message": "登录成功",
		"data":    "username:" + data.Username + ",password:" + data.Password,
	})
}

```

此处主要解释下Param中的`model.LoginQ`  为前端发送参数的结合体。其内容为，在对参数进行bind验证（下节细说）后，便可直接调用使用了，也初步验证了参数的类型、数量是否合法。

```go
type LoginQ struct {
	Username string `json:"username"`
	Password string `json:"password"`
}
```

PS 还有两种常见方式可自行使用,第一种query主要用于前端在url上添加参数使用。后一种FormData主要用于前端传输文件至后端。更多还请参见官方文档

```go
// @Param        id   query  int     true  "Account ID"
id := c.Request.URL.Query().Get("id")

// @Param        code     formData  file                        true  "代码文件"
```

### 效果



![image-20220420150743106](img/swagger-jie-shao/image-20220420150743106.png)

前端或后端可直接通过此界面来想指定位置发送请求以测试。

![image-20220420150928110](img/swagger-jie-shao/image-20220420150928110.png)

运行请求结果

![image-20220420150812736](img/swagger-jie-shao/image-20220420150812736.png)
