---
id: Swagger常用注解
sidebar_position: 1
---

# Swagger 常用注解

## 1. @Api

用在 Controller 上，表示对类的说明

### 代码示例

```java
@Api(tags = "首页模块")
@RestController
public class IndexController{
  // 业务代码 .....
}
```

### 常用属性



| 注解属性 |   类型    |                  描述                   |
| :------: | :-------: | :-------------------------------------: |
|   tags   | String[ ] | 描述请求类的作用，非空时会覆盖value的值 |
|  value   |  String   |            描述请求类的作用             |

### 非常用属性

|    注解属性    | 类型             | 描述                                                         |
| :------------: | ---------------- | ------------------------------------------------------------ |
|    produces    | String           | 设置响应 MIME 类型，例：“application/json, application/xml”，默认为空 |
|    consumes    | String           | 设置请求 MIME 类型，例：“application/json, application/xml”，默认为空 |
|   protocols    | String           | 协议，例：http， https， ws， wss                            |
| authorizations | Authorization[ ] | 获取授权列表（安全声明），如果未设置，则返回一个空的授权值   |
|     hidden     | boolean          | 是否隐藏，默认为 false，配置为 true 将在文档中隐藏           |

## 2. @ApiOperation

用在 Controller 中的方法上，说明方法的用途和作用

### 代码示例

```java
@ApiOperation(value = "问好")
public String hello(String name){
  // 业务代码 ...
}
```

### 常用属性

| 注解属性 |  类型  |      描述      |
| :------: | :----: | :------------: |
|  value   | String | 方法的简要说明 |
|  notes   | String | 方法的详细说明 |

### 非常用属性

|     注解属性      |       类型       |                             描述                             |
| :---------------: | :--------------: | :----------------------------------------------------------: |
|       tags        |    String[ ]     |              方法标签，非空时会覆盖 vlaue 的值               |
|     response      |     Class<?>     |                    响应类型（即返回对象）                    |
| responseContainer |      String      |        包装响应的容器类型，有效值为 List、Set 或  Map        |
| responseReference |      String      | 指定对响应类型的引用。 指定的引用可以是本地的或远程的，将按原样使用，并将覆盖任何指定的 response() 类 |
|    httpMethod     |      String      |                          HTTP 方法                           |
|     nickname      |      String      |                 第三方工具唯一标识，默认为空                 |
|     produces      |      String      | 设置响应 MIME 类型，例：“application/json, application/xml”，默认为空 |
|     consumes      |      String      | 设置请求 MIME 类型，例：“application/json, application/xml”，默认为空 |
|     protocols     |      String      |            设求协议，例：http， https， ws， wss             |
|  authorizations   | Authorization[ ] |  获取授权列表（安全声明），如果未设置，则返回一个空的授权值  |
|      hidden       |     boolean      |      是否隐藏，默认为 false，配置为 true 将在文档中隐藏      |
|  responseHeaders  | ResponseHeader[] |                          响应头列表                          |
|       code        |       int        |                响应的 HTTP 状态代码，默认 200                |
|    extensions     |   Extension[ ]   |                       扩展属性列表数组                       |
|  ignoreJsonView   |     boolean      |      在解析操作和类型时忽略 JsonView 注释，默认为 false      |

## 3. @ApiParam

可用于方法、方法形参、类的成员变量上，一般用于请求参数上，描述参数信息。

### 代码示例

```java
// 1. 用于方法上
@ApiParam(name = "username", value = "用户名", required = true, defaultValue = "理邦")
public String hello(String username){
  // 业务代码 ...
}
// 2. 用于方法形参上
public String hello(@ApiParam(name = "username", value = "用户名")String username){
  // 业务代码 ...
}
// 3. 用于类的成员变量上
public class Student {
    @ApiParam(name = "name", value = "姓名")
    private String name;
}
```

### 常用属性

|   注解属性   |  类型   |           描述           |
| :----------: | :-----: | :----------------------: |
|     name     | String  |        参数的名称        |
|    value     | String  |      参数的简要说明      |
|   required   | boolean | 是否必传递，默认为 false |
| defaultValue | String  |          默认值          |

### 非常用属性



