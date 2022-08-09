## get 管理

- 注释：先建立仓库再进行操作，以下命令行不代表顺序，根据工作实际情况查看

```css
提交验证名(登录)
git config --global user.name "Your Name"
提交用户邮箱(公司给的个人邮箱地址)
git config --global user.email "邮箱"
用户名查看
git config user.name
邮箱查看
git config user.email
配置信息查看
git config --list
然后初始化本地仓库就可以克隆了
-----
初始化
git init
克隆项目分支
git clone 项目分支名
创建分支
git branch 分支名
切换分支
git checkout 分支名

跟踪新文件
git add index.html

查看文件跟踪状态
git status
添加至暂存-即跟踪所有文件
git add .
取消暂存的文件
git reset HEAD index.vue
添加到本地仓库
git commit -m"注释"
和远程仓库建立 联系
git remote add origin 地址
如果提示错误 没有上游分支 就设置当前主分支为默认上游分支
git push --set-upstream origin master
查看所有远程仓库状态
git remote -v 
提交到远程仓库主分支
git push -u origin master // 设置好号，后续只要git push即可
git push origin 分支名  // 或提交到分支上
删除关联的联系地址
git remote remove origin 第一种
完整提交
git push -u origin master
移除文件 在本地移除指定文件且在下一次提交仓库时将仓库之前提交的该文件也删除
git rm -f index.html
删除本地分支 注:删除分支时不能处于当前分支之下，得切换出去到主分支对其进行删除
git branch -d 分支名
删除远程分支
git push origin --delete 远程分支名
查看分支
git branch
合并分支(先切换到主分支，再将某分支合并到当前主分支)
git merge 被合并的分支名  ; ->合并前记得拉取最新代码先，然后合并完毕再提交 否则切回分支文件就没了
接取服务器代码 (需要处在主分支，即拉取主分支最新的代码，没有更新就会提示没有最新代码)

取消合并
git merge --abort
拉取代码 第一次克隆仓库使用该命令拉取的是主分支的代码
git clone
拉取主分支最新代码 (注:合并分支时都需要进行此操作-即先拉去下最新代码再合并)
git pull
拉取分支代码
git fetch --all
重置代码 回滚到指定版本
git reset --hard 版本号eb317ff5d3cccd1b96d83d4b7b9c4e340f97c2c1  / 让本地冲突文件与服务器文件一致
查看提交历史log
git log 或者 git log --pretty=online
如果回滚过版本-那么就无法查看所有历史记录了，就需要使用如下命令查看所有log记录
git reflog --oneline
回滚以后 如果有需要回到最新版本那么根历史记录跳转最新版本
git reset --hard  最新版本号(历史记录最新的那个编号)
拉取服务器代码至当前分支并进行替换
git reset --hard origin/远程分支名
添加多个远程仓库 
git remote add 17MOX  远程仓库名 ：推送和拉取的时候
git reset --soft HEAD^ 回退到上一版本并保留当前代码修改

-----分割线-----
ssh配置
1：在项目仓库复制公钥生产命令到命令面板回车生成
2：到电脑用户内的.ssh的id_rsa私钥，id_rsa.pud 公钥
3：将私钥复制到仓库对应的ssh密钥框保存即可
```



- 实际开发是多人开发，为了避免冲突，需要拉取 主分支代码，再建立自己的分支并切换到自己的分支写代码，
- 写完自己的代码先 add缓存，再commit -m提交本地仓库，
- 再切换到主分支进行更新git fetch --all;或者git pull
- 接着合并(git merge 自己的分支名 )，合并完成推送 git push -u origin xxx分支名
- 完成再切换到自己的分支写代码。切记 推送完代码一段要切回自己的分支再写剩下的代码
- 如果团队有最新代码或者代码改动。可以切回主分支拉取最新代码
- 如果是开发中代码没写完就先不用删除自己的分支，直接再切回自己的分支写代码即可

总结：自己写什么功能分支就起什么名字。实际工作中不需要我们合并代码一般由部门老大操作合并流程

我们一般是去老大创建的分支拉取代码(此时分支就相当于次主分支，其他同事的代码也是合并到这个次分支)

## 路由模块化

- 一般多人协同开发 要么在路由文件下新建一个文件夹，每个负责的人创建自己的模块化路由，
- 项目比较简单时 就共用一个路由



## 冲突合并

- 在不同分支修改了相同文件时产生的冲突

```css
git checkkout maseter // 先切换到主分支
git merge 次分支      // 合并
// 报错  在两处分支修改了同一个文件，git会在文件冲突出进行提示，程序员可以手动对其进行更改

到主分支合并冲突，再主分支进行如下操作即可
// 手动更改冲突--> 采用当前更改(主分支的更改), 采用传入的更改(次分支), 保留双方更改, 比较变更
// 手动更改冲突后 再添加暂存和提交本地
git add .
gite commit -m "解决了合并冲突问题"
```



## 跟踪分支

- 即从远程仓库下载分支
  - 可以先删除本地分支 再下载远程分支

```css
git checkout 远程分支名称

// 如果想对远程仓库分支进行重命名可以如下操作:
git checkout -b 本地分支名 远程分支名  // 这样可以把远程分支下载来进行重命名
```





## pull拉取最新代码

- 当远程分支上的文件被同事在远程仓库直接编辑修改后，那么本地就需要拉取最新代码
- 使用 git  pull拉取，再进行其他操作 比如合并之类得



## 公钥配置

- 添加公钥可以多人同时使用一个仓库，公司会对仓库不同项目单独生成权限
  - 账户-SSH公钥-输入命令
- 安装完成使用  ssh -T git@gitee.com   检查

```css
ssh-keygen -t ed25519 -C '这里写邮箱或其他随机字母都可以'
 // 接着根据提示3次回车
// 可自行到C盘找到文件或者用git输入命令查找文件
 ~/.ssh/id_ed25519.pub
// 一般在C盘用户里面 C:\Users\大菲\.ssh
// 找到.ssh文件夹里面的 id_rsa.pub  打开复制公钥添加到gitee即可
```



### 备注

- login分支写完登录功能 git status查看代码改动情况，git branch查看所处分支
- git add . 提交缓存，
- git commit -m '完成登录功能';  提交的本地暂存区
- git checkout master  切换到主分支
- git merge login 合并login分支代码到主分支
- git push  推送到主分支
- git checkout login 再切换到次分支写代码   切记
  - 如果想码云仓库也显示有login次分支的话需要进行如下设置
  - 确保已处在次分支login，然后 git push -u origin login
  - 这样就吧本地次分支login 也推送到了远程仓库 
  - 这样我们写的代码就可以选择推到次分支，主分支的代码合并可以交由部门老大去做，以避免出错
- 自己合并代码到主分支
  - git checkout master
  - git  merge 要合并的分支
  - 合并完成再 push到远程仓库 git push
- 这样次分支代码就合并到了主分支，写代码即使切换其它分支也不会造成其它影响
- 接着需要创建其它分支直接创建即可，不会丢失其它分支代码
  - git checkout -b xxx分支名



















