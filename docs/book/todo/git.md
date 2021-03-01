1. 删除master提交记录, 保留代码: 创建一个不含提交记录的分支, 重新提交, 删除原来分支, 重命名分支为master, 强制更新
```shell
git checkout --orphan tmp
git add .
git commit -am "init: init commit"
git branch -D master
git branch -m master
git push -f origin master

```

2. 
```shell
git config --global --unset http.proxy
```