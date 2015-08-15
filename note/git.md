# 经典 git work follow

![git-work](/resource/git-work.png)

# 推荐的 git 多人协同模式

## 前置约定

```
1. 以代码为主的项目,项目主负责人在项目初始化时,确保中心仓库有两个远程分支: master, dev.其中 master 是稳定的主线分支; dev 是从 master 上切出来的功能开发分支.
2. 建议项目负责人上 gitlab,在项目 Settings 里面将默认分支设定为 dev.
```

## 几点约定

- 尽可能的在自己的本地分支上开发新功能.
- dev 分支功能开发分支,是可供运行与测试的 hot code 分支,请确保每个合并到 dev 分支的功能可运行,可测试.
- master 分支为稳定主线分支,以在 master 分支打 tag 的静态版本来上线或回滚.
- 专人负责将 dev merge 到 master, 打 tag 和上线.
- 除非有必要,不建议将本地分支推送到远端
- 合并代码的基本原则: 先人后己

## 多人协作


### 开发排期较长(约定2天以上为长开发周期)

```
1. 参与项目,从中心仓库拉取代码
    $ git clone 工作项目的gitlab路径
2. 创建本地开发分支,以 feature 为例,具体的开发分支尽量取有意义的英文名
    $ git checkout master       # 从 master 分支创建最新开发分支,限定当前开发分支不依赖其他任何分支代码
    $ git pull origin master    # 容错操作
    $ git branch feature        # 创建本地开发分支
    $ git checkout feature      # 切到本地的开发分支
    $ git push origin feature   # 多人参与一个开发分支,所以需要将此分支推送到远端
  仅参与开发的同学,在执行完第1步后,执行以下命令:
  $ git fetch --all
  $ git checkout feature
3. 在本地分支开发新功能
4. 将新功能代码提交到本地分支中
    $ git add 新文件/修改的文件
    $ git commit            # 提交到本地
5. 将新功能推送到远端
    $ git push origin feature
6. 发起提测邮件进入测试流程.如果有问题,请重复 2-5 步;如果测试确认没有问题,发起上线.邮件形式通知相关人员
7. 项目负责人做 code review,并将 feature merge 到 master,完成上线
    $ git checkout master
    $ git pull origin master    # master 上有可能存在 hotfixes 代码
    $ git merge feature
    $ git push origin master
    打 tag,推送 tag 到远端,执行上线.
8. 完成上线后将新上线的功能 merge 到 dev 分支,并通知 dev 分支开发的同学拉取最新代码
    $ git checkout dev
    $ git pull origin dev   # 拉取远端最新代码
    $ git merge master      # 将 master 上 feature 功能 merge 到 dev 分支
    $ git push origin dev   # 将 feature 功能推到 dev 的远端
9. 为了防止以后误操作,做好善后
    $ git push origin :feature  # 删除已经完成上线的远端分支
    $ git branch -D feature     # 删除本地分支
```

### dev 分支多人开发

适合短平快的开发模式,开发周期在2天以内

```
1. 参与项目,从中心仓库拉取代码
    $ git clone 工作项目的gitlab路径
2. 创建本地开发分支,以 feature 为例
    $ git branch feature    # 创建本地开发分支
    $ git checkout feature  # 切到本地的开发分支
3. 在本地分支开发新功能
4. 将新功能代码提交到本地分支中
    $ git add 新文件/修改的文件
    $ git commit            # 提交到本地
5. 切到 dev 分支,并更新 dev,将他人的成果 merge 到本地分支,确保代码与中心库一致
    $ git checkout dev
    $ git pull origin dev
    $ git checkout feature
    $ git merge dev
merge 如果有冲突,请解决冲突后,再执行第 4 步.如果没有冲突,刚可以将新功能代码 merge 到 dev 分支.
    $ git checkout dev
    $ git merge feature
6. 将新功能推送到远端
    $ git push origin dev
7. 发提测邮件,进入测试流程.如果有问题,请重复 2-6 步;如果测试确认没有问题,发起上线邮件申请.
8. 项目负责人 code reveiw,并将 dev merge 到 master,完成上线
    $ git checkout master
    $ git pull origin master
    $ git merge dev
    $ git push origin master
    打 tag,推送 tag 到远端,执行上线.
```

### hotfixes

难免线上出现紧急问题,紧急修复流程如下:

```
1. 拉取最新线上代码,并创建紧急修复分支
    $ git checkout master
    $ git pull origin master
    $ git branch hotfixes
    $ git checkout hotfixes
2. 完成修复并提交代码
    $ git add 修复的代码文件
    $ git commit    # 提交到本地
    $ git checkout master
    $ git merge hotfixes
    $ git push origin master
3. 在线上预览机回归
4. 完成上线后的善后
    $ git checkout master
    $ git pull origin master
    $ git checkout dev
    $ git pull origin dev
    $ git merge master  # 把 hotfixes 的代码 merge 到 dev 分支
    $ git branch -D hotfixes    # 删除临时分支,防止以后误操作
```

## 申请测试的提测邮件模板

`收件人`: rd, qa, pm, op

`标题`: 『提测』xx功能

`正文`:

1、提测项目与分支

```
# 以 pandora 为例子
项目: pandora
分支: dev
```

2、功能点说明

```
  1). 例行升级维护
  2). 其他
```

3、预计上线时间和其他说

```
# 请简单说明
```

## 申请上线的邮件模板

回复『提测』邮件,将标题换为『上线』

在正文件中贴上最后提交记录,形如:

```
07125f0..0ebf345  master -> master
```

以上内容作用有2点:

- 供 op 在上线后确认线上代码版本与代码库一致,确保代码上线正确性
- 回滚操作的重要依据.
