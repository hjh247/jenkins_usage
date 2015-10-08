Jenkins 使用探索       
==========
<br />

Jenkins介绍
----
**Jenkins**是一个**可扩展的,跨平台的，开源**的持续集成引擎。

官方网站地址[http://jenkins-ci.org](http://jenkins-ci.org "官方网站")

[https://wiki.jenkins-ci.org/display/JENKINS/Home](https://wiki.jenkins-ci.org/display/JENKINS/Home"wiki地址")中有jenkins的使用介绍。

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

###系统配置
点击Configure System可以进入系统配置页面。其中可以配置Jenkins的工作目录、访问端口、邮件通知服务器，git/jdk/ant/maven等工具的参数等。

###安全配置
点击Configure Global Security进入安全配置页面。其中可以配置安全验证等。

###插件管理
点击Manage Plugins进入插件管理页面。其中可以进行插件的更新、安装、卸载等。

###系统信息
点击System Information进入系统信息页面。其中可以看到系统信息，环境变量，插件信息等。

###负载统计
点击Load Statistics进入负载统计页面。其中可以看到资源利用情况。

###还有很多其他选项如System Log等

Jenkins插件
----
Jenkins中很多功能，如与版本控制工具的连接，都是通过插件来完成的。

通过主页->Manage Jenkins->Manage Plugins进入插件管理页面，地址为[http://localhost:8080/pluginManager/](http://localhost:8080/pluginManager/)。
其中可以进行

to be continue
===
一个简单的定时任务
----



一个代码驱动的任务
----



总结
----


!
### Built exclusively for Markdown ###
A single backtick in a code span: `` ` ``

A backtick-delimited string in a code span: 
<!--lang: cpp-->
	using namespace std;
	
	int main(void){
		int a=1;
		cout<< a<<endl;
	}

Enjoy first-class Markdown support with easy access to  Markdown syntax and convenient keyboard shortcuts.

Give them a try:

- **Bold** (`Ctrl+B`) and *Italic* (`Ctrl+I`)
- Quotes (`Ctrl+Q`)
- Code blocks (`Ctrl+K`)
- Headings 1, 2, 3 (`Ctrl+1`, `Ctrl+2`, `Ctrl+3`)
- Lists (`Ctrl+U` and `Ctrl+Shift+O`)

### See your changes instantly with LivePreview ###

Don't guess if your [hyperlink syntax](http://markdownpad.com) is correct; LivePreview will show you exactly what your document looks like every time you press a key.

### Make it your own ###

Fonts, color schemes, layouts and stylesheets are all 100% customizable so you can turn MarkdownPad into your perfect editor.

### A robust editor for advanced Markdown users ###

MarkdownPad supports multiple Markdown processing engines, including standard Markdown, Markdown Extra (with Table support) and GitHub Flavored Markdown.

With a tabbed document interface, PDF export, a built-in image uploader, session management, spell check, auto-save, syntax highlighting and a built-in CSS management interface, there's no limit to what you can do with MarkdownPad.
