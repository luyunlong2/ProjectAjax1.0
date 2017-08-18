# ProjectAjax1.0
Ajax实现前台页面实时访问服务器刷新数据。
--登录注册页面
<!DOCTYPE html>
<html>
  <head>
    <title>login.html</title>
    <script src="js/jquery-1.8.3.min.js"></script>
    <script src="js/register.js"></script>
    <script src="js/login.js"></script>
    <!--<link rel="stylesheet" type="text/css" href="./styles.css">-->
  </head>
  
  <body>
  <form method="post">
  <table border="1">
   	 <tr>注册:</tr>
  	 <tr><td> name<input type="text" name="name" id="name"></td><td><span id="span1"></span></td></tr>
	  <tr><td>pass<input type="password" name="pwd" id="pwd"></td><td><span id="span2"></span></td></tr>
   	 <tr><td colspan="2"><input type="button" value="登录" id="login"></td>
    </table>
    </form>
  </body>
</html>
--登录注册js文件
$(function(){
	login();
})

function login(){
	$("#login").bind("click",function(){
		var name=$("#name").val();
		var pwd=$("#pwd").val();
		$.post("servlet/UserServlet?type=getUser",{"name":name,"pwd":pwd},function(data){
				if(data==1)
					location.href="list.html";
				else
					alert("用户名或密码错误");
			})
	})
}

$(function(){
	initEvent();
})

//注册验证s
function initEvent(){
	$("#name").bind("change",function(){
		$.get("servlet/UserServlet?type=getUserByName",{"name":this.value},function(data){
			
			if(data==1)
				$("#span1").html("用户名已存在!不可用");
			else
				$("#span1").html("可以使用！");
		});
	})
}

--显示和分页导航界面
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<link href="pagination/asset/normalize/normalize.css" type="text/css" rel="stylesheet" media="all">
<link href="pagination/css/layout.css" type="text/css" rel="stylesheet">
<link href="pagination/css/autoc.min.css" type="text/css" rel="stylesheet">
<link href="pagination/css/pagination.css" type="text/css" rel="stylesheet">
<script type="text/javascript" src="pagination/asset/jquery/jquery.js"></script>
<script type="text/javascript" src="pagination/js/autoc.js"></script>
<script type="text/javascript" src="pagination/js/pagination.js"></script>

<script type="text/javascript" src="http://www.5imvc.com/scripts/jbox/jBox/jquery-1.4.2.min.js"></script>
    <!--jbox-->
    <link id="skin" rel="stylesheet" href="http://www.5imvc.com/scripts/jbox/jBox/Skins/Default/jbox.css" />
    <script type="text/javascript" src="http://www.5imvc.com/scripts/jbox/jBox/jquery.jBox-2.3.min.js"></script>
    <script type="text/javascript" src="http://www.5imvc.com/scripts/jbox/jBox/i18n/jquery.jBox-zh-CN.js"></script>
<script type="text/javascript">

var webName="/projectajax/UserServlet?type=";
$(function(){
	loadList(1,10);
	loadPagination();
})

function loadList(pageIndex,pageSize){
	$.getJSON(webName+"getUserPage",{"pageIndex":pageIndex,"pageSize":pageSize},function(data){
		var tab=$("#tab1")
		var msg="";
		tab.html("");
		for(var i=0;i<data.length;i++){
			msg+="<tr>";
			msg+="<td>"+data[i].id+"</td>";
			msg+="<td>"+data[i].name+"</td>";
			msg+="<td>"+data[i].pwd+"</td>";
			msg+="<td>"+data[i].rinfo.name+"</td>";
			msg+="<td><a href='javascript:deleteUser("+data[i].id+")'>删除</a>&nbsp;&nbsp;<a href='javascript:updateMSg()'>修改</a></td>";
			msg+="</tr>";
		}
		tab.html(tab.html()+msg);
	});
}

function updateMSg(){
	$.jBox($("#update").html(), { title: "编辑用户信息",submit:function(){
			
	}});
}

function deleteUser(){
	var submit = function (v, h, f) {
	    if (v){
	        jBox.tip("YES", 'info');
	    	
	    }
	    else
	        jBox.tip("NO", 'info');
	    return true;
	};
	// 自定义按钮
	$.jBox.confirm("是否确定删除数据", "系统提示", submit, { buttons: { 'YES': true, 'NO': false} });
}

