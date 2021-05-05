# Git与Github关联

### 注册+检查.ssh秘钥：由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以我们需要配置验证信息(No such file or directory表示第一次)

### 创建SSH Key： ssh-keygen -t rsa -C 1453344072@qq.com

成功的话会在C:\Users\mimi下生成.ssh文件夹，进去，打开id_rsa.pub，复制里面的key。

### 粘贴id_rsa.pub内容到Github

然后将生成的rsa 的key添加到版本库中即可，方法：
打开自己的版本库，点击右边的 Settings 进入配置页。
然后点击左边导航栏的： Deploy keys 进入添加key页面
然后点击： Add deploy keys ，将自己的内容输入进去就可以了。
这样就完成了。

最后继续提交更改的代码，使用：

git push -u origin master

## 补充：

如果要使用 `git push`简短提交代码：

```
git push1
```

需要配置 :

```
git config --global push.default simple1
```

或者：

```
git config --global push.default matching
```