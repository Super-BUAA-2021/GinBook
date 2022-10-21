# 如何移植Gin-demo

 虽说在Gin的[介绍页](https://super-buaa-2021.github.io/GinBook/post-preparation/di-yi-zhang-gin-jie-shao.html) 中提到Gin的一个轻量化框架，配置简单等。但是一路下来真正配置的还是很多。但是盛如Django在初学时好像也不需要讲究这么多文件结构等等。不过个人理解上，Django作为相对大而全的框架，你在使用时大多需要自零开始搭建架构，还需要配置很多东西。但是本例所用的Gin上，可以根据已有的项目结构进行快速移植为新项目使用。且配置大多中间件，文件结构清晰明了。此外，虽然本Ginbook讲的相对繁琐，但是包含了不少个人在学习后端一年所了解到的许多方便工具。许多部分，例如日志，jwt鉴权。如果不用后端同样可以进行。不过相对粗糙和鉴权繁琐罢了。

 下面简单介绍一些如何将Gin-demo简单移植为你的后端项目。

1. 全局搜索并替换`github.com/Super-BUAA-2021/Gin-demo` 为你想要的包名。 本例为了符合大多项目对于module的设置，以及方便其他人的使用。将module设置为此。也在使用`service`包都需要`import "github.com/Super-BUAA-2021/Gin-demo/service"` 。
2. 全局搜索demo，并替换你想要的项目名。包括但不限于：日志文件名，配置文件名
3. `main.go`更改Swagger注释更改为个人以及项目信息，并更新后端运行端口号。
4. 在配置文件中更改为你自己的数据库以及jwt-secret等信息。

相信完成上述四步，运行main便可将此移植为你的项目了



<script src="https://utteranc.es/client.js"
        repo="Super-BUAA-2021/GinBook"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>