function loadPagination(){
	var Pager = new Pagination({
	    parent: '#div2',
	    totalPage: 4,
	    prevText:"上一页",
	    nextText:"下一页",
	    pageSize:5
	});

	Pager.on('afterChange', function(args){
		var index=args.currentPage
		loadList(index,10);
		
		
	});
}
</script>
</head>
<body>
	<div id="update" style="display: none">
		编号:1
		用户名:<input type="text" name="name"><br>
		密码:<input type="text" name="pwd"><br>
		角色<select></select>
	</div>

	<div id="div1">
		<table border="1">
			<thead>
				<tr>
					<th>编号</th>
					<th>姓名</th>
					<th>密码</th>
					<th>角色</th>
					<th>操作</th>
				</tr>
			</thead>
			<tbody id="tab1">
			
			</tbody>
		</table>
	</div>
	<div id="div2"></div>
</body>
</html>
<!DOCTYPE html>
<html>
  <head>
    <title>list.html</title>
    <link href="pagination/asset/normalize/normalize.css" type="text/css" rel="stylesheet" media="all">
	<link href="pagination/css/layout.css" type="text/css" rel="stylesheet">
	<link href="pagination/css/autoc.min.css" type="text/css" rel="stylesheet">
	<link href="pagination/css/pagination.css" type="text/css" rel="stylesheet">
	<script type="text/javascript" src="pagination/asset/jquery/jquery.js"></script>
	<script type="text/javascript" src="pagination/js/autoc.js"></script>
	<script type="text/javascript" src="pagination/js/pagination.js"></script>
	<script type="text/javascript" src="http://www.5imvc.com/scripts/jbox/jBox/jquery-1.4.2.min.js"></script>
    <!--jbox-->
    <link id="skin" rel="stylesheet" href="http://www.5imvc.com/scripts/jbox/jBox/Skins/Default/jbox.css" />
    <script type="text/javascript" src="http://www.5imvc.com/scripts/jbox/jBox/jquery.jBox-2.3.min.js"></script>
    <script type="text/javascript" src="http://www.5imvc.com/scripts/jbox/jBox/i18n/jquery.jBox-zh-CN.js"></script>
	  <script src="js/list.js"></script>
  </head>
  
  <body>
  	<div id="update" style="display: none;">
  		编号:<input type="hidden" name="id"><br>
  		用户名:<input type="text" name="name" id="name"><br>
  		密码:<input type="password" name="pwd" id="pwd"><br>
  		角色:<select >
  			<option>程序员</option>
  			<option>设计师</option>
  			<option>酱油师</option>
  			<option>测试员</option>
  		</select><br>
  		<input type="button" value="确定" id="but1">
  	</div>
    <div id="list">
    	<table border="1">
    	<thead>
    		<tr>
    			<th>编号</th>
    			<th>姓名</th>
    			<th>密码</th>
    			<th>角色</th>
    			<th>操作</th>
    		</tr>
    		</thead>
    		<tbody id="tab1">
    			
    		</tbody>
    	</table>
    </div>
    <div id="div2"></div>
  </body>
