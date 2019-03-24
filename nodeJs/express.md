var app = express();
app.get()
app.listen()
res.send

## Express 应用生成器
~~~
npm install express-generator -g
express app
~~~
启动这个应用（MacOS 或 Linux 平台）：

~~~
$ DEBUG=myapp npm start

~~~

Windows 平台使用如下命令：

~~~
> set DEBUG=myapp & npm start
~~~

## 路由


## 托管静态文件
~~~
app.use(express.static('public'));

app.use('/static', express.static('public'));
~~~