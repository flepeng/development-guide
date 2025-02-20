## 接口定义

接口定义规范参见 [RESTful 设计指南](/frontend_backend_separation/RESTful%20设计指南.md)


## 接口数据格式

### 使用 JSON

使用 JSON 格式传输数据，JSON 是一种可以跨平台高扩展的轻量级的数据交换格式。易于人阅读和编写，同时也易于机器解析和生成。

在 HTTP 请求头和响应头申明 `Content-Type`。返回的数据结构应该做到尽可能简单，不要过于包装。响应状态应该包含在响应头中！

**Request**

```
Accept: application/json
Content-Type: application/json;charset=UTF-8
```

**Response**

```
Content-Type: application/json;charset=UTF-8
```


### JSON 属性定义建议

1.  属性名应该是具有定义语义的有意义的名称；
2.  首字母不能使用大写(大小写友好)；
3.  使用小驼峰命名，这条由组内开发成员根据开发习惯而定，也可使用下划线方式命名；
4.  属性和字符串值必须使用双引号`""`；
5.  包含“多个结果”的字段，注意名词的复数形式的使用
    1.  当value为普通数组时，使用“名词复数”的形式命名key
    2.  当value为嵌套了JSON对象的数组时，使用名词 + list 结尾的形式来命名key
    ```
    {
      "tags": ["食物", "粤菜", "卤水"],
      "memberList": [
        {
          "uid": 111,
          "name": "张三",
          "age": 22
        },
        {
          "uid": 222,
          "name": "李四",
          "age": 27
        }
      ]
    }
    ```
6.  避免使用重复的key


### JSON 属性值准则

*   属性值必须是 Unicode 的 booleans（布尔）, 数字(numbers), 字符串(strings), 对象(objects), 数组(arrays), 或 null；
*   接口遵循“输入宽容，输出严格”原则，输出的数据结构中空字段的值一律为 `null`, 是 `null` 不是字符串 `"null"`；
*   空类型一般不会单独作为一个字段的值出现在接口中，更多时候是用来代替某个字段原本应该出现但缺失了的值。当数据为空的时候，按照目前约定俗成的做法，基本上有以下三种处理方案
    1.  接口返回 null。当一条记录不存在的时候，接口可以返回一个null值给前端，前端判断为null的时候，就知道这条数据是没有的
    2.  接口不返回该字段（目前运用比较广泛的处理方案）。前端读取到这个字段的时候会返回undefined，前端同学在需要用到这个字段的时候，可以灵活处理数据：
        ```
        //直接使用该字段的时候，如果字段不存在，可以预设一个默认值。
        const KEY = data.key || 'key';
        
        //涉及需要二次处理该字段数据的时候，则加上一个判断
        if ( 'key' in data ) {
          console.log(data.key);
        }
        ```
    3.  接口返回该字段类型的默认“空值”(也是一个运用的比较广泛的方法)。这里的“空值”是指，根据原来设定的数据类型，返回一个初始默认值，如果原来是个数组，那么“空值”就应该是[]


## Response 数据格式建议

### 响应基本数据格式

```
{
    "code": 200,
    "data": "",
    "message": "success"
}
```


### 响应实体格式

```
{
    "code": 200,
    "message": "success",
    "data": {
        "entity": {
            "id": 1,
            "name": "XXX",
            "phone": "XXX"
        }
    }
}
```

*entity: 响应返回的实体数据*

### 响应列表格式

```
{
    "code": 200,
    "message": "success",
    "data": {
        "list":[
            {
                "id": 1,
                "name": "XXX",
                "code": "XXX"
            },
            {
                "id": 2,
                "name": "XXX",
                "code": "XXX"
            }
        ]
    }
}
```

*list: 响应返回的列表数据*


### 响应分页格式

```
{
    "code": 200,
    "message":"success",
    "data": {
        "totalCount": 2, // 总记录数
        "totalPage": 1  // 总页数
        "pageNo": 1, // 当前页码
        "pageSize": 10, // 每页大小
        "list":[
            {
                "id": 1,
                "name": "XXX",
                "code": "XXX"
            },
            {
                "id": 2,
                "name": "XXX",
                "code": "XXX"
            }
        ],
    }
}
```


### 响应特殊数据格式

对于特定组件数据格式由后端统一处理后返回前端，如（echart、ztree等组件）


### 特殊内容规范

*   布尔类型：关于布尔类型，一律返回`BOOLEN`类型值
*   日期格式：关于日期类型，JSON 数据传输中一律使用字符串格式时间戳，具体日期格式因业务而定


## 身份验证以及角色分配

所有 API 接口 除了文档等特定的接口外都需要通过身份验证。支持通过登录接口使用账号密码获取，在请求接口时使用`Authorization: Bearer #{token}` 头标或者 `token` 参数的值的方式进行验证。

在前后端分离中，项目前端无法获取用户角色信息，可以根据如下设计获取角色信息以及 token:

1. 浏览器一旦加载完该项目前端页面之后，前端主动发起 `/current_username` 请求，

2. 测试环境，从 `/current_username` 返回默认数据，请求拿到用户角色信息和 token 之后，将 token 信息注入之后的所有 API 请求的 header 头里，通过服务器端身份验证，合法请求到数据。

前后端分离项目中都是通过此种方法获得用户角色信息和类型从而进行权限限制，让不同角色的人看见不同的菜单。


## reference

*   https://github.com/paddingme/Frontend-Backend-Separation/blob/master/api.md
*   https://google.aip.dev/193