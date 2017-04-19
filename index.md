<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
</head>
<body>

<p>
android studio作为google官方推荐开发工具，随着android系统不断更新，可以预见会在以后逐步替代eclipse成为android application的主要开发工具。
</p>
<div style="width: 800px">
<h5>
<a href="https://developer.android.google.cn/index.html?hl=zh-cn" style="text-decoration: none;">Android studio官方下载地址及安装方式</a>
</h5>
1.android studio在windows下有包含sdk和不包含sdk的安装方式，及无sdk的无安装绿色版。<br>
2.以.zip安装作为例子。<br>
<img alt="下载" src="url.png" width="500px"><br>
3.下载后解压到任何目录下即可，依次进入android-studio > bin > 运行studio64.exe<br>
4.初次运行会提示是否使用既有配置、是否使用代理、是否连网下载android tools及platforms等相关选项，根据自己的系统环境自主选择。
</div>

<div style="width: 800px">

<h5>Android studio配置</h5>
1.android studio安装好后进入到欢迎界面，常用功能包括创建新项目，打开已有as项目，从版本管理中导出项目，导入eclipse项目。<br>
2.可以在欢迎页面中设置相关功能，包括IDE及项目的相关设置。<br>
3.与as相关的配置及环境存放位置，as IDE集成了jre及相关plugins（...android-studio\jre|plugins），默认安装好后是可以直接新建及运行项目的。
sdk会下载到as默认路径或配置的sdk路径下，可以在configure>settings>Appearance&behavior>system settings>android sdk修改。<br>
4.与as配置相关的文件和AndroidStudioProjects存放在同一目录下，.android（存放keystore等），.AndroidStudio2.3（IDE配置存档），.gradle（gradle工具）...。<br>
</div>

<div style="width: 800px">
<h5>Android studio Project</h5>
1.File>New下面可以进行新建、导入、版本管理获取项目，也可以新建、导入Module（功能与ADT libray相同）。<br>
2.File>open Recent下面可以打开其他的as项目。<br>
3.File>close project关闭当前项目返回到欢迎界面。<br>
4.File>settings下面执行IDE及项目的相关设置。<br>
5.File>Project Structure里进行设置。设置项包括sdk、jdk、ndk Location，gradle、plugin Version，plugin、librar Repository。
Modules下可以选择设置app及plugin的build.gradle，包括编译版本，测试版本及相关引用等设置。<br>
</div>

<div style="width: 800px">
<h5>Android studio快捷键</h5>
1.代码提示补全快捷键ctrl+alt+space。<br>
2.导包快捷键alt+enter。<br>
3.生成set和get方法，重写父类方法快捷键alt+insert。<br>
</div>


<div style="width: 800px">
<h5>AS开发小技巧</h5>
1.jcenter库中为项目自动下载添加库，比如：向项目中添加gson解析功能，as>File>Project Structure>Modules>Dependencies>+
Library dependency>Choose Library Dependency>search gson>add ok>自动下载添加build.gradle\dependencies\compile'gson pkg'>自动引用到External Libraries
</div>

</body>
</html>
