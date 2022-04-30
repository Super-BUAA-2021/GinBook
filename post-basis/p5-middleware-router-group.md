# 第五章 路由分组与中间件

本章节会简单介绍一些路由分组，以及一些中间件的使用。主要则是在路由分组中，使用中间件对路由进行简单划分。后续有时间会补充一些其他的中间件。

关于路由分组，参见大多数的写法，我将之放在`router/router.go` 中，首先在`main.go` 中，有着如下代码创建了最基本的路由

```go
// 创建Router
	r := gin.New()
	initialize.InitRouter(r)
```

随后在具体的`InitRouter` 中，大体结构便如下所示。 首先进行跨域配置，此处简单只是进行了默认的跨域配置。

并用`r.Use(cors.New(config))` 来应用。 也即`r`的所有路由都遵守这个规则。

```go
func InitRouter(r *gin.Engine) {
	//跨域配置
	config := cors.DefaultConfig()
	config.AllowAllOrigins = true
	config.AllowHeaders = append(config.AllowHeaders, "x-token")
	r.Use(cors.New(config))

	r.GET("/swagger/*any", ginSwagger.WrapHandler(swaggerFiles.Handler))

	r.GET("/hello", HelloGin)
	// 禁用代理访问
	if err := r.SetTrustedProxies(nil); err != nil {
		global.LOG.Panic("初始化失败：禁止使用代理访问失败")
	}

	// 登录模块
	rawRouter := r.Group("/api/v1")
	{
		rawRouter.POST("/login", v1.Login)
	}

}
```

而路由分组则是如下，根据路由规则以及自己的喜好设置路由路径。 其中POST便是代表使用POST方法来访问该路由时，调用相应的API。

```go
// 登录模块
	rawRouter := r.Group("/api/v1")
	{
		rawRouter.POST("/login", v1.Login)
	}
```



而中间件呢，此处首先使用自行定义的验证模块，`Auth` 其作用为当前端发来请求时，检测请求中的`x-tokens`，若违规或是确实该关键词便直接返回用户未登录的信息。 

```go
basicRouter := rawRouter.Group("/")
	basicRouter.Use(middleware.AuthRequired())
```

其中`middleware.AuthRequired()` 如下。大体内容则是先验证token的存在合理性。随后到数据库中检查是否存在该用户。

```go
func AuthRequired() gin.HandlerFunc {
	return func(c *gin.Context) {
		token := c.GetHeader("x-token")
		id, err := utils.ValidateToken(token)
		if err != nil {
			c.JSON(http.StatusOK, gin.H{"success": false, "message": "用户校验失败"})
			c.Abort()
			return
		}
		if user, notFound := service.GetUserByID(id); notFound {
			c.JSON(http.StatusOK, gin.H{"success": false, "message": "用户不存在"})
			c.Abort()
		} else {
			c.Set("user", user)
		}
	}
}
```

由此，在`	basicRouter` 路由组下设置的路由，均要通过**用户验证**后方可访问。不过用户验证的API接口`utils.ValidateToken(token)`  的具体使用，还需要在讲解jwt的使用中细说。不过原理很简单，便是对用户的个人信息，例如`uid` 或是用户名等其他信息进行加密，随后后端再进行解密出token中的信息。当然加密时应包含一段代码中不易猜得的独立信息随之加密。否则便易于破解。 具体当`Gin-demo` 完结时可参见jwt部分。



<script src="https://utteranc.es/client.js"
        repo="Super-BUAA-2021/GinBook"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>