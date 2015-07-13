# JAVA过滤器
---
[TOC]
### 生命周期
1. 实例化--->Web.xml
2. 初始化--->init()
3. 过滤--->doFilter()
4. 销毁--->destroy()
### 默认方法
* init()
:	初始化方法，读取web.xml中过滤器参数
* doFilter()
:	核心方法，实际的过滤操作。FilterChain可以调用chain.doFilter方法，将请求传给下一个过滤器，或利用转发重定向到别的资源。
* destroy()
:	web容器销毁实例前调用的方法
### Web.xml配置
```java
	<filter>
		<filter-name>过滤器名</filter-name>
		<filter-class>类名</filter-class>
	多个过滤器
	若针对一个用户请求，有多个过滤器，或根据<filter-mapping>的顺序形成链
		<init-param>
			<description>描述信息</description>
			<param-name>参数名</param-name>
			<param-value>参数值</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>过滤器名</filter-name>
		<url-pattern>url</url-pattern>
		<dispatcher></dispatcher>
	</filter-mapping>
```

### 多个过滤器
若针对一个用户请求，有多个过滤器，或根据<filter-mapping>的顺序形成链
```java
	public class LogFilter implements Filter{
		public void  init(FilterConfig config) 
                         throws ServletException{
      // 获取初始化参数
      		String testParam = config.getInitParameter("test-param"); 

      // 输出初始化参数
      		System.out.println("Test Param: " + testParam); 
   		}
   		public void  doFilter(ServletRequest request, 
                 ServletResponse response,
                 FilterChain chain) 
                 throws java.io.IOException, ServletException {

      		// 获取客户机的 IP 地址   
      		String ipAddress = request.getRemoteAddr();

      		// 记录 IP 地址和当前时间戳
     		 System.out.println("IP "+ ipAddress + ", Time "+ new Date().toString());

     		// 把请求传回过滤链
      		chain.doFilter(request,response);
   		}	

   		public void destroy( ){
      	/* 在 Filter 实例被 Web 容器从服务移除之前调用 */
		}
	}
```

### 过滤器的分类
:	在xml中的dispatcher中配置 默认request  

* request
:	用户访问页面时调用的过滤器

* forward
:	请求转发时用forward时用过滤器

* include
:	类似forward
