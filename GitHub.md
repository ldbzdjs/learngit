1.安装git:
1) 在Linux上安装Git
1）Debian或Ubuntu Linux：sudo apt-get install git
2）老一点的Debian或Ubuntu Linux，要把命令改为sudo apt-get install git-core
3）其他Linux版本:先从Git官网下载源码，然后解压，依次输入：./config，make，sudo make install这几个命令安装就好了。
4）安装完成后，还需要最后一步设置，在命令行输入：
     $ git config --global user.name "Your Name"
     $ git config --global user.email "email@example.com"
注意：git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

-----------------------------------------------------------

2.创建版本库
1）创建一个空目录：mkdir learngit
2）把learngit目录变成git可以管理的仓库：
	在learngit目录里输入：git init 
	然后查看是否有.git目录
3）添加文件到learngit仓库：例如readme.txt文件
	第一步，用命令git add告诉Git，把文件添加到仓库：git add readme.txt
	第二步，用命令git commit告诉Git，把文件提交到仓库：git commit readme.txt
4）告知我们当前仓库的状态：是否被修改过，但还没有准备提交：git status
5）查看被修改的内容(仅限于添加到了缓存区，还未提交)：git diff learngit.txt
6）工作区和暂缓存区：
工作区：就是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区；
暂缓存区：git版本库里存了很多东西，其中最重要的就是称为stage(或者叫index)的暂存区，我们往git版本库里添加信息时，分为两步骤：
第一步：git add 把文件添加进去，实际上就是把文件修改添加到暂存区；
第二步：git commit 提交更改，实际上就是把暂存区的所有内容提交到当前分支。
注意：创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

-----------------------------------------------------------

3.版本回退：
1)显示从最近到最远的日志提交：git log --pretty=oneline
2）在git中用HEAD表示当前版本，上一个版本可以用HEAD^表示，再上一个版本可以用HEAD^^ 表示，如过要回退到上一个版本可以使用：git reset --hard HEAD^ 或 git reset --hard <commit id(提交是自动生成的版本号id)>
4）要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

-----------------------------------------------------------

4.管理修改：
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file（命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令）
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

-----------------------------------------------------------

5.删除文件
1）从版本库中删除文件：git rm <name>,然后git commit -m "备注"
2）删除错了，版本库中还存在，需要恢复到最新版本：git checkout -- <name>
备注：git checkout 其实是用版本库里的版本替换掉工作区的版本，无论工作区是修改（仅限于未执行git add <name>提交到暂存区的文件，已经提交到暂存区的文件执行git checkout -- <name>无法还原）还是删除，都可以“一键还原”。

-----------------------------------------------------------

5.远程仓库：
1）由于你本地git仓库和GitHub仓库之间的传输是通过SSH加密的，所以需要进行设置：
a）在用户主目录下，寻找.ssh文件，有则查看是否有id_rsa和id_rsa.pub文件，没有则进行创建：
ssh-keygen -t -rsa -C "youremail@example.com"
执行后可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以给别人；
b）登陆GitHub，打开“Account settings”，“SSH Keys”页面，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容，点“Add Key”；

2）添加远程库
场景：已经创建了本地git仓库，又想在git上创建一个仓库，让这两个仓库远程同步。
a）登录GitHub，创建一个和本地仓库同名的仓库；
b）在本地learngit仓库下执行：git remote add origin git@github.com:<你自己的GitHub账户名>/learngit.git
示例：git remote add origin git@github.com:test/learngit.git
注意：远程库的名字就是origin，这是Git默认的叫法，也可以改成别的。
c）把本地内容推送到远程仓库中：git push -u origin master
解析：git push命令是把当前分支master推送到远程master；第一次推送加上 -u ，不但会把本地master分支内容推送到远程master分支，还会将本地master与远程master关联，方便以后的推送或者拉取，以后本地做了提交只需要执行 git push origin master 命令即可。

3）删除远程库
a）可以先查看下远程库信息：git remote -v,然后再执行删除命令：git remote rm <name>(此处的“删除”是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除)

4）远程库克隆
场景：已经在github上创建一个仓库，想拉去到本地。
命令：git clone git@github.com:<自己的git账号名>/<远程的git仓库名.git>
 示例：git clone git@github.com:mygithubname/gittest.git

-----------------------------------------------------------

6.分支管理
1）创建与合并分支
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>或者git switch <name>
创建+切换分支：git checkout -b <name>或者git switch -c <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>

2）解决冲突
a）Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容。
b）查看分支合并情况命令：git log --graph --pretty=oneline --abbrev-commit

3）分支管理策略：
通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
操作命令步骤：
1）git add <name>
2）git commit -m “备注信息” <name>
3）git merge --no-ff -m "备注信息" <name>


4）Bug分支
场景：开发过程中修复Bug
a）需要先储藏当前未完成的开发分支，比如dev,在dev分支下执行git stash, 此时用 git status查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。
b）确认在那个分支上修复bug，加入在master上修复，就从master上建立临时分支：git checkout -b issue-101,在临时分支issue-101上修复提交完毕后，删除临时分支，切换到还未完成的开发分支dev。
c）查看储藏的工作区：
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
d）恢复储藏的工作区：
方法一：用git stash apply，恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
方法二：用git stash pop，恢复的同时把stash内容也删了。
e）此时同样的bug在dev上也要修复，可以使用命令：git cherry-pick <issue-101提交后的commitid>

5)feature分支
开发一个新feature，最好新建一个分支；
如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

6）多人协作
a）当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin；要查看远程库的信息，用 git remote 或者，用 git remote -v 显示更详细的信息
b）推送分支
把本地所有的提交都推送到远程库，推送时指定本地分支，这样git就会把该分支推送到远程库对应的远程分支上: git push origin master ,如果要推送其他分支，比如dev：git push origin dev
c）哪些分支需要推送
    master分支是主分支，因此要时刻与远程同步；
    dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
    bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
    feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。
7）抓取分支
场景：多人协作时，大家都会往master 和dev 分支上推送各自的修改，别人提交后你提交。
    查看远程库信息，使用git remote -v；
    本地新建的分支如果不推送到远程，对其他人就是不可见的；
    从本地推送分支，使用git push origin <branch-name>，如果推送失败，先用git pull抓取远程的新提交；
    在本地创建和远程分支对应的分支，使用git checkout -b <branch-name> origin/<branch-name>，本地和远程分支的名称最好一致；
    建立本地分支和远程分支的关联，使用git branch --set-upstream <branch-name> origin/<branch-name>；
    从远程抓取分支，使用git pull ，如果有冲突，要先处理冲突。

-----------------------------------------------------------

8.标签管理
1）创建标签
a）敲命令git tag <name>就可以打一个新标签；git tag 查看所有标签。
注意：默认打标签是打在最新提交的commit上的。如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，方法是找到历史提交的commit id，然后打上就可以了。
命令：
git log --pretty=oneline --abbrev-commit
git log tag <指定的标签名> <commit id>：示例：git tag v0.9 f52c633
b）查看标签内容：git show <tagname>
c）创建带有说明的标签，用-a指定标签名，-m指定说明文字
命令示例：git tag -a v0.1 -m "version 0.1" 1094ada

2）操作标签
a）删除打错的标签：git tag -d <tagname>
备注：因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。
b）推送标签至远程：git push origin <tagname> ,或者一次性推送全部尚未推送到远程的本地标签：git push origin --tags
c）删除远程标签，需要先删除本地：git tag -d <tagname> ,然后在删除远程：git push origin :refs/tags/<tagname>
