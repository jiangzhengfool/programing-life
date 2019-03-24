~~~
$.ajax的使用
//1.$.ajax带json数据的异步请求  
var aj = $.ajax( {    
    url:'productManager_reverseUpdate',// 跳转到 action    
    data:{    
             selRollBack : selRollBack,    
             selOperatorsCode : selOperatorsCode,    
             PROVINCECODE : PROVINCECODE,    
             pass2 : pass2    
    },    
    type:'post',    
    cache:false,    
    dataType:'json',    
    success:function(data) {    
        if(data.msg =="true" ){    
            // view("修改成功！");    
            alert("修改成功！");    
            window.location.reload();    
        }else{    
            view(data.msg);    
        }    
     },    
     error : function() {    
          // view("异常！");    
          alert("异常！");    
     }    
});  
	  
	  
//2.$.ajax序列化表格内容为字符串的异步请求  
function noTips(){    
    var formParam = $("#form1").serialize();//序列化表格内容为字符串    
    $.ajax({    
        type:'post',        
        url:'Notice_noTipsNotice',    
        data:formParam,    
        cache:false,    
        dataType:'json',    
        success:function(data){    
        }    
    });    
}    
  
  
//3.$.ajax拼接url的异步请求  
var yz=$.ajax({    
     type:'post',    
     url:'validatePwd2_checkPwd2?password2='+password2,    
     data:{},    
     cache:false,    
     dataType:'json',    
     success:function(data){    
          if( data.msg =="false" ) //服务器返回false，就将validatePassword2的值改为pwd2Error，这是异步，需要考虑返回时间    
          {    
               textPassword2.html("<font color='red'>业务密码不正确！</font>");    
               $("#validatePassword2").val("pwd2Error");    
               checkPassword2 = false;    
               return;    
           }    
      },    
      error:function(){}    
});   
  
  
//4.$.ajax拼接data的异步请求  
$.ajax({     
    url:'<%=request.getContextPath()%>/kc/kc_checkMerNameUnique.action',     
    type:'post',     
    data:'merName='+values,     
    async : false, //默认为true 异步     
    error:function(){     
       alert('error');     
    },     
    success:function(data){     
       $("#"+divs).html(data);     
    }  
});


jsp：
var xmlHttp = $.post("elecUserAction_checkUser.do",{"logonName":object.value},function(data,textStatus){
        //alert("data:"+data);
        //alert("textStatus:"+textStatus);
        //alert("xmlHttp.responseText:"+xmlHttp.responseText);
        //alert("xmlHttp.readyState:"+xmlHttp.readyState);
        //alert("xmlHttp.status:"+xmlHttp.status);
        if(data==1){
        	document.getElementById("check").innerHTML = "<font color='red'>当前登录名已存在</font>"
        	object.focus();
        }
        else{
        	document.getElementById("check").innerHTML = "<font color='green'>当前登录名可以使用</font>"
        }
    });
//或者
$.ajax({
    type: "POST",
    async:true,  // 设置异步方式
    cache:false, // 保证ajax数据的实时性，不从缓存中读取数据，数据是最新的数据
    url: "elecUserAction_checkUser.do",
    data:"logonName="+logonName,

    success:function(data){
    	if(data==1){
			//alert("当前登录名已经存在");
			document.getElementById("check").innerHTML = "<font color='red'>当前登录名已存在</font>";
			//object.value = "";
			object.focus();
		}else{
			document.getElementById("check").innerHTML = "<font color='green'>当前登录名可用</font>";
		}
    }
});
       
action类：
方案一：使用struts2的ajax 
1：导入struts2-json-plugin-2.3.3.jar
2：在修改struts.xml的extends为json-default（因为json-default继承了struts-default)
添加：
<result name="checkUser" type="json">
	<!-- 使用root参数指定从服务器端返回到客户端的文本内容 -->
 	<param name="root">message</param>
</result>
3: 在模型驱动中添加message的属性
4：添加方法
public String checkUser(){
	//获取登录名
	String logonName = elecUser.getLogonName();
	/**
	 * 以登录名作为查询条件，获取用户信息列表
	 *   如果列表的值不为null，说明登录名在数据库中存在记录，返回flag=1
		 如果列表的值为null，说明登录名在数据库中不存在记录，返回flag=2
	 */
	String flag = elecUserService.findUserByLogonName(logonName);
	elecUser.setMessage(flag);
	
	return "checkUser";
}
方案二：ajax的基本调用
1：添加方法
public String checkUser(){
	try {
		//处理编码格式
		request.setCharacterEncoding("UTF-8");
		response.setContentType("text/html;charset=utf-8");
		//获取登录名
		String logonName = elecUser.getLogonName();
		/**
		 * 以登录名作为查询条件，获取用户信息列表
		 *   如果列表的值不为null，说明登录名在数据库中存在记录，返回flag=1
			 如果列表的值为null，说明登录名在数据库中不存在记录，返回flag=2
		 */
		String flag = elecUserService.findUserByLogonName(logonName);
		PrintWriter out = response.getWriter();
		out.println(flag);
	} catch (IOException e) {
		e.printStackTrace();
	}
	return null;
}
~~~