更新	
`git pull`

提交	
`git push`

设置和查看用户名密码
~~~
git config --global user.name   //获取当前登录的用户
git config --global user.email  //获取当前登录用户的邮箱
git config --global user.name 'userName'    //设置git账户，userName为你的git账号，
git config --global user.email 'email'
~~~

回滚版本
~~~
git reset --hard HEAD/commit_id
~~~