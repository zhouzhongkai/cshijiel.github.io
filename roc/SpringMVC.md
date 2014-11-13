# SpringMVC学习
**陈鹏** 2014年11月13日 19:28:57 [返回首页][1]

---
> Spring Web MVC 是一种基于`Java`的实现了Web MVC设计模式的请求驱动类型的轻量级Web框架，框架的目的就是帮助我们简化开发，Spring Web MVC也是要简化我们日常Web开发的。

## 1. SpringMVC是什么？
Spring Web MVC 是一种基于`Java`的实现了Web MVC设计模式的请求驱动类型的轻量级Web框架，框架的目的就是帮助我们简化开发，Spring Web MVC也是要简化我们日常Web开发的。

	类似于之前学习的Struts2，是一种`Controller`工具。
	这种工具都是处理请求URL分配到合适的类去处理，然后选择合适的视图为用户渲染。

SpringMVC前端控制器是DispatcherServlet;应用控制器其实拆为**处理器映射器**（`Handler Mapping`）进行处理器管理和视图解析器（`View Resolver`）进行视图管理；页面控制器/动作/处理器为`Controller`接口（仅包括`ModelAndView handleRequest(request,response)`方法）的实现。提供了强大的**约定大于配置**（惯例优先原则）的契约式编程支持。

## 2. SpringMVC能帮我们做什么？
- 让我们能非常简单的设计出**干净的Web层和薄薄的Web层**；
- 进行更简洁的Web层的开发；
- **天生与Spring框架集成**（如IoC容器、AOP等）；
- 提供强大的**约定大于配置**的契约式编程支持；
- 能简单的进行Web层的**单元测试**；
- **支持灵活的URL到页面控制器的映射；**
- 非常容易与其他视图技术集成，如`Velocity`、`FreeMarker`等等，因为模型数据不放在特定的API里，而是放在一个Model里（Map数据结构实现，因此很容易被其他框架使用）；
- 非常灵活的数据验证、格式化和数据绑定机制，能使用任何对象进行数据绑定，不必实现特定框架的API；
- 提供一套强大的JSP标签库，简化JSP开发；
- 支持灵活的本地化、主题等解析；
- 更加简单的异常处理；
- 对静态资源的支持；
- 支持`Restful`风格。

## 3. SpringMVC架构
SpringMVC是一个基于**请求驱动**（请求URL还是_____?）的Web框架，并且也是用了**前端控制器模式**来进行设计，再根据**请求映射规则**分发给相应的页面控制器（动作/处理器）进行处理。下面是SpringMVC处理请求的流程：
###3.1 SpringMVC处理请求的流程：



  [1]: http://cshijiel.github.io