#Django-模型与管理后台

#####数据库安装

-	打开mysite/settings.py配置文件，这是整个Django项目的设置中心。Django默认使用SQLite数据库，因为Python源生支持SQLite数据库，所以你无须安装任何程序，就可以直接使用它。当然，如果你是在创建一个实际的项目，可以使用类似PostgreSQL的数据库，避免以后数据库迁移的相关问题。

-	mysql 为例

		pymysql.install_as_MySQLdb()
		
		
		DATABASES = {
		    'default': {
		        'ENGINE': 'django.db.backends.mysql',
		        'NAME': 'mysite',
		        'HOST':'10.235.102.233',
		        'USER':"root",
		        'PASSWORD':'123456',
		        'PORO':'3306',
		    }
		}

#####注册应用

	配置文件添加
	INSTALLED_APPS
	'polls',
	python manage.py migrate
	migrate命令将遍历INSTALLED_APPS设置中的所有项目，在数据库中创建对应的表，并打印出每一条动作信息。如果你感兴趣，可以在你的数据库命令行下输入：\dt (PostgreSQL)、 SHOW TABLES;(MySQL)或 .schema(SQLite) 来列出 Django 所创建的表。

#####创建模型

	from django.db import models
	
	class Question(models.Model):
	    question_text = models.CharField(max_length=200)
	    pub_date = models.DateTimeField('date published')
	
	class Choice(models.Model):
	    question = models.ForeignKey(Question, on_delete=models.CASCADE)
	    choice_text = models.CharField(max_length=200)
	    votes = models.IntegerField(default=0)

#####启用模型

	#为改动创建迁移记录
	python manage.py makemigrations polls
	python manage.py sqlmigrate polls 0001
	#将操作同步到数据库
	python manage.py migrate

#####使用模型的API

	http://www.liujiangblog.com/course/django/88

#####admin后台管理站点

-	创建管理员用户

		python manage.py createsuperuser

-	启动开发服务器

		服务器启动后，在浏览器访问http://127.0.0.1:8000/admin/。你就能看到admin的登陆界面了：
-	在admin中注册投票应用


		vim polls/admin.py
		from django.contrib import admin
		from .models import Question
		
		admin.site.register(Question)