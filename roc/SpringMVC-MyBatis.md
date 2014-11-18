# SpringMVC控制器下的一个小项目
**陈鹏** 2014年11月18日 22:42:29 [返回首页][1]

---

### 1.数据绑定

- 基本数据类型
	
	很简单,该怎么写就怎么写.

	controller代码
		
			@RequestMapping  
		    public void test1(String name, Integer age, Double income, Boolean isMarried, String[] interests)  
		    {  
		        System.out.println("简单数据类型绑定=========");  
		        System.out.println("名字:" + name);  
		        System.out.println("年龄:" + age);  
		        System.out.println("收入:" + income);  
		        System.out.println("已结婚:" + isMarried);  
		        System.out.println("兴趣:");  
		        for (String interest : interests)  
		        {  
		            System.out.println(interest);  
		        }  
		        System.out.println("====================");  
		    } 
- 简单对象类型

	与基本类型相拟,只不过绑定到对象上更加简洁.(类拟struts的ActionForm)

	controller代码

			@RequestMapping  
		    public void test2(User user)  
		    {  
		        System.out.println("简单对象类型绑定=========");  
		        System.out.println("名字:" + user.getName());  
		        System.out.println("年龄:" + user.getAge());  
		        System.out.println("收入:" + user.getIncome());  
		        System.out.println("已结婚:" + user.getIsMarried());  
		        System.out.println("========================");  
		    }  

### 2.提交数据
1. request对象
2. 参数提交
3. 实体绑定
4. 数组提交
5. 获取Cookie（使用方法3最直接）






> **如何在SpringMVC中获取request对象？**

1.注解法

	@Autowired  
	private  HttpServletRequest request;  
 
 
2. 在web.xml中配置一个监听
 
		<listener>    
		        <listener-class>    
		            org.springframework.web.context.request.RequestContextListener    
		        </listener-class>    
		</listener>    
 
之后在程序里可以用

	HttpServletRequest request = ((ServletRequestAttributes)RequestContextHolder.getRequestAttributes()).getRequest();    
 
 3.直接在参数中引入

	public String hello(HttpServletRequest request,HttpServletResponse response)  


### 3.SpringMVC 登陆拦截器实现登陆控制

> [知识来源][2]

**思路**：未登录时，拦截除了登录页面值外的所有资源和页面；登陆之后，放开资源。如果有角色管理，交由角色管理。

	/**
	 * 登陆拦截器.
	 *
	 * @author leizhimin 2014/6/26 16:08
	 */
	public class LoginInterceptor extends HandlerInterceptorAdapter {
	    private static final String[] IGNORE_URI = {"/login.jsp", "/Login/","backui/","frontui/"};
	 
	    @Override
	    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
	        boolean flag = false;
	        String url = request.getRequestURL().toString();
	        System.out.println(">>>: " + url);
	        for (String s : IGNORE_URI) {
	            if (url.contains(s)) {
	                flag = true;
	                break;
	            }
	        }
	        if (!flag) {
	            T_supplier_user user = LoginController.getLoginUser(request);
	            if (user != null) flag = true;
	        }
	        return flag;
	    }
	 
	    @Override
	    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
	        super.postHandle(request, response, handler, modelAndView);
	    }
	}
***
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean id="loginInterceptor" class="net.xiucheren.web.interceptor.LoginInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>

### 4.显示列表,分页
- 搜索
- 分页
	- 内存-‘假分页’
	- SQL分页

MySQL:

	SELECT * 
	FROM user_info u
	ORDER BY u.user_code
	LIMIT 3,3

SqlServer:
	
	SELECT TOP 页大小 *
	FROM TestTable
	WHERE (ID NOT IN
	          (SELECT TOP (页大小*(页数-1)) id
	         FROM 表
	         ORDER BY id))
	ORDER BY ID

[来源：SQL Server 分页语句][3]

### 5.Google Java编程风格指南
> 1. 前言
> 2. 源文件基础
> 3. 源文件结构
> 4. 格式
> 5. 命名约定
> 6. 编程实践
> 7. Javadoc
> 8. 后记

> [来源：Google Java编程风格指南-Hawstein][4]


  [1]: http://cshijiel.github.io
  [2]: http://lavasoft.blog.51cto.com/62575/1433011
  [3]: http://www.cnblogs.com/szytwo/archive/2012/08/30/2663811.html
  [4]: http://www.hawstein.com/posts/google-java-style.html