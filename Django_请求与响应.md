#Django
###请求与响应
#####新建项目
	django-admin startproject mysite
#####启动开发服务器
	python manage.py runserver
#####创建应用
	python manage.py startapp polls
#####配置文件
	django
	MTV
		models：定义数据库相关内容，一般放在models.py文件
		templates：定义静态内容
		views：定义主业务逻辑，相当于控制器
	
	创建django项目名后生成目录结构
	mysite
		_init_.py：一个定义包的空文件。
		settings.py：项目的主配置文件，非常重要！
		urls.py：所有的任务都是从这里开始分配，相当于Django驱动站点的内容表格，非常重要！
		wsgi.py：一个基于WSGI的web服务器进入点，提供底层的网络通信功能，通常不用关心。
	templates：html文件归置目录
	manage.py：django管理主程序

#####编写视图
	在应用目录内编辑views.py
	from django.http import HttpResponse
	
	def index(request):
	    return HttpResponse("Hello, world. You're at the polls index.")
#####调用视图
	在应用目录内创建编辑urls.py
	from django.conf.urls import url
	from . import views
	
	urlpatterns = [
	    url(r'^$',views.index, name='index')
	]
	
	在项目主urls文件添加urlpattern条目，指向polls的url文件
	from django.conf.urls import include,url
	from django.contrib import admin
	
	urlpatterns = [
	    url(r'^polls/',include('polls.urls')),
	    url(r'^admin/',admin.site.urls),
	]

#####url()方法：
-	url()方法可以接收4个参数，其中2个是必须的：regex和view，以及2个可选的参数：kwargs和name。
-	regex：
	-	regex是正则表达式的通用缩写，它是一种匹配字符串或url地址的语法。Django拿着用户请求的url地址，在urls.py文件中对urlpatterns列表中的每一项条目从头开始进行逐一对比，一旦遇到匹配项，立即执行该条目映射的视图函数或下级路由，其后的条目将不再继续匹配。因此，url路由的编写顺序非常重要！

	-	需要注意的是，regex不会去匹配GET或POST参数或域名，例如对于https://www.example.com/myapp/，regex只尝试匹配myapp/。对于https://www.example.com/myapp/?page=3,regex也只尝试匹配myapp/。
-	view
	-	view指的是处理当前url请求的视图函数。当正则表达式匹配到某个条目时，自动将封装的HttpRequest对象作为第一个参数，正则表达式“捕获”到的值作为第二个参数，传递给该条目指定的视图view。如果是简单捕获，那么捕获值将作为一个位置参数进行传递，如果是命名捕获，那么将作为关键字参数进行传递。
	
-	kwargs
	-	任意数量的关键字参数可以作为一个字典传递给目标视图。

-	name
	-	对你的URL进行命名，让你能够在Django的任意处，尤其是模板内显式地引用它。这是一个非常强大的功能，相当于给URL取了个全局变量名，不会将url匹配地址写死。