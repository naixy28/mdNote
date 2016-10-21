# 跨域资源共享CORS

## 机制
* 对浏览器端： 一旦发现跨域，浏览器自动添加头信息（发送额外请求）
* 对服务器： 判断Request的Origin，自行配置。。

## 两种请求类型
* 分为`simple request`简单请求与`not-so-simple request`非简单请求
* 区分方法

  ```
  若是简单请求
  
    1. 请求方法属于：
        HEAD 
        GET 
        POST
        
    2. 请求头内字段不超出：
        Accept
        Accept-Language
        Content-Language
        Last-Event-ID
        Content-Type: application/x-www-form-urlencoded | multipart/form-data | text/plain   
        
        //一般类型为application/json之类的就都不是simplw request
    
  ```

### 简单请求

#### 流程
1. 浏览器发出请求时在头中加入`Origin`字段
2. 服务端收到请求，判断`Origin`是否在许可范围内
    * 若不许可，返回正常的HTTP回应
    * 若许可，响应中会增加几个头信息字段（以`Access-Control-Allow`开头）
        1. `Access-Control-Allow-Origin`   
            必须字段，等于请求时`Origin`的值或`*`
        2. `Access-Control-Allow-Credentials`  
            可选，true则包含Cookie（除此之外需要配置`withCredentials` ）
        3. `Access-Control-Expose-Headers`  
            可选，指定了可以额外加入请求头中的字段名（除Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma之外）

#### `withCredentials`属性
* 与`Access-Control-Allow-Credentials=true`搭配使用（服务端），在AJAX请求中设置为true（浏览器）
* **//重要// 若要发送Cookie,`Access-Control-Allow-Origin`不能为`*`星号，必须是明确的域名**
* 遵循同源原则，只能上传来自被允许的域名的Cookie，且无关的域内的网页代码中不能用`document.cookie`获取该Cookie

### 非简单请求
分为两个请求发送

#### 预检请求（preflight）
* 目的是询问服务器该域是否被许可，以及询问哪些头信息和方法被允许，只有在得到允许后才会发出第二次请求
* 请求方法类型为**`OPTION`**
* 包含字段：
    1. `Origin`
    2. `Access-Control-Request-Method`
        必须，列出会用到哪些HTTP方法
    3. `Access-Control-Request-Method`
        逗号分隔的字符串，指定CORS请求额外发送的头字段
        
#### 预检的回应
* 若服务器检查了Origin、Access-Control-Request-Method和Access-Control-Request-Headers字段以后允许跨域，可以发出回应  
    ```
    Access-Control-Allow-Origin: * //回应中的关键字段，代表了允许跨域的域名范围
    ```
* 若不允许跨域，返回正常的HTTP回应，不包含CORS相关字段，同时`XMLHttpRequest.onerror`捕获错误信息输出
* 相关字段：
    1. Access-Control-Allow-Methods  
        必须
    2. Access-Control-Allow-Headers
    3. Access-Control-Allow-Credentials
    4. Access-Control-Max-Age  
        可选，此次预检的有效期

#### 浏览器的正常请求和回应
在通过了预检后，以后浏览器每次发送正常的CORS请求，都和简单请求类似，包含`Origin`头字段，回应中也会有`Access-Control-Max-Age`头字段


> 整理自 「跨域资源共享 CORS 详解」　


    
