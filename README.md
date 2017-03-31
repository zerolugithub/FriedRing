#FriedRing
##简介
通过mitmproxy实现了交互脚本的录制，通过multimechanize实现了并发测试和测试报告（html格式）生产，同时格式化了mitmproxy脚本为requests格式
##基础
1、mitmproxy

2、multimechanize

3、requests

##安装mitmproxy和multimechanize
###Mac or Unbuntu
	pip install mitmproxy
	pip install -U multi-mechanize
	pip install requests
###Windows
	python -m pip install --upgrade pip(最支持版本8.1.2,多次运行可以升级到对应版本) []()
	python -m pip install netlib pyopenssl pyasn1 urwid pil lxml flask
	python -m pip install pyamf protobuf
	python -m pip install pil
	python -m pip install nose pathod countershape
	python -m pip install matplotlib
	python -m pip install mitmproxy
	pip install -U multi-mechanize
	pip install requests
##安装FiredRing

	pip install -U FiredRing

##使用FriedRing
####录制脚本
首先，输入命令

	 fr -p 8888 -w scriptsolution
	 
-p 端口号，-w 测试脚本文件夹

其次，在测试浏览器或者测试手机中设置代理（ip为运行主机ip，端口为888）
按照功能测试流程进行功能测试，在当前文件夹中会产生一个scriptsolition的文件夹，结构如下：
	
	scriptsolution/config.cfg(multimechan的配置文件）
	
	scriptsolution/test _ scripts/v_user.py（默认的初始化脚步）
	
	scriptsolution/test _ scripts/script.py（生成的测试脚步）

在录制完成后，需要修改scriptsolution/test _ scripts/script.py文件，去掉不属于本次测试的请求。

同时可以通过加入assert等信息做断言（详情可以参考requests包）



####运行脚本
#####Mac or Unbuntu
在scriptsolution的父文件夹（也就是fr的workspace），执行

	fr -r s 
	
	fr -r p

参数说明：
s - 线性执行当前父文件夹（workspace）下的全部性能测试场景
p - 并发执行执行当前父文件夹（workspace）下的全部性能测试场景

测试结果在当前父文件夹（workspace）下的Report文件夹内，分为并发测试报告（Report/Parralle_Result/文件夹下）和线性执行测试报告（Report/Serial_Result/)

fr -r p后的扩展参数：

	 -t is runtime that duration of test (seconds)
	 -u is rampup that duration of user rampup
	 -i is resultinterval that time series interval for results analysis (seconds) 
	 -b is progressbar that turn on/off console progress bar during test run default = on
	 -c is consolelogging that turn on/off logging to stdout default = on
	 -x is xmlreport that turn on/off xml/jtl report default = off
	 -v is vusers that number of threads/virtual users for each scenrio default=10
	

#####Windows

在scriptsolution的父文件夹，执行

	C:\FriedRing>python c:\Python27\Lib\site-packages\multimechanize\utilities\run.py scriptsolution
	
##查看结果
结果在scriptsolution文件夹下的results里面，按照时间顺序生产的文件夹，里面有一个result.html，用浏览器打开就可以看到结果信息了。
## 源代码地址
	https://github.com/crisschan/FriedRing
## config文件

config文件在脚本的根目录，文件名字config.cfg
格式如下：

	[global]
	run_time = 300
	rampup = 300
	results_ts_interval = 30
	progress_bar = on
	console_logging = off
	xml_report = off
	results_database = sqlite:///my_project/results.db
	post_run_script = python my_project/foo.py
	
	[user_group-1]
	threads = 30
	script = vu_script1.py
	
	[user_group-2]
	threads = 30
	script = vu_script2.py


其中[global]是场景全局配置[user_group-*]是各个脚本的配置


Global Options



	run_time: 测试时长 (seconds) [required]
	rampup: vuser也就是虚拟用户的启动时间（例如100个vusers，rampup要是10秒的话，就是1秒钟启动10个vusers） (seconds) [required]
	results_ts_interval: 结果分析采样点时间间隔 (seconds) [required]
	progress_bar: 测试过程中console是不是显示执行进度条（on/off） [optional, default = on]
	console_logging: 标准输出日志开关on/off [optional, default = off]
	xml_report: xml格式报告开关on/off [optional, default = off]
	results_database: 数据库连接字符串 [optional]
	post_run_script: 测试完成后要调用的脚本[optional]
User Groups

	threads: 并发线程数（vusers）
	script: 测试脚本