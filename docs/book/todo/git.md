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

3. git pull 的时候 error: cannot lock ref xxx unable to update local ref
   或许是因为分支存在大小写同名，pull 之前 gc 一下即可 （设置 ignorecase 似乎不起作用）
```shell
git gc
# or
git gc --prune=now
```

4. 设置大小写属性
```shell
git config --get core.ignorecase
git config core.ignorecase false
```