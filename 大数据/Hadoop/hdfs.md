## 常用命令
#### 显示文件
`hadoop fs -ls`
`hadoop fs -lsr`	递归查看
#### 文件数量
`hadoop fs -count /目录`
#### 文件取样
`hadoop fs -cat 目录/* | shuf -n 5`
`hadoop fs -cat 目录/* | head -100`
`hadoop fs -cat 目录/* | tail -5`
#### 获取文件
`hadoop fs -get  /目录   目录`
#### 存入文件
`hadoop fs -put hadoop-env.sh /user `
#### 查看内容
`hadoop fs -cat 文件`
#### 内容行数
`hadoop fs -cat  /文件* | wc -l`
#### 删除文件
`hadoop fs -rmr /user/hello`
`hadoop fs -rm /user`
#### 移动/复制
`hadoop fs -mv /hello /user`
`hadoop fs -cp /user/hello /user/root`
#### 文件大小
`hadoop fs -du -s -h 目录或者文件`
#### 创建
`hadoop fs -mkdir 目录`
`hadoop fs -touchz /hello`

