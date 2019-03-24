环境变量
~~~
Anaconda2
Anaconda2\Scripts
Anaconda2\Library\bin
~~~

创建环境
`conda create --name python36 python=3.6`

激活
`activate python36 #for windows`

取消激活
`deactivate`

conda的包管理与pip类似。conda可以理解为一个工具，其核心功能是包管理与环境管理.
conda install pyspark
conda list # 查看已安装的packages

更新conda
~~~
conda  update conda #不仅仅会更新conda的版本，同时会自动更新相关的包
conda update anaconda #更新Anaconda版本
~~~

创建新环境
~~~
conda create --name snowflakes biopython
~~~

其中snowflakes代指环境的名称，biopython指要在新环境中添加的软件包，这里并没有指定新的环境所要使用的Python版本，所以会使用当前环境使用的Python版本

查看当前环境
`conda info --envs`

复制环境
`conda create --name flowers --clone snowflakes`

导出配置文件
`conda env export --name snowflakes > snowflakes.yml`

根据配置文件导入环境
`conda env create -f snowflakes.yml`

查找软件包
`conda search beautifulsoup4 # 罗列出所有可用的版本并在已经安装的版本前加*`

安装软件包
`conda install --name beautifulsoup4=4.4.1`

更新软件包
conda update --name snowflakes beautifulsoup4=4.5.1

卸载包
`conda remove --name snowflakes biopython	# 删除指定环境中的指定包`

卸载环境

`conda remove --name snakes --all 	# --all参数表示移除环境中的所有软件包，即删除整个环境`