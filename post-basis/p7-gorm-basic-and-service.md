# 第七章：初识GROM使用GORM进行数据库操作

ORM（对象关系一行社）可以在关系型数据库与对象之间建立映射，可以想操纵对象一样操作数据库。而在Go中，我所指的有Gorm与Beego ORM两种ORM库用一操作数据库。而本Book中选择Gorm。

### 为什么选择Gorm

Gorm是Go语言中的一个性能较好的ORM库。对开发人员友好，功能强大，调用方便。许多大厂都在使用[1]。

此外还有一些特点：

* 支持关联（Has one Has Many….)
* 支持预加载
* 支持Hook（增删改查前后进行操作）
* 支持事务处理。（很重要）



### Gorm的安装

```
go get -u github.com/gorm.io/gorm
```





## Gorm的具体使用

### 数据库连接

由于Mysql的普遍性，此处以Mysql为例进行数据库连接

首先预定义好特定的字符串，使用如下代码即可连接，在`Gin-demo` 中，使用读取yml文件的方式，来避免敏感信息泄露。

```go
dsn := fmt.Sprintf(`%s:%s@tcp(%s)/%s?charset=utf8&parseTime=%t&loc=%s`,
		username,
		password,
		host,
		database,
		true,
		"Local")
	db, err := gorm.Open(mysql.Open(dsn), &gorm.Config{})
if err != nil {
		panic("failed to connect database")
	}

```



在实际使用数据库中，大致可以归类为一下六种操作[3]

1. **数据库表结构迁移**

   ```go
   	// 1. Auto migration for given models
   	db.AutoMigrate(&Product{})
   ```

   TODO

2. **插入数据**

   ```go
   // 2. Insert the value into database
   	if err := db.Create(&Product{Code: "D42", Price: 100}).Error; err != nil {
   		log.Fatalf("Create error: %v", err)
   	}
   	PrintProducts(db)
   ```

   

3. **获取某个符合条件的数据**

   ```go
   // 3. Find first record that match given conditions
   	product := &Product{}
   	if err := db.Where("code= ?", "D42").First(&product).Error; err != nil {
   		log.Fatalf("Get product error: %v", err)
   	}
   ```

   

4. **更新数据表项**

   ```go
   // 4. Update value in database, if the value doesn't have primary key, will insert it
   	product.Price = 200
   	if err := db.Save(product).Error; err != nil {
   		log.Fatalf("Update product error: %v", err)
   	}
   	PrintProducts(db)
   ```

   

5. **删除某表项**

   ```go
   // 5. Delete value match given conditions
   	if err := db.Where("code = ?", "D42").Delete(&Product{}).Error; err != nil {
   		log.Fatalf("Delete product error: %v", err)
   	}
   	PrintProducts(db)
   ```

   

6. **获取一系列表记录**

   ```go
   // List products
   func PrintProducts(db *gorm.DB) {
   	products := make([]*Product, 0)
   	var count int64
   	d := db.Where("code like ?", "%D%").Offset(0).Limit(2).Order("id desc").Find(&products).Offset(-1).Limit(-1).Count(&count)
   	if d.Error != nil {
   		log.Fatalf("List products error: %v", d.Error)
   	}
   
   	log.Printf("totalcount: %d", count)
   	for _, product := range products {
   		log.Printf("\tcode: %s, price: %d\n", product.Code, product.Price)
   	}
   }
   ```

   

​		

## 高级查询



TODO



## 原生SQL查询



## Hook

TODO



# 参考

1. 极客时间：Go项目开发实战：30
2. [Gorm](https://github.com/go-gorm/gorm)
3. [六种操作源码](https://github.com/marmotedu/gopractise-demo/blob/master/gorm/main.go)
