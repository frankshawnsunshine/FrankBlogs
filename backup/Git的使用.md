# Git使用
## 常用方法

 1. 初始化git库
     ```sh
    git init
     ```
 2. 关联远程仓库（git@172.20.253.19:/home/git/git_web/web.git）
    ```sh
    git remote add origin git@172.20.253.19:/home/git/git_web/web.git
    ```
3. 将修改添加到暂存区
    ```sh
    git add .
    ```
4. 提交修改到本地仓库
    ```sh
    git commit -m '修改信息'
    ```
5. 将本地仓库推送到远程仓库
    ```sh
    git push origin master
    ```
6. 设置名字和邮箱
    ```sh
    git config user.name/email '   '
    ```
7. 生成公钥和私钥
    ```sh
    ssh-keygen -t rsa -C 'email'
    ```
8. 放弃工作区修改
    ```sh
    git checkout -- file
    ```
9. 暂存区回退到工作区
    ```sh
    git reset HEAD file
    ```
10. 强制拉取
    ```sh
    git fetch --all
    git reset --hard origin/master 
    git pull origin master
    ```
11. 重新关联远程仓库
    ```sh
    git remote rm origin
    git remote add origin [url]
    ```

## 分支操作
### 查看分支
查看的git命令如下：
```sh
git branch 列出本地已经存在的分支，并且当前分支会用*标记
git branch -r 查看远程版本库的分支列表
git branch -a 查看所有分支列表（包括本地和远程，remotes/开头的表示远程分支）
git branch -v 查看一个分支的最后一次提交
git branch --merged  查看哪些分支已经合并到当前分支
git branch --no-merged 查看所有未合并工作的分支
```
### 创建和切换分支
- 创建新分支
    ```sh
    git branch 新分支名称
    ```
- 切换分支
    ```sh
    git checkout 分支名称
    ```
- 创建分支的同时，切换到该分支上
    ```sh
    git checkout -b 新分支名称
    ```
### 从远程仓库pull（拉取）代码到本地分支
- 指定远程分支，和本地分支
    ```sh
    git pull origin 远程分支名称:本地分支名称
    ```
### 将新分支推送到远程仓库
```sh
git push origin 分支名称
```
假设我本地创建了一个名为dev的分支，远程仓库还没有这个分支，推送的命令是：
```sh
git push --set-upstream origin dev
```
### 删除分支
- 删除本地分支（不能删除当前所在的分支，如果要删除，必须先切换到其他分支上）
    ```sh
    git branch -d 分支名称
    ```
如果删除时报错：**error: The branch '分支名称' is not fully merged.** （意思是：分支未完全合并）。解决方法是使用 -D 强制删除，代码如下：
```sh
git branch -D 分支名称
```
- 删除远程分支
    ```sh
    git push origin :分支名称
    ```
**注意：分支名称前有个冒号，分支名前的冒号代表删除**
### 合并分支
1. 假如我们现在位于分支dev上，刚开发完自己负责的功能，执行了下列命令：
    ```sh
    git add .
    git commit -m '某某功能已完成，提交到[分支名称]分支'
    git push -u origin 分支名称
    ```
2. 首先切换到master分支上
    ```sh
    git checkout master
    ```
3. 如果是多人开发的话，需要把远程master分支上的代码pull下来
    ```sh
    git pull origin master
     ```
4. 然后把dev分支的代码合并到master上
    ```sh
    git merge 分支名称
    ```
如果git merge的时候出现冲突，可以执行下面的命令取消merge：
```sh
git merge --abort
```
5. 然后查看状态
    ```sh
    git status
    ```
6. 最后一步，Push推送到远程仓库
    ```sh
    git push origin master
    ```

# 给Git配置多个SSH Key
1. 复制命令 ssh-keygen -t rsa -C 'xxxx@youremail.com' -f ~/.ssh/gitee_id_rsa 生成一个Gitee的 SSH Key，一路回车就可以了（记得把邮箱改成自己的）
    ```sh
    ssh-keygen -t rsa -C 'xxxx@youremail.com' -f ~/.ssh/gitee_id_rsa
    ```
2. 复制 gitee_id_rsa.pub ，复制ssh开头的那一串公钥，添加到Gitee仓库
3. 使用命令 `touch ~/.ssh/config`，在 ~/.ssh 文件夹下添加config文件，可以看到文件夹下面多了一个config文件
    ```sh
    touch ~/.ssh/config
    ```
复制以下信息添加到config文件保存，其中 Host 和 HostName 填写git服务器的域名，IdentityFile 填写私钥的路径
```
# gitee
Host gitee.com
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/gitee_id_rsa
```

# 使用Git相关问题
## 集成在IDEA中的问题
1. 将文件上传至github中时问题：OpenSSL SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443
解决：在项目根目录打开git命令行，输入：`git config --global --unset http.proxy`
2. IDEA中git项目无法拉取或更新
    ```sh
    git branch --set-upstream-to=origin/master
    ```

