# GIT

[toc]

## 0.设置

```shell
git config --global user.name"Your Name"
git config --global user.email"email@example.com"
```

## 1.创建版本库

`git init` 

## 2.提交

```
git add readme.txt #把文件添加到暂存区
git commit -m "wrote a readme file" #把文件提交仓库，-m后面输入本次提交的说明
```

## 3.仓库状态

`git status`

```
git diff readme.txt #对比readme.txt自身膝盖前后不同
git diff HEAD -- readme.txt #对比版本库里最新版本和工作区版本的不同
```

## 4.查看日志

### 1、提交历史

`git log`
`git log --pretty=oneline`

### 2、命令历史

`git reflog`

## 5.版本回退

### 1、回退到历史版本

```
git log
git reset --hard HEAD^ #回退到上一个版本
git reset --hard HEAD^^ #回退到上上个版本
git reset --hard HEAD~100 #回退到前100个版本
git reset --hard commitID #回退到某个版本号
```



### 2、回退到未来版本

```
git reflog #必须用此命令找到之前被撤销版本的commit id
git reset --hard commitID
```

## 6.撤销修改

`git checkout -- readme.txt`
把readme.txt文件在工作区的修改全部撤销，让这个文件回到最近一次git commit或git add时的状态

## 7.删除文件

```
rm test.txt
git checkout -- test.txt #误删恢复
```



## 8.强制pull覆盖现有代码

```shell
git fetch --all
git reset --hard origin/master
git pull
```

