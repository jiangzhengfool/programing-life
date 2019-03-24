快速入门
	基本步骤：
	1.创建WebClient对象:
	//无参构造
	WebClient webClient=new WebClient();
	//BrowserVersion有chrome、firefox、ie等选择，创建指定的浏览器对象
	WebClient webClient=new WebClient(BrowserVersion.CHROME);
	//使用代理创建对象
	WebClient webClient=new WebClient(BrowserVersion.CHROME,"myproxyserver",myProxyPort);
	final DefaultCredentialsProvider credentialsProvider = (DefaultCredentialsProvider) webClient.getCredentialsProvider();

			credentialsProvider.addCredentials("username", "password");//设置代理用户名，密码
	2.设置webclient参数
	webClient.getOptions().setJavaScriptEnabled(true);//开启js引擎

	webClient.getOptions().setCssEnabled(false);//设置CSS样式关闭

	webClient.setAjaxController(new NicelyResynchronizingAjaxController());//开启ajax

	webClient.getOptions().setThrowExceptionOnScriptError(false);//关闭js错误抛出异常


	3.获取页面getPage(url)
	HtmlPage page=webClient.getPage(String url);
	4.对获取页面进行处理
	4.1 通过id定位元素
	HtmlElement ln = page.page.getElementById("id");
	4.2通过name定位元素
	HtmlElement name=page.getElementByName("name");
	4.3通过xpath定位元素
	List<?> divs = page.getByXPath("//div");
	HtmlDivision div = (HtmlDivision) page.getByXPath("//div[@name='John']").get(0);
	4.4表单提交
	//获取要处理得form表单

	HtmlForm form = page1.getFormByName("myform");

	//定位提交按钮

	HtmlSubmitInput button = form.getInputByName("submitbutton");

	//定位输入框

	HtmlTextInput textField = form.getInputByName("userid");

	//给输入框设值

	textField.setValueAttribute("root");

	//触发按钮单击事件，感觉是在人工操作页面一样的

	HtmlPage page2 = button.click();

	//对提交表单后的页面处理page2