|     注解属性     |  类型   |                        描述                         |
| :--------------: | :-----: | :-------------------------------------------------: |
| allowableValues  | String  |                 限制参数的可接受值                  |
|      access      | String  |               允许从API文档中过滤参数               |
|  allowMultiple   | boolean |   是否可以通过多次出现来接收多个值，默认为 false    |
|      hidden      | boolean | 是否隐藏，默认为 false，配置为 true 将在文档中隐藏  |
|     example      | String  |                      参数示例                       |
|     examples     | Example |          参数示例。仅适用于 BodyParameters          |
|       type       | String  |                        类型                         |
|      format      | String  |                        格式                         |
| allowEmptyValue  | boolean |            是否允许为空值，默认为 false             |
|     readOnly     | boolean |               是否只读，默认为 false                |
| collectionFormat | String  | 添加了使用 `array` 类型覆盖 collectionFormat 的能力 |

## 4. @ApiImplicitParams 和 @ApiImplicitParam

@ApiImplicitParams 用在请求的方法上，表示一组参数说明，里面是 @ApiImplicitParam 列表。@ApiImplicitParam 属性和 @ApiParam 相同

### 代码示例

```java
    @ApiImplicitParams({
            @ApiImplicitParam(name = "username", value = "用户名"),
            @ApiImplicitParam(name = "age", value = "年龄")
    })
    public String hello(String username, Integer age) {
        // 业务代码 ...
    }
```

## 5. @ApiResponses 和 @ApiResponse

@ApiResponses 用在请求方法上，表示一组响应。@ApiResponse 用在 @ApiResponses 中，用于表示单个响应类型，一般用于表示出现异常或错误时的响应。

### 代码示例

```java
    @ApiResponses({
            @ApiResponse(code = 401, message = "没有权限"),
            @ApiResponse(code = 404, message = "页面没找到")
    })
    public String hello(String username) {
        // 业务代码 ...
    }
```



### 常用属性

| 注解属性 |  类型  |        描述        |
| :------: | :----: | :----------------: |
|   code   |  int   | 响应的 HTTP 状态码 |
| message  | String |      响应信息      |

### 非常用属性

|     注解属性      |       类型       |                             描述                             |
| :---------------: | :--------------: | :----------------------------------------------------------: |
|     response      |     Class<?>     |                    响应类型（即返回对象）                    |
|     reference     |      String      | 指定对响应类型的引用。 指定的引用可以是本地的或远程的，将按原样使用，并将覆盖任何指定的 response() 类 |
|  responseHeaders  | ResponseHeader[] |                          响应头列表                          |
| responseContainer |      String      |        包装响应的容器类型，有效值为 List、Set 或  Map        |
|     examples      |     Example      |                           响应示例                           |

## 6. @ApiModel

用在实体类上，描述实体类信息。

### 代码示例

```java
@ApiModel(value = "学生实体", description = "学生相关信息")
public class Student {}
```

### 常用属性

|  注解属性   |  类型  |    描述    |
| :---------: | :----: | :--------: |
|    value    | String |  实体名称  |
| description | String | 实体类描述 |

### 非常用属性

|   注解属性    |    类型    |                             描述                             |
| :-----------: | :--------: | :----------------------------------------------------------: |
|    parent     |  Class<?>  |                             父类                             |
| discriminator |   String   | 支持模型继承和多态，这是用作鉴别器的字段的名称，基于此字段，可以断言需要使用哪个子类型 |
|   subTypes    | Class<?>[] |                             子类                             |
|   reference   |   String   |                           引用类型                           |

## 7. ApiModelProperty

用在实体类属性上，描述属性相关信息。

### 示例代码

```java
public class Student {
    @ApiModelProperty(value = "用户名", name = "username")
    private String username;
}
```



### 常用属性

| 注解属性 |  类型   |          描述          |
| :------: | :-----: | :--------------------: |
|  value   | String  |     属性的简要描述     |
|   name   | String  |        属性名称        |
| dataType | String  |        属性类型        |
| required | boolean | 是否比传，默认为 false |
| example  | String  |          示例          |

### 非常用属性

|    注解属性     |    类型     |             描述             |
| :-------------: | :---------: | :--------------------------: |
| allowableValues |   String    |          可接受的值          |
|     access      |   String    |  允许从 API 文档中过滤属性   |
|    position     |     int     |      指定在属性中的顺序      |
|     hidden      |   boolean   |    是否隐藏，默认为 false    |
|   accessMode    | AccessMode  |        允许的访问模式        |
|    reference    |   String    |           引用类型           |
| allowEmptyValue |   boolean   | 是否允许为空值，默认为 false |
|   extensions    | Extension[] |           扩展数组           |

## 部分注解与接口文档的关系

![示意图](https://i.imgur.com/MBVZ1gg.png)





