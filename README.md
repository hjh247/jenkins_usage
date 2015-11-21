Jenkins 使用探索       
==========
<br />

Jenkins介绍
----
**Jenkins**是一个**可扩展的,跨平台的，开源**的持续集成引擎。

官方网站地址[http://jenkins-ci.org](http://jenkins-ci.org "官方网站")

[https://wiki.jenkins-ci.org/display/JENKINS/Home](https://wiki.jenkins-ci.org/display/JENKINS/Home "wiki地址")中有jenkins的使用介绍。

中文介绍可以见项目中《jenkins入门手册.pdf》

Jenkins原本是hudson的一个分支,由于oracle声称拥有hodson的商标权而分离出来。Jenkins继续坚持开源的道路。

**主要用途：**

- 持续、自动化构建
- 持续、自动化测试
- 持续、自动化部署
- 定时执行任务
- 监控定时任务

**特性**

- 跨平台。可以在多个平台上部署
- 易安装。部署war包即可
- 易配置。所有配置都可以通过web页面完成
- 丰富的插件系统。通过插件可支持各种版本控制及构建工具
- 易扩展。可以根据需要定制Jenkins程序及插件
- 支持分布式构建。可以支持分布式的构建、测试
- 完善的日志系统。任务执行都有完善的日志记录

下文将就Jenkins做简单的探索和介绍。

Jenkins安装
----
Jenkins支持多种操作系统和版本控制软件（包括CC）。
安装教程见[https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins)

本文选取开发环境 Ubuntu14.04，git-server(部署在本地的git仓库)。

安装方法:

	sudo apt-get install jenkins

安装完成后，jenkins会自动启动服务，端口号默认8080。
在web浏览器中，访问 [localhost:8080](localhost:8080)即可，界面如图所示

![](http://i.imgur.com/2wuFnt6.jpg)

Jenkins配置
----
在安装完成后，可以通过[localhost:8080/manage](localhost:8080/manage)配置Jenkins服务的参数。

#####系统配置
点击Configure System可以进入系统配置页面。其中可以配置Jenkins的工作目录、访问端口、邮件通知服务器，git/jdk/ant/maven等工具的参数等。

#####安全配置
点击Configure Global Security进入安全配置页面。其中可以配置安全验证等。

#####插件管理
点击Manage Plugins进入插件管理页面。其中可以进行插件的更新、安装、卸载等。

#####系统信息
点击System Information进入系统信息页面。其中可以看到系统信息，环境变量，插件信息等

#####负载统计
点击Load Statistics进入负载统计页面。其中可以看到资源利用情况。

#####还有很多其他选项如System Log等

Jenkins插件
----
Jenkins中很多功能，如与版本控制工具的连接，都是通过插件来完成的。

通过主页->Manage Jenkins->Manage Plugins进入插件管理页面，地址为[http://localhost:8080/pluginManager/](http://localhost:8080/pluginManager/)。
其中可以进行插件的更新、安装、卸载等。如图：
![](http://i.imgur.com/GDVJnt4.png)

- Updates：可更新的插件列表
- Avaiable：可安装的插件列表
- Installed：已安装的插件列表
- Advanced：设置代理、上传插件

Jenkins默认没有安装git插件，需要安装`git plugin`插件

**注意：**在安装插件时，最好处在翻墙状态。安装完成后，需重启Jenkins服务

一个简单的定时任务
----
Jenkins的执行单元为任务(job)。每一个Job都有自己的工作空间，默认在`var/lib/jenkins/job/job_name/workspace`中。

###任务的创建和配置

首先创建一个简单的定时任务。

点击主页中的New Item,进入创建页面，如图：
![](http://i.imgur.com/2j8ODxx.jpg)

这里选取Freestyle project，ok进入job配置页面。job配置页面中有许多配置选项：

#####Source Code Management
Source Code Management中可以设置代码的来源，如git,Subversion等。下一节有详细的介绍。
#####Build Triggers
Build Triggers中设置执行的触发器。设置定时任务时，选取Build periodically选项，如图
![](http://i.imgur.com/CquBVju.png)

时间的格式为`MINUTE HOUR DOM MONTH DOW`，详细介绍可以见Jenkins此处的帮助，点击Schedule栏右边的`？`即可

图中，`H/30 * * * *`代表每30分钟执行一次任务（不一定是整点）

#####Build
Build中设置构建的详细方法，共四种构建动作：

- Execute Windows batch command
- Execute shell
- Invoke Ant
- Invoke top-level Maven targets

在Execute shell时，最好将shell命令写成shell脚本，放入job的workspace中执行。简单的命令最好分多个step执行。

定时任务的设置如图：
![](http://i.imgur.com/bjXGf5V.png)

`date +%T>>result`将当前时间输出到result文件中，`./main`执行main程序。

**注意：**执行shell时的当前路径是此job的workspace,这里的文件路径都是相对workspace来说的。

#####Post-build Actions
Post-build Actions中设置构建后的动作，包括E-mail Notification, Publish JUnit test result report, Deploy war/ear to a container(需要`Deploy plugin`)等。

###任务的执行和记录
配置好任务后，任务会按照设定好的时间去执行。每次执行不仅会有成功或失败的返回值，还会有这次执行的详细记录。具体记录可以见`Console Output`选项，如图：
![](http://i.imgur.com/i6RPM50.png)

一个代码驱动的任务
----

在实际应用中，Jenkins更多地通过插件是与各种版本管理工具SCM结合起来，比如与SVN、CVS、Git、ClearCase等工具。当代码仓库中代码出现变化后，Jenkins可以自动执行构建、部署、测试等任务。

本文采用了Git作为代码仓库。Git仓库搭建在本地服务器上，采用ssh-key连接。

###任务的创建和配置

首先创建一个简单的代码驱动任务。

在创建job时，选取Freestyle project，ok进入job配置页面。job配置页面中有许多配置选项

#####Source Code Management
Source Code Management中选择代码的来源，如图：
![](http://i.imgur.com/VxlQRP4.jpg)

Repository URL中输入git工程的地址，图中为本地的代码仓库
Credentials中选取所使用的信用凭证。没有的时候可以Add一个，kind选取SSH Username with private key,Private Key直接输入私钥值。public，private key的生成方法同普通git一样。本地仓库中，public key写入代码仓库所在的用户主目录认证文件中，这里为`/home/hjh/.ssh/authorized_keys`。

#####Build Triggers
Build Triggers中设置执行的触发器。设置代码驱动任务时，选取Poll SCM选项，如图
![](http://i.imgur.com/7kC06TY.jpg)

图中，`0,15,30,45 * * * *`代表每15分钟检测代码仓库中的变化，如果变化就拉取新的代码。

#####Build
在代码驱动的任务中，最适合的Build的构建动作应该是ant和maven等工具，适合对于Java工程的自动构建。这里源代码是C++,所以只采用了`make`来构建


###任务的执行和记录
同其他任务一样，任务的每次执行都会有这次执行的详细记录。特别地，还有拉取代码时的记录。

使用hooks监视代码
----
如上所述，Jenkins的使用通常与代码仓库联系在一起。通常的需求是当代码仓库中代码变化时，Jenkins能迅速知晓，并执行给定任务。对应的实现方法有两种：

1. 采用轮询机制(Poll SCM)，让Jenkins定时访问代码库，检查是否出现变化。前一节即采取了这个办法。该方法有一定的延迟，缺点明显。
2. 采用代码仓库的hooks作为触发器，触发Jenkins的自动构建。本节下文将以Git为例介绍该方法。

###Git Hooks
钩子(hooks)是Git等工具中的一些脚本，按照特定名字命名，在指定条件（如pre-commit, post-commit）下执行。
在Git中，每个Git仓库有一个hooks目录，本文中Git仓库为`/home/hjh/gitRepo/project.git/`，其对应hooks目录为`/home/hjh/gitRepo/project.git/hooks` 。在默认情况下，每个hook脚本是以`.sample`结尾的，代表不生效；去掉这个结尾，则会生效。常见hook分为服务端（如post-receive）和客户端(如post-commit)。本文使用了`post-receive`脚本，以达到服务器端接收新的push之后，触发Jenkins执行任务的目的。

###post-receive
`post-receive`脚本，是在服务器端接收新的push之后自动执行的。本文以`shell`为执行语音，其实`python`、`perl`都可以，只要服务器端有解释环境。

因为Jenkins是用浏览器管理的，因此为每个任务的立即执行给出了url。在`post-receive`脚本直接访问该url即可。以下为该脚本的全部代码, 其对应的任务名为test2：

	#!/bin/bash
	curl localhost:8080/job/test2/build?delay=0sec

**需要注意的是：**

本文中Jenkins设置比较简单，没有账号管理和权限功能。如果增加这些功能的话，首先登录用户，得到cookies，才能再访问此url。可以用`python`+`urllib`+`urllib2`+`cookielib`实现。

###执行效果

当在本地修改代码（增加一次输出`output once again in 1121`）并push之后，会立即触发Jenkins的任务执行，输出如图所示：
![](http://i.imgur.com/8CheUL7.jpg)

总结
----
同Jenkins所宣传的一样，Jenkins的确实现了跨平台、可扩展、易安装、易配置等特性，很好地支持了代码的自动化构建、部署及测试。

关于本文任何问题，可以联系`zeker@qq.com`。
