# GitNotes  

### git本地仓库和github仓库关联命令  
```
cd folder/
git init
git add .
git commit -m 'first commit'
git remote add origin https://github.com/ZhySir/GitNotes.git
git pull --rebase origin master
git push -u origin master
```
* 进入到需要推送的文件夹
* 初始化目录：git init
* 添加所有文件到本地库：git add .
* 提交文件到本地库：git commit -m 'first commit'
* 将本地库和远程库进行关联：git remote add origin https://github.com/ZhySir/GitNotes.git
* 拉取远程库文件同步到本地：git pull --rebase origin master
* 将本地库文件退到远程库：git push -u origin master
  
### git修改提交者用户名和邮箱  
##### 主动配置：  
```
// 设置全局
git config --global user.name "Author Name"
git config --global user.email "Author Email"
// 设置当前项目库配置
git config user.name "Author Name"
git config user.email "Author Email"
```
##### 如果只需要修改最近一次提交，那么很简单直接使用 git commit --amend 就可以搞定。  
```
git commit --amend --author="NewAuthor <NewEmail>"
```
##### 如果是多个修改，那么就需要使用到 git filter-branch 这个工具来做批量修改为了方便大家使用，封装了一个简单的shell脚本，直接修改 "XXX" 中的变量为对应的值即可。  
```
#!/bin/sh

git filter-branch --env-filter '

OLD_EMAIL="Your Old Email"
CORRECT_NAME="Your Correct Name"
CORRECT_EMAIL="Your Correct Email"

if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```
* 将该脚本另存为 changGitInfo.sh
* 为该文件添加可执行权限
* 将该文件拷贝至指定项目的根目录中
* 执行指令 sh changGitInfo.sh
* 提交到远程库 git push --force
  
