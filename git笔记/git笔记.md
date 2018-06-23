# git安装



#	git设置签名

级别优先级：

​	项目级别优先于系统用户级别，两者都有采用项目级别

```shell
#项目级别/仓库级别 仅在本地库范围有效
git config user.name margo
git config user.email 395308649@qq.com
#查看user 信息保存位置
cat .git/config

#系统用户级别
git config --global user.name margo
git config --global user.email 395308649@qq.com
cat ~/.gitconfig
```

### git config命令编辑配置文件

```shell
格式：git config [-–local|-–global|–-system] -e
查看仓库级的config，即.git/.config，命令：git config -–local -e，
# –list参数不同的是，git config -e默认是编辑仓库级的配置文件。
#section.key 类似前面添加的 user.name
# 添加一个配置项
git config [-–local|–-global|–-system] –-add section.key
# 获取一个配置项
git config [-–local|–-global|–-system] –-get section.key


git config [-–local|-–global|-–system] –-unset section.key 
```



# git命令

## 部分git命令：

```shell
git status
git add [filename]
git commit -m "此处应有备注" [filename]

#查看历史记录
git log #最完整形式
git log #  --pretty=oneline 好看一些 
git log #--oneline
git reflog  Head{[num]}可查看恢复版本的移动次数
#版本回退或前进
git reset --hard [hash索引值]

```



## 远程git命令

## 设置远程仓库别名

```shell
#origin 是远程库别名
git remote add origin https://github.com/being0727/springgitDemo.git
#查看别名记录
git remote -v 

```



## 提交操作

```shell
#提交 推送操作 
git push [远程库别名] [分支名]
git push origin master 

```

## 克隆操作

​	完整的把远程库下载到本地

​	创建origin远程地址别名

​	初始化本地库（生成 .git 文件夹）

```shell
#克隆操作
git clone [url]
#可查看别名记录 会查看到远程库有别名
git remote -v 
```

## 保存凭据的地方

![保存凭据](.\img\保存凭据.jpg)

# 邀请成员加入团队

## github发送邀请,成员接受邀请

![](.\img\邀请成员.png)

margo0727需要打开邀请链接，接受邀请



# 远程库修改的拉取

```shell
#  origin/master  为[远程库别名]/[分支名]
# pull = fetch + merge
git pull origin/master

#拉取修改的文件
git fetch origin/master
#查看修改的内容
git checkout origin/master
#合并拉取下来的内容
git merge origin/master

```

# 协同开发冲突的解决

## 组内成员

如果不是基于远程苦的最新版所做的修改，不能推送，必须先拉取，拉取下来后如果进入冲突状态，则按照“分支冲突解决”操作解决即可

## 非组内成员（例如john）

### John的操作

1. John在github上fork 原来的项目
2. John 将fork的项目clone到本地
3. 本地修改，然后推送到远程库 pull

### 项目主管

1.  在github上接受pull request
2. ![](.\img\查看pull request .png)

3.merge pull request

![](.\img\merge_pull_request.png)





# SSH免密登录

进入当前用户的家目录 $ cd ~ 

删除.ssh 目录 

$ rm -rvf .ssh 

 运行命令生成.ssh 密钥目录 

$ ssh-keygen -t rsa -C  atguigu2018ybuq@aliyun.com   

  [注意： 这里-C 这个参数是大写的 C] 

 进入.ssh 目录查看文件列表 

$ cd .ssh  

$ ls -lF 

 查看 id_rsa.pub 文件内容并复制密钥信息

 $ cat id_rsa.pub 

复制 id_rsa.pub 文件内容， 登录 GitHub， 点击用户头像→Settings→SSH and GPG keys

New SSH Key 

 输入复制的密钥信息

 回到 Git bash 创建远程地址别名 

git remote add origin_ssh  git@github.com:atguigu2018ybuq/huashan.git 



# eclipse使用git

1.Eclipse特定文件需要进行忽略

忽略文件规则：

https://github.com/github/gitignore

https://github.com/github/gitignore/blob/master/Java.gitignore

操作：

1. 新建java.gitignore文件
2. 在~/.gitconfig文件中引入上述文件

```shell
  [core]
	excludesfile = C:/Users/Lenovo/Java.gitignore   
#注意Windows系统下 路径中一定要使用“/”
```

3. 推送到远程库













