## 实现Servlet
~~~
public class BaseServlet extends HttpServlet {
	protected void service(HttpServletRequest req, HttpServletResponse res)
			throws ServletException, IOException {
		req.setCharacterEncoding("UTF-8");
		res.setContentType("text/html;charset=utf-8");

		// 例如：http://localhost:8080/demo1/xxx?method=login
		String methodName = req.getParameter("method");// 它是一个方法名称
		
		// 当没用指定要调用的方法时，那么默认请求的是execute()方法。
		if(methodName == null || methodName.isEmpty()) {
			methodName = "execute";
		}
		Class c = this.getClass();
		try {
			// 通过方法名称获取方法的反射对象
			Method m = c.getMethod(methodName, HttpServletRequest.class,
					HttpServletResponse.class);
			// 反射方法目标方法，也就是说，如果methodName为add，那么就调用add方法。
			String result = (String) m.invoke(this, req, res);
			// 通过返回值完成请求转发
			if(result != null && !result.isEmpty()) {
				req.getRequestDispatcher(result).forward(req, res);
			}
		} catch (Exception e) {
			throw new ServletException(e);
		}
	}
}
~~~

Servlet线程安全问题	
Servlet中的filter和listener	
web容器启动	
Servlet、Interceptor、Listener、Filter 	Servlet接收请求返回响应，最原始的web业务处理类。
	Interceptor拦截器，可以实现HandlerInterceptor接口自定义拦截器，在日志记录、权限检查、性能监控、通用行为等场景使用，本质是AOP。
	Listener监听器常用于统计在线人数等纵向功能。
	Filter过滤器在请求接口处理业务之前改变requset，在业务处理之后响应用户之前改变response。如果某些数据不加密，很容易用抓包工具加filter作弊


## JSP中动态INCLUDE与静态INCLUDE的区别？