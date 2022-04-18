# 第二章 gin的请求以及获取参数的方式



​	上一节HelloGin中说到，注册路由后，来选择对应调用的方法。本小节主要介绍如何来接收并简单处理前端所送来的请求的。

以一简单的登录处理程序为例：

```go
func Login(c *gin.Context) {
	username := c.Request.FormValue("username")
	password := c.Request.FormValue("password")
    
    // 处理登录信息

	c.JSON(http.StatusOK, gin.H{
		"success": true,
		"message": "登录成功",
        "code": 200 ,
		"data":    "username:" + username + "password" + password,
	})
}

```

首先`c *gin.Context` 表示上下文信息，也就是其中包含了前端发给我们的数据。此处使用`c.Request.FormValue("username")` 来获取前端以username为key的内容并转为字符串形式。 此处便可应对大多情况，后端可能还会按需将字符串格式化为其他形式例如数组、字典等。处理完成后以JSON形式返回信息给前端。



此外，Gin还可接收路由中的特定信息，例如对下面这条路由信息时

```go
userRouter.GET("/:id", v1.GetUser)
```

可对应使用来获取指定的id

```go
id := c.Param("id")
```



只用这种方式大致可以处理绝大多数情况了。下一小节将会介绍通过bind绑定参数来使得一行接收到所有的信息并进行简单验证。但是一次只能接收到一个参数（纯压行也不是一个好办法），还需要额外的对参数进行验证。若只是这么粗暴处理往往会导致api接口函数过于冗长。

~~否则便可能会像我一样曾经在api接口函数中写下了80行接收并验证参数的蠢事。。~~
