# meizitu

	爬虫练习当然得从宅男福利开始，本阁主从网站上爬取了1063套图片，现在开始进行分析。
	首先是导入所需包，代码我就不贴了。
	从数据库中读取文件，pandas有很多相关函数，可以读取各种文件：read_csv，read_excel，read_sql等，具体的可以自行阅读官方文档，百度也可以搜到。
	pd.read_sql(SQL,conn)，需要两个参数，SQL是就是SQL语句，看你想要读取哪个table的哪些column的数据，conn是连接数据库的，windows系统中建议写绝对路径。
	![image]https://github.com/liubo-lb68/lb68/blob/master/%E4%BB%A3%E7%A0%811.PNG)
	爬取网站上的套图，并将套图的名字、编号和图片数量存入数据库中。
	为了增强代码的复用性，我将编写了几个类，完成不同的任务，其中getimg负责解析网页并下载到本地文件夹中。
	sqlite_lb.py中定义了class Sqlite，负责连接数据库，插入数据到数据库，参数SQL是要插入的内容。这个数据库我已经通过命令行建好了，如果没建就会出错，至于为什么我要在命令行中建，而不通过python建表，那是因为在cmd中简单，一句话就行了，而且建一个表就行了，代码没有复用，没有必要用python写。
