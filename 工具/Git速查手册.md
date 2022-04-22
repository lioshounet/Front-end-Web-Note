# Git速查手册

配置

```bash
    git config --add --local user.name 'liyi'
    #配置名字
    git config --add --local user.email 'liyi'
    #配置邮箱
    git config --local --list
    #查看配置
```

分支

```bash
    git branch -av
    #查看分支情况
    
    git checkout -b 分支 远端分支
    #本地和远端分支关联
    
    #创建分支
    git branch lio_dev

    #切换分支
    git checkout lio_dev
    
    #创建分支并切换
    git branch -b lio_dev
```

合并

```bash
git merge lio_dev
#把lio_dev分支合并到当前分支

git merge lio_dev -abort
#忽略其他分支，保留原来的分支

#或者自己删除多余代码
```

附加提示

```bash
#创建仓库第一次上传
git remote add origin https://github.com/lioshounet/Tab_Self_proofreading.git
#删除远程分支
git push origin --delete liyi_dev
#新建分支后第一次同步
git push --set-upstream origin liyi_dev
```