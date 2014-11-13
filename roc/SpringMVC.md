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

### 3.1 SpringMVC处理请求的流程：
![SpringMVC-Sequence][2]

具体执行过程如下：

1. 首先用户发送请求————>前端控制器，前端控制器根据请求信息（如URL）来决定选择哪一个页面控制器进行处理并把请求委托给它，即以前的控制器的控制逻辑部分；图2-1中的1、2步骤；
2.  页面控制器接收到请求后，进行功能处理，首先需要收集和绑定请求参数到一个对象，这个对象在Spring Web MVC中叫命令对象，并进行验证，然后将命令对象委托给业务对象进行处理；处理完毕后返回一个ModelAndView（模型数据和逻辑视图名）；图2-1中的3、4、5步骤；
3.  前端控制器收回控制权，然后根据返回的逻辑视图名，选择相应的视图进行渲染，并把模型数据传入以便视图渲染；图2-1中的步骤6、7；
4.  前端控制器再次收回控制权，将响应返回给用户，图2-1中的步骤8；至此整个结束。

###3.2 SpringMVC架构
Spring Web MVC核心架构图，如图
![SpringMVC core][3]
架构图对应的DispatcherServlet核心代码如下：

	//前端控制器分派方法
	protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
		HttpServletRequest processedRequest = request;
		HandlerExecutionChain mappedHandler = null;
		int interceptorIndex = -1;

		try {
			ModelAndView mv;
			boolean errorView = false;

			try {
                   //检查是否是请求是否是multipart（如文件上传），如果是将通过MultipartResolver解析
				processedRequest = checkMultipart(request);
                   //步骤2、请求到处理器（页面控制器）的映射，通过HandlerMapping进行映射
				mappedHandler = getHandler(processedRequest, false);
				if (mappedHandler == null || mappedHandler.getHandler() == null) {
					noHandlerFound(processedRequest, response);
					return;
				}
                   //步骤3、处理器适配，即将我们的处理器包装成相应的适配器（从而支持多种类型的处理器）
				HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

                  // 304 Not Modified缓存支持
			    //此处省略具体代码

				// 执行处理器相关的拦截器的预处理（HandlerInterceptor.preHandle）
				//此处省略具体代码

				// 步骤4、由适配器执行处理器（调用处理器相应功能处理方法）
				mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

				// Do we need view name translation?
				if (mv != null && !mv.hasView()) {
					mv.setViewName(getDefaultViewName(request));
				}

				// 执行处理器相关的拦截器的后处理（HandlerInterceptor.postHandle）
				//此处省略具体代码
			}
			catch (ModelAndViewDefiningException ex) {
				logger.debug("ModelAndViewDefiningException encountered", ex);
				mv = ex.getModelAndView();
			}
			catch (Exception ex) {
				Object handler = (mappedHandler != null ? mappedHandler.getHandler() : null);
				mv = processHandlerException(processedRequest, response, handler, ex);
				errorView = (mv != null);
			}

			//步骤5 步骤6、解析视图并进行视图的渲染
	//步骤5 由ViewResolver解析View（viewResolver.resolveViewName(viewName, locale)）
	//步骤6 视图在渲染时会把Model传入（view.render(mv.getModelInternal(), request, response);）
			if (mv != null && !mv.wasCleared()) {
				render(mv, processedRequest, response);
				if (errorView) {
					WebUtils.clearErrorRequestAttributes(request);
				}
			}
			else {
				if (logger.isDebugEnabled()) {
					logger.debug("Null ModelAndView returned to DispatcherServlet with name '" + getServletName() +
							"': assuming HandlerAdapter completed request handling");
				}
			}

			// 执行处理器相关的拦截器的完成后处理（HandlerInterceptor.afterCompletion）
			//此处省略具体代码


		catch (Exception ex) {
			// Trigger after-completion for thrown exception.
			triggerAfterCompletion(mappedHandler, interceptorIndex, processedRequest, response, ex);
			throw ex;
		}
		catch (Error err) {
			ServletException ex = new NestedServletException("Handler processing failed", err);
			// Trigger after-completion for thrown exception.
			triggerAfterCompletion(mappedHandler, interceptorIndex, processedRequest, response, ex);
			throw ex;
		}

		finally {
			// Clean up any resources used by a multipart request.
			if (processedRequest != request) {
				cleanupMultipart(processedRequest);
			}
		}
	}

核心架构的具体流程步骤如下：
1. 首先用户发送请求——>DispatcherServlet，前端控制器收到请求后自己不进行处理，而是委托给其他的解析器进行处理，作为统一访问点，进行全局的流程控制；
2. DispatcherServlet——>HandlerMapping， HandlerMapping将会把请求映射为HandlerExecutionChain对象（包含一个Handler处理器（页面控制器）对象、多个HandlerInterceptor拦截器）对象，通过这种策略模式，很容易添加新的映射策略；
3. DispatcherServlet——>HandlerAdapter，HandlerAdapter将会把处理器包装为适配器，从而支持多种类型的处理器，即适配器设计模式的应用，从而很容易支持很多类型的处理器；
4.  HandlerAdapter——>处理器功能处理方法的调用，HandlerAdapter将会根据适配的结果调用真正的处理器的功能处理方法，完成功能处理；并返回一个ModelAndView对象（包含模型数据、逻辑视图名）；
5.  ModelAndView的逻辑视图名——> ViewResolver， ViewResolver将把逻辑视图名解析为具体的View，通过这种策略模式，很容易更换其他视图技术；
6.  View——>渲染，View会根据传进来的Model模型数据进行渲染，此处的Model实际是一个Map数据结构，因此很容易支持其他视图技术；
7.  返回控制权给DispatcherServlet，由DispatcherServlet返回响应给用户，到此一个流程结束。
 
此处我们只是讲了核心流程，没有考虑拦截器、本地解析、文件上传解析等，后边再细述。

参考资料：http://jinnianshilongnian.iteye.com/blog/1594806

http://mooc.guokr.com/post/610231/

  [1]: http://cshijiel.github.io
  [2]: http://cshijiel.github.io/roc/images/SpringMVC-Sequence.jpg
  [3]: http://cshijiel.github.io/roc/images/SpringMVC-core.jpg