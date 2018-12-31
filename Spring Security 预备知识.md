

# 第三章：使用Spring MVC开发RESTful API

## 3-1 、Restful简介

|    | 普通接口 | REST接口 |
| ---------- | -------- | ---------- |
| 查询 | /user/query?name=tom     GET | /user?name=tom            GET |
| 详情 | /user/getInfo?id=1             GET | /user/1                            GET |
| 创建 | /user/create?name=tom    POST | /user                               POST |
| 修改 | /user/update?id=1&name=jerry  POST | /user/1                            PUT |
| 删除 | /user/delete?id=1              GET | /user/1                             DELETE |

早传统的接口中：

1. 在URL中描述行为和资源。
2. HTTP请求方法大多数用POST、GET。而POST和GET只是用来传递多或较少的信息
3. 成功或者失败返回同种状态码，不同的HTTP报文的请求体中，可能是错误信息也可能是正确信息
4. 数据交互在URL或者POST方式的请求体

而在Restful中：

1. 用URL描述资源，比如“1”能表示用户id为1的资源
2. 使用HTTP方法描述行为。使用多种请求方法，描述对资源的行为。POST创建，GET查询，PUT改，DELETE删除
3. 使用HTTP状态码来表示不同的结果，而不是在HTTP报文的请求体包含的内容显示是失败还是成功的
4. 请求体内，使用JSON交互数据。
5. RESTful只是一种风格，并不是强制的标准

REST成熟度模型

![REST成熟度模型](C:\Users\Administrator\Desktop\权限框架论\img\REST成熟度模型.png)

## 3-2、SpringMVC开发RESTful API 常用注解

```
@RestController 标明此Controller提供RestAPI
@RequestMapping 及其变体。映射http请求url到java方法
@RequestParam 映射请求参数到java方法的参数
@PageableDefault 指定分页参数默认值
@RequestParam 绑定单个请求参数值
@PathVariable 映射url片段到java方法的参数，在url声明中使用正则表达式
@JsonView 控制json输出内容
@RequestBody 映射请求体到java方法的参数 日期类型参数的处理
@Valid 注解和BindingResult请求参数的合法性把那个处理校验结果
```

@JsonView使用步骤

1. 	使用接口来声明多个视图
	. 	在值对象的get方法上指定视图
	. 	在Controller方法上使用地图

## 3-3、Hibernate Valiator 



校验过程

校验常用注解

| 注解                                               | 描述                                                         |
| -------------------------------------------------- | ------------------------------------------------------------ |
| @Null                                              | 被注释的元素必须为 null                                      |
| @NotNull                                           | 被注释的元素必须不为 null                                    |
| @Pattern(value)                                    | 被注释的元素必须符合指定的正则表达式                         |
| @Size(max, min)                                    | 被注释的元素的大小必须在指定的范围内                         |
| @CreditCardNumber                                  | 被注释的字符串必须通过Luhn校验算法， 银行卡，信用卡等号码一般都用Luhn 计算合法性 |
| @Email                                             | 被注释的元素必须是电子邮箱地址                               |
| @Length(min=, max=)                                | 被注释的字符串的大小必须在指定的范围内                       |
| @NotBlank(message =)                               | 验证字符串非null，且长度必须大于0                            |
| @NotEmpty                                          | 被注释的字符串的必须非空，集合有元素                         |
| @Range(min=, max=)                                 | 被注释的元素必须在合适的范围内                               |
| @SafeHtml (whitelistType=, additionalTags=)        | classpath中要有jsoup包，字符串是安全的html                   |
| @URL(protocol=, host=,    port=,  regexp=, flags=) | 被注释的字符串必须是一个合法的url                            |
| @AssertTrue                                        | 被注释的元素必须为 true                                      |
| @AssertFalse                                       | 被注释的元素必须为 false                                     |
| @DecimalMin(value)                                 | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值     |
| @DecimalMax(value)                                 | 被注释的元素必须是一个数字，其值必须小于等于指定的最大值     |
| @Digits (integer, fraction)                        | 被注释的元素必须是一个数字，其值必须在可接受的范围内         |
| @Future                                            | 被注释的元素必须是一个将来的日期                             |
| @Past                                              | 被注释的元素必须是一个过去的日期                             |
| @Max(value)                                        | 被注释的元素必须是一个数字，其值必须小于等于指定的最大值     |
| @Min(value)                                        | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值     |

自定义消息：每一个注解定义一个message，即可自定义校验消息

自定义校验注解：



## 3-4、服务异常处理

### 	Spring Boot中默认的错误处理机制

 	浏览器发送的请求，返回的是html；客户端发送的，返回的是json。这是SpringBoot，根据是不是浏览器发送的，响应不同的格式的响应体

```
BasicErrorController 中有@RequestMapping(produces = "text/html")
请求头中Content-Type: text/html是否存在，如果存在执行这个注解的方法返回HTML格式，否则执行其他方法，返回其他格式
```

	自定义异常处理：

```
	如果前端传过来的请求不合法，springboot默认把这些错误信息收集起来，拼成errors信息返回给前台，前台根据这些信息可以明确的告诉用户请求哪些字段有问题，这事springboot默认对400和不合法的异常情况的处理。 
	但是一般请求进入到我们的方法里面来，被spring框架挡回去了，返回错误信息
	在开发中比较常见的是请求进入到controller方法中，然后调用服务层的一些服务，由这些服务来抛出这些异常，默认的返回json对象中message包含了异常信息
	
```

## 3-5、RESTful API 的拦截

- 	过滤器（Filter）
  - 可以拿到原始HTTP请求和响应的信息，但是拿不到 真正处理请求方法的信息
	 	拦截器（Interceptor）
  - 可以拿到原始HTTP请求和响应，也能拿到真正处理请求的方法信息，但是拿不到处理请求方法被调用的时候，那方法参数的值
	 	切片（Aspect）
  - 可以拿到被调用方法的参数值，却拿不到原始HTTP请求和响应

## 3-6、文件上传与下载





## 3-7、使用多线程提高REST服务

- 	使用Runnable异步处理Rest服务
	 	使用DeferredResult异步处理Rest服务
	 	异步处理配置

线程复用吗？

## 3-8、与前端开发并行工作

	使用swagger自动生成html文档
		http://localhost:8060/swagger-ui.html
	使用WireMock快速伪造RESTful服务



	