</html>
--显示和分页导航js
$(function(){
	loadList(1,10);
	loadPagination(10);
})
//分页数据
function loadList(pageIndex,pageSize){
	$.getJSON("servlet/UserServlet?type=getUserPage",{"pageIndex":pageIndex,"pageSize":pageSize},function(data){
			var tab=$("#tab1");
			tab.html("");
			var msg="";
			for(var i=0;i<data.length;i++){
				msg+="<tr>"
				msg+="<td>"+data[i].id+"</td>"
				msg+="<td>"+data[i].name+"</td>"
				msg+="<td>"+data[i].pwd+"</td>"
				msg+="<td>"+data[i].rinfo.name+"</td>"
				msg+="<td><a href='javascript:deleteUser("+data[i].id+")'>删除</a>&nbsp;&nbsp<a href='javascript:updateUser("+data[i].id+")'>修改</a></td>"
				msg+="</tr>"
			}
			$("#tab1").html(tab.html()+msg);
	})
}
//更新信息
function updateUser(id){	
	$("#update").show();
		$("#but1").bind("click",function(){
			$.getJSON("servlet/UserServlet?type=updateUser",{"id":id,"name":$("#name").val(),"pwd":$("#pwd").val()},function(data){
				$("#update").hide();
			})
		})
}
//删除信息
function deleteUser(id){
	$.get("servlet/UserServlet?type=deleteUser",{"id":id},function(){
		var submit = function (v, h, f) {
		    if (v){
		        jBox.tip("YES", 'info');
		    	
		    }
		    else
		        jBox.tip("NO", 'info');
		    return true;
		};
		// 自定义按钮
		$.jBox.confirm("是否确定删除数据", "系统提示", submit, { buttons: { 'YES': true, 'NO': false} });	
	})
}
//分页导航
function loadPagination(pageSize){
	$.get("servlet/UserServlet?type=getCount",{"pageSize":pageSize},function(data){
		var Pager = new Pagination({
		    parent: '#div2',
		    totalPage: data,
		    prevText:"Prev",
		    nextText:"Next",
		    pageSize:5
		
	});

	Pager.on('afterChange', function(args){
		var index=args.currentPage
		loadList(index,10);
	})	
		
	});
}
--servlet处理
import java.io.IOException;
import java.io.PrintWriter;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.jxnd.bean.UserInfo;
import org.jxnd.service.IUserService;
import org.jxnd.service.impl.UserServiceImpl;

import com.google.gson.Gson;

public class UserServlet extends HttpServlet {
	IUserService service=new UserServiceImpl();
	Gson gson=new Gson();
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setCharacterEncoding("UTF-8");
		String type=request.getParameter("type");
		try {
			Method mt=UserServlet.class.getMethod(type, HttpServletRequest.class,HttpServletResponse.class);
			
				mt.invoke(this, request,response);
			} catch (Exception e) {
		}
	}
	public void getUserByName(HttpServletRequest request,HttpServletResponse response) throws ServletException,IOException{
		String name =request.getParameter("name"); 
		if(service.getUserByName(name))
			response.getWriter().print("1");
		else
			response.getWriter().print("2");
			
	}
	public void updateUser(HttpServletRequest request,HttpServletResponse response)throws ServletException,IOException{
		int id=request.getParameter("id")==null?0:Integer.parseInt(request.getParameter("id").toString());
		String name=request.getParameter("name");
		String pwd=request.getParameter("pwd");
		UserInfo info=null;
		if(service.updateUser(name,pwd,id)){
			info.setName(name);
			info.setPwd(pwd);
			info.setId(id);
			String msg=gson.toJson(info);
			response.getWriter().print(msg);
		}
	}
	
	public void deleteUser(HttpServletRequest request,HttpServletResponse response)throws ServletException,IOException{
		int id=request.getParameter("id")==null?0:Integer.parseInt(request.getParameter("id").toString());
		if(service.deleteUser(id))
			response.getWriter().print("1");
		else
			response.getWriter().print("2");
	}
	public void getCount(HttpServletRequest request,HttpServletResponse response)throws ServletException,IOException{
		int pageSize=request.getParameter("pageSize")==null?10:Integer.parseInt(request.getParameter("pageSize"));
		int count=service.getCount();
		int data= (int) Math.ceil(count/pageSize);
		response.getWriter().print(data);
	}
	public void getUserPage(HttpServletRequest request,HttpServletResponse response)throws ServletException,IOException{
		int pageIndex=request.getParameter("pageIndex")==null?1:Integer.parseInt(request.getParameter("pageIndex"));
		int pageSize=request.getParameter("pageSize")==null?10:Integer.parseInt(request.getParameter("pageSize"));
		List<UserInfo> list=service.getUserPage(pageIndex, pageSize);
		if(list!=null&&list.size()>0){
			String msg=gson.toJson(list);
			response.getWriter().print(msg);
		}else
			response.getWriter().print("1");
		response.getWriter().flush();
	}
	public void getUser(HttpServletRequest request,HttpServletResponse response) throws ServletException,IOException{
		String name=request.getParameter("name");
		String pwd=request.getParameter("pwd");
		UserInfo info=service.getUser(name, pwd);
		if(info!=null){
			request.getSession().setAttribute("info", info);
			response.getWriter().print("1");
		}	
		else
			response.getWriter().print("2");
	}
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		this.doGet(request, response);
	}

}
......
