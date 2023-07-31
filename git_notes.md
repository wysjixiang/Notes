#### Git使用



### 一. 基本操作

1. 建立本地与remote的链接，这样才能提交代码到remote

​	git branch --set-upstream-to=origin/新分支名

2.  Git的简单使用，包括git status、git fetch、 git pull、 git push、 git -m、git add、 git restore、git checkout、git branch等
3. git merge、git rebase、 git cherry-pick的区别：[链接](https://blog.csdn.net/weixin_64314555/article/details/121567879#:~:text=merge%20%E6%8A%8A%E5%8F%A6%E4%B8%80%E4%B8%AA%E5%88%86%E6%94%AF%E5%90%88%E5%B9%B6%E5%88%B0%E5%BD%93%E5%89%8D%E5%88%86%E6%94%AF%E4%B8%8A%E3%80%82%20rebase%20%E6%8A%8A%E5%BD%93%E5%89%8D%E5%88%86%E6%94%AF%E7%9A%84%E6%8F%90%E4%BA%A4%E5%9C%A8%E5%8F%A6%E4%B8%80%E5%88%86%E6%94%AF%E4%B8%8A%E9%87%8D%E6%BC%94%E3%80%82%20%EF%BC%88%E5%A6%82%E6%9E%9C%E5%8F%AF%E4%BB%A5%E6%88%90%E5%8A%9F%E9%87%8D%E6%BC%94%EF%BC%8C%E6%9C%AC%E5%88%86%E6%94%AF%E5%B0%86%E4%BC%9A%E6%B6%88%E5%A4%B1%EF%BC%89,cherry%20-%20pic%20k%20%E6%8A%8A%E6%9C%AC%E5%88%86%E6%94%AF%E6%88%96%E8%80%85%E5%85%B6%E4%BB%96%E5%88%86%E6%94%AF%E7%9A%84%E6%9F%90%E4%B8%80%E6%AC%A1%E6%88%96%E6%9F%90%E5%87%A0%E6%AC%A1%E6%8F%90%E4%BA%A4%EF%BC%8C%E5%9C%A8%E5%BD%93%E5%89%8D%E5%88%86%E6%94%AF%E4%B8%8A%E9%87%8D%E6%BC%94%E3%80%82)
   1. git merge相当于多了一个合并的记录，合并的分支会指向这个合并后的head
   2. git rebase会变基，当前分支会相对于另一个分支变基，这样会生成一个线性的提交记录。遇到冲突时当前分支的第一条提交会去和另一个分支的最新内容比较
   3. 





### 二. 历史回退

1. 当你需要回退的时候，并且不想保留某个提交之后的记录，那么可以在本地使用git reset --hard 来回退，这时候HEAD指针会指到回退的版本。但此时远程仓库（如果你有的话）的记录并没有更改，此时你可以有两种方式来解决冲突
   1. 使用git push -f  参数来强制推送到远程仓库，并且同时删除远程仓库的相关记录。这个方法方便简单，但是如果有其他共同开发人员就不太适合了。
   2. 新建一个分支，然后再由版本管理人员来合并到主分支
