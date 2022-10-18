## Cookiecutter Django 

目录
- 一、[Project Generation Options](#Project_Generation_Options)
-  二、[Getting Up and Running Locally](#Getting_Up_and_Running_Locally)




 Cookiecutter Django是一种用于快速构建django项目的项目模板。这个模板提供了一些可选项，通过一下篇章，可以了解学习如何配置这些可选项。 

### <span id = "Project_Generation_Options">Project Generation Options</span> 

 这个章节主要列出所有的cookiecutter_django模型可选项，在cookiecutter命令行界面中，可以通过配置这些可选项构建自己的项目框架。

`project_name`:项目名称，允许使用大写字母和空格

<span id = "project_slug">`project_slug`</span> :项目名称简写，用于为repo命名，或者用于当你的项目需要被导入的时候。一般默认与project_name一致，不做修改

`description`:用于描述你的项目，一般用在README.rst或者其他类似的地方

`author_name`:项目作者，你本人

`email`:填写邮箱

 
`domain_name`: 计划在项目上线后使用的域名,可以随时更改
 
`version`: 版本号


`open_source_license`:开放源许可证:
可选项：
 MIT、 BSD、 GPLv3、Apache Software 、License 2.0、Not open source

`timezone`: 时区配置，默认是TIME_ZONE = 'America/Chicago'，需要修改

```python
#datetime.datetime.now() - 东八区时间 / datetime.datetime.utcnow() => utc时间
TIME_ZONE = 'Asia/Shanghai'
# 影响自动生成数据库时间字段；
#       USE_TZ = True，创建UTC时间写入到数据库。
#       USE_TZ = False，根据TIME_ZONE设置的时区进行创建时间并写入数据库
USE_TZ = False
```

`windows`:表示是否配置成在windows可开发的项目
 
`use_pycharm`:是否配置成可在pycharm开发的项目

`use_docker`:是否配置成使用docker和docker组件

`postgresql_version`:选择pg版本，可选项有：10~14

`cloud_provider`:选择cloud provider,表示提供kubernetes与云厂商基础服务的对接能力。可选项： AWS(Kubernetes 核心库内置的)、GCP(Kubernetes 核心库内置的)、None，如果填了None,那么 media files 将无法运行

`mail_service`:选择一个Django-Anymail提供的email服务，Mailgun、Amazon SES、Mailjet、Mandrill、Postmark、SendGrid、SendinBlue、SparkPost、Other SMTP

`use_async`:
https://www.pudn.com/news/62e0ba10864d5c73ac12e0f0.html
表示项目是否使用Uvicorn + Gunicorn.

`use_celery`:表示是否配置成使用Celery，Celery是基于Python开发的一个分布式任务队列框架，支持使用任务队列的方式在分布的机器/进程/线程上执行任务调度。比如在Django Web平台开发中，碰到一些请求执行的任务时间较长（几分钟），为了加快用户的响应时间，可以采用异步任务的方式在后台执行这些任务。这时候就需要构建django+celery框架。详见https://blog.csdn.net/zzddada/article/details/119718282

<span id = "use_mailhog">`use_mailhog`</span>:是否使用MailHog，这是一个电子邮件测试工具。一般在开发过程中使用。

`use_sentry`:是否使用sentry,sentry是一个开源的监控系统，能支持服务端与客户端的监控，还有个强大的后台错误分析、报警平台。Sentry 本身是基于 Django 开发的。

`use_whitenoise`:是否使用whitenoise,Whitenoise是最棒的静态资源服务器。
多年来，托管网站的静态资源——图片、Javascript、CSS——都是一件很痛苦的事情。Django 内建的 django.views.static.serve 视图，“在生产环境中不可靠，所以只应为开发环境的提供辅助功能。”但使用一个“真正的” Web 服务器，如 NGINX 或者借助 CDN 来托管媒体资源，配置起来会比较困难。Whitenoice 很简洁地解决了这个问题。它可以像在开发环境那样轻易地在生产环境中设置静态服务器，并且针对生产环境进行了加固和优化。

`use_heroku`:表示本框架是否配置成可在Heroku开发，Heroku是一个支持多种编程语言的云平台。
 
`ci_tool`: 选择持续集成工具，可选项如下： None、Travis CI、Gitlab CI、Github Actions


`keep_local_envs_in_vcs`:是否将项目的.envs/.local/保存到VCS虚拟环境中，一般用于对本地环境再现性有要求的项目中。`注意`：.env(s)只在启用docker或者heroku时才能使用中（ `use_docker`，`use_heroku`两个选项都配置成True）

`debug`:是否配置成可debug

## 
### <span id = "Getting_Up_and_Running_Locally">Getting Up and Running Locally</span> 
### 配置开发环境

需要提前准备的环境有：


    Python 3.9

    PostgreSQL.

    Redis, if using Celery

    Cookiecutter

1、创建虚拟环境:

```python
python3.9 -m venv <virtual env path>
```
2、激活已创建的虚拟环境:

 ```python
 source <virtual env path>/bin/activate
 ```

3、安装cookiecutter-django:

```python
cookiecutter gh:cookiecutter/cookiecutter-django
```
4、安装配置开发要求

```python
cd 项目所在目录

pip install -r requirements/local.txt

git init # A git repo is required for pre-commit to install

pre-commit install
```
pre-commit默认都安装，相关知识点后续章节中会提到。

5、创建PostgreSql：

```python
createdb --username=postgres <project_slug>
```

[project_slug](#project_slug) 前面提到过，默认是项目名称。

`注意`：如果你的电脑在此之前没有创建过数据库，你需要提前做关于postgreSql的初始化配置。  

6、设置数据库的环境变量：

```python
 export DATABASE_URL=postgres://postgres:<password>@127.0.0.1:5432/<DB name given to createdb>
# Optional: set broker URL if using Celery
 export CELERY_BROKER_URL=redis://localhost:6379/0
```
你也可以在项目根目录下创建一个.env文件，将需要定义的变量全部写入，然后只要配置DJANGO_READ_DOT_ENV_FILE=True，那么所有的变量将自动导入。如：
```python
#.env文件内容
# 增加环境变量
DEBUG=True
SECRET_KEY=your-secret-key
DATABASE_URL=postgres://postgres:<password>@127.0.0.1:5432/<DB name given to createdb>
CELERY_BROKER_URL=redis://localhost:6379/0
```

建议使用direnv管理本地环境

7、使用 migrations:

$ python manage.py migrate

8、同步运行脚本：

    $ python manage.py runserver 0.0.0.0:8000

异步运行脚本:

$ uvicorn config.asgi:application --host 0.0.0.0 --reload --reload-include '*.html'

### Setup Email Backend
#### MailHog

前提是[MailHog](#MailHog)这里设置为y

MailHog用于在开发过程中接受email,mailhog是一个电子邮件测试测试工具，使用Go语言写的，且没有多余依赖。

使用场景：使用MailHog，我们的 django-allauth包可以向新用户发送验证码。

mac配置步骤：
1、下载最新的MailHog

2、将文档重命名为MailHog

3、将下载重命名后的文档复制到项目目录下

4、将文档设置为可执行

```$ chmod +x MailHog```

5、在终端输入```./MailHog```打开它

6、http://127.0.0.1:8025/  检查是否正常运行

如果你已经在构建cookiecutter_django框架中将MaiHog设置为n,那么可以通过设置EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'实现邮件的发送，生产中是使用mailgun.

#### Celery

如果项目设置为可以使用Celery作为任务调度处理器，那么在本地开发时任务默认在主线程中执行。关于Celery的配置设置完毕后，只要在config/settings/local.py中设置```CELERY_TASK_ALWAYS_EAGER = False```即可，在本地运行Celery要求先安装redis-server，详见 https://redis.io/topics/quickstart ，先在一个终端把redis-server跑起来，然后在另一个终端使用以下命令开启Celery，
 
```celery -A config.celery_app worker --loglevel=info```

### Sass Compilation & Live Reloading

如果你选择Gulp作为前端管道，那么框架将配置Sass编译器和实时加载。一旦你修改Sass/JS源文件，任务将自动重建相关的CSS和JS，并且在不需要刷新页面的前提下重载到浏览器。
1、先安装Node.js v16

2、在项目根目录安装依赖：

```python
 npm install
 ```

3、在虚拟环境已激活的情况下，执行以下语句把应用跑起来：

```python
npm run dev
 ```
 此时，应用在前端动态变化时，满足实时重载的。

 ### Getting Up and Running Locally With Docker

The steps below will get you up and running with a local development environment. All of these commands assume you are in the root of your generated project.

